---
title: Ciclo de vida de activación y desactivación de hospedaje de Azure Service Fabric
description: Obtenga información sobre el ciclo de vida de una aplicación y una instancia de ServicePackage en un nodo.
author: tugup
ms.topic: conceptual
ms.date: 05/01/2020
ms.author: tugup
ms.openlocfilehash: d8585d0b39e4a4ef9cf77f40ea878ddb47bcb0de
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97831829"
---
# <a name="azure-service-fabric-hosting-life-cycle"></a>Ciclo de vida de hospedaje de Azure Service Fabric

En este artículo se proporciona información general sobre los eventos que se producen en Azure Service Fabric cuando se activa una aplicación en un nodo. Se explican diversas configuraciones de clúster que controlan el comportamiento.

Antes de continuar, asegúrese de que entiende los conceptos y las relaciones que se explican en [Modelar una aplicación en Service Fabric][a1]. 

> [!NOTE]
> En este artículo se usa alguna terminología de forma especializada. A menos que se indique lo contrario:
>
> - La palabra *réplica* hace referencia a una réplica de un servicio con estado y a una instancia de un servicio sin estado.
> - *CodePackage* se trata como equivalente a un proceso de un elemento ServiceHost que registra una instancia de ServiceType. Hospeda réplicas de servicios de esa instancia de ServiceType.
>

## <a name="activate-an-applicationpackage-or-servicepackage"></a>Activación de un elemento ApplicationPackage o ServicePackage

Para activar un elemento ApplicationPackage o ServicePackage:

1. Descargue la instancia de ApplicationPackage (por ejemplo, *ApplicationManifest.xml*).
2. Configure un entorno para la aplicación. Los pasos incluyen, por ejemplo, la creación de usuarios.
3. Inicie el seguimiento de la aplicación para su desactivación.
4. Descargue la instancia de ServicePackage (por ejemplo, *ServiceManifest.xml*, el código, los archivos de configuración y los paquetes de datos).
5. Configure un entorno para el elemento ServicePackage. Los pasos incluyen, por ejemplo, la configuración de un firewall y la asignación de puertos para los puntos de conexión.
6. Inicie el seguimiento de la instancia de ServicePackage para su desactivación.
7. Inicie el valor SetupEntryPoint de las instancias de CodePackage y espere a que finalice.
8. Inicie el valor MainEntryPoint de las instancias de CodePackage.

### <a name="servicetype-blocklisting"></a>Inclusión de ServiceType en la lista de bloqueados
`ServiceTypeDisableFailureThreshold` determina el número de errores de activación, descarga y CodePackage permitidos. Una vez alcanzado el umbral, la instancia de ServiceType se programa para su inclusión en la lista de bloqueados. El primer error de activación, error de descarga o bloqueo de CodePackage programa la inclusión de ServiceType en la lista de bloqueados. 

La configuración `ServiceTypeDisableGraceInterval` determina el intervalo de gracia antes de la inclusión de ServiceType en la lista de bloqueados del nodo. Mientras se desarrolla este proceso, se producen reintentos de reinicio de la activación, la descarga y CodePackage en paralelo. Un reintento significa, por ejemplo, que se programa que el elemento CodePackage se inicie de nuevo después del bloqueo, o que Service Fabric intenta descargar paquetes otra vez.

Cuando un elemento ServiceType se incluye en la lista de bloqueados, se ve un error de estado: `'System.Hosting' reported Error for property 'ServiceTypeRegistration:ServiceType'. The ServiceType was disabled on the node.`

Si se produce alguno de los siguientes eventos, ServiceType se vuelve a habilitar en el nodo:
- La activación se realiza correctamente. O bien, si se produce un error, alcanza el número máximo de reintentos `ActivationMaxFailureCount`.
- La descarga se realiza correctamente. O bien, si se produce un error, alcanza el número máximo de reintentos `DeploymentMaxFailureCount`.
- Un CodePackage que se ha bloqueado se inicia y registra correctamente el ServiceType.

