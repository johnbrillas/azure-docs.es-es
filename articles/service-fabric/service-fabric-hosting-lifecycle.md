---
title: Ciclo de vida de activación y desactivación del hospedaje de Azure Service Fabric
description: Se explica el ciclo de vida de ApplicationPackage y ServicePackage en un nodo.
author: tugup
ms.topic: conceptual
ms.date: 05/1/2020
ms.author: tugup
ms.openlocfilehash: f049b19703d37412d1ee1961aee6cb327efabe7c
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96779607"
---
# <a name="azure-service-fabric-hosting-lifecycle"></a>Ciclo de vida de hospedaje de Azure Service Fabric

En este artículo se proporciona información general sobre los eventos que se producen en Azure Service Fabric cuando se activa una aplicación en un nodo y se usan varias configuraciones de clúster para controlar el comportamiento.

Antes de continuar, asegúrese de comprender los distintos conceptos y relaciones que se explican en [Modelar una aplicación en Service Fabric][a1]. 

> [!NOTE]
> En este artículo, a menos que se mencione explícitamente que significa algo distinto:
>
> - La palabra *réplica* hace referencia a una réplica de un servicio con estado y a una instancia de un servicio sin estado.
> - *CodePackage* recibe un tratamiento equivalente al proceso de *ServiceHost* que registra una instancia de *ServiceType* y hospeda réplicas de servicios de dicha instancia de *ServiceType*.
>

## <a name="activation-of-applicationservicepackage"></a>Activación de Application/ServicePackage

La canalización para la activación es la siguiente:

1. Descargue ApplicationPackage. Por ejemplo: ApplicationManifest.xml, etc.
2. Configure el entorno de Application, p. ej.: crear usuarios, etc.
3. Inicie el seguimiento de Application para la desactivación.
4. Descargue ServicePackage. Por ejemplo: ServiceManifest.xml, código, configuración, paquetes de datos, etc.
5. Configure el entorno para ServicePackage, p. ej.: configure el firewall, asigne puertos para puntos de conexión, etc.
6. Inicie el seguimiento de ServicePackage para la desactivación.
7. Inicie SetupEntryPoint de CodePackages y espere a que finalice.
8. Inicie MainEntryPoint de CodePackages.

### <a name="servicetype-blocklisting"></a>Lista de bloqueo de ServiceType
**ServiceTypeDisableFailureThreshold** determina el número de errores (activación, descarga y errores de CodePackage) después de los cuales ServiceType se programa para bloqueo. El primer error de activación, error de descarga o bloqueo de CodePackage programarán la lista de bloqueados de ServiceType. La configuración de **ServiceTypeDisableGraceInterval** determina el intervalo de gracia después del cual ServiceType se marca como incluido en la lista de bloqueados de ese nodo. Mientras ocurre todo esto, la activación, la descarga y el reinicio de CodePackage se reintentan en paralelo. Reintentar significa, por ejemplo, que CodePackage se programará para que se inicie de nuevo después del bloqueo, o Service Fabric intentará descargar paquetes de nuevo.

Cuando Service Type se incluya en la lista de bloqueados, verá un error de estado "'System.Hosting' notificó Error para la propiedad 'ServiceTypeRegistration:ServiceType'. El ServiceType se deshabilitó en el nodo".

ServiceType se habilitará de nuevo en el nodo si se produce alguna de las siguientes acciones:
- La activación se realiza correctamente o alcanza **ActivationMaxFailureCount** reintentos en caso de error.
- La descarga se realiza correctamente o alcanza **DeploymentMaxFailureCount** reintentos en caso de error.
- Un CodePackage que se ha bloqueado se inicia y registra correctamente el ServiceType.

**ActivationMaxFailureCount** y **DeploymentMaxFailureCount** son el número máximo de intentos que hará Service Fabric para activar o descargar una aplicación en un nodo, después del cual Service Fabric volverá a habilitar el ServiceType para la activación. Esto es para dar al servicio otra oportunidad de activación, que podría realizarse correctamente, dando lugar a la solución automática del problema. Una nueva operación de activación o descarga desencadenada por la colocación y activación de una réplica puede desencadenar la inclusión en la lista de bloqueados de ServiceType de nuevo o puede completarse correctamente.

> [!NOTE]
> Si se bloquea su CodePackage que no registra un ServiceType, no afecta a ServiceType. Solo el bloqueo de CodePackage que hospeda la réplica afectará a ServiceType.
>

### <a name="codepackage-crash"></a>Bloqueo de CodePackage
Cuando se bloquea un CodePackage, Service Fabric usa un retroceso para iniciarlo de nuevo. El retroceso es independiente de si el paquete de código ha registrado un tipo con el runtime de Service Fabric o no.

