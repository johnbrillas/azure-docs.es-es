---
title: Códigos de error de codificación de Azure Media Services | Microsoft Docs
description: En este tema se indican los códigos de error que se podrían devolver en caso de que se encuentre un error durante la ejecución de la tarea de Encoding.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: 7e6848fb49dd63fa67a639d09754a28dd5953a32
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2021
ms.locfileid: "103013489"
---
# <a name="encoding-error-codes"></a>Códigos de error de Encoding

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

En la tabla siguiente se indican los códigos de error que se podrían devolver en caso de que se encuentre un error durante la ejecución de la tarea de codificación.  Para obtener detalles del error en el código .NET, use la clase [ErrorDetails](/previous-versions/azure/jj126075(v=azure.100)) . Para obtener detalles del error en el código REST, use la API de REST [ErrorDetail](/rest/api/media/operations/errordetail) .

| ErrorDetail.Code | Posibles causas de error |
| --- | --- |
| Desconocido |Error desconocido al ejecutar la tarea |
| ErrorDownloadingInputAssetMalformedContent |Categoría de errores en la que se incluyen errores en la descarga de recursos de entrada, como nombres de archivo no válidos, archivos de longitud cero, formatos incorrectos, etc. |
| ErrorDownloadingInputAssetServiceFailure |Categoría de errores en la que se incluyen problemas en el servicio, por ejemplo, errores de red o de almacenamiento durante la descarga. |
| ErrorParsingConfiguration |Categoría de errores en la que la tarea \<see cref="MediaTask.PrivateData"/> (configuración) no es válida; por ejemplo, la configuración no es un valor preestablecido del sistema válido o contiene XML no válido. |
| ErrorExecutingTaskMalformedContent |Categoría de errores durante la ejecución de la tarea en la que problemas en los archivos multimedia de entrada provocan errores. |
| ErrorExecutingTaskUnsupportedFormat |Categoría de errores en la que el procesador de contenido multimedia no puede procesar los archivos proporcionados: formato de contenido multimedia no compatible o que no coincide con la configuración. Por ejemplo, intentar producir una salida de solo audio desde un activo que tenga solo vídeo. |
| ErrorProcessingTask |Categoría de otros errores que detecta el procesador de contenido multimedia durante el procesamiento de la tarea y que no están relacionados con el contenido. |
| ErrorUploadingOutputAsset |Categoría de errores al cargar el activo de salida. |
| ErrorCancelingTask |Categoría de errores en la que se incluyen errores al intentar cancelar la tarea. |
| TransientError |Categoría de errores en la que se incluyen problemas transitorios (p. ej., problemas de red temporales con Azure Storage). |

Para obtener ayuda del equipo de **Media Services** , abra una [incidencia de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="media-services-learning-paths"></a>Rutas de aprendizaje de Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Artículos relacionados
* [Realización de tareas de codificación avanzadas mediante la personalización de valores preestablecidos de Media Encoder Estándar](media-services-custom-mes-presets-with-dotnet.md)
* [Cuotas y limitaciones](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