`ActivationMaxFailureCount` y `DeploymentMaxFailureCount` son el número máximo de intentos que Service Fabric realiza para activar o descargar una aplicación en un nodo. Una vez alcanzado el máximo, Service Fabric habilita de nuevo el elemento ServiceType para su activación. 

Estos umbrales ofrecen al servicio otra oportunidad de activación. Si la activación se realiza correctamente, el problema se resuelve automáticamente. 

Una réplica recién colocada o activada puede desencadenar una nueva operación de activación o descarga. Esta acción se realiza correctamente o vuelve a desencadenar la inclusión de ServiceType en la lista de bloqueados.

> [!NOTE]
> Un elemento CodePackage bloqueado que no registra un ServiceType no afecta a la instancia de ServiceType. Solo un elemento CodePackage bloqueado que hospede una réplica afecta a la instancia de ServiceType.
>

### <a name="codepackage-crash"></a>Bloqueo de CodePackage
Cuando se bloquea un CodePackage, Service Fabric usa un retroceso para iniciarlo de nuevo. El retroceso se aplica independientemente de si CodePackage ha registrado un tipo con el runtime de Service Fabric.

El valor de retroceso es `Min(RetryTime, ActivationMaxRetryInterval)`. El valor se incrementa en cantidades constantes, lineales o exponenciales en función del valor de configuración `ActivationRetryBackoffExponentiationBase`:

- **Constante**: si `ActivationRetryBackoffExponentiationBase == 0`, `RetryTime = ActivationRetryBackoffInterval`.
- **Lineal**:  si `ActivationRetryBackoffExponentiationBase == 0`, `RetryTime = ContinuousFailureCount ActivationRetryBackoffInterval`, donde `ContinuousFailureCount` es el número de veces que un elemento CodePackage se bloquea o no se activa.
- **Exponencial**: `RetryTime = (ActivationRetryBackoffInterval in seconds) * (ActivationRetryBackoffExponentiationBase ^ ContinuousFailureCount)`.
    
Puede controlar el comportamiento cambiando los valores. Por ejemplo, si quiere varios intentos de reinicio rápido, puede usar cantidades lineales si establece `ActivationRetryBackoffExponentiationBase` en `0` y `ActivationRetryBackoffInterval` en `10`. Si CodePackage se bloquea, el intervalo de inicio se produce después de 10 segundos. Si el paquete sigue bloqueándose, el retroceso cambia a 20 segundos, 30 segundos, 40 segundos, etc., hasta que la activación del elemento CodePackage se realiza correctamente o este se desactiva. 
    
La cantidad máxima de tiempo que Service Fabric retrocede (es decir, espera) tras un error se rige por `ActivationMaxRetryInterval`.
    
Si el elemento CodePackage se bloquea y se pone en marcha de nuevo, debe permanecer activo durante el período de tiempo especificado por `CodePackageContinuousExitFailureResetInterval`. Después de este intervalo, Service Fabric lo considera en buen estado. Luego Service Fabric sobrescribe el informe de estado de error anterior como correcto y restablece `ContinuousFailureCount`.

### <a name="codepackage-not-registering-servicetype"></a>CodePackage que no registra ServiceType
Si un elemento CodePackage permanece activo y se espera que registre una instancia de ServiceType pero no lo hace, Service Fabric genera un informe de estado de advertencia después de `ServiceTypeRegistrationTimeout`. El informe indica que ServiceType no se ha registrado en el tiempo esperado.

### <a name="activation-failure"></a>Error de activación
Cuando Service Fabric detecta un error durante la activación, siempre usa un retroceso lineal, igual que con un bloqueo de CodePackage. La operación de activación se abandona después de los reintentos en estos intervalos: (0 + 10 + 20 + 30 + 40) = 100 segundos. (El primer reintento es inmediato). Después de esta secuencia, no se vuelve a intentar la activación.
    
El retroceso de activación máximo puede ser `ActivationMaxRetryInterval`. El reintento puede ser `ActivationMaxFailureCount`.

