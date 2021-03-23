---
title: Almacenamiento de imágenes de contenedor
description: Información detallada sobre cómo se almacenan las imágenes de contenedor y otros artefactos en Azure Container Registry, incluida la seguridad, la redundancia y la capacidad.
ms.topic: article
ms.date: 03/03/2021
ms.custom: references_regions
ms.openlocfilehash: ec4328b44d5493b8d765fa30c548adc3d747d446
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102183274"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Almacenamiento de imágenes en Azure Container Registry

Cada instancia de Azure Container Registry, ya sea del nivel [Básico, Estándar o Prémium](container-registry-skus.md), se beneficia de características avanzadas de almacenamiento de Azure, como el cifrado en reposo. En las secciones siguientes se describen las características y los límites del almacenamiento de imágenes en Azure Container Registry (ACR).

## <a name="encryption-at-rest"></a>Cifrado en reposo

Todas las imágenes de contenedor y otros artefactos del registro se cifran en reposo. Azure cifra automáticamente una imagen antes de almacenarla y la descifra sobre la marcha cuando usted o sus aplicaciones y servicios extraen la imagen. También puede aplicar una capa de cifrado adicional con una [clave administrada por el cliente](container-registry-customer-managed-keys.md).

## <a name="regional-storage"></a>Almacenamiento regional

Azure Container Registry almacena los datos en la región en la que se crea el registro para ayudar a los clientes a satisfacer los requisitos de residencia y cumplimiento de los datos.

Para ayudar a protegerse frente a interrupciones del centro de datos, algunas regiones ofrecen [redundancia de zona](zone-redundancy.md), donde los datos se replican entre varios centros de datos en una región determinada.

Los clientes que quieran tener sus datos almacenados en varias regiones para conseguir un mejor rendimiento en distintas zonas geográficas o que quieran tener resistencia en caso de una interrupción regional deben habilitar la [replicación geográfica](container-registry-geo-replication.md).

## <a name="geo-replication"></a>Replicación geográfica

En escenarios que requieren una garantía de alta disponibilidad, considere el uso de la característica [replicación geográfica](container-registry-geo-replication.md) de los registros prémium. La replicación geográfica ayuda a protegerse contra la pérdida de acceso al registro en caso de un error regional. La replicación geográfica ofrece otras ventajas, como almacenamiento de imágenes cercano a la red para una inserción y extracción más rápida en escenarios de desarrollo o implementación distribuidos.

## <a name="zone-redundancy"></a>Redundancia de zona

Para crear un registro de contenedor de Azure resistente y de alta disponibilidad, tiene la opción de habilitar la [redundancia de zona](zone-redundancy.md) en regiones de Azure seleccionadas. Una característica del nivel de servicio prémium, la redundancia de zona, usa [zonas de disponibilidad](../availability-zones/az-overview.md) de Azure para replicar el registro en tres zonas independientes como mínimo en cada región habilitada. Combine la replicación geográfica y la redundancia de zona para mejorar la confiabilidad y el rendimiento de un registro. 

## <a name="scalable-storage"></a>Almacenamiento escalable

Azure Container Registry le permite crear tantos repositorios, imágenes, capas o etiquetas como necesite, hasta el [límite de almacenamiento del registro](container-registry-skus.md#service-tier-features-and-limits). 

Unas cifras altas de etiquetas y repositorios pueden afectar al rendimiento del registro. Elimine periódicamente repositorios, etiquetas e imágenes no utilizados como parte de la rutina de mantenimiento del registro y, si lo desea, establezca una [directiva de retención](container-registry-retention-policy.md) para los manifiestos no etiquetados. Los recursos de registro eliminados, como los repositorios, imágenes o etiquetas *no pueden* recuperarse después de la eliminación. Para obtener más información acerca de cómo eliminar recursos del registro, consulte [Eliminación de imágenes de contenedor en Azure Container Registry](container-registry-delete.md).

## <a name="storage-cost"></a>Coste del almacenamiento

Para información completa acerca de los precios, consulte los [precios de Azure Container Registry][pricing].

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los registros de contenedor Básico, Estándar y Premium, consulte [SKU de Azure Container Registry](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: https://aka.ms/acr/pricing

<!-- LINKS - Internal -->
