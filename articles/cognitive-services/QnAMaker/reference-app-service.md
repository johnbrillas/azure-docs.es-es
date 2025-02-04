---
title: 'Configuración del servicio: QnA Maker'
description: Comprenda cómo y dónde configurar los recursos.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 11/9/2020
ms.openlocfilehash: da46084c8c2616284c31ef155927e8dbcbd19e0e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102209366"
---
# <a name="service-configuration"></a>Configuración del servicio

Cada versión de QnA Maker usa un conjunto diferente de recursos de Azure (servicios). En este artículo se describen las personalizaciones admitidas para estos servicios. 

## <a name="app-service"></a>App Service

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

QnA Maker usa una instancia de App Service para proporcionar el tiempo de ejecución de la consulta que usa [generateAnswer API](/rest/api/cognitiveservices/qnamaker4.0/runtime/generateanswer).

Esta configuración está disponible en Azure Portal para la instancia de App Service. La configuración está disponible seleccionando **Configuración** y **Configuración**.

Puede establecer una configuración individual a través de la lista de configuración de la aplicación o modificar varias opciones de configuración seleccionando **Edición avanzada**.

|Resource|Configuración|
|--|--|
|AzureSearchAdminKey|Cognitive Search: se usa para el almacenamiento de pares de QnA y clasificador n.º 1|
|AzureSearchName|Cognitive Search: se usa para el almacenamiento de pares de QnA y clasificador n.º 1|
|DefaultAnswer|Texto de respuesta cuando no se encuentra ninguna coincidencia|
|UserAppInsightsAppId|Registro de chat y telemetría|
|UserAppInsightsKey|Registro de chat y telemetría|
|UserAppInsightsName|Registro de chat y telemetría|
|QNAMAKER_EXTENSION_VERSION|Siempre se establece en _latest_. Este valor inicializará la extensión de sitio QnA Maker en App Service.|

Debe **reiniciar** el servicio en la página **Información general** de Azure Portal, una vez que haya terminado de realizar los cambios.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

Las personalizaciones de App Service no se aplican a QnA Maker administrado (versión preliminar).

---

## <a name="qna-maker-service"></a>Servicio QnA Maker

El servicio QnA Maker proporciona la configuración para que los siguientes usuarios colaboren en un único servicio QnA Maker y en todas sus bases de conocimiento.

Aprenda a [agregar colaboradores](./reference-role-based-access-control.md) al servicio.

## <a name="change-azure-cognitive-search"></a>Cambio de Azure Cognitive Search

Aprenda [a cambiar Azure Cognitive Search](./how-to/configure-QnA-Maker-resources.md#configure-qna-maker-to-use-different-cognitive-search-resource) vinculado a un servicio QnA Maker.

## <a name="change-default-answer"></a>Cambio de la respuesta predeterminada

Aprenda [a cambiar el texto de las respuestas predeterminadas](How-To/change-default-answer.md). 

## <a name="telemetry"></a>Telemetría

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

Application Insights se usa para supervisar la telemetría con QnA Maker GA. No hay valores de configuración específicos de QnA Maker.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

Aprenda [a agregar datos de telemetría a un servicio QnA Maker administrado (versión preliminar)](How-To/get-analytics-knowledge-base.md). 

---

## <a name="app-service-plan"></a>Plan de servicio de aplicación

# <a name="qnamaker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

El plan de App Service no tiene valores de configuración específicos de QnA Maker.

# <a name="qnamaker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

App Service Plan no se utiliza con QnA Maker administrado (versión preliminar).

---

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los [formatos](reference-document-format-guidelines.md) para los documentos y las direcciones URL que desea importar en una base de conocimiento.
