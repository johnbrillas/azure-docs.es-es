---
title: 'Componentes y versiones de Apache Hadoop: Azure HDInsight'
description: Conozca los componentes y las versiones de Apache Hadoop disponibles en Azure HDInsight.
ms.service: hdinsight
ms.topic: conceptual
author: deshriva
ms.author: deshriva
ms.date: 02/08/2021
ms.openlocfilehash: 53ca2ac73fdec9d3b39ffc04cbb24aca707a72eb
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103490456"
---
# <a name="azure-hdinsight-versions"></a>Versiones de Azure HDInsight

HDInsight agrupa componentes de entorno de Apache Hadoop y la plataforma HDInsight en un paquete que se implementa en un clúster. Para obtener más información, consulte el artículo sobre [cómo funciona el control de versiones de HDInsight](hdinsight-overview-versioning.md).

## <a name="supported-hdinsight-versions"></a>Versiones compatibles de HDInsight

En la tabla siguiente se enumeran las versiones de HDInsight que están disponibles en Azure Portal y otros métodos de implementación como PowerShell, la CLI y el SDK de .NET.

| Versión de HDInsight | SISTEMA OPERATIVO DE LA MÁQUINA VIRTUAL | Fecha de la versión | Fecha de expiración del soporte técnico | Fecha de retirada | Alta disponibilidad |
| --- | --- | --- | --- | --- | --- |
| [HDInsight 4.0](hdinsight-40-component-versioning.md) |Ubuntu 16.0.4 LTS |24 de septiembre de 2018 | | |Sí |
| [HDInsight 3.6](hdinsight-36-component-versioning.md) |Ubuntu 16.0.4 LTS |4 de abril de 2017      | *30 de junio de 2021 |30 de junio de 2021 |Sí |

*Estamos ampliando el período de soporte técnico para determinados tipos de clúster de HDInsight 3.6. Consulte el artículo sobre las [versiones de componentes de HDInsight 3.6](hdinsight-36-component-versioning.md).

## <a name="release-notes"></a>Notas de la versión

Consulte las [Notas de la versión de HDInsight](hdinsight-release-notes.md) para conocer otras notas de las últimas versiones de HDInsight.

## <a name="support-options-for-hdinsight-versions"></a>Opciones de soporte técnico para las versiones de HDInsight

HDInsight ofrece compatibilidad estándar, que se define como un período de tiempo durante el cual una versión de HDInsight es compatible con el Soporte técnico y el Servicio al cliente de Microsoft.

La **Expiración del soporte técnico** significa que Microsoft ya no proporcionará soporte técnico para la versión especificada de HDInsight. Y ya no está disponible mediante Azure Portal para la creación del clúster.

La **retirada** de una versión de HDInsight significa que los clústeres existentes seguirán ejecutándose tal cual. Los nuevos clústeres de esta versión no se pueden crear por ningún medio (ni con la CLI ni con los SDK). No se garantiza que otras características del plano de control, como el escalado manual y la escalabilidad automática, funcionen después de la fecha de retirada. El soporte técnico no está disponible para las versiones retiradas.

## <a name="versioning-considerations"></a>Consideraciones de control de versiones
- Una vez que un clúster se implementa con una imagen, ese clúster no se actualiza automáticamente a la versión de la imagen más reciente. Al crear nuevos clústeres, se implementará la versión más reciente de la imagen.
- Los clientes deben probar y validar que las aplicaciones se ejecutan correctamente cuando se usa la nueva versión de HDInsight.
- HDInsight se reserva el derecho de cambiar la versión predeterminada sin previo aviso. Si tiene una dependencia de la versión, especifique la versión de HDInsight al crear clústeres.
- HDInsight puede retirar una versión de componente de software de código abierto antes de retirar la versión de HDInsight.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de clúster para Apache Hadoop, Spark, etc. en HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Paquete de seguridad de la empresa](./enterprise-security-package.md)
- [Trabajo en Apache Hadoop en HDInsight desde un equipo Windows](hdinsight-hadoop-windows-tools.md)