---
title: Migración del controlador de entrada de Azure Application Gateway de Helm al complemento de AGIC
description: En este artículo se proporcionan instrucciones sobre cómo migrar de AGIC implementado a través de Helm a AGIC implementado como un complemento de AKS.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 03/02/2021
ms.author: caya
ms.openlocfilehash: e83834fd5f8ca95826118c952f7884a494c7abbb
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102050841"
---
# <a name="migrate-from-agic-helm-to-agic-add-on"></a>Migración de Helm de AGIC al complemento de AGIC 

Si ya tiene AGIC implementado a través de Helm pero quiere migrar a AGIC implementado como complemento de AKS, los siguientes pasos le ayudarán en el proceso de migración. 

## <a name="prerequisites"></a>Prerrequisitos 
Antes de comenzar el proceso de migración, hay algunas cosas que debe comprobar. 
  - ¿Usará alguna característica con AGIC Helm que [actualmente no sea compatible con el complemento de AGIC](ingress-controller-overview.md#difference-between-helm-deployment-and-aks-add-on)?
  - ¿Usará más de una implementación de AGIC Helm por clúster de AKS? 
  - ¿Usará varias implementaciones de AGIC Helm para tener como destino una instancia de Application Gateway? 

Si respondió afirmativamente a cualquiera de las preguntas anteriores, el complemento de AGIC aún no será compatible con su caso de uso, por lo que será mejor que siga usando AGIC Helm por el momento. De lo contrario, continúe con el proceso de migración a continuación fuera del horario comercial. 

## <a name="find-the-application-gateway-resource-id-that-agic-helm-is-currently-targeting"></a>Búsqueda del identificador de recurso de Application Gateway que AGIC Helm tiene como destino actual 
Navegue hasta la instancia de Application Gateway que la implementación de AGIC Helm tiene como destino. Copie y guarde el id. de recurso de esa instancia de Application Gateway. Necesitará el id. de recurso en un paso posterior. El id. de recurso se puede encontrar en el portal, en la pestaña Propiedades de la instancia de Application Gateway o a través de la CLI de Azure. En el ejemplo siguiente se guarda el id. de recurso de Application Gateway en el elemento *appgwId* de una puerta de enlace denominada *myApplicationGateway* en el grupo de recursos *myResourceGroup*.

```azurecli-interactive
appgwId=$(az network application-gateway show -n myApplicationGateway -g myResourceGroup -o tsv --query "id") 
```

## <a name="delete-agic-helm-from-your-aks-cluster"></a>Eliminación de AGIC Helm del clúster de AKS
A través de la CLI de Azure, elimine la implementación de AGIC Helm del clúster. En primer lugar, deberá eliminar la implementación de AGIC Helm para habilitar el complemento de AGIC AKS. Tenga en cuenta que los cambios que se produzcan en el clúster de AKS entre el momento en que elimine la implementación de AGIC Helm y el momento en que habilite el complemento de AGIC no se verán reflejados en la instancia de Application Gateway y, por lo tanto, este proceso de migración debe realizarse fuera del horario comercial para minimizar su impacto. Application Gateway seguirá almacenando la última configuración que haya aplicado AGIC, por lo que las reglas de enrutamiento existentes no se verán afectadas. 

## <a name="enable-agic-add-on-using-your-existing-application-gateway"></a>Habilitación del complemento de AGIC con la instancia de Application Gateway existente 
Ahora puede habilitar el complemento de AGIC en el clúster de AKS para dirigirse a la instancia de Application Gateway existente a través de la CLI de Azure o el portal. Ejecute el siguiente comando de la CLI de Azure para habilitar el complemento de AGIC en el clúster de AKS. En el ejemplo se habilita el complemento en un clúster denominado *myCluster*, en un grupo de recursos denominado *myResourceGroup* mediante el id. de recurso de Application Gateway *appgwId* que hemos guardado en el paso anterior. 


```azurecli-interactive
az aks enable-addons -n myCluster -g myResourceGroup -a ingress-appgw --appgw-id $appgwId
```

También puede ir al clúster de AKS en el portal a través de este [vínculo](https://portal.azure.com/?feature.aksagic=true) y habilitar el complemento de AGIC en la pestaña Redes del clúster. Seleccione la instancia de Application Gateway existente en el menú desplegable cuando elija a qué instancia de Application Gateway debe dirigirse el complemento. 

![Portal del controlador de entrada de Application Gateway](./media/tutorial-ingress-controller-add-on-existing/portal-ingress-controller-add-on.png)

## <a name="next-steps"></a>Pasos siguientes
- [**Solución de problemas del controlador de entrada de Application Gateway**](ingress-controller-troubleshoot.md): guía de solución de problemas de AGIC 
- [**Anotaciones sobre el controlador de entrada de Application Gateway**](ingress-controller-annotations.md): lista de anotaciones sobre AGIC 