---
title: 'Tutorial: Eliminación de una aplicación que se ejecuta en Azure Service Fabric Mesh'
description: En este tutorial, sabrá cómo quitar una aplicación que se ejecuta en Service Fabric Mesh y eliminar recursos.
author: georgewallace
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: gwallace
ms.custom: mvc, devcenter
ms.openlocfilehash: d449343fd00ff958470a71ecb3a37d585d7ff8ed
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99626617"
---
# <a name="tutorial-remove-an-application-and-resources"></a>Tutorial: Eliminación de una aplicación y recursos

> [!IMPORTANT]
> Se ha retirado la versión preliminar de Azure Service Fabric Mesh. Ya no se permitirán nuevas implementaciones a través de la API de Service Fabric Mesh. La compatibilidad con las implementaciones existentes continuará hasta el 28 de abril de 2021.
> 
> Para más información, consulte este artículo sobre la [retirada de la versión preliminar de Azure Service Fabric Mesh](https://azure.microsoft.com/updates/azure-service-fabric-mesh-preview-retirement/).

Este tutorial es la cuarta parte de una serie. Obtendrá información sobre cómo quitar una aplicación en ejecución que [se implementó previamente en Service Fabric Mesh](service-fabric-mesh-tutorial-template-deploy-app.md). 

En la parte número cuatro de la serie, se aprende a:

> [!div class="checklist"]
> * Eliminar una aplicación que se ejecuta en Service Fabric Mesh
> * Eliminar los recursos de la aplicación

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Implementar una aplicación en Service Fabric Mesh mediante una plantilla](service-fabric-mesh-tutorial-template-deploy-app.md)
> * [Escala de una aplicación que se ejecuta en Service Fabric Mesh](service-fabric-mesh-tutorial-template-scale-services.md)
> * [Actualizar una aplicación que se ejecuta en Service Fabric Mesh](service-fabric-mesh-tutorial-template-upgrade-app.md)
> * Eliminación de una aplicación

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar este tutorial:

* Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Abra [Azure Cloud Shell](service-fabric-mesh-howto-setup-cli.md) o [instale la CLI de Azure y la CLI de Service Fabric Mesh localmente](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="delete-the-resource-group-and-all-the-resources"></a>Eliminación del grupo de recursos y todos los recursos

Cuando ya no los necesite, elimine todos los recursos que haya creado. Anteriormente, [creó un grupo de recursos](service-fabric-mesh-tutorial-template-deploy-app.md#create-a-container-registry) para hospedar la instancia de Azure Container Registry y los recursos de aplicación de Service Fabric Mesh.  Puede eliminar este grupo de recursos, lo que suprimirá todos los recursos asociados.

```azurecli
az group delete --resource-group myResourceGroup
```

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="individually-delete-the-resources"></a>Eliminación individual de los recursos
También puede eliminar individualmente la instancia de ACR, la aplicación Service Fabric Mesh y los recursos de red.

Para eliminar la instancia de ACR, siga estos pasos:

```azurecli
az acr delete --resource-group myResourceGroup --name myContainerRegistry
```

Para crear una aplicación Service Fabric Mesh, realice lo siguiente:

```azurecli
az mesh app delete --resource-group myResourceGroup --name todolistapp
```

Para eliminar la red, siga estos pasos:
```azurecli
az mesh network delete --resource-group myResourceGroup --name todolistappNetwork
```

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:

> [!div class="checklist"]
> * Eliminar una aplicación que se ejecuta en Service Fabric Mesh
> * Eliminar los recursos de la aplicación
