---
title: Aumento de escala de Apache Kafka en Azure HDInsight
description: Aprenda a configurar discos administrados para clústeres de Apache Kafka en Azure HDInsight para aumentar la escalabilidad.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/09/2019
ms.openlocfilehash: 9aa11be42aca59458fea0462a90b6aeb70df893d
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104863146"
---
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>Configuración del almacenamiento y la escalabilidad de Apache Kafka en HDInsight

Aprenda a configurar el número de discos administrados usados por [Apache Kafka](https://kafka.apache.org/) en HDInsight.

Kafka en HDInsight utiliza el disco local de las máquinas virtuales del clúster de HDInsight. Como Kafka tiene muchas E/S, [Azure Managed Disks](../../virtual-machines/managed-disks-overview.md) se utiliza para proporcionar un alto rendimiento y un mayor espacio de almacenamiento por nodo. Si los discos duros virtuales (VHD) tradicionales se utilizaron para Kafka, cada nodo se limita a 1 TB. Con Managed Disks, puede utilizar varios discos para lograr hasta 16 TB para cada nodo del clúster.

El diagrama siguiente proporciona una comparación entre Kafka en HDInsight antes de usar Managed Disks y Kafka en HDInsight ya con este:

:::image type="content" source="./media/apache-kafka-scalability/kafka-with-managed-disks-architecture.png" alt-text="Arquitectura de Kafka con discos administrados" border="false":::

## <a name="configure-managed-disks-azure-portal"></a>Configuración de Managed Disks: Azure Portal

1. Siga los pasos que se explican en [Creación de un clúster de HDInsight](../hdinsight-hadoop-create-linux-clusters-portal.md) para comprender los pasos habituales para crear un clúster mediante el portal. No termine el proceso de creación del portal.

2. En la sección **Configuración y precios**, use el campo __Número de nodos__ para configurar el número de discos.

    > [!NOTE]  
    > El tipo de disco administrado puede ser __Estándar__ (HDD) o __Premium__ (SSD). Los discos Premium se utilizan con máquinas virtuales de las series DS y GS. Todos los otros tipos de máquina virtual usan discos estándar.

    :::image type="content" source="./media/apache-kafka-scalability/azure-portal-cluster-configuration-pricing-kafka-disks.png" alt-text="Sección de tamaño del clúster con los discos por nodo de trabajo resaltada" border="true":::

## <a name="configure-managed-disks-resource-manager-template"></a>Configuración de Managed Disks: Plantilla de Resource Manager

Para controlar el número de discos usados por los nodos de trabajo en un clúster de Kafka, utilice la siguiente sección de la plantilla:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

Puede encontrar una plantilla completa que muestra cómo configurar discos administrados en [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo trabajar con Apache Kafka en HDInsight, consulte los documentos siguientes:

* [Uso de MirrorMaker para crear una réplica de Apache Kafka en HDInsight](apache-kafka-mirroring.md)
* [Uso de Apache Storm con Apache Kafka en HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Uso de Apache Spark con Apache Kafka en HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Conexión a Apache Kafka a través de una instancia de Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)

* [Blog de HDInsight sobre los discos administrados con Kafka](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)
