---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: 493c674fa161bf33436e560fdcbb9196410db931
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102209481"
---
> [!TIP]
> Después de iniciar sesión, verá una lista de suscripciones asociadas con su cuenta de Azure. La información de suscripción con `isDefault: true` es la suscripción actualmente activada para los comandos de la CLI de Azure. Esta suscripción debe ser la misma que la que contiene el área de trabajo de Azure Machine Learning. Puede buscar el identificador de suscripción en [Azure Portal](https://portal.azure.com); para ello, visite la página de información general del área de trabajo. También puede usar el SDK para obtener el identificador de suscripción del objeto del área de trabajo. Por ejemplo, `Workspace.from_config().subscription_id`.
> 
> Para seleccionar otra suscripción, utilice el comando `az account set -s <subscription name or ID>` y especifique el nombre o identificador de la suscripción a los que desea cambiar. Para obtener más información sobre la selección de la suscripción, consulte [Uso de varias suscripciones de Azure](/cli/azure/manage-azure-subscriptions-azure-cli).