### <a name="download-failure"></a>Error de descarga
Service Fabric siempre usa un retroceso lineal cuando detecta un error durante una descarga. La operación de activación se abandona después de los reintentos en estos intervalos: (0 + 10 + 20 + 30 + 40) = 100 segundos. (El primer reintento es inmediato). Después de esta secuencia, no se vuelve a intentar la descarga. 

El retroceso lineal de una descarga es igual a `ContinuousFailureCount` * `DeploymentRetryBackoffInterval`. El retroceso máximo puede ser `DeploymentMaxRetryInterval`. Al igual que las activaciones, las operaciones de descarga pueden reintentarse durante el límite `ActivationMaxFailureCount`.

> [!NOTE]
> Antes de cambiar esta configuración, tenga en cuenta los ejemplos siguientes:
>
>* Si el elemento CodePackage se sigue bloqueando y retrocediendo, se deshabilita la instancia de ServiceType. Pero si la configuración de activación tiene un reinicio rápido, el elemento CodePackage puede activarse varias veces antes de que ServiceType se incluya realmente en la lista de bloqueados. 
>
>    Por ejemplo, imagine que CodePackage se activa, registra el elemento ServiceType en Service Fabric y luego se bloquea. En ese caso, después de que el hospedaje reciba un registro de tipo, se cancela el período `ServiceTypeDisableGraceInterval`. Este proceso puede repetirse hasta que el elemento CodePackage retrocede a un valor superior al período `ServiceTypeDisableGraceInterval`. Luego la instancia de ServiceType se incluye en la lista de bloqueados del nodo. La inclusión de ServiceType en la lista de bloqueados puede tardar más de lo esperado.
>
>* En el caso de las activaciones, cuando un sistema de Service Fabric necesita colocar una réplica en un nodo, el Agente de reconfiguración pide al subsistema de hospedaje que active la aplicación. Vuelve a intentar la solicitud de activación cada 15 segundos. (La duración se rige por el valor de configuración `RAPMessageRetryInterval`). Service Fabric no puede saber que la instancia de ServiceType se ha incluido en la lista de bloqueados a menos que la operación de activación en el hospedaje esté activa durante un período más largo que el intervalo de reintento y `ServiceTypeDisableGraceInterval`. 
>
>    Por ejemplo, imagine que el valor `ActivationMaxFailureCount` del clúster está establecido en 5 y `ActivationRetryBackoffInterval` está establecido en 1 segundo. En este caso, la operación de activación se abandona después de los intervalos de (0 + 1 + 2 + 3 + 4) = 10 segundos. (El primer reintento es inmediato). Después de esta secuencia, el hospedaje deja de intentarlo. La operación de activación finaliza y no se vuelve a intentar después de 15 segundos. 
>
>    Service Fabric ha agotado todos los reintentos permitidos en 15 segundos. Así, cada reintento del Agente de reconfiguración crea una nueva operación de activación en el subsistema de hospedaje, y el patrón se sigue repitiendo. Como resultado, la instancia de ServiceType nunca se incluye en la lista de bloqueados del nodo. Dado que la instancia de ServiceType no se incluye en la lista de bloqueados del nodo, la réplica no se mueve ni se intenta en otro nodo.
> 

## <a name="deactivation"></a>Desactivación

Cuando se activa un elemento ServicePackage en un nodo, se realiza un seguimiento para su desactivación. La desactivación funciona de dos maneras:

- **Desactivación periódica**:  en cada `DeactivationScanInterval`, el sistema busca los paquetes de servicio que *nunca* han hospedado una réplica y los marca como candidatos para la desactivación.
- **Desactivación de ReplicaClose**: si una réplica está cerrada, Deactivator obtiene un valor `DecrementUsageCount`. Un recuento está en 0 cuando el elemento ServicePackage no hospeda ninguna réplica y, por tanto, es un candidato para la desactivación.

El modo de activación determina cuándo se programan los candidatos para su desactivación. En modo compartido, los candidatos para la desactivación se programan después de `DeactivationGraceInterval`. En modo exclusivo, se programan después de `ExclusiveModeDeactivationGraceInterval`. Si se produce la colocación de una nueva réplica en ese lapso, se cancela la desactivación. 

