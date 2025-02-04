---
title: Próximos cambios importantes en Azure Security Center
description: Próximos cambios en Azure Security Center que debe tener en cuenta y para los que puede que necesite un plan
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: overview
ms.date: 03/18/2021
ms.author: memildin
ms.openlocfilehash: b9a93286b6a546160b6c621d084437f671eab4d3
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104773579"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Próximos cambios importantes en Azure Security Center

> [!IMPORTANT]
> La información de esta página está relacionada con productos o características en versión preliminar que pueden modificarse sustancialmente antes de lanzarse comercialmente, si es que lo hacen finalmente. Microsoft no ofrece ningún compromiso o garantía, ni expresa ni implícita, con respecto a la información que se proporciona aquí.

En esta página, obtendrá información sobre los cambios planeados para Security Center. Describe las modificaciones planeadas en el producto que pueden afectar a aspectos como los flujos de trabajo o la puntuación segura.

Si busca las notas de la versión más recientes, puede encontrarlas en [Novedades de Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Cambios planeados

| Cambio planeado                                                                                                                                                        | Fecha estimada del cambio |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| [Dos recomendaciones del control de seguridad "Aplicar actualizaciones del sistema" entran en desuso](#two-recommendations-from-apply-system-updates-security-control-being-deprecated) | Marzo de 2021                |
| [Desuso de 11 alertas de Azure Defender](#deprecation-of-11-azure-defender-alerts)                                                                                   | Marzo de 2021                |
| [21 recomendaciones de movimiento entre controles de seguridad](#21-recommendations-moving-between-security-controls)                                                           | Abril de 2021                |
| [Otras dos recomendaciones del control de seguridad "Aplicar actualizaciones del sistema" entran en desuso](#two-further-recommendations-from-apply-system-updates-security-control-being-deprecated)                                                                                         | Abril de 2021                |
| [Las recomendaciones de AWS se lanzarán para disponibilidad general (GA)](#recommendations-from-aws-will-be-released-for-general-availability-ga)                     | Abril de 2021                |
| [Mejoras en la recomendación de clasificación de datos de SQL](#enhancements-to-sql-data-classification-recommendation)                                                     | Segundo trimestre de 2021                   |
|                                                                                                                                                                       |                           |


### <a name="two-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>Dos recomendaciones del control de seguridad "Aplicar actualizaciones del sistema" entran en desuso 

**Fecha estimada del cambio:** marzo de 2021

Las dos recomendaciones siguientes están programadas para dejar de usarse en febrero de 2021:

- **Las máquinas deben reiniciarse para aplicar las actualizaciones del sistema**. Esto puede dar lugar a un leve impacto en la puntuación de seguridad.
- **El agente de supervisión debe instalarse en las máquinas**. Esta recomendación solo está relacionada con máquinas locales y parte de su lógica se transferirá a otra recomendación, **Se deben resolver los problemas de estado del agente de Log Analytics en las máquinas**. Esto puede dar lugar a un leve impacto en la puntuación de seguridad.

Se recomienda comprobar las configuraciones de exportación continua y de automatización del flujo de trabajo para ver si estas recomendaciones están incluidas en ellas. Además, los paneles u otras herramientas de supervisión que puedan estar usándolas se deben actualizar en consecuencia.

Más información sobre estas recomendaciones en la [página de referencia de las recomendaciones de seguridad](recommendations-reference.md).

### <a name="deprecation-of-11-azure-defender-alerts"></a>Desuso de 11 alertas de Azure Defender

**Fecha estimada del cambio:** marzo de 2021

El mes que viene, las once alertas de Azure Defender que se enumeran a continuación dejarán de usarse.

- Las nuevas alertas reemplazarán estas dos y proporcionarán una mejor cobertura:

    | AlertType                | AlertDisplayName                                                         |
    |--------------------------|--------------------------------------------------------------------------|
    | ARM_MicroBurstDomainInfo | VERSIÓN PRELIMINAR: Se detectó la ejecución de la función "Get-AzureDomainInfo" del kit de herramientas de MicroBurst. |
    | ARM_MicroBurstRunbook    | VERSIÓN PRELIMINAR: Se detectó la ejecución de la función "Get-AzurePasswords" del kit de herramientas de MicroBurst.  |
    |                          |                                                                          |

- Estas nueve alertas se refieren a un conector de Azure Active Directory Identity Protection que ya está en desuso:

    | AlertType           | AlertDisplayName              |
    |---------------------|-------------------------------|
    | UnfamiliarLocation  | Propiedades de inicio de sesión desconocidas |
    | AnonymousLogin      | Dirección IP anónima          |
    | InfectedDeviceLogin | Dirección IP vinculada al malware     |
    | ImpossibleTravel    | Viaje atípico               |
    | MaliciousIP         | Dirección IP malintencionada          |
    | LeakedCredentials   | Credenciales con fugas            |
    | PasswordSpray       | Difusión de contraseñas                |
    | LeakedCredentials   | Inteligencia de Azure AD sobre amenazas  |
    | AADAI               | IA de Azure AD                   |
    |                     |                               |
 




### <a name="21-recommendations-moving-between-security-controls"></a>21 recomendaciones de movimiento entre controles de seguridad 

**Fecha estimada del cambio:** abril de 2021

Las siguientes recomendaciones se van a mover a otro control de seguridad. Los controles de seguridad son grupos lógico de recomendaciones de seguridad relacionadas y reflejan las superficies de ataque vulnerables. Este traslado garantiza que cada una de estas recomendaciones está en el control más adecuado para cumplir su objetivo. 

Obtenga información sobre qué recomendaciones se encuentran en cada control de seguridad en Controles de seguridad y sus recomendaciones.

|Recomendación |Cambio e impacto  |
|---------|---------|
|La evaluación de vulnerabilidades debe estar activada en sus servidores de SQL Server.<br>La evaluación de vulnerabilidad debe estar habilitada en las instancias administradas de SQL.<br>Las vulnerabilidades de sus bases de datos SQL deben remediarse ahora<br>Las vulnerabilidades de las bases de datos SQL en máquinas virtuales deben corregirse     |Cambia de Corregir vulnerabilidades (con un valor de 6 puntos)<br>a Corregir configuraciones de seguridad (con un valor de 4 puntos)<br>En función del entorno, estas recomendaciones tendrán menor impacto en la puntuación.|
|Debe haber más de un propietario asignado a su suscripción<br>Las variables de cuenta de automatización deben cifrarse<br> Dispositivos IoT: el proceso auditado dejó de enviar eventos.<br> Dispositivos IoT: error de validación de línea base del sistema operativo<br> Dispositivos IoT: es preciso actualizar el conjunto de cifrado de TLS.<br>Dispositivos IoT: puertos abiertos en el dispositivo.<br>Dispositivos IoT: se encontró una directiva de firewall permisiva en una de las cadenas.<br> Dispositivos IoT: se encontró una regla de firewall permisiva en la cadena de entrada.<br> Dispositivos IoT: se encontró una regla de firewall permisiva en la cadena de salida.<br>Los registros de diagnóstico de IoT Hub deben estar habilitados<br>Dispositivos IoT: mensajes infrautilizados de envío del agente.<br>Dispositivos IoT: la directiva de filtro de IP predeterminada debe ser Denegar.<br>Dispositivos IoT: intervalo de IP amplio de la regla del filtro de IP.<br>Dispositivos IoT: se deben ajustar tanto el tamaño como los intervalos de los mensajes de los agentes<br>Dispositivos IoT: credenciales de autenticación idénticas<br>Dispositivos IoT: el proceso auditado dejó de enviar eventos.<br>Dispositivos IoT: la configuración de la línea base del sistema operativo (SO) debe corregirse|Cambia a **Implementación de procedimientos recomendados de seguridad**.<br>Cuando una recomendación pasa al control de seguridad Implementar prácticas recomendadas de seguridad, que no vale ningún punto, la recomendación deja de afectar a la puntuación segura.|
|||


### <a name="two-further-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>Otras dos recomendaciones del control de seguridad "Aplicar actualizaciones del sistema" entran en desuso

**Fecha estimada del cambio:** abril de 2021

Las dos recomendaciones siguientes están en desuso:

- **La versión del sistema operativo debe actualizarse en los roles del servicio en la nube**: de manera predeterminada, Azure actualiza periódicamente el sistema operativo invitado a la imagen compatible más reciente dentro de la familia de sistema operativo que ha especificado en la configuración del servicio (.cscfg), como Windows Server 2016.
- Los **servicios de Kubernetes deben actualizarse a una versión de Kubernetes no vulnerable**: las evaluaciones de esta recomendación no son tan amplias como nos gustaría. La versión actual de esta recomendación se reemplazará finalmente por una versión mejorada que se ajuste más a las necesidades de seguridad de los clientes.


### <a name="recommendations-from-aws-will-be-released-for-general-availability-ga"></a>Las recomendaciones de AWS se lanzarán para disponibilidad general (GA)

**Fecha estimada del cambio:** abril de 2021

Azure Security Center protege las cargas de trabajo de Azure, Amazon Web Services (AWS) y Google Cloud Platform (GCP).

Las recomendaciones que provienen de AWS Security Hub han estado en versión preliminar desde que se introdujeron los conectores en la nube. Las recomendaciones marcadas como **Versión preliminar** no se incluyen en los cálculos de la puntuación segura, sino que deben corregirse siempre que sea posible, de modo que cuando el período de versión preliminar finalice, contribuyan a la puntuación.

Con este cambio, dos conjuntos de recomendaciones de AWS pasarán a disponibilidad general:

- [Controles de PCI DSS de Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html)
- [Controles del banco de pruebas de CIS AWS Foundations de Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html)

Cuando se encuentren en versión de disponibilidad general y las evaluaciones se ejecuten en los recursos de AWS, los resultados afectarán a la puntuación segura combinada para todos los recursos de nube híbrida y multinube. 



### <a name="enhancements-to-sql-data-classification-recommendation"></a>Mejoras en la recomendación de clasificación de datos de SQL

**Fecha estimada del cambio:** Segundo trimestre de 2021

La recomendación **Los datos confidenciales de las bases de datos SQL deben clasificarse** del control de seguridad **Aplicación de la clasificación de datos** se reemplazará por una nueva versión que esté más alineada con la estrategia de clasificación de datos de Microsoft. Como consecuencia, el identificador de la recomendación también cambiará (actualmente es b0df6f56-862d-4730-8597-38c0fd4ebd59).



## <a name="next-steps"></a>Pasos siguientes

Para ver todos los cambios recientes realizados en el producto, consulte [Novedades de Azure Security Center](release-notes.md).