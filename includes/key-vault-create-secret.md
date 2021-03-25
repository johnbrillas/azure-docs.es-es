---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: d1a67a131a94bc017eaf2199546ef08f0291cd54
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102245011"
---
Vamos a crear un secreto llamado **mySecret** cuyo valor sea **¡Correcto!** . Un secreto puede ser una contraseña, una cadena de conexión SQL o cualquier otra información que necesite mantener segura y disponible para la aplicación. 

Para agregar un secreto al almacén de claves recién creado, use el comando [az keyvault secret set](/cli/azure/keyvault/secret#az-keyvault-secret-set) de la CLI de Azure:

```azurecli
az keyvault secret set --vault-name "<your-unique-keyvault-name>" --name "mySecret" --value "Success!"
```