Para obtener más información, vea el [modo exclusivo y el modo compartido][a2].

### <a name="periodic-deactivation"></a>Desactivación periódica
Estos son algunos ejemplos de desactivación periódica:

* **Ejemplo 1**: imagine que Deactivator inicia un examen en el tiempo T (`DeactivationScanInterval`). El siguiente examen será en 2T. Imagine que se ha producido la activación de un elemento ServicePackage en T + 1. Este no hospeda ninguna réplica, por lo que debe desactivarse. 

    Para ser un candidato para la desactivación, no debe hospedar ninguna réplica durante al menos el tiempo T. Será apto para la desactivación en 2T + 1. Por lo tanto, el examen en 2T no identifica este elemento ServicePackage como candidato para la desactivación. 

    El siguiente ciclo de desactivación, 3T, programa este elemento ServicePackage para la desactivación, ya que ahora el paquete ha estado en un estado sin réplica durante el tiempo T.  

* **Ejemplo 2**: imagine que un elemento ServicePackage se activa en el tiempo T - 1 y Deactivator inicia un examen en T. El elemento no hospeda ninguna réplica. En el siguiente examen, 2T, se identifica como candidato para la desactivación y, por tanto, se programa para ella.  

* **Ejemplo 3**: imagine que un elemento ServicePackage se activa en T - 1 y Deactivator inicia un examen en T. El elemento aún no hospeda ninguna réplica. Ahora, en T + 1, se coloca una réplica. Es decir, el hospedaje obtiene un valor `IncrementUsageCount`, lo que significa que se crea una réplica. 

    En 2T, este elemento ServicePackage no se programa para la desactivación. Dado que el paquete contiene una réplica, la desactivación sigue la lógica de ReplicaClose, como se explica en la sección siguiente de este artículo.

* **Ejemplo 4**: imagine que el elemento ServicePackage tiene 10 GB. Dado que el paquete es grande, tarda en descargarse en el nodo. Una vez que se activa una aplicación, Deactivator realiza un seguimiento de su ciclo de vida. Si `DeactivationScanInterval` se establece en un valor pequeño, es posible que la instancia de ServicePackage no tenga tiempo de activarse en el nodo debido al tiempo que ha tardado en descargarse. Para solucionar este problema, puede [descargar previamente la instancia de ServicePackage en el nodo][p1] o aumentar el valor `DeactivationScanInterval`. 

> [!NOTE]
> Una instancia de ServicePackage puede desactivarse en cualquier lugar entre (`DeactivationScanInterval` a 2 * `DeactivationScanInterval`) + `DeactivationGraceInterval`/`ExclusiveModeDeactivationGraceInterval`. 
>

### <a name="replicaclose-deactivation"></a>Desactivación de ReplicaClose

> [!NOTE]
> Esta sección trata sobre los valores de configuración siguientes:
> - **DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval**: tiempo concedido a una instancia de ServicePackage para hospedar otra réplica si ya ha hospedado alguna. 
> - **DeactivationScanInterval**: tiempo mínimo concedido a una instancia de ServicePackage para hospedar una réplica si *nunca* ha hospedado una, es decir, si no se ha usado.
>

El sistema almacena el recuento de las réplicas que contiene una instancia de ServicePackage. Si un elemento ServicePackage contiene una réplica y esta se cierra o desactiva, el hospedaje obtiene un valor `DecrementUsageCount`. Cuando se abre una réplica, el hospedaje obtiene un valor `IncrementUsageCount`. 

El decremento indica que el número de réplicas que hospeda la instancia de ServicePackage se ha reducido en una. Cuando el recuento desciende a 0, se programa la desactivación de la instancia de ServicePackage. El tiempo tras el que se desactiva es `DeactivationGraceInterval`/`ExclusiveModeDeactivationGraceInterval`. 

