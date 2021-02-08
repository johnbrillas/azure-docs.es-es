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
ms.date: 01/25/2021
ms.author: memildin
ms.openlocfilehash: d5de16c8156762a229d6c707080bc197dc206a7c
ms.sourcegitcommit: 445ecb22233b75a829d0fcf1c9501ada2a4bdfa3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99475597"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Próximos cambios importantes en Azure Security Center

> [!IMPORTANT]
> La información de esta página está relacionada con productos o características en versión preliminar que pueden modificarse sustancialmente antes de lanzarse comercialmente, si es que lo hacen finalmente. Microsoft no ofrece ningún compromiso o garantía, ni expresa ni implícita, con respecto a la información que se proporciona aquí.

En esta página, obtendrá información sobre los cambios planeados para Security Center. Describe las modificaciones planeadas en el producto que pueden afectar a aspectos como los flujos de trabajo o la puntuación segura.

Si busca las notas de la versión más recientes, puede encontrarlas en [Novedades de Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Cambios planeados

- [Pronto se publicarán recomendaciones de protección de cargas de trabajo de Kubernetes para disponibilidad general (GA)](#kubernetes-workload-protection-recommendations-will-soon-be-released-for-general-availability-ga)
- [Dos recomendaciones del control de seguridad "Aplicar actualizaciones del sistema" entran en desuso](#two-recommendations-from-apply-system-updates-security-control-being-deprecated)
- [Mejoras en la recomendación de clasificación de datos de SQL](#enhancements-to-sql-data-classification-recommendation)


### <a name="kubernetes-workload-protection-recommendations-will-soon-be-released-for-general-availability-ga"></a>Pronto se publicarán recomendaciones de protección de cargas de trabajo de Kubernetes para disponibilidad general (GA)

**Fecha estimada del cambio:** Febrero de 2021

Las recomendaciones de protección de cargas de trabajo de Kubernetes que se describen en [Protección de cargas de trabajo de Kubernetes](kubernetes-workload-protections.md) se encuentran actualmente en versión preliminar. Mientras una recomendación está en versión preliminar no representa un recurso incorrecto y no se incluye en los cálculos de puntuación segura.

Pronto se publicarán estas recomendaciones para la disponibilidad general (GA), por lo que se *incluirán* en el cálculo de puntuaciones. Si aún no ha hecho nada al respecto, esto podría afectar negativamente a su puntuación segura.

Corríjalas siempre que sea posible (aprenda cómo en [Recomendaciones de corrección en Azure Security Center](security-center-remediate-recommendations.md)).

Estas son las recomendaciones de protección de cargas de trabajo de Kubernetes:

- El complemento Azure Policy para Kubernetes debe estar instalado y habilitado en sus clústeres
- Debe aplicar los límites de CPU y memoria de los contenedores.
- Deben evitarse los contenedores con privilegios.
- El sistema de archivos raíz inmutable (de solo lectura) debe aplicarse para los contenedores.
- Debe evitar los contenedores con elevación de privilegios.
- Debe evitar la ejecución de contenedores como usuario raíz.
- Deben evitarse los contenedores que comparten espacios de nombres de host confidenciales.
- Deben aplicarse funcionalidades de Linux con privilegios mínimos para los contenedores
- El uso de montajes de volúmenes HostPath de pod debe estar restringido a una lista conocida.
- Los contenedores solo deben escuchar en los puertos permitidos.
- Los servicios solo deben escuchar en los puertos permitidos.
- El uso de puertos y redes de hosts debe estar restringido.
- La opción de reemplazar o deshabilitar el perfil de AppArmor de los contenedores debe estar restringida.
- Las imágenes de contenedor solo deben implementarse desde registros de confianza.             

Obtenga más información sobre estas recomendaciones en [Protección de cargas de trabajo de Kubernetes](kubernetes-workload-protections.md).

### <a name="two-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>Dos recomendaciones del control de seguridad "Aplicar actualizaciones del sistema" entran en desuso 

**Fecha estimada del cambio:** Febrero de 2021

Las dos recomendaciones siguientes están programadas para dejar de usarse en febrero de 2021:

- **Las máquinas deben reiniciarse para aplicar las actualizaciones del sistema**. Esto puede dar lugar a un leve impacto en la puntuación de seguridad.
- **El agente de supervisión debe instalarse en las máquinas**. Esta recomendación solo está relacionada con máquinas locales y parte de su lógica se transferirá a otra recomendación, **Se deben resolver los problemas de estado del agente de Log Analytics en las máquinas**. Esto puede dar lugar a un leve impacto en la puntuación de seguridad.

Se recomienda comprobar las configuraciones de exportación continua y de automatización del flujo de trabajo para ver si estas recomendaciones están incluidas en ellas. Además, los paneles u otras herramientas de supervisión que puedan estar usándolas se deben actualizar en consecuencia.

Más información sobre estas recomendaciones en la [página de referencia de las recomendaciones de seguridad](recommendations-reference.md).


### <a name="enhancements-to-sql-data-classification-recommendation"></a>Mejoras en la recomendación de clasificación de datos de SQL

**Fecha estimada del cambio:** Segundo trimestre de 2021

La versión actual de la recomendación **Se deben clasificar los datos confidenciales de las bases de datos SQL** del control de seguridad **Aplicar clasificación de datos** se reemplazará por una nueva versión que esté más alineada con la estrategia de clasificación de datos de Microsoft. Como resultado:

- La recomendación ya no afectará a la puntuación de seguridad.
- El control de seguridad ("Aplicar clasificación de datos") ya no afectará a la puntuación de seguridad.
- El identificador de la recomendación también cambiará (actualmente es b0df6f56-862d-4730-8597-38c0fd4ebd59).



## <a name="next-steps"></a>Pasos siguientes

Para ver todos los cambios recientes realizados en el producto, consulte [Novedades de Azure Security Center](release-notes.md).
