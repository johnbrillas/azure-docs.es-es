---
title: Imágenes de arquitectura múltiple en el Registro
description: Uso de Azure Container Registry para crear, importar, almacenar e implementar imágenes de arquitectura múltiple (multiarquitectura)
ms.topic: article
ms.date: 02/07/2021
ms.custom: ''
ms.openlocfilehash: f8467cd3108ae4faea9ecb4c9d9ae339f476c311
ms.sourcegitcommit: 49ea056bbb5957b5443f035d28c1d8f84f5a407b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "100012285"
---
# <a name="multi-architecture-images-in-your-azure-container-registry"></a>Imágenes de arquitectura múltiple en Azure Container Registry

En este artículo se presentan imágenes de *arquitectura múltiple*  (*multiarquitectura*) y cómo puede usar las características de Azure Container Registry para ayudarle a crearlas, almacenarlas y usarlas. 

Una imagen arquitectura múltiple es un tipo de imagen de contenedor que puede combinar variantes para distintas arquitecturas y, a veces, para sistemas operativos diferentes. Cuando se ejecuta una imagen con compatibilidad con arquitectura múltiple, los clientes de contenedor seleccionarán automáticamente una variante de imagen que coincida con el sistema operativo y la arquitectura.

## <a name="manifests-and-manifest-lists"></a>Manifiestos y listas de manifiestos

Las imágenes de arquitectura múltiple se basan en manifiestos de imagen y listas de manifiestos.

### <a name="manifest"></a>Manifest

