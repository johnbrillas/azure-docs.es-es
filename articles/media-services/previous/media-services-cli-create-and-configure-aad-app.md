---
title: Uso de la CLI de Azure para crear una aplicación de Azure AD y configurarla para acceder a la API de Azure Media Services | Microsoft Docs
description: En este tema se explica cómo usar la CLI de Azure para crear una aplicación de Azure AD y configurarla para acceder a la API de Azure Media Services.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: 6517cc8224a1a4e2e3319fde52fb621e4f7ab564
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103016940"
---
# <a name="use-azure-cli-to-create-an-azure-ad-app-and-configure-it-to-access-media-services-api"></a>Uso de la CLI de Azure para crear una aplicación de Azure AD y configurarla con acceso a la API de Media Services

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!NOTE]
> No hay características o funcionalidades nuevas para agregar a Media Services, versión 2. <br/>Finalice la compra de la versión más reciente, [Media Services v3](../latest/index.yml). Consulte también la [guía de migración de la versión v2 a v3](../latest/migrate-v-2-v-3-migration-introduction.md).

En este tema se muestra cómo usar la CLI de Azure para crear una aplicación de Azure Active Directory (Azure AD) y la entidad de servicio para acceder a recursos de Azure Media Services. 

## <a name="prerequisites"></a>Prerequisites

- Una cuenta de Azure. Para más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Una cuenta de Media Services. Para más información, vea [Creación de una cuenta de Azure Media Services mediante Azure Portal](media-services-portal-create-account.md).

## <a name="use-the-azure-cloud-shell"></a>Uso de Azure Cloud Shell

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Inicie Cloud Shell desde el panel de navegación superior del portal.

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

Para más información, vea [Introducción a Azure Cloud Shell](../../cloud-shell/overview.md).

## <a name="create-an-azure-ad-app-and-configure-access-to-the-media-account-with-azure-cli"></a>Creación de una aplicación de Azure AD y configuración del acceso a la cuenta multimedia con la CLI de Azure
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> 
az role assignment create --assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

Por ejemplo:

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

En este ejemplo, el **ámbito** es la ruta de acceso completa al recurso para la cuenta de Media Services. Sin embargo, el **ámbito** puede estar en cualquier nivel.

Por ejemplo, podría ser uno de los siguientes niveles:
 
* El nivel de **suscripción**.
* El nivel de **grupo de recursos**.
* El nivel de **recurso** (por ejemplo, una cuenta multimedia).

Para más información, vea [Creación de una entidad de servicio de Azure con la CLI de Azure](/cli/azure/create-an-azure-service-principal-azure-cli)

Vea también [Incorporación o eliminación de asignaciones de roles de Azure mediante la CLI de Azure](../../role-based-access-control/role-assignments-cli.md). 

## <a name="next-steps"></a>Pasos siguientes

Empiece a trabajar en la [carga de archivos en la cuenta](media-services-portal-upload-files.md).