El valor de retroceso es Min(RetryTime, **ActivationMaxRetryInterval**) y este se incrementa en cantidades constantes, lineales o exponenciales, según la opción de configuración de **ActivationRetryBackoffExponentiationBase**.

- Constante: Si **ActivationRetryBackoffExponentiationBase** == 0, entonces RetryTime = **ActivationRetryBackoffInterval**;
- Lineal:  Si **ActivationRetryBackoffExponentiationBase** == 0, entonces RetryTime = ContinuousFailureCount* **ActivationRetryBackoffInterval**, donde ContinousFailureCount es la cantidad de veces que un CodePackage se bloquea o no logra activarse.
- Exponencial: RetryTime = (**ActivationRetryBackoffInterval** en segundos) * (**ActivationRetryBackoffExponentiationBase** ^ ContinuousFailureCount);
    
Puede controlar el comportamiento cambiando los valores. Por ejemplo, si desea varios intentos de reinicio rápido, puede usar Lineal estableciendo **ActivationRetryBackoffExponentiationBase** en 0 y **ActivationRetryBackoffInterval** en 10. Con esta configuración, si se bloqueó un CodePackage, el intervalo de inicio será después de 10 segundos. Si el paquete siguiera bloqueándose, el retroceso cambiaría a 20, 30, 40 segundos, etc., hasta que la activación de CodePackage se realizara correctamente o se desactivara el paquete de código. 
    
La cantidad máxima de tiempo que Service Fabric se interrumpe (espera) tras un error se rige por **ActivationMaxRetryInterval**.
    
Si el CodePackage se bloquea y se vuelve a activar, debe mantenerse durante **CodePackageContinuousExitFailureResetInterval** para que Service Fabric lo considere como correcto, en cuyo momento sobrescribirá el informe de estado de error anterior como Correcto y restablecerá ContinousFailureCount.

### <a name="codepackage-not-registering-servicetype"></a>CodePackage que no registra ServiceType
Si un CodePackage permanece activo y se espera que registre un ServiceType con nosotros, pero nunca lo hace, Service Fabric generará una advertencia HealthReport después de **ServiceTypeRegistrationTimeout** para indicar que ServiceType no se ha registrado en el tiempo esperado.

### <a name="activation-failure"></a>Error de activación
Service Fabric siempre usa una interrupción lineal (igual que el bloqueo de CodePackage) cuando encuentra un error durante la activación. Esto significa que la operación de activación desistirá después de (0 + 10 + 20 + 30 + 40) = 100 segundos (el primer reintento es inmediato). Después de esto, no se vuelva a intentar esta activación.
    
La interrupción de activación máxima puede ser **ActivationMaxRetryInterval** y volver a intentarse **ActivationMaxFailureCount** veces.

### <a name="download-failure"></a>Error de descarga
Service Fabric siempre usa una interrupción lineal cuando encuentra un error durante la descarga. Esto significa que la operación de activación desistirá después de (0 + 10 + 20 + 30 + 40) = 100 segundos (el primer reintento es inmediato). Después de esto, no se vuelve a intentar la descarga. La interrupción lineal para la descarga es igual a ContinuousFailureCount**_DeploymentRetryBackoffInterval_* y puede tener una interrupción máxima de **DeploymentMaxRetryInterval**. Al igual que las activaciones, la operación de descarga se puede volver a intentar **ActivationMaxFailureCount** veces.

> [!NOTE]
> Antes de cambiar estas configuraciones, aquí tiene algunos ejemplos que debe tener en cuenta.

* Si el CodePackage se sigue bloqueado e interrumpiendo, se deshabilitará ServiceType. Sin embargo, si la configuración de activaciones es tal que tiene un reinicio rápido, el CodePackage puede aparecer varias veces antes de que el ServiceType se incluya en la lista de bloqueados realmente. Por ejemplo, supongamos que CodePackage se activa, registra el ServiceType en Service Fabric y luego se bloquea. En ese caso, una vez que el host reciba un registro de tipos, se cancela el período **ServiceTypeDisableGraceInterval**. Y esto puede repetirse hasta que CodePackage se interrumpa en un valor mayor que **ServiceTypeDisableGraceInterval** y, luego, ServiceType se incluirá en la lista de bloqueados en el nodo. Puede tardar en ver el ServiceType incluido en la lista de bloqueados más de lo esperado.

