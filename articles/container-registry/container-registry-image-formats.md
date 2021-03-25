---
title: Formatos de contenido compatibles
description: Obtenga información sobre los formatos de contenido que admite Azure Container Registry, como imágenes de contenedor compatibles con Docker, gráficos de Helm, e imágenes y artefactos OCI.
ms.topic: article
ms.date: 08/30/2019
ms.openlocfilehash: b2a54c65d149a27ed9eae85c3308d657ed3471a3
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100008339"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Formatos de contenido admitidos en Azure Container Registry

Use un repositorio privado en Azure Container Registry para administrar uno de los siguientes formatos de contenido. 

## <a name="docker-compatible-container-images"></a>Imágenes de contenedor compatibles con Docker

Se admiten los siguientes formatos de imagen de contenedor de Docker:

* [Manifiesto de imagen de Docker V2, esquema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Manifiesto de imagen de Docker V2, esquema 2](https://docs.docker.com/registry/spec/manifest-v2-2/): incluye listas de manifiesto que permiten que los registros almacenen [imágenes con varias arquitecturas](push-multi-architecture-images.md) en una referencia `image:tag` única.

## <a name="oci-images"></a>Imágenes OCI

Azure Container Registry admite imágenes que cumplen la [especificación de formato de imagen de Open Container Initiative (OCI)](https://github.com/opencontainers/image-spec/blob/master/spec.md) que incluyen la especificación de [índice de imágenes](https://github.com/opencontainers/image-spec/blob/master/image-index.md) opcional. Entre los formatos de empaquetado se incluye [Singularity Image Format (SIF)](https://github.com/sylabs/sif).

## <a name="oci-artifacts"></a>Artefactos de OCI

Azure Container Registry admite la [especificación de distribución OCI](https://github.com/opencontainers/distribution-spec), una especificación independiente de la nube y del proveedor para almacenar, compartir, proteger e implementar imágenes de contenedor y otros tipos de contenido (artefactos). La especificación permite que un registro almacene una amplia variedad de artefactos además de imágenes de contenedor. Use las herramientas adecuadas para el artefacto para insertar y extraer artefactos. Para ver un ejemplo, consulte [Inserción y extracción de un artefacto de OCI con un registro de contenedor de Azure](container-registry-oci-artifacts.md).

Para más información sobre los artefactos de OCI, consulte los repositorios [OCI Registry as Storage (ORAS)](https://github.com/deislabs/oras) y [OCI Artifacts](https://github.com/opencontainers/artifacts) en GitHub.

## <a name="helm-charts"></a>Gráficos de Helm

Azure Container Registry también puede hospedar repositorios para [gráficos de Helm](https://helm.sh/), un formato de empaquetado que se usa para administrar e implementar rápidamente aplicaciones de Kubernetes. Se admite la versión 2 (2.11.0 o posterior) del [cliente de Helm](https://docs.helm.sh/using_helm/#installing-helm).

## <a name="next-steps"></a>Pasos siguientes

* Vea cómo [insertar y extraer](container-registry-get-started-docker-cli.md) imágenes con Azure Container Registry.

* Use [ACR Tasks](container-registry-tasks-overview.md) para compilar y probar imágenes de contenedor. 

* Use [Moby BuildKit](https://github.com/moby/buildkit) para compilar y empaquetar contenedores en formato OCI.

* Configure un [repositorio de Helm](container-registry-helm-repos.md) hospedado en Azure Container Registry. 


