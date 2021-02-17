---
title: Evitar errores de configuración con Azure Security Center
description: Aprenda a usar las opciones "Aplicar" y "Denegar" de Security Center en las páginas de detalles de recomendaciones.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 02/04/2021
ms.author: memildin
ms.openlocfilehash: 2607ff3a6c4bb7bf301818925990521572777ab9
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100379203"
---
# <a name="prevent-misconfigurations-with-enforcedeny-recommendations"></a>Evitar errores de configuración con las recomendaciones Aplicar o Denegar

Los errores de configuración de seguridad son una causa importante de los incidentes de seguridad. Security Center ofrece ahora la posibilidad de ayudar a *evitar* errores de configuración de los nuevos recursos con respecto a recomendaciones específicas. 

Esta característica puede ayudar a mantener protegidas las cargas de trabajo y a estabilizar la puntuación segura.

La aplicación de una configuración segura, basada en una recomendación específica, se ofrece de dos modos:

- Con el efecto **Denegar** de Azure Policy, puede impedir que se creen recursos en mal estado.
- Con la opción **Aplicar**, puede sacar partido del efecto **DeployIfNotExist** de Azure Policy y corregir automáticamente los recursos no compatibles tras la creación.

Esta opción se puede encontrar en la parte superior de la página de detalles del recurso de las recomendaciones de seguridad seleccionadas (consulte [Recomendaciones con las opciones denegar o aplicar](#recommendations-with-denyenforce-options)).

## <a name="prevent-resource-creation"></a>Impedir la creación de recursos

1. Abra la recomendación que deben satisfacer los nuevos recursos y seleccione el botón **Denegar** en la parte superior de la página.

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-deny-button.png" alt-text="Página de recomendaciones con el botón Denegar resaltado":::

    Se abre el panel de configuración con las opciones de ámbito. 

1. Establezca el ámbito seleccionando la suscripción o el grupo de administración en cuestión.

    > [!TIP]
    > Puede usar los tres puntos que hay al final de la fila para cambiar una única suscripción, o bien usar las casillas para seleccionar varias suscripciones o grupos y, luego, elegir **Change to Deny** (Cambiar a Denegar).

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-prevent-resource-creation.png" alt-text="Establecimiento del ámbito de denegación de Azure Policy":::


## <a name="enforce-a-secure-configuration"></a>Aplicación de una configuración segura

1. Abra la recomendación para la que se implementará una plantilla si los nuevos recursos no la satisfacen y seleccione el botón **Aplicar** en la parte superior de la página.

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-enforce-button.png" alt-text="Página de recomendaciones con el botón Aplicar resaltado":::

    Se abre el panel de configuración con todas las opciones de configuración de directiva. 

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-enforce-config.png" alt-text="Aplicación de opciones de configuración":::

1. Establezca el ámbito, el nombre de la asignación y otras opciones pertinentes.

1. Seleccione **Revisar + crear**.

## <a name="recommendations-with-denyenforce-options"></a>Recomendaciones con las opciones denegar o aplicar

Estas recomendaciones se pueden usar con la opción **Denegar**:

[!INCLUDE [azure-security-center-recommendations-deny](../../includes/asc/recommendations-with-deny.md)]

Estas recomendaciones se pueden usar con la opción **Aplicar**:

- La auditoría de SQL Server debe estar habilitada
- Azure Backup debería habilitarse en las máquinas virtuales
- Azure Defender para SQL debe estar habilitada en los servidores de SQL
- Se deben habilitar los registros de diagnóstico en Azure Stream Analytics
- Se deben habilitar los registros de diagnóstico en las cuentas de Batch
- Los registros de diagnóstico de Data Lake Analytics deben estar habilitados
- Los registros de diagnóstico del centro de eventos deben estar habilitados
- Los registros de diagnóstico en Key Vault deben estar habilitados
- Los registros de diagnóstico de Logic Apps deben estar habilitados
- Los registros de diagnóstico de los servicios de Search deben estar habilitados.
- Los registros de diagnóstico de Service Bus deben estar habilitados
