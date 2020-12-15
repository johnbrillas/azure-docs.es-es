---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/14/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 4ceae4e7e2d10c80a929a4a822c877da8d8478f0
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2020
ms.locfileid: "96748254"
---
Se puede implementar FTP y Git local en una aplicación web de Azure mediante un *usuario de implementación*. Una vez configurado este usuario de implementación, podrá usarlo en todas las implementaciones de Azure. El nombre de usuario y la contraseña en el nivel de cuenta son diferentes de las credenciales de suscripción de Azure. 

Para configurar el usuario de implementación, ejecute el comando [az webapp deployment user set](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) en Azure Cloud Shell. Reemplace \<username> y \<password> por un nombre de usuario y una contraseña de usuario de implementación. 

- El nombre de usuario debe ser único dentro de Azure y no debe contener el símbolo "\@" para las inserciones de Git local. 
- La contraseña debe tener al menos ocho caracteres y dos de los tres elementos siguientes: letras, números y símbolos. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

La salida JSON muestra la contraseña como `null`. Si se produce un error `'Conflict'. Details: 409`, cambie el nombre de usuario. Si se produce un error `'Bad Request'. Details: 400`, use una contraseña más segura. 

Anote el nombre de usuario y la contraseña que se usarán para implementar las aplicaciones web.
