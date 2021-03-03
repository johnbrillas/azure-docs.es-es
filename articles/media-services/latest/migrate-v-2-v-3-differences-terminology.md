---
title: Terminología y cambios de entidad de Media Services v3
description: En este artículo se describen las diferencias de terminología entre Azure Media Services v2 y v3.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: 6f677c8753f09e146d300186e19217568952b417
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101705405"
---
# <a name="terminology-and-entity-changes-between-media-services-v2-and-v3"></a>Terminología y cambios de entidad entre Azure Media Services v2 y v3

![logotipo de la guía de migración](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![pasos de migración 2](./media/migration-guide/steps-2.svg)

En este artículo se describen las diferencias de terminología entre Azure Media Services v2 y v3.

## <a name="naming-conventions"></a>Convenciones de nomenclatura

Revise las convenciones de nomenclatura que se aplican a los recursos de Media Services v3. Revise también los [blobs de nomenclatura](assets-concept.md#naming).

## <a name="terminology-changes"></a>Cambios de terminología

- Un *localizador* ahora se denomina *localizador de streaming*.
- Un *canal* ahora se denomina *evento en directo*.
- Un *programa* ahora se denomina *salida en directo*.
- Una *tarea* ahora se denomina *JobOutput*, que forma parte de un trabajo.

## <a name="entity-changes"></a>Cambios de entidades
| **Entidad de v2**<!-- row --> | **Entidad de v3** | **Guía** | **Accesible para v3** | **Actualizado en v3** |
|--|--|--|--|--|
| `AccessPolicy`<!-- row --> | <!-- empty --> |  La entidad `AccessPolicies` no existe en la versión v3. | No | No |
| `Asset`<!-- row --> | `Asset` | <!-- empty --> | Sí | Sí |
| `AssetDeliveryPolicy`<!-- row --> | `StreamingPolicy` | <!-- empty --> | Sí | No |
| `AssetFile`<!-- row --> | <!-- empty --> |La entidad `AssetFiles` no existe en v3. Aunque los archivos (blobs de almacenamiento) que se cargan se siguen considerando archivos.<br/><br/> En su lugar, use las API de Azure Storage para enumerar los blobs de un contenedor. Hay dos maneras de aplicar una transformación a los archivos mediante un trabajo:<br/><br/>Archivos que ya están cargados en el almacenamiento: El URI incluiría el id. de recurso para los trabajos que se van a realizar en los recursos de una cuenta de almacenamiento.<br/><br/>Archivos que se cargan durante el proceso de transformación y trabajo: el recurso se crea en el almacenamiento, se devuelve una dirección URL de SAS, los archivos se cargan en el almacenamiento y, a continuación, la transformación se aplica a los archivos. | No | No |
| `Channel`<!-- row --> | `LiveEvent` | Los eventos en directo reemplazan a los canales de la API v2. Incluyen la mayoría de las funciones y tienen más características nuevas, como transcripciones en vivo, modo en espera y soporte para la ingesta de RTMP. <br/><br/>Consulte [evento en directo de streaming en vivo basado en escenarios](migrate-v-2-v-3-migration-scenario-based-live-streaming.md) | No | No |
| `ContentKey`<!-- row --> | <!-- empty --> | `ContentKeys` ya no es una entidad; ahora es una propiedad de un localizador de streaming.<br/><br/>  En v3, los datos de la clave de contenido están asociados al `StreamingLocator` (para el cifrado de salida) o al propio recurso (para el cifrado de almacenamiento del lado cliente). | Sí | No |
| `ContentKeyAuthorizationPolicy`<!-- row --> | `ContentKeyPolicy` | <!-- empty --> | Sí | No |
| `ContentKeyAuthorizationPolicyOption` <!-- row --> | <!-- empty --> |  `ContentKeyPolicyOptions` están incluidas en `ContentKeyPolicy`. | Sí | No |
| `IngestManifest`<!-- row --> | <!-- empty --> | La entidad `IngestManifests` no existe en v3. La carga de archivos en v3 involucra a la API de Azure Storage. Primero se crean los recursos y después los archivos de cargan al contenedor de almacenamiento asociado. Hay muchas maneras de enviar datos a un contenedor de Azure Storage que puede usar en su lugar. `JobInputHttp` también ofrece una manera de descargar una entrada de trabajo de una dirección URL determinada si así se desea. | No | No |
| `IngestManifestAsset`<!-- row --> | <!-- empty --> | Hay muchas maneras de enviar datos a un contenedor de Azure Storage que puede usar en su lugar. `JobInputHttp` también ofrece una manera de descargar una entrada de trabajo de una dirección URL determinada si así se desea. | No | No |
| `IngestManifestFile`<!-- row --> | <!-- empty --> | Hay muchas maneras de enviar datos a un contenedor de Azure Storage que puede usar en su lugar. `JobInputHttp` también ofrece una manera de descargar una entrada de trabajo de una dirección URL determinada si así se desea. | No | No |
| `Job`<!-- row --> | `Job` | Cree un elemento `Transform` antes de crear un `Job`. | No | No |
| `JobTemplate`<!-- row --> | `Transform` | En lugar de ello, usa un `Transform`. Una transformación es una entidad independiente de un trabajo y es reutilizable. | No | No |
| `Locator`<!-- row --> | `StreamingLocator` | <!--empty --> | Sí | No |
| `MediaProcessor`<!-- row --> | <!-- empty --> | En lugar de buscar el `MediaProcessor` que usará por nombre, use el valor preestablecido deseado al definir una transformación. El valor preestablecido que use determinará el procesador multimedia utilizado por el sistema de trabajo. Consulte los temas sobre codificación en [codificación basada en escenarios](migrate-v-2-v-3-migration-scenario-based-encoding.md). <!--Probably needs a link to its own article so customers know Indexerv1 maps to AudioAnalyzerPreset in basic mode, etc.--> | No | N/D (solo lectura en v2) |
| `NotificationEndPoint`<!-- row --> | <!--empty --> | Las notificaciones en v3 se controlan a través de Azure Event Grid. La entidad`NotificationEndpoint` se sustituye por el registro de suscripciones de Event Grid, que también encapsula la configuración de los tipos de notificaciones que se recibirán (en v2 se controlaba mediante el elemento `JobNotificationSubscription` del trabajo, el `TaskNotificationSubscription` de la tarea y la `ComponentMonitoringSetting` de telemetría). La telemetría de v2 se dividió entre Azure Event Grid y Azure Monitor para ajustarse a las mejoras del ecosistema de Azure más amplio. | No | No |
| `Program`<!-- row --> | `LiveOutput` | Ahora, las salidas en directo reemplazan a los programas de la API v3.  | No | No |
| `StreamingEndpoint`<!-- row --> | `StreamingEndpoint` | Los puntos de conexión de streaming siguen siendo los mismos. Se usan para el empaquetado dinámico, el cifrado y la entrega de contenido HLS y DASH para el streaming a petición y el streaming en directo ya sea directamente desde el origen, o a través de la red CDN. Entre las nuevas características se incluye la compatibilidad para mejorar la integración y la generación de gráficos de Azure Monitor. |  Sí | Sí |
| `Task`<!-- row --> | `JobOutput` | Se reemplazó por `JobOutput` (que ya no es una entidad independiente de la API).  Consulte los temas sobre codificación en [codificación basada en escenarios](migrate-v-2-v-3-migration-scenario-based-encoding.md). | No | No |
| `TaskTemplate`<!-- row --> | `TransformOutput` | Se reemplazó por `TransformOutput` (que ya no es una entidad independiente de la API). Consulte los temas sobre codificación en [codificación basada en escenarios](migrate-v-2-v-3-migration-scenario-based-encoding.md). | No | No |
| `Inputs`<!-- row --> | `Inputs` | Las entradas y salidas se encuentran ahora en el nivel de trabajo. Consulte los temas sobre codificación en [codificación basada en escenarios](migrate-v-2-v-3-migration-scenario-based-encoding.md) | No | No |
| `Outputs`<!-- row --> | `Outputs` | Las entradas y salidas se encuentran ahora en el nivel de trabajo.  En la versión v3, el formato de metadatos cambió de XML a JSON.  Los objetos LiveOutput comienzan al crearlos y se detienen cuando se eliminan. Consulte los temas sobre codificación en [codificación basada en escenarios](migrate-v-2-v-3-migration-scenario-based-encoding.md) | No | No |


| **Otros cambios** | **V2**  | **V3** |
|---|---|---|
| **Almacenamiento** <!--new row --> |||
| Almacenamiento <!--new row --> | | Los SDK de la versión v3 ahora se han desacoplado del SDK de Storage, lo que ofrece mayor control sobre la versión del SDK de Storage que quiera usar y evita problemas de control de versiones.                      |
| **Encoding** <!--new row --> |||
| Velocidades de bits de codificación <!--new row --> | velocidades de bits medida en kbps; por ejemplo: 128 (kbps)| bits por segundo; por ejemplo: 128000 (bits/segundo)|
| Codificación FairPlay DRM <!--new row --> | En Media Services v2, se puede especificar el vector de inicialización. | En Media Services v3, no se puede especificar el vector de inicialización de FairPlay.|
| Codificador Premium <!--new row --> | Codificador Premium e indizador heredado| No se puede acceder al [codificador Premium](../previous/media-services-encode-asset.md) y los [procesadores de análisis multimedia](../previous/legacy-components.md) heredados (Azure Media Services Indexer 2 Preview, Face Redactor, etc.) mediante la versión v3. Hemos agregado compatibilidad con la asignación de canales de audio al codificador Standard.  Consulte la sección sobre [audio en la documentación de codificación en Media Services de Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2020-05-01/Encoding.json).  | Consulte los temas sobre codificación en [codificación basada en escenarios](migrate-v-2-v-3-migration-scenario-based-encoding.md) |
| **Transformaciones y trabajos** <!--new row -->|||
| HTTPS de procesamiento basado en trabajos <!--new row --> |<!-- empty -->| Para el procesamiento de trabajos basados en archivos, puede usar una dirección URL de HTTPS como entrada. No es necesario tener contenido ya almacenado en Azure, ni es necesario crear recursos. |
| Plantillas de ARM para trabajos <!--new row --> | Las plantillas de ARM no existían en v2. | Una transformación puede usarse para crear configuraciones reutilizables, crear plantillas de Azure Resource Manager y aislar los valores de procesamiento entre varios clientes o inquilinos. |
| **Eventos en directo** <!--new row --> |||
| extremo de streaming <!--new row --> | Un punto de conexión de streaming representa un servicio de streaming que puede entregar contenido directamente a una aplicación de reproducción de cliente o a una red de entrega de contenido (CDN) para su posterior distribución. | Los puntos de conexión de streaming siguen siendo los mismos. Se usan para el empaquetado dinámico, el cifrado y la entrega de contenido HLS y DASH para el streaming a petición y el streaming en directo ya sea directamente desde el origen, o a través de la red CDN.  Entre las nuevas características se incluye la compatibilidad para mejorar la integración y la generación de gráficos de Azure Monitor. |
| Canales de eventos en directo <!--new row --> | Los canales son los responsables de procesar el contenido de streaming en vivo. Un canal proporciona un extremo de entrada (dirección URL de introducción) que luego se brinda a un transcodificador en vivo. El canal recibe flujos de entrada en vivo desde el transcodificador en vivo y los deja a disposición del streaming a través de uno o más puntos de conexión de streaming. Los canales también proporcionan un extremo de vista previa (dirección URL de vista previa) que se puede utilizar para obtener una vista previa y validar el flujo antes de mayor procesamiento y entrega.| Los eventos en directo reemplazan a los canales de la API v2. Incluyen la mayoría de las funciones y tienen más características nuevas, como transcripciones en vivo, modo en espera y soporte para la ingesta de RTMP. |
| Programas de eventos en directo <!--new row --> | Un programa le permite controlar la publicación y almacenamiento de segmentos en una secuencia en directo. Los canales administran los programas. La relación entre canales y programas es similar a los medios tradicionales, donde un canal tiene un flujo constante de contenido y un programa se enfoca en algún evento programado en dicho canal. Puede especificar el número de horas que quiere conservar el contenido grabado para el programa al establecer la propiedad `ArchiveWindowLength`. Este valor se puede establecer desde un mínimo de cinco minutos a un máximo de 25 horas.| Ahora, las salidas en directo reemplazan a los programas de la API v3. |
| Duración de evento en directo <!--new row --> |<!-- empty -->| Puede transmitir eventos en directo 24/7 al usar Media Services para transcodificar una fuente de contribución de velocidad de bits única en un flujo de salida que tiene varias velocidades de bits.|
| Latencia de evento en directo <!--new row --> |<!-- empty -->| Los eventos en directo ahora admiten streaming en vivo de baja latencia. |
| Versión preliminar de evento en directo <!--new row --> |<!-- empty -->| La versión preliminar de Evento en directo admite empaquetado dinámico y cifrado dinámico. Esto habilita la protección de contenido en la vista previa, así como el empaquetado DASH y HLS. |
| RTMPS de evento en directo <!--new row --> |<!-- empty-->| Compatibilidad mejorada con RTMPS, con mayor estabilidad y mejor compatibilidad con codificadores de origen. |
| Ingesta segura de RTMPS de evento en directo <!--new row --> | | Cuando se crea un evento en directo, el usuario recibe cuatro direcciones URL de ingesta. Las cuatro direcciones URL de ingesta son casi idénticas, tienen el mismo token de streaming `AppId` y solo se diferencian en el número de puerto. Dos de las direcciones URL son principal y de respaldo para RTMPS.| 
| Transcripción de eventos en directo <!--new row --> |<!-- empty--> | Azure Media Services ofrece vídeo, audio y texto en diferentes protocolos. Cuando publica el streaming en vivo mediante MPEG-DASH o HLS/CMAF, el servicio proporcionará junto con el vídeo y el audio, el texto transcrito en formato IMSC1.1 compatible con TTML.|
| Modo de espera de los eventos en directo <!--new row --> | No había ningún modo de espera para v2. | El modo de espera es una nueva característica de la versión v3 que ayuda a administrar grupos activos de eventos en directo. Ahora los clientes pueden iniciar un evento en directo en modo de espera a un costo más bajo antes de pasarlo al estado en ejecución. Esto mejora los tiempos de inicio del canal y reduce los costos de operación de los grupos frecuentes para arranques más rápidos. |
| Facturación de eventos en directo <!--new row --> | <!-- empty-->| La facturación de eventos en directo se basa en los medidores de canal en vivo. |
| Salidas en directo <!--new row --> | Los programas debían iniciarse después de crearse. | Los objetos LiveOutput comienzan al crearlos y se detienen cuando se eliminan. |

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]