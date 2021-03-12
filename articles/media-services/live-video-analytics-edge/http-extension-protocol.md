---
title: 'Protocolo de la extensión HTTP: Azure'
description: En este artículo, obtendrá información sobre el uso del protocolo de la extensión HTTP para enviar mensajes entre el módulo de Live Video Analytics y el módulo de inteligencia artificial o CV.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: c3a9b5b70467479fdb9b1473a1f6738eaa8a7e1b
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102455897"
---
# <a name="http-extension-protocol"></a>Protocolo de la extensión HTTP

Live Video Analytics en IoT Edge permite ampliar las funcionalidades de procesamiento de grafos multimedia mediante un [nodo de extensión de grafos](/azure/media-services/live-video-analytics-edge/media-graph-extension-concept). Si usa el procesador de extensiones HTTP como nodo de extensión, la comunicación entre el módulo de Live Video Analytics y su módulo de IA o CV se realiza a través de HTTP.

En este artículo, obtendrá información sobre el uso del protocolo de la extensión HTTP para enviar mensajes entre el módulo de Live Video Analytics y el módulo de inteligencia artificial o CV. 

El contrato HTTP se define entre los dos componentes siguientes:

* Servidor HTTP
* El módulo de Live Video Analytics en IoT Edge que actúa como cliente HTTP

## <a name="request"></a>Solicitud

Las solicitudes del módulo Live Video Analytics al servidor HTTP serían así:

|Clave|Value|
|---|---|
|POST|`https://hostname/optional-path?optional-query`|
|Accept|application/json,  */*|
|Authorization|Básica, Implícita, Portador (por medio de compatibilidad con encabezado personalizado)|
|Content-Type|image/jpeg<br/>image/png<br/>image/bmp<br/>image/x-raw|
|Longitud de cuerpo longitud de contenido, en bytes.|
|User-Agent|Azure Media Services|
|Body|Bytes de imagen, codificados en binario en uno de los tipos de contenido admitidos.|

### <a name="example"></a>Ejemplo

```
POST http://localhost:8080/inference HTTP/1.1
Host: localhost:8080
x-ms-client-request-id: d6050cd4-c9f2-42d3-9adc-53ba7e440f17
Content-Type: image/bmp
Content-Length: 519222

(Image Binary Content)
```

## <a name="response"></a>Response

Las respuestas del módulo al módulo de Live Video Analytics deben ser como sigue:

|Clave|Value|
|---|---|
|Códigos de estado|200 Correcto: Resultados de inferencia encontrados<br/>204 Sin contenido: La IA no ha encontrado ningún contenido<br/>400 Solicitud incorrecta: Inesperado<br/>500 Error interno del servidor: Inesperado<br/>503 Servidor ocupado: AMS se interrumpe en función del encabezado "Retry-after" o de una cantidad de tiempo predeterminada en caso de que el encabezado no esté preestablecido.|
|Content-Type|application/json|
|Content-Length|Longitud del cuerpo, en bytes|
|Body|Objeto JSON con una sola propiedad "inferences".|

### <a name="example"></a>Ejemplo

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 468
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 17 Apr 2020 04:44:01 GMT

{
  "inferences": [
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.9048132 },
        "box": { "l": 0.42681578, "t": 0.47660735, "w": 0.019501392, "h": 0.020954132 }
      }
    },
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.8953932 },
        "box": { "l": 0.55083525, "t": 0.4843858, "w": 0.046550274, "h": 0.046502113 }
      }
    }    
  ]
}
```

Se recomienda encarecidamente que las respuestas se devuelvan mediante documentos JSON válidos y que se siga el esquema preestablecido definido en el [modelo de objetos del esquema de metadatos de inferencia](/azure/media-services/live-video-analytics-edge/inference-metadata-schema). Esto garantiza mejor la interoperabilidad con otros componentes y posibles capacidades futuras agregadas al módulo Live Video Analytics.

Si el módulo devuelve una respuesta en la que el tipo de contenido no es "application/json", Live Video Analytics codifica el mensaje como contenido Base 64 y lo serializa como una carga JSON opaca.

Si el módulo devuelve una respuesta cuyo tipo de contenido es "application/json" pero el esquema JSON no sigue el esquema de metadatos de inferencia descrito a continuación, la carga del mensaje se reenvía a través de la canalización, pero se reduce la interoperabilidad. [Aquí](/azure/media-services/live-video-analytics-edge/inference-metadata-schema) puede encontrar información detallada y actualizada relativa al esquema de metadatos de inferencia.

> [!NOTE]
> Si el módulo no genera ningún resultado, debe devolver el código de estado HTTP 204 (Sin contenido) con un cuerpo de respuesta vacío. Live Video Analytics lo entiende como un resultado vacío y no reenvía el evento a través de la canalización.


## <a name="next-steps"></a>Pasos siguientes

[Protocolo de la extensión gRPC](./grpc-extension-protocol.md)
