---
title: 'Actualización a Azure Defender: Azure Security Center'
description: En este inicio rápido se muestra cómo realizar la actualización a Azure Defender de Security Center para incrementar la seguridad.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: quickstart
ms.date: 02/17/2021
ms.author: memildin
ms.openlocfilehash: 2f37a59d5db3883754b602b2b2525e07b57206b7
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102099359"
---
# <a name="quickstart-set-up-azure-security-center"></a>Inicio rápido: Configuración de Azure Security Center

Azure Security Center proporciona administración unificada de la seguridad y protección contra amenazas para cargas de trabajo híbridas y de varias nubes. Aunque las características gratuitas solo ofrecen una seguridad limitada para los recursos de Azure, al habilitar Azure Defender, se amplían estas funcionalidades tanto en un entorno local como en otras nubes. Azure Defender le ayuda a encontrar y corregir los puntos vulnerables de seguridad, aplicar controles de acceso y de aplicación para bloquear actividades malintencionadas, detectar amenazas mediante análisis e inteligencia, y responder rápidamente en caso de ataque. Puede probar Azure Defender sin costo alguno. Para más información, consulte la [página de precios](https://azure.microsoft.com/pricing/details/security-center/).

Esta sección de inicio rápido le guiará por todos los pasos recomendados para habilitar Azure Security Center y Azure Defender. Cuando haya completado todos los pasos de inicio rápido, tendrá:

> [!div class="checklist"]
> * Security Center habilitado en las suscripciones de Azure
> * Azure Defender habilitado en las suscripciones de Azure
> * Configuración de la recopilación automática de datos
> * Configuración de notificaciones por correo electrónico de alertas de seguridad
> * Las máquinas híbridas y de varias nubes conectadas a Azure

## <a name="prerequisites"></a>Prerrequisitos
Para empezar a trabajar con el Centro de seguridad, debe disponer de una suscripción a Microsoft Azure. Si no tiene una suscripción, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).

Para habilitar Azure Defender en una suscripción, debe tener asignados los roles Propietario de la suscripción, Colaborador de la suscripción o Administrador de seguridad.

## <a name="enable-security-center-on-your-azure-subscription"></a>Habilitación de Security Center en una suscripción de Azure

> [!TIP]
> Para habilitar Security Center en todas las suscripciones de un grupo de administración, consulte el artículo sobre la [habilitación de Security Center en varias suscripciones de Azure](onboard-management-group.md).

1. Inicie sesión en el [Portal de Azure](https://azure.microsoft.com/features/azure-portal/).

1. En el menú del portal, seleccione **Security Center**. 

    Se abre la página de información general de Security Center.

    :::image type="content" source="./media/security-center-get-started/overview.png" alt-text="Panel de información general de Security Center" lightbox="./media/security-center-get-started/overview.png":::

    **Security Center - Información general** proporciona una vista unificada de la situación de seguridad de las cargas de trabajo de la nube híbrida, lo que le permite detectar y evaluar la seguridad de las cargas de trabajo e identificar y mitigar riesgos. Security Center habilita de forma automática y sin costo alguno cualquiera de las suscripciones de Azure que no fueran incorporadas previamente por usted o por otro usuario de la suscripción.

Puede ver la lista de suscripciones y filtrarla, si hace clic en el elemento de menú **Suscripciones**. Security Center ajustará la pantalla para reflejar la postura de seguridad de las suscripciones seleccionadas. 

A los pocos minutos de iniciar Security Center la primera vez, puede ver:

- **Recomendaciones** de formas de mejorar la seguridad de los recursos de Azure.
- Un inventario de los recursos que Security Center está evaluando, junto con la postura de seguridad de cada uno.

Para sacar el máximo partido de Security Center, continúe con los pasos siguientes de la sección de inicio rápido.



## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido ha habilitado Azure Security Center. El siguiente paso consiste en habilitar Azure Defender para la administración de seguridad unificada y protección contra amenazas en las cargas de trabajo de la nube híbrida.

> [!div class="nextstepaction"]
> [Inicio rápido: Habilitación de Azure Defender](enable-azure-defender.md)