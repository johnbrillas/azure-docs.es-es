---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtenga sugerencias de desarrollo y detalles sobre Hadoop, Spark, R Server, Hive, etc.
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/08/2021
ms.openlocfilehash: f6b7000812f1adfe6ff7bd93711c9b8fe4ff9adc
ms.sourcegitcommit: 7e117cfec95a7e61f4720db3c36c4fa35021846b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99988356"
---
# <a name="azure-hdinsight-release-notes"></a>Notas de la versión de Azure HDInsight

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto en Azure.

Si quiere suscribirse a las notas de la versión, vea las versiones de [este repositorio de GitHub](https://github.com/hdinsight/release-notes/releases).

## <a name="release-date-02052021"></a>Fecha de lanzamiento: 05/02/2021

Esta versión se aplica a HDInsight 3.6 y HDInsight 4.0. La versión de HDInsight se pone a disposición de todas las regiones durante varios días. Esta fecha de lanzamiento indica la fecha de lanzamiento de la primera región. Si no ve los cambios siguientes, espere unos días a que la versión se active en su región.

## <a name="new-features"></a>Nuevas características
### <a name="dav4-series-support"></a>Compatibilidad con la serie Dav4
HDInsight ha agregado compatibilidad con la serie Dav4 en esta versión. Obtenga más información sobre la [serie Dav4 aquí](https://docs.microsoft.com/azure/virtual-machines/dav4-dasv4-series).

### <a name="kafka-rest-proxy-ga"></a>Disponibilidad general del proxy de REST de Kafka 
El proxy REST de Kafka le permite interactuar con el clúster de Kafka mediante una API REST a través de HTTPS. El proxy de REST de Kafka está disponible con carácter general a partir de esta versión. Obtenga más información sobre el [proxy de REST de Kafka aquí](https://docs.microsoft.com/azure/hdinsight/kafka/rest-proxy).

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Movimiento a conjuntos de escalado de máquinas virtuales
Ahora HDInsight usa máquinas virtuales de Azure para aprovisionar el clúster. El servicio se migra gradualmente a [conjuntos de escalado de máquinas virtuales de Azure](../virtual-machine-scale-sets/overview.md). Todo el proceso puede tardar meses. Después de migrar las regiones y las suscripciones, los clústeres de HDInsight recién creados se ejecutarán en conjuntos de escalado de máquinas virtuales sin acciones del cliente. No se espera ningún cambio importante.

## <a name="deprecation"></a>Desuso
### <a name="disabled-vm-sizes"></a>Tamaños de máquina virtual deshabilitados
A partir del 9 de enero de 2021, HDInsight impedirá que todos los clientes creen clústeres con los tamaños de máquina virtual standand_A8, standand_A9, standand_A10 y standand_A11. Los clústeres existentes se ejecutarán tal cual. Considere pasar a HDInsight 4.0 para evitar la posible interrupción del sistema o del soporte técnico.

## <a name="behavior-changes"></a>Cambios de comportamiento
### <a name="default-cluster-vm-size-changes-to-ev3-series"></a>El tamaño de la máquina virtual del clúster predeterminado cambia en la serie Ev3 
Los tamaños de máquinas virtuales de clústeres predeterminados se cambiarán de la serie D a la serie Ev3. Este cambio se aplica a los nodos principales y los nodos de trabajo. Para evitar que este cambio afecte a los flujos de trabajo probados, especifique los tamaños de máquina virtual que desea usar en la plantilla de Resource Manager.

### <a name="network-interface-resource-not-visible-for-clusters-running-on-azure-virtual-machine-scale-sets"></a>El recurso de la interfaz de red no es visible para clústeres que se ejecutan en conjuntos de escalado de máquinas virtuales de Azure.
HDInsight se está migrando gradualmente a conjuntos de escalado de máquinas virtuales de Azure. Los clientes ya no pueden ver las interfaces de red de las máquinas virtuales de los clústeres que usan conjuntos de escalado de máquinas virtuales de Azure.


### <a name="breaking-change-for-net-for-apache-spark-100"></a>Cambio importante de .NET para Apache Spark 1.0.0
HDInsight introducirá la primera versión principal oficial de .NET para Apache Spark en la próxima versión. Proporciona la integridad de la API DataFrame para Spark 2.4.x y Spark 3.0.x junto con otras características. Habrá cambios importantes en esta versión principal. Consulte [esta guía de migración](https://github.com/dotnet/spark/blob/master/docs/migration-guide.md#upgrading-from-microsoftspark-0x-to-10) para comprender los pasos necesarios para actualizar el código y las canalizaciones. Obtenga más información [aquí](https://docs.microsoft.com/azure/hdinsight/spark/spark-dotnet-version-update#using-net-for-apache-spark-v10-in-hdinsight).

## <a name="upcoming-changes"></a>Próximos cambios
En las próximas versiones, se realizarán los siguientes cambios.

### <a name="default-cluster-version-will-be-changed-to-40"></a>La versión predeterminada del clúster cambiará a la 4.0
A partir de febrero de 2021, la versión predeterminada del clúster de HDInsight cambiará de la 3.6 a la 4.0. Para más información sobre las versiones disponibles, consulte [este artículo](./hdinsight-component-versioning.md#available-versions). Más información sobre las novedades de [HDInsight 4.0](./hdinsight-version-release.md).

### <a name="os-version-upgrade"></a>Actualización de la versión del sistema operativo
HDInsight está actualizando la versión del sistema operativo de Ubuntu 16.04 a 18.04. La actualización se completará antes de abril de 2021.

### <a name="hdinsight-36-end-of-support-on-june-30-2021"></a>Fin del soporte técnico de HDInsight 3.6 el 30 de junio de 2021
El soporte técnico para HDInsight 3.6 finaliza. A partir del 30 de junio de 2021, los clientes no pueden crear clústeres de HDInsight 3.6. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Considere pasar a HDInsight 4.0 para evitar la posible interrupción del sistema o del soporte técnico.

## <a name="bug-fixes"></a>Corrección de errores
HDInsight continúa realizando mejoras en la confiabilidad y el rendimiento del clúster. 

## <a name="component-version-change"></a>Cambio de versión de componentes
No hay cambio de versión de componentes para esta versión. En [este documento](./hdinsight-component-versioning.md) puede encontrar las versiones actuales de los componentes para HDInsight 4.0 y HDInsight 3.6.

