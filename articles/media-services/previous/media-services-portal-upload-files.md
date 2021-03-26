---
title: Carga de archivos en una cuenta de Media Services en Azure Portal | Microsoft Docs
description: Este tutorial le guía por los pasos que hay que seguir para cargar archivos en una cuenta de Media Services en Azure Portal.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: c2dc193d65ff1c85837477c0a8fd345f11d59bcd
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103009762"
---
# <a name="upload-files-to-a-media-services-account-in-the-azure-portal"></a>Carga de archivos en una cuenta de Media Services en Azure Portal

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!div class="op_single_selector"]
> * [Portal](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 

> [!NOTE]
> No hay características o funcionalidades nuevas para agregar a Media Services, versión 2. Para ver los archivos de carga actualizados con el portal, consulte [Uso del portal para cargar, codificar y transmitir contenido](../latest/manage-assets-quickstart.md).<br/>Consulte también: [Media Services v3](../latest/index.yml). Consulte también la [guía de migración de la versión v2 a v3](../latest/migrate-v-2-v-3-migration-introduction.md).

En Azure Media Services, cargue los archivos digitales en un recurso. El recurso puede contener vídeo, audio, imágenes, colecciones de miniaturas, pistas de texto y archivos de subtítulos (y los metadatos de estos archivos). Una vez cargados los archivos, el contenido se almacena de forma segura en la nube para un posterior procesamiento y streaming.

Media Services tiene un tamaño máximo de archivo para el procesamiento de archivos. Para obtener más información acerca de los límites de tamaño de archivo, consulte [Cuotas y limitaciones de Media Services](media-services-quotas-and-limitations.md).

Para completar este tutorial, deberá tener una cuenta de Azure. Para más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="upload-files"></a>Carga de archivos
1. En [Azure Portal](https://portal.azure.com/), seleccione la cuenta de Azure Media Services.
2. Seleccione **Configuración** > **Activos**. Después, seleccione el botón **Cargar**.
   
    ![Carga de archivos](./media/media-services-portal-vod-get-started/media-services-upload.png)
   
    Aparecerá la ventana **Upload a video asset** (Cargar un recurso de vídeo).
   
   > [!NOTE]
   > Media Services no limita el tamaño de archivo en la carga de vídeos.
 
3. En el equipo, vaya al vídeo que desea cargar. Seleccione el vídeo y, después, haga clic en **Aceptar**.  
   
    Comienza la carga. Puede ver el progreso debajo del nombre de archivo.  

Cuando haya finalizado la carga, el nuevo recurso se muestra en el panel **Recursos**. 

## <a name="media-services-learning-paths"></a>Rutas de aprendizaje de Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Pasos siguientes
* Aprenda a [codificar los recursos cargados](media-services-portal-encode.md).

* También puede usar Azure Functions para desencadenar un trabajo de codificación cuando un archivo llegue al contenedor configurado. Para más información, consulte el ejemplo en [Media Services: Integración de Azure Media Services con Azure Functions y Logic Apps](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/).
