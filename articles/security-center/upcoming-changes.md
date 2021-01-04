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
ms.date: 12/14/2020
ms.author: memildin
ms.openlocfilehash: 052758079d8d413f7b0fead2a5abf3b47b9a691e
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2020
ms.locfileid: "97511338"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Próximos cambios importantes en Azure Security Center

> [!IMPORTANT]
> La información de esta página está relacionada con productos o características en versión preliminar que pueden modificarse sustancialmente antes de lanzarse comercialmente, si es que lo hacen finalmente. Microsoft no ofrece ningún compromiso o garantía, ni expresa ni implícita, con respecto a la información que se proporciona aquí.

En esta página, obtendrá información sobre los cambios planeados para Security Center. Describe las modificaciones planeadas en el producto que pueden afectar a aspectos como los flujos de trabajo o la puntuación segura.

Si busca las notas de la versión más recientes, puede encontrarlas en [Novedades de Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Cambios planeados

- [Recursos en estado "No aplicable" que se van a notificar como "Compatible" en las valoraciones de Azure Policy](#not-applicable-resources-to-be-reported-as-compliant-in-azure-policy-assessments)
- [Se han agregado 35 recomendaciones en versión preliminar para aumentar la cobertura de Azure Security Benchmark](#35-preview-recommendations-being-added-to-increase-coverage-of-azure-security-benchmark).

### <a name="not-applicable-resources-to-be-reported-as-compliant-in-azure-policy-assessments"></a>Recursos en estado "No aplicable" que se van a notificar como "Compatible" en las valoraciones de Azure Policy

**Fecha estimada del cambio:** 2021 de enero

Actualmente, los recursos que se evalúan para una recomendación y se detecta que son **no aplicables** aparecen en in Azure Policy como "No compatible". Ninguna acción del usuario puede cambiar su estado a "Compatible". A partir de este cambio planeado, se notificarán como "Compatible" para aumentar la claridad.

El único impacto se verá en Azure Policy, donde el número de recursos compatibles aumentará. Esto no afectará a la puntuación de seguridad de Azure Security Center.

### <a name="35-preview-recommendations-being-added-to-increase-coverage-of-azure-security-benchmark"></a>Se han agregado 35 recomendaciones en versión preliminar para aumentar la cobertura de Azure Security Benchmark.

**Fecha estimada del cambio:** Diciembre de 2020

Azure Security Benchmark es el conjunto de directrices específico de Azure creado por Microsoft para ofrecer los procedimientos recomendados de seguridad y cumplimiento basados en marcos de cumplimiento comunes. [Mas información sobre Azure Security Benchmark](../security/benchmarks/introduction.md).

Se agregarán a Security Center las siguientes 35 recomendaciones en versión preliminar para aumentar la cobertura de la prueba comparativa.

Las recomendaciones de la versión preliminar no representan un recurso incorrecto y no se incluyen en los cálculos de una puntuación segura. Corríjalas siempre que sea posible, de tal forma que, cuando finalice el período de versión preliminar, contribuyan a la puntuación. Puede encontrar más información sobre cómo responder a estas recomendaciones en [Recomendaciones de corrección en Azure Security Center](security-center-remediate-recommendations.md).

| Control de seguridad                     | Nuevas recomendaciones                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Habilitación del cifrado de datos en reposo            | - Las cuentas de Azure Cosmos DB deben usar claves administradas por el cliente para cifrar los datos en reposo.<br>- Las áreas de trabajo de Azure Machine Learning deben cifrarse con una clave administrada por el cliente (CMK).<br>- La protección de datos de Bring Your Own Key debe estar habilitada para los servidores MySQL.<br>- La protección de datos de Bring Your Own Key debe estar habilitada para los servidores PostgreSQL.<br>- Las cuentas de Cognitive Services deben habilitar el cifrado de datos con una clave administrada por el cliente (CMK).<br>- Las instancias de Container Registry se deben cifrar con una clave administrada por el cliente (CMK).<br>- Las instancias administradas de SQL deben usar claves administradas por el cliente para cifrar los datos en reposo.<br>- Los servidores SQL deben usar claves administradas por el cliente para cifrar los datos en reposo.<br>- Las cuentas de almacenamiento deben usar la clave administrada por el cliente (CMK) para el cifrado.                                                                                                                                                              |
| Implementación de procedimientos recomendados de seguridad    | - Las suscripciones deben tener una dirección de correo electrónico de contacto para los problemas de seguridad.<br> - El aprovisionamiento automático del agente de Log Analytics debe estar habilitado en su suscripción.<br> - La opción para enviar notificaciones por correo electrónico para alertas de gravedad alta debe estar habilitada.<br> - La opción para enviar notificaciones por correo electrónico al propietario de la suscripción en relación a alertas de gravedad alta debe estar habilitada.<br> - Los almacenes de claves deben tener habilitada la protección contra operaciones de purga.<br> - Los almacenes de claves deben tener habilitada la eliminación temporal. |
| Administración de acceso y permisos        | - Las aplicaciones de funciones deben tener la opción "Certificados de cliente (certificados de cliente entrantes)" habilitada. |
| Protección de aplicaciones contra ataques DDoS | - Web Application Firewall (WAF) debe estar habilitado para Application Gateway.<br> - Web Application Firewall (WAF) debe estar habilitado para Azure Front Door Service. |
| Restricción de los accesos de red no autorizados | - El firewall debe estar habilitado en Key Vault.<br> - Se debe configurar un punto de conexión privado para Key Vault.<br> - App Configuration debe usar un vínculo privado.<br> - Azure Cache for Redis debe residir en una red virtual.<br> - Los dominios de Azure Event Grid deben usar un vínculo privado.<br> - Los temas de Azure Event Grid deben usa un vínculo privado.<br> - Las áreas de trabajo de Azure Machine Learning deben usar un vínculo privado.<br> Azure SignalR Service debe usar un vínculo privado.<br> - Azure Spring Cloud debe usar la inserción de red.<br> -Las instancias de Container Registry no deben permitir el acceso de red sin restricciones.<br> - Las instancias de Container Registry deben usar vínculo privado.<br> -El acceso a redes públicas debe estar deshabilitado para los servidores de MariaDB.<br> El acceso a las redes públicas debe estar deshabilitado para los servidores de MySQL<br> - El acceso a redes públicas debe estar deshabilitado para los servidores de PostgreSQL.<br> - La cuenta de almacenamiento debería utilizar una conexión de vínculo privado<br> - Las cuentas de almacenamiento deben restringir el acceso a la red mediante el uso de reglas de red virtual.<br> - Las plantillas de VM Image Builder deben usar un vínculo privado.|
|                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

Vínculos relacionados:

- [Más información sobre Azure Security Benchmark](../security/benchmarks/introduction.md)
- [Más información sobre Azure Database for MariaDB](../mariadb/overview.md)
- [Más información sobre Azure Database for MySQL](../mysql/overview.md)
- [Más información sobre Azure Database for PostgreSQL](../postgresql/overview.md)





## <a name="next-steps"></a>Pasos siguientes

Para ver todos los cambios recientes realizados en el producto, consulte [Novedades de Azure Security Center](release-notes.md).