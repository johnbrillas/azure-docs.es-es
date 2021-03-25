---
title: Cambio de tamaño de un clúster de Azure Stream Analytics
description: Obtenga información acerca de cómo escalar y reducir verticalmente el tamaño de un clúster de Azure Stream Analytics.
author: sidramadoss
ms.author: sidram
ms.service: stream-analytics
ms.topic: overview
ms.custom: mvc
ms.date: 09/22/2020
ms.openlocfilehash: e469e9bfd13bf8d89084b66e954cb51b27c2ebc2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98020033"
---
# <a name="resize-an-azure-stream-analytics-cluster"></a>Cambio de tamaño de un clúster de Azure Stream Analytics

La capacidad de un clúster de Stream Analytics se mide en unidades de streaming (SU). Se pueden ejecutar varios trabajos en paralelo en el mismo clúster, siempre y cuando la suma de unidades de streaming asignadas a todos los trabajos en ejecución no supere la capacidad del clúster.

La capacidad del clúster se puede escalar y reducir verticalmente para que coincida con el tamaño de las cargas de trabajo de streaming. El escalado de un clúster lleva un tiempo, y no está previsto realizar escalados frecuentes. Le recomendamos que planifique y aprovisione un clúster con un número exacto de unidades de streaming que planee consumir.

## <a name="change-the-scale-of-your-cluster"></a>Cambio de la escala del clúster

1. En Azure Portal, busque y seleccione el clúster de Stream Analytics.

1. En la sección **Información general**, seleccione **Escala**. Puede ver cuántas de unidades de streaming están asignados al clúster. Use el selector para aumentar o disminuir las unidades de streaming según sea necesario.

   ![Escalado del clúster](./media/scale-cluster/scale-cluster.png)

La operación de escalado no afecta a los trabajos que se están ejecutando.

## <a name="next-steps"></a>Pasos siguientes

Ahora sabe cómo puede escalar y reducir verticalmente los clústeres de Stream Analytics. A continuación, puede obtener información sobre la administración de puntos de conexión privados y el escalado automático de los trabajos:

* [Administración de puntos de conexión privados en un clúster de Azure Stream Analytics](private-endpoints.md)
* [Administración de trabajos en un clúster de Azure Stream Analytics](manage-jobs-cluster.md)
