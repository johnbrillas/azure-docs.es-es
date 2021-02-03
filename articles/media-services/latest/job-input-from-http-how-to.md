---
title: Crear una entrada de trabajo desde una dirección URL de HTTPS
description: En este tema se muestra cómo crear una entrada de trabajo de Azure Media Services a partir de una dirección URL HTTPS.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 1073a13f477894e1b35d732fd1cd6191747a62a2
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98955994"
---
# <a name="create-a-job-input-from-an-https-url"></a>Crear una entrada de trabajo desde una dirección URL de HTTPS

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

En Media Services v3, cuando se envían trabajos para procesar los vídeos, es necesario indicar a Media Services dónde encontrar el vídeo de entrada. Una de las opciones es especificar una dirección URL HTTP(s) como entrada de trabajo (como se muestra en este ejemplo). Tenga en cuenta que, actualmente, AMS v3 no admite la codificación de transferencia fragmentada a través de direcciones URL de HTTPS. Para un ejemplo completo, consulte este [ejemplo de GitHub](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

> [!TIP]
> Antes de comenzar con el desarrollo, vea [Desarrollo con las API de Media Services v3](media-services-apis-overview.md) (incluye información sobre cómo acceder a las API, convenciones de nomenclatura, etc.).

## <a name="net-sample"></a>Ejemplo de .NET

El código siguiente muestra cómo crear un trabajo con una entrada de dirección URL HTTPS.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>Códigos de error de trabajo

Consulte [Códigos de error](/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Pasos siguientes

[Cree una entrada de trabajo a partir de un archivo local](job-input-from-local-file-how-to.md).