Por ejemplo, imagine que se produce un decremento en T y la instancia de ServicePackage está programada para desactivarse en 2T + X (`DeactivationGraceInterval`/`ExclusiveModeDeactivationGraceInterval`). Durante este tiempo, si el hospedaje obtiene un valor `IncrementUsage` porque se ha creado una réplica, se cancela la desactivación.

### <a name="ctrl--c"></a>Ctrl + C
Si una instancia de ServicePackage pasa `DeactivationGraceInterval`/`ExclusiveModeDeactivationGraceInterval` y aún no hospeda ninguna réplica, la desactivación no se puede cancelar. Las instancias de CodePackage reciben un controlador Ctrl + C. Por lo tanto, la canalización de desactivación debe finalizar para desconectar el proceso. 

Durante este tiempo, si se intenta colocar una nueva réplica para el mismo elemento ServicePackage, se produce un error, ya que el proceso no puede pasar de la desactivación a la activación.

## <a name="cluster-configurations"></a>Configuraciones de clústeres

En esta sección se indican las configuraciones que tienen valores predeterminados que afectan a la activación y la desactivación.

### <a name="servicetype"></a>ServiceType
- **ServiceTypeDisableFailureThreshold**: Valor predeterminado: 1. Umbral del recuento de errores; una vez que se alcanza, se notifica a FailoverManager que deshabilite el tipo de servicio en el nodo y pruebe otro nodo para la colocación.
- **ServiceTypeDisableGraceInterval**: Valor predeterminado: 30 segundos. Intervalo de tiempo después del cual se puede deshabilitar el tipo de servicio.
- **ServiceTypeRegistrationTimeout**: Valor predeterminado: 300 segundos. Tiempo de espera para que la instancia de ServiceType se registre en Service Fabric.

### <a name="activation"></a>Activación
- **ActivationRetryBackoffInterval**: Valor predeterminado: 10 segundos. Intervalo de retroceso de cada error de activación.
- **ActivationMaxFailureCount**: Valor predeterminado: 20. Recuento máximo que el sistema reintenta una activación errónea antes de desistir. 
- **ActivationRetryBackoffExponentiationBase**: Valor predeterminado: 1.5.
- **ActivationMaxRetryInterval**: Valor predeterminado: 3600 segundos. Intervalo máximo de reintentos de retroceso para la activación tras los errores.
- **CodePackageContinuousExitFailureResetInterval**: Valor predeterminado: 300 segundos. Tiempo de espera para restablecer el recuento continuo de errores de salida de CodePackage.

### <a name="download"></a>Descargar
- **DeploymentRetryBackoffInterval**: Valor predeterminado: 10. Intervalo de retroceso del error de implementación.
- **DeploymentMaxRetryInterval**: Valor predeterminado: 3600 segundos. Intervalo de retroceso máximo de la implementación después de los errores.
- **DeploymentMaxFailureCount**: Valor predeterminado: 20. La implementación de la aplicación se reintenta `DeploymentMaxFailureCount` veces antes de generar un error en el nodo.

### <a name="deactivation"></a>Desactivación
- **DeactivationScanInterval**: Valor predeterminado: 600 segundos. Tiempo mínimo concedido a la instancia de ServicePackage para hospedar una réplica si nunca ha hospedado una (es decir, si no se ha usado).
- **DeactivationGraceInterval**: Valor predeterminado: 60 segundos. En un modelo de proceso *compartido*, tiempo concedido a una instancia de ServicePackage para volver a hospedar otra réplica si ya ha hospedado alguna.
- **ExclusiveModeDeactivationGraceInterval**: Valor predeterminado: 1 segundo. En un modelo de proceso *exclusivo*, tiempo concedido a una instancia de ServicePackage para volver a hospedar otra réplica si ya ha hospedado alguna.

## <a name="next-steps"></a>Pasos siguientes

- [Empaquete una aplicación][a3] y prepárela para la implementación.
- [Implementación y eliminación de aplicaciones con PowerShell][a4].

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-hosting-model.md
[a3]: service-fabric-package-apps.md
[a4]: service-fabric-deploy-remove-applications.md

[p1]: /powershell/module/servicefabric/copy-servicefabricservicepackagetonode