* En el caso de las activaciones, cuando el sistema de Service Fabric necesita colocar una réplica en un nodo, RA (ReconfigurationAgent) pide al subsistema de hospedaje que active la aplicación y reintenta la solicitud de activación cada 15 segundos (regida por la opción de configuración de **RAPMessageRetryInterval**). Para que Service Fabric sepa que ServiceType se ha incluido en la lista de bloqueados, la operación de activación en el hospedaje debe vivir durante un período más largo que el intervalo de reintento y **ServiceTypeDisableGraceInterval**. Por ejemplo: supongamos que el clúster tiene **ActivationMaxFailureCount** establecido en 5 y **ActivationRetryBackoffInterval** establecido en 1 segundo. Esto significa que la operación de activación finalizará tras (0 + 1 + 2 + 3 + 4) = 10 segundos (recuerde que el primer reintento es inmediato) y después de que Hosting deje de reintentarlo. En este caso, la operación de activación se completará y no se volverá a intentar después de 15 segundos. Esto sucede porque Service Fabric agotó todos los reintentos permitidos en 15 segundos. Por lo tanto, cada reintento de ReconfigurationAgent crea una nueva operación de activación en el subsistema de Hosting y el patrón se seguirá repitiendo. Como resultado, el ServiceType nunca se incluirá en la lista de bloqueados en el nodo. Como ServiceType no se incluirá en la lista de bloqueados en el nodo, la réplica no se moverá ni intentará en otro nodo.
> 

## <a name="deactivation"></a>Desactivación

Cuando se activa un ServicePackage en un nodo, se realiza un seguimiento de la desactivación. 

La desactivación funciona de dos maneras:

- Periódicamente:  En cada **DeactivationScanInterval**, comprueba los ServicePackages que NUNCA han hospedado una réplica y los marca como candidatos para la desactivación.
- ReplicaClose: Si una réplica está cerrada, Deactivator obtiene un DecrementUsageCount. Si el recuento llega a 0, significa que el ServicePackage no hospeda ninguna réplica y, por lo tanto, es un candidato para la desactivación.

 Según el modo de activación [exclusivo/compartido][a2], los candidatos para la desactivación se programan después de **DeactivationGraceInterval** para SharedMode y de **ExclusiveModeDeactivationGraceInterval** para ExclusiveMode. Si se produce la colocación de una nueva réplica en este lapso, se cancela la desactivación.

### <a name="periodically"></a>Periódicamente:
Analicemos algunos ejemplos de desactivación periódica:

Ejemplo 1: Supongamos que Deactivator realiza un examen en el tiempo T(**DeactivationScanInterval**). El siguiente examen será en 2T. Supongamos que se ha producido una activación de ServicePackage en T+1. Este ServicePackage no hospeda una réplica y, por tanto, debe desactivarse. Para que ServicePackage sea un candidato para la desactivación, debe estar en el estado sin réplica durante un tiempo de T como mínimo. Esto significa que será válido para la desactivación en 2T+1. Por lo tanto, el examen en 2T no encontrará este ServicePackage como candidato para la desactivación. El siguiente ciclo de desactivación 3T programará este ServicePackage para la desactivación, ya que ahora ha permanecido en el estado sin réplica durante el tiempo T.  

Ejemplo 2: Supongamos que un ServicePackage se activa en el tiempo T-1 y Deactivator realiza un examen en T. ServicePackage no hospeda una réplica. Después, en el siguiente examen 2T, este ServicePackage se encontrará como candidato para la desactivación y, por tanto, se programará para la desactivación.  

Ejemplo 3: Supongamos que un ServicePackage se activa en T-1 y Deactivator realiza un examen en T. ServicePackage no hospeda una réplica aún. Ahora, en T+1, se coloca una réplica, es decir, el hospedaje obtiene un IncrementUsageCount, lo que significa que se crea una réplica. Ahora, en 2T, este ServicePackage no se programará para la desactivación. Dado que contiene una réplica, la desactivación pasará a la lógica de ReplicaClose que se explica a continuación.

Ejemplo 4: supongamos que el ServicePackage es grande, digamos de unos 10 GB, y puede tardar un poco en descargarse en el nodo. Una vez que se activa una aplicación, Deactivator realiza un seguimiento de su ciclo de vida. Ahora, si tiene la configuración de **DeactivationScanInterval** establecida en un valor bajo, puede que surjan problemas en los que su ServicePackage no obtenga tiempo para activarse en el nodo porque todo el tiempo se dedicó a la descarga. Para solucionar el problema, puede [descargar previamente el ServicePackage en el nodo][p1] o aumentar el **DeactivationScanInterval**. 

