---
title: Introducción a Azure Managed Instance for Apache Cassandra
description: Conozca Azure Managed Instance for Apache Cassandra. Este servicio administra la implementación y el escalado de instancias nativas de código abierto de Apache Cassandra en Azure.
author: TheovanKraay
ms.author: thvankra
ms.service: managed-instance-apache-cassandra
ms.topic: overview
ms.date: 03/02/2021
ms.openlocfilehash: d99c62e7d88510c351f87d85b4f8c5c271767cb3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101747838"
---
# <a name="what-is-azure-managed-instance-for-apache-cassandra-preview"></a>¿Qué es Azure Managed Instance for Apache Cassandra? (versión preliminar)

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El servicio Azure Managed Instance for Apache Cassandra proporciona operaciones automatizadas de implementación y escalado para centros de datos administrados de código abierto de Apache Cassandra. Este servicio le ayuda a acelerar los escenarios híbridos y a reducir el mantenimiento continuo. Dispondrá de funcionalidades de integración profunda y movimiento de datos con la [API de Cassandra de Azure Cosmos DB](../cosmos-db/cassandra-introduction.md) en su versión general.

:::image type="content" source="./media/introduction/icon.gif" alt-text="Azure Managed Instance for Apache Cassandra es un servicio administrado para Apache Cassandra." border="false":::

## <a name="key-benefits"></a>Ventajas principales

### <a name="hybrid-deployments"></a>Implementaciones híbridas

Puede usar este servicio para colocar fácilmente instancias administradas de centros de datos de Apache Cassandra, que se implementan automáticamente como conjuntos de escalado de máquinas virtuales, en una instancia de Azure Virtual Network nueva o existente. Estos centros de datos se pueden agregar al anillo de Apache Cassandra que se ejecuta de forma local a través de [Azure ExpressRoute](/azure/architecture/reference-architectures/hybrid-networking/expressroute) en Azure, o en otro entorno de nube.

- **Implementación simplificada:** una vez establecida la conectividad híbrida, la implementación es fácil a través del protocolo de chismes.
- **Métricas hospedadas:** las métricas se hospedan en [Prometheus](https://prometheus.io/docs/introduction/overview/) para supervisar la actividad en el clúster.

### <a name="simplified-scaling"></a>Escalado simplificado

En la instancia administrada, el escalado vertical y horizontal de los nodos en un centro de datos está totalmente administrado. Solo tiene que especificar el número de nodos que necesita, y el orquestador de escalado se encarga de establecer su operación dentro del anillo de Cassandra.

### <a name="managed-and-cost-effective"></a>Totalmente administrado y rentable

El servicio proporciona operaciones de administración para las siguientes tareas comunes de Apache Cassandra:

- Aprovisionamiento de un clúster
- Aprovisionamiento de un centro de datos
- Escalado de un centro de datos
- Eliminación de un centro de datos
- Inicio de una acción de reparación en un espacio de claves
- Cambio de la configuración de un centro de datos
- Configuración de copias de seguridad

El modelo de precios es flexible, a petición, basado en instancias y no tiene tarifas de licencia. Este modelo de precios le permite ajustarse a sus necesidades de carga de trabajo específicas. Puede elegir el número de núcleos, la SKU de la máquina virtual, el tamaño de memoria y el tamaño de espacio en disco que necesita.

## <a name="next-steps"></a>Pasos siguientes

Comience con uno de nuestros inicios rápidos:

* [Creación de un clúster de instancia administrada desde Azure Portal](create-cluster-portal.md)
* [Implementación de un clúster de Apache Spark administrado con Azure Databricks](deploy-cluster-databricks.md)
* [Administración de recursos de Azure Managed Instance for Apache Cassandra mediante la CLI de Azure](manage-resources-cli.md)
