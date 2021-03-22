---
title: Próximos cambios importantes en Azure Security Center
description: Próximos cambios en Azure Security Center que debe tener en cuenta y para los que puede que necesite un plan
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/04/2021
ms.author: memildin
ms.openlocfilehash: 6bec9f0a1c22691d818566cec3f59c1ec0f3d3bb
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051623"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Próximos cambios importantes en Azure Security Center

> [!IMPORTANT]
> La información de esta página está relacionada con productos o características en versión preliminar que pueden modificarse sustancialmente antes de lanzarse comercialmente, si es que lo hacen finalmente. Microsoft no ofrece ningún compromiso o garantía, ni expresa ni implícita, con respecto a la información que se proporciona aquí.

En esta página, obtendrá información sobre los cambios planeados para Security Center. Describe las modificaciones planeadas en el producto que pueden afectar a aspectos como los flujos de trabajo o la puntuación segura.

Si busca las notas de la versión más recientes, puede encontrarlas en [Novedades de Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Cambios planeados

- [Las recomendaciones de AWS se lanzarán para disponibilidad general (GA)](#recommendations-from-aws-will-be-released-for-general-availability-ga)
- [Dos recomendaciones heredadas ya no escribirán datos directamente en el registro de actividad de Azure](#two-legacy-recommendations-will-no-longer-write-data-directly-to-azure-activity-log)
- [Dos recomendaciones del control de seguridad "Aplicar actualizaciones del sistema" entran en desuso](#two-recommendations-from-apply-system-updates-security-control-being-deprecated)
- [Mejoras en la recomendación de clasificación de datos de SQL](#enhancements-to-sql-data-classification-recommendation)
- [Desuso de 11 alertas de Azure Defender](#deprecation-of-11-azure-defender-alerts)


### <a name="recommendations-from-aws-will-be-released-for-general-availability-ga"></a>Las recomendaciones de AWS se lanzarán para disponibilidad general (GA)

**Fecha estimada del cambio:** abril de 2021

Azure Security Center protege las cargas de trabajo de Azure, Amazon Web Services (AWS) y Google Cloud Platform (GCP).

Las recomendaciones que provienen de AWS Security Hub han estado en versión preliminar desde que se introdujeron los conectores en la nube. Las recomendaciones marcadas como **Versión preliminar** no se incluyen en los cálculos de la puntuación segura, sino que deben corregirse siempre que sea posible, de modo que cuando el período de versión preliminar finalice, contribuyan a la puntuación.

Con este cambio, dos conjuntos de recomendaciones de AWS pasarán a disponibilidad general:

- [Controles de PCI DSS de Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html)
- [Controles del banco de pruebas de CIS AWS Foundations de Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html)

Cuando se encuentren en versión de disponibilidad general y las evaluaciones se ejecuten en los recursos de AWS, los resultados afectarán a la puntuación segura combinada para todos los recursos de nube híbrida y multinube. 



### <a name="two-legacy-recommendations-will-no-longer-write-data-directly-to-azure-activity-log"></a>Dos recomendaciones heredadas dejarán de escribir datos directamente en el registro de actividad de Azure 

**Fecha estimada del cambio:** marzo de 2021

Security Center pasa los datos de casi todas las recomendaciones de seguridad a Azure Advisor que, a su vez, los escribe en el [registro de actividad de Azure](../azure-monitor/essentials/activity-log.md).

En caso de dos recomendaciones, los datos se escriben simultáneamente directamente en el registro de actividad de Azure. Con este cambio, Security Center dejará de escribir datos para estas recomendaciones de seguridad heredadas directamente en el registro de actividad. En su lugar, exportaremos los datos a Azure Advisor como hacemos las restantes recomendaciones. 

Las dos recomendaciones heredadas son:
- Los problemas de estado de protección de puntos de conexión se deben resolver en las máquinas
- Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas

Si ha accedido a la información de estas dos recomendaciones en la categoría "Recomendación de tipo TaskDiscovery" del registro de actividad, ya no estará disponible.

### <a name="two-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>Dos recomendaciones del control de seguridad "Aplicar actualizaciones del sistema" entran en desuso 

**Fecha estimada del cambio:** marzo de 2021

Las dos recomendaciones siguientes están programadas para dejar de usarse en febrero de 2021:

- **Las máquinas deben reiniciarse para aplicar las actualizaciones del sistema**. Esto puede dar lugar a un leve impacto en la puntuación de seguridad.
- **El agente de supervisión debe instalarse en las máquinas**. Esta recomendación solo está relacionada con máquinas locales y parte de su lógica se transferirá a otra recomendación, **Se deben resolver los problemas de estado del agente de Log Analytics en las máquinas**. Esto puede dar lugar a un leve impacto en la puntuación de seguridad.

Se recomienda comprobar las configuraciones de exportación continua y de automatización del flujo de trabajo para ver si estas recomendaciones están incluidas en ellas. Además, los paneles u otras herramientas de supervisión que puedan estar usándolas se deben actualizar en consecuencia.

Más información sobre estas recomendaciones en la [página de referencia de las recomendaciones de seguridad](recommendations-reference.md).


### <a name="enhancements-to-sql-data-classification-recommendation"></a>Mejoras en la recomendación de clasificación de datos de SQL

**Fecha estimada del cambio:** Segundo trimestre de 2021

La recomendación **Los datos confidenciales de las bases de datos SQL deben clasificarse** del control de seguridad **Aplicación de la clasificación de datos** se reemplazará por una nueva versión que esté más alineada con la estrategia de clasificación de datos de Microsoft. Como consecuencia, el identificador de la recomendación también cambiará (actualmente es b0df6f56-862d-4730-8597-38c0fd4ebd59).


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
 



## <a name="next-steps"></a>Pasos siguientes

Para ver todos los cambios recientes realizados en el producto, consulte [Novedades de Azure Security Center](release-notes.md).