> [!NOTE]
> Un ServicePackage puede desactivarse en cualquier momento entre (**DeactivationScanInterval** y 2 **_DeactivationScanInterval_*) + **DeactivationGraceInterval**/** ExclusiveModeDeactivationGraceInterval**. 
>

### <a name="replica-close"></a>ReplicaClose:
la desactivación mantiene el recuento de réplicas que contiene un ServicePackage. Si un ServicePackage contiene una réplica y la réplica se cierra o desactiva, el hospedaje obtiene un DecrementUsageCount. Cuando se abre una réplica, el hospedaje obtiene un IncrementUsageCount. El decremento significa que ServicePackage ahora hospeda una réplica menos y, cuando el recuento llega a 0, el ServicePackage se programa para la desactivación, y el tiempo después del cual se desactivará es **DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval**. 

Por ejemplo, supongamos que se produce un decremento en T y ServicePackage está programado para desactivar en 2T+X(**DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval**). Si durante este tiempo el hospedaje obtiene un IncrementUsage, lo que significa que se crea una réplica, se cancela la desactivación.

> [!NOTE]
> ¿Qué significan estas opciones de configuración?
**DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval**: El tiempo dado a un ServicePackage para hospedar otra réplica después de haber hospedado cualquier réplica. 
**DeactivationScanInterval**: El tiempo mínimo dado a ServicePackage para hospedar una réplica si no ha hospedado NUNCA ninguna réplica, es decir, si no se ha usado.
>

### <a name="ctrlc"></a>Ctrl+C
Una vez que un ServicePackage supera el **DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval** y aún no hospeda una réplica, la desactivación no se puede cancelar. Se emite un controlador Ctrl+C para CodePackage, lo que significa que ahora la canalización de desactivación tiene que pasar para desactivar el proceso. Durante este tiempo, si se intenta colocar una nueva réplica para el mismo ServicePackage, se producirá un error, ya que no se puede realizar la transición de la desactivación a la activación.

## <a name="cluster-configs"></a>Configuraciones de clúster

Configuraciones con valores predeterminados que afectan a la activación/desactivación.

### <a name="servicetype"></a>ServiceType
**ServiceTypeDisableFailureThreshold**: El valor predeterminado es 1. El umbral del recuento de errores después del cual se notifica a FM(FailoverManager) que deshabilite el tipo de servicio en ese nodo y pruebe un nodo diferente para la colocación.
**ServiceTypeDisableGraceInterval**: El valor predeterminado es de 30 segundos. Intervalo de tiempo después del cual se puede deshabilitar el tipo de servicio.
**ServiceTypeRegistrationTimeout**: El valor predeterminado es de 300 segundos. Tiempo de espera para que el ServiceType se registre en Service Fabric.

### <a name="activation"></a>Activación
**ActivationRetryBackoffInterval**: El valor predeterminado es de 10 segundos. Intervalo de interrupción en cada error de activación.
**ActivationMaxFailureCount**: El valor predeterminado es 20. Recuento máximo que el sistema reintentará la activación que ha generado error antes de desistir. 
**ActivationRetryBackoffExponentiationBase**: El valor predeterminado es 1,5.
**ActivationMaxRetryInterval**: El valor predeterminado es de 3600 segundos. Interrupción máxima para la activación cuando hay errores.
**CodePackageContinuousExitFailureResetInterval**: El valor predeterminado es de 300 segundos. El tiempo de espera para restablecer el recuento continuo de errores de salida para CodePackage.

### <a name="download"></a>Descargar
**DeploymentRetryBackoffInterval**: El valor predeterminado es 10. Intervalo de espera para el error de implementación.
**DeploymentMaxRetryInterval**: El valor predeterminado es de 3600 segundos. Interrupción máxima para la implementación cuando hay errores.
**DeploymentMaxFailureCount**: El valor predeterminado es 20. La implementación de la aplicación se reintentará las veces especificadas en DeploymentMaxFailureCount antes de que genere un error en el nodo.

### <a name="deactivation"></a>Desactivación
**DeactivationScanInterval**: El valor predeterminado es de 600 segundos. El tiempo mínimo dado a ServicePackage para hospedar una réplica si no ha hospedado nunca ninguna réplica, es decir, si no se ha usado.
**DeactivationGraceInterval**: El valor predeterminado es de 60 segundos. El tiempo dado a un ServicePackage para hospedar otra réplica después de haber hospedado cualquier réplica en caso del modelo de proceso **compartido**.
**ExclusiveModeDeactivationGraceInterval**: El valor predeterminado es de 1 segundo. El tiempo dado a un ServicePackage para hospedar otra réplica después de haber hospedado cualquier réplica en caso del modelo de proceso **exclusivo**.

## <a name="next-steps"></a>Pasos siguientes
[Empaquete una aplicación][a3] y prepárela para la implementación.

[Implementar y quitar aplicaciones][a4]. En este artículo se describe cómo usar PowerShell para administrar las instancias de aplicaciones.

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-hosting-model.md
[a3]: service-fabric-package-apps.md
[a4]: service-fabric-deploy-remove-applications.md

[p1]: /powershell/module/servicefabric/copy-servicefabricservicepackagetonode
