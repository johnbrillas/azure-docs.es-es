---
title: Escalado de unidades reservadas de multimedia (MRU) con CLI
description: En este tema se muestra cómo usar la CLI para escalar el procesamiento multimedia con Azure Media Services.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/30/2020
ms.author: inhenkel
ms.openlocfilehash: a07c4a20b854e09daf3b320b8c99757ca99b2578
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102213817"
---
# <a name="how-to-scale-media-reserved-units"></a>Escalado de unidades reservadas de multimedia

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

En este artículo se muestra cómo escalar las unidades reservadas de multimedia (MRU) para una codificación más rápida.

## <a name="prerequisites"></a>Prerrequisitos

[Cree una cuenta de Media Services](./create-account-howto.md).

Conocimientos sobre las [Unidades reservadas de multimedia](concept-media-reserved-units.md).

## <a name="scale-media-reserved-units-with-cli"></a>Escalado de unidades reservadas de multimedia con CLI

Ejecute el comando `mru`.

El comando [mru de la cuenta de ams az](/cli/azure/ams/account/mru) siguiente establece las unidades reservadas de multimedia en cuenta "amsaccount" mediante los parámetros **count** y **type**.

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Facturación

Se le cobra en función del número de minutos que se aprovisionan las unidades reservadas de multimedia en su cuenta. Esto ocurre independientemente de si se ejecutan trabajos en la cuenta. Para obtener una explicación detallada, vea la sección de preguntas más frecuentes de la página de [precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/).   

## <a name="next-step"></a>Paso siguiente

[Análisis de vídeos](analyze-videos-tutorial-with-api.md)

## <a name="see-also"></a>Consulte también

* [Cuotas y límites](limits-quotas-constraints.md)
