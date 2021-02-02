---
title: 'Ejemplo de script de la CLI de Azure: publicación de un recurso'
description: En este artículo se muestra cómo utilizar el script de la CLI de Azure para publicar un recurso.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: c283c39f1ea90275c42de1481a9cb9006f2b2c5f
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98897095"
---
# <a name="cli-example-publish-an-asset"></a>Ejemplo de CLI: publicación de un recurso

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

El script de la CLI de Azure de este artículo muestra cómo crear un localizador de streaming y regresar a las direcciones URL de streaming. 

## <a name="prerequisites"></a>Prerequisites 

[Cree una cuenta de Media Services](./create-account-howto.md).

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/publish-asset/Publish-Asset.sh "Publish an asset")]

## <a name="next-steps"></a>Pasos siguientes

[Introducción a Media Services](media-services-overview.md)
