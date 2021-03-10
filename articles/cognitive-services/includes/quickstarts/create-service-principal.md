---
title: Creación de una entidad de servicio de Azure
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/01/2020
ms.author: pafarley
ms.openlocfilehash: 2f60759fbbae2c0f712a475ef397ca260e1f0415
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102473694"
---
## <a name="create-an-azure-service-principal"></a>Creación de una entidad de servicio de Azure

Para que la aplicación interactúe con su cuenta de Azure, necesita una entidad de servicio de Azure para administrar los permisos. Siga las instrucciones de [Creación de una entidad de servicio de Azure](/powershell/azure/create-azure-service-principal-azureps).

Al crear una entidad de servicio, verá que tiene un valor secreto, un identificador y un identificador de aplicación. Guarde el identificador de aplicación y el secreto en una ubicación temporal para los pasos posteriores.
