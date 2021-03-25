---
title: Supervisión de la referencia de datos de Media Services
description: Material de referencia importante necesario al supervisar Media Services
author: IngridAtMicrosoft
ms.author: inhenkel
manager: femila
ms.topic: reference
ms.service: media-services
ms.date: 03/17/2021
ms.openlocfilehash: 866b2faf473f06fc3f85cdb434d6555504a7f6a8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104598191"
---
# <a name="monitoring-media-services-data-reference"></a>Supervisión de la referencia de datos de Media Services

En este artículo se tratan los datos que son útiles para la supervisión de Media Services. Para más información sobre todas las métricas de la plataforma que se admiten en Azure Monitor, consulte [Métricas compatibles con Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported).

## <a name="media-services-metrics"></a>Métricas de Media Services

Las métricas se recopilan a intervalos regulares independientemente de si cambia el valor o no. Son útiles para las alertas, dado que se pueden muestrear con frecuencia y se puede activar una alerta con rapidez con una lógica relativamente sencilla.

Media Services permite supervisar las métricas para los siguientes recursos:

* Cuenta
* Punto de conexión de streaming

### <a name="account"></a>Cuenta

Puede supervisar las siguientes métricas de cuenta.

|Nombre de métrica|Nombre para mostrar|Descripción|
|---|---|---|
|AssetCount|Número de recursos|Recursos en su cuenta.|
|AssetQuota|Cuota de recursos|Cuota de recursos en su cuenta.|
|AssetQuotaUsedPercentage|Porcentaje de cuota de recursos usado|Porcentaje de la cuota de recursos ya utilizada.|
|ContentKeyPolicyCount|Número de directivas de clave de contenido|Directivas de clave de contenido en su cuenta.|
|ContentKeyPolicyCount|Cuota de directiva de clave de contenido|Cuota de directivas de clave de contenido en su cuenta.|
|ContentKeyPolicyQuotaUsedPercentage|Porcentaje utilizado de la cuota de directivas de clave de contenido|Porcentaje de la cuota de la directiva de clave de contenido ya utilizado.|
|StreamingPolicyCount|Número de directivas de streaming|Directivas de streaming en su cuenta.|
|StreamingPolicyQuota|Cuota de directivas de streaming|Cuota de directivas de streaming en su cuenta.|
|StreamingPolicyQuotaUsedPercentage|Porcentaje usado de la cuota de directiva de streaming|Porcentaje de la cuota de la directiva de streaming ya utilizado.|

También debe revisar las [cuotas y límites](../limits-quotas-constraints.md) de la cuenta.

### <a name="streaming-endpoint"></a>Punto de conexión de streaming

Se admiten estas métricas de [Puntos de conexión de streaming](/rest/api/media/streamingendpoints) de Media Services:

|Nombre de métrica|Nombre para mostrar|Descripción|
|---|---|---|
|Requests|Requests|Proporciona el número total de solicitudes HTTP atendidas por el punto de conexión de streaming.|
|Salida|Salida|Bytes de salida totales por minuto por punto de conexión de streaming.|
|SuccessE2ELatency|Latencia correcta de extremo a extremo|Tiempo que transcurre desde que el punto de conexión de streaming recibió la solicitud hasta el momento en que se envió el último byte de la respuesta.|
|Uso de CPU| | Uso de CPU para puntos de conexión de streaming prémium. Estos datos no están disponibles para los puntos de conexión de streaming estándar. |
|Ancho de banda de salida | | Ancho de banda de salida en bits por segundo.|

## <a name="metric-dimensions"></a>Dimensiones de métricas

Para obtener más información sobre las dimensiones de métricas, consulte [Métricas multidimensionales](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#multi-dimensional-metrics).

<!--**PLACEHOLDER** for dimensions table.-->

## <a name="resource-logs"></a>Registros del recurso

## <a name="media-services-diagnostic-logs"></a>Registros de diagnóstico de Media Services

Los registros de diagnóstico proporcionan datos exhaustivos y frecuentes acerca del funcionamiento de un recurso de Azure. Para más información, aprenda a [recopilar y usar los datos de registro provenientes de los recursos de Azure](https://docs.microsoft.com/azure/azure-monitor/essentials/platform-logs-overview.md).

Azure Media Services admite los siguientes registros de diagnóstico:

* Entrega de claves

### <a name="key-delivery"></a>Entrega de claves

|Nombre|Descripción|
|---|---|
|Solicitud de servicio de entrega de claves|Registros que muestran la información de la solicitud de servicio de entrega de claves. Para más información, consulte los [esquemas](monitor-media-services-data-reference.md).|

## <a name="schemas"></a>Esquemas

Para obtener una descripción detallada del esquema de registros de diagnóstico de nivel superior, consulte [Servicios, esquemas y categorías admitidos en los registros de diagnóstico de Azure](https://docs.microsoft.com/azure/azure-monitor/essentials/resource-logs-schema.md).

## <a name="key-delivery-log-schema-properties"></a>Propiedades del esquema de registro de entrega de claves

Estas propiedades son específicas del esquema de registro de entrega de claves.

|Nombre|Descripción|
|---|---|
|keyId|Identificador de la clave solicitada.|
|keyType|Puede ser uno de los siguientes valores: "Clear" (sin cifrado), "FairPlay", "PlayReady" o "Widevine".|
|policyName|Nombre de Azure Resource Manager de la directiva.|
|tokenType|El tipo del token.|
|statusMessage|Mensaje de estado.|

### <a name="example"></a>Ejemplo

Propiedades del esquema de solicitudes de entrega de claves.

```json
{
    "time": "2019-01-11T17:59:10.4908614Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Succeeded",
    "resultSignature": "OK",
    "durationMs": 315,
    "identity": {
        "authorization": {
            "issuer": "http://testacs",
            "audience": "urn:test"
        },
        "claims": {
            "urn:microsoft:azure:mediaservices:contentkeyidentifier": "3321e646-78d0-4896-84ec-c7b98eddfca5",
            "iss": "http://testacs",
            "aud": "urn:test",
            "exp": "1547233138"
        }
    },
    "level": "Informational",
    "location": "uswestcentral",
    "properties": {
        "requestId": "b0243468-d8e5-4edf-a48b-d408e1661050",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "JWT",
        "statusMessage": "OK"
    }
} 
```

```json
 {
    "time": "2019-01-11T17:59:33.4676382Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Failed",
    "resultSignature": "Unauthorized",
    "durationMs": 2,
    "level": "Error",
    "location": "uswestcentral",
    "properties": {
        "requestId": "875af030-b77c-416b-b7e1-58f23ebec182",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "None",
        "statusMessage": "No token present in authorization header or URL."
    }
} 
```

>[!NOTE]
> Widevine es un servicio que ofrece Google Inc. y que está sujeto a los términos del servicio y la directiva de privacidad de Google, Inc.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [monitoring-next-steps](../includes/monitoring-next-steps.md)]
