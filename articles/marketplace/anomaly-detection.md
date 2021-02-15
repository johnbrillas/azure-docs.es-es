---
title: Detección de anomalías para la facturación de uso medido | Azure Marketplace
description: Obtenga información sobre cómo la detección automática de anomalías para la facturación de uso medido ayuda a asegurarse de que se facture correctamente a los clientes para el uso medido de la oferta de marketplace comercial.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 1/09/2021
author: sayantanroy83
ms.author: sroy
ms.openlocfilehash: d4fb88854359dcd6e383b47d2a8ce4e9c91f867a
ms.sourcegitcommit: 7e117cfec95a7e61f4720db3c36c4fa35021846b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99989246"
---
# <a name="anomaly-detection-for-metered-billing"></a>Detección de anomalías para la facturación de uso medido

En este artículo se proporcionan detalles sobre el servicio de medición de Marketplace y la funcionalidad de detección de anomalías automatizada asociada para ayudar a garantizar la facturación correcta a los clientes para su uso medido. La opción de facturación de uso medido está disponible actualmente para las ofertas de[software como servicio](plan-saas-offer.md) (SaaS) y las [aplicaciones de Azure](plan-azure-application-offer.md#types-of-plans) con un plan de aplicación administrada. Esta opción permite a los asociados crear ofertas en el programa de marketplace comercial que se cobran según unidades no estándar.

Los asociados que tienen medidores personalizados implementados para SaaS y aplicaciones administradas pueden ver una desviación del comportamiento de uso esperado como anomalías para los _eventos de uso por encima del límite_ en _medidores personalizados_ específicos del Centro de partners. Para reducir este riesgo, el Centro de partners usa un servicio de detección de anomalías que aplica algoritmos de aprendizaje automático para determinar el comportamiento normal de la facturación de uso medido, analizar el uso de facturación medida y detectar anomalías con una intervención mínima del usuario. El uso de _modelos de detección de anomalías_ en los conjuntos de datos de uso de facturación medida, el Centro de partners pretende informar al publicador cada vez que el uso notificado supere el uso esperado.

## <a name="usability-experience"></a>Experiencia de facilidad de uso

Microsoft confía en el asociado para notificar el uso por encima del límite de su cliente de sus ofertas de aplicaciones administradas de Azure o SaaS antes de que Microsoft facture al cliente. Si se notifica el uso incorrecto, el cliente podría recibir una factura incorrecta, perjudicando la credibilidad tanto del asociado como de Microsoft.

Para ayudar a mitigar esto, se proporciona una característica de detección de anomalías automatizada para los planes de la aplicación administrada de la aplicación de Azure y las aplicaciones SaaS. Esta característica es un modelo de aprendizaje automático que supervisa de forma proactiva el uso de facturación medida y predice el valor esperado de uso dentro del intervalo esperado. Si el uso está fuera del intervalo esperado, se trata como una anomalía y se muestra una notificación de alerta al asociado en la página Información general de la oferta en el programa de marketplace comercial del Centro de partners.

El modelo de aprendizaje automático analiza el uso por encima del límite de forma diaria. El publicador puede ver todas las anomalías que se indican en relación con el uso por encima del límite de sus clientes para cada una de las dimensiones de medidor personalizadas de la oferta.

### <a name="view-and-manage-metered-usage-anomalies"></a>Visualización y administración de anomalías de uso medido

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard/home).
1. En el menú de navegación izquierdo, seleccione **Marketplace comercial** > **Analizar**.
1. Seleccione la pestaña **Anomalías de uso medido**.

    [![Muestra la pestaña Anomalías de uso medido en la página Uso.](./media/anomaly-detection/metered-usage-anomalies.png)](./media/anomaly-detection/metered-usage-anomalies.png#lightbox)
    ***Figura 1: Pestaña Anomalías de uso medido***

1. En el caso de las anomalías de uso detectadas en la facturación de uso medido, como publicador, se le pedirá que investigue y confirme si la anomalía es verdadera o no. Seleccione **Marcar como anomalía** para confirmar el diagnóstico.

     [![Muestra el cuadro de diálogo Marcar como anomalía.](./media/anomaly-detection/mark-as-anomaly.png)](./media/anomaly-detection/mark-as-anomaly.png#lightbox)
    ***Figura 2: Cuadro de diálogo Marcar como anomalía***

1. Si cree que la anomalía de uso por encima del límite que detectamos no es auténtica, puede proporcionar esos comentarios seleccionando **No es una anomalía** para la anomalía marcada del Centro de partners en el uso por encima del límite en particular.

    [![Muestra el cuadro de diálogo ¿Por qué no es una anomalía?](./media/anomaly-detection/why-is-it-not-an-anomaly.png)](./media/anomaly-detection/why-is-it-not-an-anomaly.png#lightbox)
    ***Figura 3: Cuadro de diálogo ¿Por qué no es una anomalía?***

1. Puede desplazarse hacia abajo en la página para ver una lista de inventario de anomalías no confirmadas. La lista proporciona un inventario de anomalías que no ha confirmado. Puede elegir marcar cualquiera de las anomalías marcadas del Centro de partners como auténtica o falsa.

   [![Muestra la lista de anomalías no confirmadas del Centro de partners en la página Uso.](./media/anomaly-detection/unacknowledged-anomalies.png)](./media/anomaly-detection/unacknowledged-anomalies.png#lightbox)
    ***Ilustración 4: Lista de anomalías no confirmadas del Centro de partners***

1. También vería un registro de acciones de anomalías que muestra las acciones que realizó en los usos por encima del límite. En el registro de acciones, podrá ver qué eventos de uso por encima del límite se marcaron como auténticos o falsos.

   [![Muestra el registro de acciones de anomalías en la página Uso.](./media/anomaly-detection/anomaly-action-log.png)](./media/anomaly-detection/anomaly-action-log.png#lightbox)
   ***Figura 5: Registro de acciones de anomalías***

1. El análisis del Centro de partners no admitirá la redefinición eventos de uso por encima del límite en los informes de exportación. El Centro de partners le permite escribir el uso por encima del límite para una anomalía y los detalles se pasan a Microsoft Teams para su investigación. En función de la investigación, Microsoft emitirá reembolsos de crédito al cliente sobrecargado, según corresponda. Al seleccionar cualquiera de las anomalías marcadas, puede seleccionar **Marcar como anomalía** para marcar la anomalía de uso por encima del límite como genuina.

   [![Muestra el cuadro de diálogo Marcar como anomalía.](./media/anomaly-detection/new-reported-usage.png)](./media/anomaly-detection/new-reported-usage.png#lightbox)
   ***Figura: 6: Cuadro de diálogo Marcar como anomalía***

La primera vez que se marque un uso por encima del límite como irregular en el Centro de partners, obtendrá una ventana de 30 días de esa instancia para marcar la anomalía como auténtica o falsa. Después del período de 30 días, como publicador, no podría actuar sobre las anomalías.

> [!IMPORTANT]
> Las unidades de uso por encima del límite sin notificar no cumplen los requisitos para la redefinición y el ajuste financiero.

Puede ver todas las anomalías (confirmadas o de otro modo) durante el período de cálculo seleccionado. Los distintos períodos de cálculo son los últimos 30 días, los últimos 60 días y los últimos 90 días.

> [!IMPORTANT]
> Se le solicitará que actúe sobre las anomalías marcadas en un plazo de 30 días desde el momento en que las anomalías se notifiquen por primera vez en el Centro de partners.

El filtro modal en el widget permite seleccionar ofertas individuales y medidores personalizados individuales.

Después de marcar un uso por encima del límite como anomalía o reconocer un modelo que marcó una anomalía como genuina, no se puede cambiar la selección a "No es una anomalía".

> [!IMPORTANT]
> Puede volver a enviar los usos por encima del límite en caso de situaciones de sobrecarga.

## <a name="see-also"></a>Consulte también
- [Facturación según uso mediante el servicio de medición de marketplace comercial](./partner-center-portal/saas-metered-billing.md)
- [Facturación según uso de aplicaciones administradas](./partner-center-portal/azure-app-metered-billing.md)
