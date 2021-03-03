---
title: Crear un recurso de Text Analytics de Cognitive Services
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo crear un recurso de Text Analytics de Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/09/2021
ms.author: aahi
ms.openlocfilehash: 5b6479d48a51ba962f2f6bfba16dac3b0886a9ff
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750662"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Crear un recurso de Text Analytics de Cognitive Services

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione **Crear un recurso** y vaya a **IA y Machine Learning** > **Text Analytics**.
   O bien, vaya a [Create Text Analytics](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) (Crear un recurso de Text Analytics).
1. Establezca todas las configuraciones necesarias:

    |Configuración|Value|
    |--|--|
    |Nombre|Escriba un nombre (2-64 caracteres).|
    |Suscripción|Seleccione la suscripción adecuada.|
    |Location|Seleccione una ubicación cercana.|
    |Plan de tarifa| Escriba **S**, que corresponde al plan de tarifa estándar.|
    |Resource group|Seleccione un grupo de recursos disponible.|

1. Seleccione **Crear** y espere a que el recurso se cree. El explorador se redirige automáticamente a la página de recursos recién creada.
1. Recopile el elemento `endpoint` configurado y una clave de API:

    |Pestaña de recursos en el portal|Configuración|Value|
    |--|--|--|
    |**Información general**|Punto de conexión|Copie el punto de conexión. Tiene un aspecto similar a `https://my-resource.cognitiveservices.azure.com/text/analytics/v3.0`.|
    |**Claves**|Clave de API|Copie una de las dos claves. Es una cadena alfanumérica de 32 caracteres sin espacios ni guiones: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
