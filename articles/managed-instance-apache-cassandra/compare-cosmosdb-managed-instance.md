---
title: Diferencias entre Azure Managed Instance for Apache Cassandra y Cassandra API de Azure Cosmos DB
description: Obtenga información sobre las diferencias entre Azure Managed Instance for Apache Cassandra y Cassandra API de Azure Cosmos DB. También aprenderá las ventajas de cada uno de estos servicios y cuándo elegirlos.
author: TheovanKraay
ms.author: thvankra
ms.service: managed-instance-apache-cassandra
ms.topic: quickstart
ms.date: 03/02/2021
ms.openlocfilehash: 3050dda4b3c0e1a05d751a4f5969bba69ad0506d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101747922"
---
# <a name="differences-between-azure-managed-instance-for-apache-cassandra-preview-and-azure-cosmos-db-cassandra-api"></a>Diferencias entre Azure Managed Instance for Apache Cassandra (versión preliminar) y Cassandra API de Azure Cosmos DB 

En este artículo aprenderá las diferencias entre Azure Managed Instance for Apache Cassandra y Cassandra API de Azure Cosmos DB. En este artículo se proporcionan recomendaciones sobre cómo elegir entre los dos servicios o cuándo hospedar su propio entorno de Apache Cassandra.

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="key-differences"></a>Principales diferencias

Azure Managed Instance for Apache Cassandra proporciona implementación, escalado y operaciones automatizadas para mantener el estado de los nodos de las instancias de Apache Cassandra de código abierto en Azure. También proporciona la funcionalidad de escalar horizontalmente la capacidad de los clústeres de Apache Cassandra existentes autohospedados en la nube o en el entorno local. Realiza el escalado horizontal mediante la adición de centros de datos administrados de Cassandra al anillo del clúster existente.

[Cassandra API](../cosmos-db/cassandra-introduction.md) de Azure Cosmos DB es una capa de compatibilidad sobre el servicio de base de datos nativo de nube de distribución global de Microsoft [Azure Cosmos DB](../cosmos-db/index.yml). La combinación de estos servicios en Azure proporciona una serie de opciones para los usuarios de Apache Cassandra en entornos de nube híbrida complejos.

## <a name="how-to-choose"></a>¿Cómo elegir?

En la tabla siguiente se muestran los escenarios comunes, los requisitos de la carga de trabajo y las aspiraciones en las que se adaptan cada uno de estos enfoques de implementación:

| |Apache Cassandra autohospedado en el entorno local o en Azure | Azure Managed Instance for Apache Cassandra | API Cassandra de Azure Cosmos DB |
|---------|---------|---------|---------|
|**Tipo de implementación**| Tiene una implementación de Apache Cassandra muy personalizada con revisiones personalizadas o adaptaciones propietarias. | Tiene una implementación estándar de Apache Cassandra de código abierto sin código personalizado. | Tiene su implementación en una plataforma que no tiene una capa de Apache Cassandra subyacente, pero que es compatible con todos los controladores de cliente de código abierto en un nivel de [protocolo de conexión](../cosmos-db/cassandra-support.md). |
| **Sobrecarga operativa**| Ya dispone de expertos en Cassandra que pueden implementar, configurar y mantener los clústeres.  | Quiere reducir la sobrecarga operativa del estado de los nodos de Apache Cassandra, pero seguir manteniendo el control sobre las configuraciones de nivel de plataforma, como la replicación y la coherencia. | Quiere eliminar la sobrecarga operativa mediante una base de datos de plataforma como servicio totalmente administrada en la nube. |
| **Requisitos del sistema operativo**| Tiene un requisito para mantener imágenes del sistema operativo de máquinas virtuales personalizadas o para comercialización. | Puede usar imágenes estándar pero desea tener control sobre las SKU, la memoria, los discos y las IOPS. | Quiere que el aprovisionamiento de la capacidad se simplifique y se exprese como una única métrica normalizada, con una relación de uno a uno con el rendimiento, como las [unidades de solicitud](../cosmos-db/request-units.md) de Azure Cosmos DB. |
| **Modelo de precios**| Quiere usar software de administración como las herramientas de Datastax y está satisfecho con los costos de las licencias. | Prefiere licencias de código abierto puras y precios basados en instancias de máquina virtual. | Quiere usar precios nativos de la nube, que incluyan ofertas de [escalado automático](../cosmos-db/manage-scale-cassandra.md#use-autoscale) y [sin servidor](../cosmos-db/serverless.md). |
| **Analytics**| Desea tener un control total sobre el aprovisionamiento de las canalizaciones analíticas, independientemente de la sobrecarga que conlleva crearlas y mantenerlas. | Quiere usar servicios analíticos basados en la nube como Azure Databricks. | Quiere que el análisis transaccional híbrido en tiempo real esté integrado en la plataforma con [Azure Synapse Link para Cosmos DB](../cosmos-db/synapse-link.md). |
| **Patrón de la carga de trabajo**| La carga de trabajo es bastante estable y no es necesario escalar los nodos del clúster con frecuencia. | La carga de trabajo es volátil y debe poder escalar o reducir verticalmente los nodos de un centro de datos o agregar o quitar centros de datos fácilmente. | La carga de trabajo suele ser volátil y debe poder escalar o reducir verticalmente de forma rápida y en un volumen significativo. |
| **SLA**| Está satisfecho con los procesos de mantenimiento de los SLA para la coherencia, el rendimiento, la disponibilidad y la recuperación ante desastres. | Está satisfecho con los procesos de mantenimiento de los SLA para la coherencia, el rendimiento y la disponibilidad, pero necesita ayuda con las copias de seguridad. | Quiere SLA completos para la coherencia, el rendimiento, la disponibilidad y la recuperación ante desastres. |

## <a name="next-steps"></a>Pasos siguientes

Comience con uno de nuestros inicios rápidos:

* [Creación de un clúster de instancia administrada desde Azure Portal](create-cluster-portal.md)