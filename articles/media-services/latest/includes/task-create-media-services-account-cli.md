---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: 5c0341087cdd348e973da5faaa1f90081780c9c8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "88602508"
---
<!--Create a media services account -->

El siguiente comando de la CLI de Azure crea una nueva cuenta de Media Services. Puede reemplazar los valores siguientes: `amsaccount`, `storageaccountforams` (deben coincidir con el valor que especificó para la cuenta de almacenamiento) y `amsResourceGroup` (debe coincidir con el valor que especificó para el grupo de recursos).

```azurecli
az ams account create --name amsaccount -g amsResourceGroup --storage-account storageaccountforams -l westus2
```