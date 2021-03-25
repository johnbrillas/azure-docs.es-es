---
title: Ejemplo de implementación de Script de la CLI de Azure
description: Cómo crear un clúster de Linux seguro de Service Fabric en Azure mediante la interfaz de la línea de comandos (CLI) de Azure.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: b69f62f6f47c656921f4f2230a296e293e423fa4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102035299"
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Creación de un clúster seguro de Linux de Service Fabric en Azure

Con este comando se crea un certificado autofirmado, se agrega a un almacén de claves y se descarga el certificado al sitio local.  El nuevo certificado se usa para proteger el clúster al implementarlo.  También puede utilizar un certificado existente en lugar de crear uno.  En cualquier caso, el nombre del firmante del certificado debe coincidir con el dominio de acceso al clúster de Service Fabric. Esta coincidencia es necesaria para proporcionar TLS a los puntos de conexión de administración HTTPS y de Service Fabric Explorer del clúster. No puede obtener un certificado TLS/SSL de una entidad de certificación (CA) para el dominio `.cloudapp.azure.com`. Debe adquirir un nombre de dominio personalizado para el clúster. Cuando solicite un certificado de una CA, el nombre de sujeto del certificado debe coincidir con el nombre del dominio personalizado del clúster.

Si es necesario, instale la [CLI de Azure](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de ejemplo

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos, el clúster y todos los recursos relacionados.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az sf cluster create](/cli/azure/sf/cluster) | Crea un clúster de Azure Service Fabric.  |

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar ejemplos adicionales de la CLI de Service Fabric para Azure Service Fabric en los [ejemplos de la CLI de Service Fabric](../samples-cli.md).
