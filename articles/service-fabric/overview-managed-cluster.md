---
title: Clústeres administrados de Service Fabric (versión preliminar)
description: Los clústeres administrados de Service Fabric son una evolución del modelo de recursos de clúster de Azure Service Fabric que agiliza la implementación y la administración de clústeres.
ms.topic: overview
ms.date: 02/15/2021
ms.openlocfilehash: 271852214097ee96ba6b10de7a94904981cd8ef8
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102041236"
---
# <a name="service-fabric-managed-clusters-preview"></a>Clústeres administrados de Service Fabric (versión preliminar)

Los clústeres administrados de Service Fabric son una evolución del modelo de recursos de clúster de Azure Service Fabric que agiliza la experiencia de implementación y administración de clústeres.

La plantilla del modelo de recursos de Azure (ARM) para los clústeres de Service Fabric tradicionales requiere la definición de un recurso de clúster junto con una serie de recursos de soporte técnico. Todos ellos deben estar "conectados" correctamente (durante la implementación y a lo largo del ciclo de vida del clúster) para que el clúster y los servicios funcionen correctamente. Por el contrario, el modelo de encapsulación para clústeres administrados de Service Fabric consta de un único recurso de *clúster administrado de Service Fabric*. Todos los recursos subyacentes para el clúster se abstraen y se administran mediante Azure en su nombre.

**Modelo de clúster tradicional de Service Fabric**
![Modelo de clúster tradicional de Service Fabric][sf-composition]

**Modelo de clúster administrado de Service Fabric**
![Modelo de clúster encapsulado de Service Fabric][sf-encapsulation]

En lo que se refiere al tamaño y la complejidad, la plantilla de ARM para un clúster administrado de Service Fabric consiste en alrededor de 100 líneas de JSON, frente a las 1000 líneas necesarias aproximadamente para definir un clúster de Service Fabric típico:

| Recursos de Service Fabric | Recursos de clúster administrado de Service Fabric |
|----------|-----------|
| Clúster de Service Fabric | Clúster administrado de Service Fabric |
| Conjuntos de escalado de máquinas virtuales | |
| Equilibrador de carga | |
| Dirección IP pública | |
| Cuentas de almacenamiento | |
| Virtual network | |

Los clústeres administrados de Service Fabric proporcionan una serie de ventajas respecto a los clústeres tradicionales:

**Implementación y administración simplificadas del clúster**
- Implementación y administración de un único recurso de Azure
- Administración y rotación automática de certificados
- Operaciones de escalado simplificadas

**Prevención de errores operativos**
- Prevención de errores de coincidencia de configuración con recursos subyacentes
- Bloqueo de operaciones no seguras (como la eliminación de un nodo de inicialización)

**Procedimientos recomendados de manera predeterminada**
- Configuración simplificada de la confiabilidad y la durabilidad

No hay ningún costo adicional por los clústeres administrados de Service Fabric más allá del costo de los recursos subyacentes necesarios para el clúster.

## <a name="service-fabric-managed-cluster-skus"></a>SKU de clúster administrado de Service Fabric

Los clústeres administrados de Service Fabric están disponibles en las SKU de nivel básico y estándar.

| Característica | Básico | Estándar |
| ------- | ----- | -------- |
| Recurso de red (SKU para [Load Balancer](../load-balancer/skus.md), [IP pública](../virtual-network/public-ip-addresses.md)) | Básico | Estándar |
| Número mínimo de nodos (instancia de máquina virtual) | 3 | 5 |
| Número máximo de nodos por tipo de nodo | 100 | 100 |
| Número máximo de tipos de nodos | 1 | 20 |
| Adición o eliminación de tipos de nodos | No | Sí |
| Redundancia de zona | No | Sí |

## <a name="whats-new-for-service-fabric-managed-clusters"></a>Novedades de los clústeres administrados de Service Fabric

Las últimas características de la versión preliminar de los clústeres administrados de Service Fabric permiten lo siguiente:

* [Implementación de aplicaciones con plantillas de ARM](how-to-managed-cluster-app-deployment-template.md)
* [Actualizaciones automáticas del sistema operativo](how-to-managed-cluster-configuration.md#enable-automatic-os-image-upgrades)
* [Cifrado de disco](how-to-enable-managed-cluster-disk-encryption.md)
* [Aplicación de reglas de NSG](how-to-managed-cluster-networking.md)

Entre las características que se van a agregar en las próximas versiones están:

* Implementación de aplicaciones con Visual Studio
* Compatibilidad con identidades administradas
* Zonas de disponibilidad
* Proxy inverso
* Escalado automático

## <a name="next-steps"></a>Pasos siguientes

Para empezar a utilizar los clústeres administrados de Service Fabric, pruebe el manual de inicio rápido:

> [!div class="nextstepaction"]
> [Creación de un clúster administrado de Service Fabric (versión preliminar)](quickstart-managed-cluster-template.md)


[sf-composition]: ./media/overview-managed-cluster/sfrp-composition-resource.png
[sf-encapsulation]: ./media/overview-managed-cluster/sfrp-encapsulated-resource.png