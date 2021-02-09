---
title: Guía de migración de codificación
description: En este artículo se proporcionan instrucciones basadas en escenarios de codificación que le ayudarán a migrar de Azure Media Services v2 a v3.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: ab819239572fd99fdf5ff3bf23f81eb3cdff3b9a
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98940091"
---
# <a name="encoding-scenario-based-migration-guidance"></a>Guía de migración basada en escenarios de codificación

![logotipo de la guía de migración](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![pasos de migración 2](./media/migration-guide/steps-4.svg)

En este artículo se proporcionan instrucciones basadas en escenarios de codificación que le ayudarán a migrar de Azure Media Services v2 a v3.

## <a name="prerequisites"></a>Requisitos previos

Antes de cambiar el flujo de trabajo de la codificación, se recomienda profundizar en las diferencias de administración del almacenamiento.  En AMS v3, se usa la API de Azure Storage para administrar las cuentas de almacenamiento asociadas a su cuenta de Media Services.

> [!NOTE]
> Los trabajos y las tareas creados en v2 no se muestran en v3, ya que no están asociados con una transformación. Se recomienda cambiar a transformaciones y trabajos de v3.

## <a name="encoding-workflow-comparison"></a>Comparación de flujo de trabajo de codificación

Dedique unos minutos a examinar los siguientes diagramas de flujo para ver una comparación visual de los flujos de trabajo de codificación de v2 y v3.

### <a name="v2-encoding-workflow"></a>Flujo de trabajo de codificación de v2

Haga clic en la imagen siguiente para ver una versión más grande.

[![Flujo de trabajo de codificación para v2](./media/migration-guide/V2-pretty.svg) ](./media/migration-guide/V2-pretty.svg#lightbox)

1. Configuración
    1. Cree un recurso o use uno existente. Si usa un recurso nuevo, cargue el contenido en dicho recurso. Si usa un recurso existente, debe codificar los archivos que ya existen en el recurso.
    2. Obtenga los valores de los elementos siguientes:
        - Id. u objeto del procesador de multimedia
        - Cadena del codificador (nombre) que quiere usar
        - Id. del recurso nuevo o existente
    3. Para la supervisión, cree una suscripción de notificación de nivel de trabajo o tarea, o un controlador de eventos de SDK.
2. Cree el trabajo que contiene las tareas. Cada tarea debe incluir los elementos anteriores, además de los siguientes:
    - Una directiva para indicar que es necesario crear un recurso de salida.  El sistema crea el recurso de salida.
    - El nombre opcional del recurso de salida.
3. Envíe el trabajo.
4. Supervise el trabajo.

### <a name="v3-encoding-workflow"></a>Flujo de trabajo de codificación de v3

[ ![Flujo de trabajo de codificación para v3](./media/migration-guide/V3-pretty.svg) ](./media/migration-guide/V3-pretty.svg#lightbox)

1. Configuración
    1. Cree un recurso o use uno existente. Si usa un recurso nuevo, cargue el contenido en dicho recurso. Si usa un recurso existente, debe codificar los archivos que ya existen en el recurso. *No debe cargar más contenido en ese recurso.*
    1. Cree un recurso de salida.  El recurso de salida es el lugar en el que se almacenarán los archivos codificados y los metadatos de entrada y salida.
    1. Obtenga los valores de la transformación:
        - Valor preestablecido de codificador estándar
        - Grupo de recursos de AMS
        - Nombre de cuenta de AMS
    1. Cree la transformación o use una existente.  Las transformaciones se pueden reutilizar. No es necesario crear una nueva transformación cada vez que quiera enviar un trabajo.
1. Creación de un trabajo
    1. En el caso del trabajo, obtenga los valores de los siguientes elementos:
        - Nombre de la transformación
        - URI base de la dirección URL de SAS para el recurso, la ruta de acceso del origen HTTPS del recurso compartido de archivos o la ruta de acceso local de los archivos. El elemento `JobInputAsset` también puede utilizar un nombre de recurso como entrada.
        - Nombres de los archivos
        - Recursos de salida
        - Un grupo de recursos
        - Nombre de cuenta de AMS  
1. Use [Event Grid](monitor-events-portal-how-to.md) para supervisar su trabajo.
1. Envíe el trabajo.

## <a name="custom-presets-from-v2-to-v3-encoding"></a>Valores preestablecidos personalizados de codificación de v2 a v3

Si el código de v2 llamó al codificador Standard con un valor preestablecido personalizado, primero debe crear una nueva transformación con el valor preestablecido de codificador Standard personalizado antes de enviar un trabajo.

Los valores preestablecidos personalizados ahora están basados en JSON, no en XML. Vuelva a crear el valor preestablecido en JSON siguiendo el esquema de valores preestablecidos personalizados, tal como se define en la documentación de la [Open API de transformaciones (Swagger)](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2020-05-01/examples/transforms-create.json).


<!-- removed because this is covered in the tutorials
Common custom [encoding](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2020-05-01/Encoding.json) scenarios:
        1. Create a custom Single Bitrate MP4 encode
        1. Create a custom [Adaptive Bitrate Encoding Ladder](autogen-bitrate-ladder.md)
        1. Creating Sprite Thumbnails
        1. Creating Thumbnails (see below for your preferred method)
        1. [Sub Clipping](subclip-video-rest-howto.md)
        1. Cropping
-->

## <a name="input-and-output-metadata-files-from-an-encoding-job"></a>Archivos de metadatos de entrada y salida de un trabajo de codificación

En la versión v2, se generan archivos de metadatos de entrada y salida como resultado de un trabajo de codificación. En la versión v3, el formato de metadatos cambió de XML a JSON. Para más información acerca de los metadatos, consulte [Metadatos de entrada](input-metadata-schema.md) y [Metadatos de salida](output-metadata-schema.md).

## <a name="premium-encoder-to-v3-standard-encoder-or-partner-based-solutions"></a>Soluciones de codificador Premium a codificador Standard v3 o soluciones de partners

La API de v2 ya no es compatible con el codificador Premium. Si anteriormente usaba el codificador Premium basado en flujos de trabajo para la codificación de HEVC, debe migrar al nuevo [codificador estándar](media-encoder-standard-formats.md) de v3, que es compatible con la codificación de HEVC.

Si necesita las características avanzadas de flujo de trabajo del codificador Premium, le recomendamos que empiece a usar una de las soluciones de codificación avanzadas de Azure de algún partner, como [Imagine Communications](https://imaginecommunications.com), [Telestream](https://www.telestream.net) o [Bitmovin](https://bitmovin.com).

## <a name="jobs-with-inputs-that-are-on-https-hosted-urls"></a>Trabajos con entradas que se encuentran en direcciones URL hospedadas en HTTPS

Ahora puede enviar trabajos en v3 a partir de archivos almacenados en Azure Storage, localmente o en servidores web externos gracias a la [compatibilidad con entradas de trabajo HTTP(S)](job-input-from-http-how-to.md).

Si anteriormente usaba flujos de trabajo para copiar archivos desde los archivos de blobs de Azure a activos vacíos antes de enviar los trabajos, puede simplificar el proceso al pasar una URL de SAS del archivo en Azure Blob Storage directamente al trabajo.

## <a name="indexer-v1-audio-transcription-to-the-new-audioanalyzer-basic-mode"></a>Transcripción de audio del indizador v1 al nuevo "modo básico" de AudioAnalyzer

En el caso de los clientes que usan el procesador del indizador v1 en la API v2, debe crear una transformación que invoque el nuevo `AudioAnalyzer` en [modo básico](how-to-create-basic-audio-transform.md) antes de enviar un trabajo.

## <a name="encoding-transforms-and-jobs-concepts-tutorials-and-how-to-guides"></a>Conceptos, tutoriales y guías de procedimientos sobre codificación, transformaciones y trabajos

### <a name="concepts"></a>Conceptos

- [Codificación de vídeo y audio con Media Services](encoding-concept.md)
- [Códecs y formatos de Standard Encoder](media-encoder-standard-formats.md)
- [Codificación con una escala de velocidad de bits generada automáticamente](autogen-bitrate-ladder.md)
- [Use el valor predeterminado de la codificación en función del contenido para encontrar el valor de velocidad de bits óptimo para una resolución dada](content-aware-encoding.md)
- [Unidades reservadas de multimedia](concept-media-reserved-units.md)
- [Metadatos de entrada](input-metadata-schema.md)
- [Metadatos de salida](output-metadata-schema.md)
- [Empaquetado dinámico en Media Services v3: códecs de audio](dynamic-packaging-overview.md#audio-codecs-supported-by-dynamic-packaging)

### <a name="tutorials"></a>Tutoriales

- [Tutorial: Codificación de un archivo remoto según una dirección URL y transmisión del vídeo: .NET](stream-files-dotnet-quickstart.md)
- [Tutorial: Carga, codificación y streaming de vídeos con Media Services v3](stream-files-tutorial-with-api.md)

### <a name="how-to-guides"></a>Guías de procedimientos

- [Creación de una entrada de trabajo desde una dirección URL de HTTPS](job-input-from-http-how-to.md).
- [Creación de una entrada de trabajo a partir de un archivo local](job-input-from-local-file-how-to.md).
- [Creación de una transformación de audio básica](how-to-create-basic-audio-transform.md)
- Con .NET
  - [Procedimiento de codificación con una transformación personalizada - .NET](customize-encoder-presets-how-to.md)
  - [Creación de una superposición con Media Encoder Standard](how-to-create-overlay.md)
  - [Generación de miniaturas mediante Encoder Standard con .NET](media-services-generate-thumbnails-dotnet.md)
- Con la CLI de Azure
  - [Procedimiento de codificación con una transformación personalizada: CLI de Azure](custom-preset-cli-howto.md)
- Con REST
  - [Cómo codificar con una transformación personalizada: REST](custom-preset-rest-howto.md)
  - [Generación de miniaturas mediante Encoder Standard con REST](media-services-generate-thumbnails-rest.md)
- [Creación de un subclip de vídeo al codificar mediante Azure Media Services: .NET](subclip-video-dotnet-howto.md)
- [Creación de un subclip de vídeo al codificar mediante Azure Media Services: REST](subclip-video-rest-howto.md)

## <a name="samples"></a>Ejemplos

También puede [comparar el código de la versión v2 y v3 en los ejemplos de código](migrate-v-2-v-3-migration-samples.md).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
