---
title: archivo de inclusión
description: archivo de inclusión
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 08/04/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 404fd10c3e3610671d2b6e5dbc6aba8bcaa70046
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100019183"
---
## <a name="push-image-to-registry"></a>Insertar la imagen en el registro

Para insertar una imagen en Azure Container Registry, primero debe tener una imagen. Si aún no tiene ninguna imagen de contenedor local, ejecute el siguiente comando [docker pull][docker-pull] para extraer una imagen pública existente. Para este ejemplo, extraiga la imagen `hello-world` desde Microsoft Container Registry.

```
docker pull mcr.microsoft.com/hello-world
```

Para poder insertar una imagen en el registro, debe etiquetarla con el nombre completo del servidor de inicio de sesión del registro. El nombre del servidor de inicio de sesión tiene el formato *\<registry-name\>.azurecr.io* (debe estar todo en minúscula); por ejemplo, *mycontainerregistry.azurecr.io*.

Etiquete la imagen mediante el comando [docker tag][docker-tag]. Reemplace `<login-server>` por el nombre del servidor de inicio de sesión de la instancia de ACR.

```
docker tag mcr.microsoft.com/hello-world <login-server>/hello-world:v1
```

Ejemplo:

```
docker tag mcr.microsoft.com/hello-world mycontainerregistry.azurecr.io/hello-world:v1
```


Por último, use el comando [docker push][docker-push] para insertar la imagen en la instancia del registro. Reemplace `<login-server>` por el nombre del servidor de inicio de sesión de la instancia del registro. Este ejemplo crea el repositorio **hello-world**, que contiene la imagen `hello-world:v1`.

```
docker push <login-server>/hello-world:v1
```

Después de insertar la imagen en el registro de contenedor, quite la imagen `hello-world:v1` de su entorno de Docker local. (Tenga en cuenta que el comando [docker rmi][docker-rmi] no quita la imagen del repositorio **hello-world** del registro de contenedor de Azure).

```
docker rmi <login-server>/hello-world:v1
```

<!-- LINKS - External -->
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/

<!-- LINKS - Internal -->