Cada imagen de contenedor se representa mediante un [manifiesto](container-registry-concepts.md#manifest). Un manifiesto es un archivo JSON que identifica de forma única la imagen, haciendo referencia a sus capas y sus tamaños correspondientes. 

Un manifiesto básico de una imagen `hello-world` de Linux tiene un aspecto similar al siguiente:

  ```json
  {
    "schemaVersion": 2,
    "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
    "config": {
        "mediaType": "application/vnd.docker.container.image.v1+json",
        "size": 1510,
        "digest": "sha256:fbf289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e"
      },
    "layers": [
        {
          "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
          "size": 977,
          "digest": "sha256:2c930d010525941c1d56ec53b97bd057a67ae1865eebf042686d2a2d18271ced"
        }
      ]
  }
  ```
    
Puede ver un manifiesto en Azure Container Registry mediante Azure Portal o herramientas como el comando [az acr repository show-manifests](/cli/azure/acr/repository#az_acr_repository_show_manifests) de la CLI de Azure.

### <a name="manifest-list"></a>Lista de manifiestos

Una *lista de manifiestos* para una imagen de arquitectura múltiple (que se conoce normalmente como un [índice de imagen](https://github.com/opencontainers/image-spec/blob/master/image-index.md) para las imágenes OCI) es una colección (índice) de imágenes y se crea una especificando uno o varios nombres de imagen. Incluye detalles sobre cada una de las imágenes, como el sistema operativo y la arquitectura admitidos, el tamaño y el hash del manifiesto. La lista de manifiestos se puede usar de la misma manera que un nombre de imagen en los comandos `docker pull` y `docker run`. 

La CLI de `docker` administra manifiestos y listas de manifiestos mediante el comando [docker manifest](https://docs.docker.com/engine/reference/commandline/manifest/).

> [!NOTE]
> Actualmente, el comando `docker manifest` y los subcomandos son experimentales. Consulte la documentación de Docker para obtener más información sobre el uso de comandos experimentales.

Puede ver una lista de manifiestos mediante el comando `docker manifest inspect`. A continuación se muestra la salida de la imagen de arquitectura múltiple `mcr.microsoft.com/mcr/hello-world:latest`, que tiene tres manifiestos: dos para las arquitecturas del sistema operativo Linux y otro para una arquitectura Windows.
```json
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.docker.distribution.manifest.list.v2+json",
  "manifests": [
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 524,
      "digest": "sha256:83c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a",
      "platform": {
        "architecture": "amd64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 525,
      "digest": "sha256:873612c5503f3f1674f315c67089dee577d8cc6afc18565e0b4183ae355fb343",
      "platform": {
        "architecture": "arm64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 1124,
      "digest": "sha256:b791ad98d505abb8c9618868fc43c74aa94d08f1d7afe37d19647c0030905cae",
      "platform": {
        "architecture": "amd64",
        "os": "windows",
        "os.version": "10.0.17763.1697"
      }
    }
  ]
}
```

Cuando se almacena una lista de manifiestos de arquitectura múltiple en Azure Container Registry, también puede ver la lista de manifiestos mediante Azure Portal o con herramientas como el comando [az acr repository show-manifests](/cli/azure/acr/repository#az_acr_repository_how_manifests).

## <a name="import-a-multi-arch-image"></a>Importación de una imagen de arquitectura múltiple 

Una imagen de arquitectura múltiple existente se puede importar a Azure Container Registry mediante el comando [az acr import](/cli/azure/acr#az_acr_import). La sintaxis de la importación de imágenes es la misma que con una imagen de arquitectura única. Al igual que la importación de una imagen de arquitectura única, la importación de una imagen de arquitectura múltiple no usa comandos de Docker. 

Obtenga más información en [Importación de imágenes de contenedor en un registro de contenedor](container-registry-import-images.md).

## <a name="push-a-multi-arch-image"></a>Inserción de una imagen de arquitectura múltiple

Cuando tenga flujos de trabajo de compilación para crear imágenes de contenedor para distintas arquitecturas, siga estos pasos para insertar una imagen de arquitectura múltiple en Azure Container Registry.

1. Etiquete e inserte cada imagen específica de la arquitectura en el registro de contenedor. En el ejemplo siguiente se suponen dos arquitecturas de Linux: arm64 y AMD64. 

   ```console
   docker tag myimage:arm64 \
     myregistry.azurecr.io/multi-arch-samples/myimage:arm64

   docker push myregistry.azurecr.io/multi-arch-samples/myimage:arm64
 
   docker tag myimage:amd64 \
     myregistry.azurecr.io/multi-arch-samples/myimage:amd64

   docker push myregistry.azurecr.io/multi-arch-samples/myimage:amd64
   ``` 

1. Ejecute `docker manifest create` para crear una lista de manifiestos para combinar las imágenes anteriores en una imagen de arquitectura múltiple.

   ```console
   docker manifest create myregistry.azurecr.io/multi-arch-samples/myimage:multi \
    myregistry.azurecr.io/multi-arch-samples/myimage:arm64 \
    myregistry.azurecr.io/multi-arch-samples/myimage:amd64
   ```

1. Inserción del manifiesto en el registro de contenedor mediante `docker manifest push`:

   ```console
   docker manifest push myregistry.azurecr.io/multi-arch-samples/myimage:multi
   ```

1. Use el comando `docker manifest inspect` para ver la lista de manifiestos. En la sección anterior se muestra un ejemplo de la salida del comando.

Después de insertar el manifiesto de arquitectura múltiple en el registro, trabaje con la imagen de arquitectura múltiple de la misma manera que con una imagen de una sola arquitectura. Por ejemplo, extraiga la imagen mediante `docker pull` y use comandos [az acr repository](/cli/azure/acr/repository#az_acr_repository) para ver las etiquetas, los manifiestos y otras propiedades de la imagen.

## <a name="build-and-push-a-multi-arch-image"></a>Creación e inserción de una imagen de arquitectura múltiple

Con las características de [ACR Tasks](container-registry-tasks-overview.md), puede crear e insertar una imagen de arquitectura múltiple en Azure Container Registry. Por ejemplo, defina una [tareas de varios pasos](container-registry-tasks-multi-step.md) en un archivo YAML que cree una imagen de arquitectura múltiple Linux.

En el ejemplo siguiente se da por supuesto que tiene Dockerfiles independientes para dos arquitecturas, arm64 y AMD64. Se crean e insertan las imágenes específicas de la arquitectura y, a continuación, crea e incorpora un manifiesto de arquitectura múltiple que tiene la etiqueta `latest`:

```yml
version: v1.1.0

steps:
- build: -t {{.Run.Registry}}/multi-arch-samples/myimage:{{.Run.ID}}-amd64 -f dockerfile.arm64 . 
- build: -t {{.Run.Registry}}/multi-arch-samples/myyimage:{{.Run.ID}}-arm64 -f dockerfile.amd64 . 
- push: 
    - {{.Run.Registry}}/multi-arch-samples/myimage:{{.Run.ID}}-arm64
    - {{.Run.Registry}}/multi-arch-samples/myimage:{{.Run.ID}}-amd64
- cmd: >
    docker manifest create
    {{.Run.Registry}}/multi-arch-samples/myimage:latest
    {{.Run.Registry}}/multi-arch-samples/myimage:{{.Run.ID}}-arm64
    {{.Run.Registry}}/multi-arch-samples/myimage:{{.Run.ID}}-amd64
- cmd: docker manifest push --purge {{.Run.Registry}}/multi-arch-samples/myimage:latest
- cmd: docker manifest inspect {{.Run.Registry}}/multi-arch-samples/myimage:latest
```

## <a name="next-steps"></a>Pasos siguientes

* Use [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines.md) para compilar imágenes de contenedor para distintas arquitecturas.
* Obtenga información sobre cómo crear imágenes multiplataforma con el complemento experimental [buildx](https://docs.docker.com/buildx/working-with-buildx/) de Docker.

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/
