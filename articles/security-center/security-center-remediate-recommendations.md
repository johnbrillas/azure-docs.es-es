---
title: Recomendaciones de corrección de Azure Security Center | Microsoft Docs
description: En este artículo se explica cómo responder a las recomendaciones de Azure Security Center para proteger los recursos y cumplir con las directivas de seguridad.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 8be947cc-cc86-421d-87a6-b1e23077fd50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/08/2020
ms.author: memildin
ms.openlocfilehash: dabd7e9e2c3c74225efc4611c7ad3523a6c76ba5
ms.sourcegitcommit: 02ed9acd4390b86c8432cad29075e2204f6b1bc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97807997"
---
# <a name="remediate-recommendations-in-azure-security-center"></a>Recomendaciones de corrección en Azure Security Center

Las recomendaciones ofrecen sugerencias sobre cómo proteger mejor los recursos. Para implementar una recomendación, siga los pasos de corrección proporcionados en la recomendación.

## <a name="remediation-steps"></a>Pasos de corrección <a name="remediation-steps"></a>

Después de revisar todas las recomendaciones, decida cuál se corrige primero. Se recomienda priorizar los controles de seguridad con el máximo potencial de aumentar la puntuación de seguridad.

1. En la lista, seleccione una recomendación.

1. Siga las instrucciones de la sección **Pasos para la corrección**. Cada recomendación tiene su propio conjunto de instrucciones. En la siguiente captura de pantalla se muestran los pasos de corrección para configurar aplicaciones, a fin de permitir solo el tráfico a través de HTTPS.

    :::image type="content" source="./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png" alt-text="Pasos de corrección manual para una recomendación" lightbox="./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png":::

1. Una vez completado, aparece una notificación que le informa de si el problema se ha resuelto.

## <a name="quick-fix-remediation"></a>Solución por corrección rápida

Para simplificar la corrección y mejorar la seguridad del entorno (y aumentar la puntuación segura), muchas recomendaciones incluyen una opción de corrección rápida.

La corrección rápida le ayuda a solucionar rápidamente una recomendación en varios recursos.

> [!TIP]
> Las soluciones de corrección rápida solo están disponibles para recomendaciones específicas. Para encontrar las recomendaciones que tienen una corrección rápida disponible, use el filtro **Acciones de respuesta** para ver la lista de recomendaciones:
> 
> :::image type="content" source="media/security-center-remediate-recommendations/quick-fix-filter.png" alt-text="Uso de los filtros sobre la lista de recomendaciones para buscar las recomendaciones que tengan la opción de corrección rápida":::

Para implementar una solución de corrección rápida:

1. En la lista de recomendaciones que tienen la etiqueta **Corrección rápida**, , seleccione una recomendación.

    [![Seleccione Corrección rápida](media/security-center-remediate-recommendations/security-center-quick-fix-select.png)](media/security-center-remediate-recommendations/security-center-quick-fix-select.png#lightbox)

1. En la pestaña **Recursos con estado incorrecto**, seleccione los recursos en los que desea implementar la recomendación y seleccione **Corregir**.

    > [!NOTE]
    > Algunos de los recursos enumerados podrían estar deshabilitados porque no tiene los permisos adecuados para modificarlos.

1. En el cuadro de confirmación, lea los detalles y las implicaciones de la corrección.

    ![Corrección rápida](./media/security-center-remediate-recommendations/security-center-quick-fix-view.png)

    > [!NOTE]
    > Las implicaciones se muestran en el cuadro gris de la ventana **Corregir recursos** que se abre después de hacer clic en **Corregir**. Estas enumeran los cambios que se producen al continuar con la solución por corrección rápida.

1. Inserte los parámetros pertinentes si es necesario y apruebe la corrección.

    > [!NOTE]
    > Una vez completada la corrección, pueden pasar varios minutos hasta que los recursos se vean en la pestaña **Recursos con estado correcto**. Para ver las acciones de la corrección, consulte el [registro de actividad](#activity-log).

1. Una vez completada, aparece una notificación que le informa si la corrección se ha realizado correctamente.

## <a name="quick-fix-remediation-logging-in-the-activity-log"></a>Registro de la corrección rápida en el registro de actividad <a name="activity-log"></a>

La operación de corrección usa una implementación de plantilla o una llamada API REST PATCH para aplicar la configuración en el recurso. Estas operaciones se registran en el [registro de actividad de Azure](../azure-resource-manager/management/view-activity-logs.md).


## <a name="next-steps"></a>Pasos siguientes

En este documento, se mostró cómo aplicar las recomendaciones de corrección de Security Center. Para obtener más información sobre Security Center, vea las páginas siguientes:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
