---
title: Uso de la CLI para crear filtros con Azure Media Services
description: En este artículo se muestra cómo usar la CLI para crear filtros con Azure Media Services v3.
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
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18, devx-track-azurecli
ms.openlocfilehash: 54e99c645b93bfcb3aee4e81cef3b2b5d6589d95
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102211471"
---
# <a name="creating-filters-with-cli"></a>Creación de filtros con la CLI

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Al entregar su contenido a los clientes (streaming de eventos en vivo o vídeo bajo demanda), es posible que el cliente necesite más flexibilidad que la descrita en el archivo de manifiesto del recurso predeterminado. Azure Media Services le permite definir filtros de cuenta y filtros de recurso para su contenido.

Para obtener una descripción detallada de esta característica y los escenarios donde se utiliza, vea [Manifiestos dinámicos](filters-dynamic-manifest-overview.md) y [Filtros](filters-concept.md).

En este tema se muestra cómo configurar un filtro para un recurso Vídeo bajo demanda y usar la CLI para Media Services v3 para crear [Filtros de cuenta](/cli/azure/ams/account-filter) y [Filtros de recurso](/cli/azure/ams/asset-filter).

> [!NOTE]
> No olvide revisar [presentationTimeRange](filters-concept.md#presentationtimerange).

## <a name="prerequisites"></a>Prerrequisitos

- [Cree una cuenta de Media Services](./create-account-howto.md). Asegúrese de recordar el nombre del grupo de recursos y el nombre de la cuenta de Media Services.

## <a name="define-a-filter"></a>Definición de un filtro

En el siguiente ejemplo se definen las condiciones de selección de seguimiento que se agregan al manifiesto final. Este filtro incluye las pistas de audio con EC-3 y las pistas de vídeo con velocidad de bits en el intervalo 0 a 1000000.

> [!TIP]
> Si va a definir **Filtros** en REST, tenga en cuenta que deberá incluir el objeto JSON de contenedor "Properties".  

```json
[
    {
        "trackSelections": [
            {
                "property": "Type",
                "value": "Audio",
                "operation": "Equal"
            },
            {
                "property": "FourCC",
                "value": "EC-3",
                "operation": "NotEqual"
            }
        ]
    },
    {
        "trackSelections": [
            {
                "property": "Type",
                "value": "Video",
                "operation": "Equal"
            },
            {
                "property": "Bitrate",
                "value": "0-1000000",
                "operation": "Equal"
            }
        ]
    }
]
```

## <a name="create-account-filters"></a>Creación de filtros de cuenta

El siguiente comando [az ams account-filter](/cli/azure/ams/account-filter) crea un filtro de cuenta con selecciones de seguimiento de filtro [definidas anteriormente](#define-a-filter).

El comando permite pasar un parámetro `--tracks` opcional que contiene un valor JSON que representa las selecciones de seguimiento.  Use @{file} para cargar el valor JSON desde un archivo. Si usa la CLI de Azure localmente, especifique la ruta de acceso completa al de archivo:

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @tracks.json
```

Vea también la sección de [ejemplos de JSON para los filtros](/rest/api/media/accountfilters/createorupdate#create-an-account-filter).

## <a name="create-asset-filters"></a>Creación de filtros de recurso

El siguiente comando [az ams asset-filter](/cli/azure/ams/asset-filter) crea un filtro de recurso con selecciones de seguimiento de filtro [definidas anteriormente](#define-a-filter). 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @tracks.json
```

Vea también la sección de [ejemplos de JSON para los filtros](/rest/api/media/assetfilters/createorupdate#create-an-asset-filter).

## <a name="associate-filters-with-streaming-locator"></a>Asociación de filtros con localizadores de streaming

Ahora puede especificar una lista de filtros de cuentas o recursos, que se aplicarían a su localizador de streaming. El [empaquetado dinámico (punto de conexión de streaming)](dynamic-packaging-overview.md) se aplica a esta lista de filtros junto con los que el cliente especifica en la dirección URL. Esta combinación genera un [manifiesto dinámico](filters-dynamic-manifest-overview.md), que se basa en los filtros de la dirección URL y en los filtros que especifique en el localizador de streaming. Se recomienda usar esta característica si quiere aplicar filtros, pero no quiere exponer los nombres de filtro en la dirección URL.

El siguiente código de CLI muestra cómo crear un localizador de streaming y especificar `filters`. Esta es una propiedad opcional que toma una lista separada por espacios de nombres de filtro de recursos o de nombres de cuenta de filtro.

```azurecli
az ams streaming-locator create -a amsAccount -g resourceGroup -n streamingLocatorName \
                                --asset-name assetName \                               
                                --streaming-policy-name policyName \
                                --filters filterName1 filterName2
                                
```

## <a name="stream-using-filters"></a>Streaming con filtros

Una vez que defina los filtros, los clientes podrán utilizarlos en la dirección URL de streaming. Se podrían aplicar filtros a protocolos de streaming con velocidad de bits adaptable: formatos Apple HTTP Live Streaming (HLS), MPEG-DASH y Smooth Streaming

En la tabla siguiente se muestran algunos ejemplos de direcciones URL con filtros:

|Protocolo|Ejemplo|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="next-step"></a>Paso siguiente

[Streaming de vídeos](stream-files-tutorial-with-api.md)

## <a name="see-also"></a>Consulte también

[CLI de Azure](/cli/azure/ams)
