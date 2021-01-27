---
title: Actualización de .NET para Apache Spark a la versión v1.0 en HDI
description: Obtenga información sobre la actualización a la versión 1.0 de .NET para Apache Spark en HDI y sobre cómo afecta eso al código y los clústeres existentes.
author: Niharikadutta
ms.author: nidutta
ms.service: hdinsight
ms.topic: how-to
ms.date: 01/05/2021
ms.openlocfilehash: a1602f29a6d0066ec3c99e990532411621652c47
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98788136"
---
# <a name="updating-net-for-apache-spark-to-version-v10--in-hdinsight"></a>Actualización de .NET para Apache Spark a la versión v1.0 en HDInsight

En este documento se habla sobre la primera versión principal de [.NET para Apache Spark](https://github.com/dotnet/spark) y sobre cómo puede afectar a las canalizaciones de producción actuales de los clústeres de HDInsight.

## <a name="about-net-for-apache-spark-version-100"></a>Acerca de la versión 1.0.0 de .NET para Apache Spark

Esta es la primera [versión principal oficial](https://github.com/dotnet/spark/releases/tag/v1.0.0) de .NET para Apache Spark y proporciona integridad de la API de DataFrame para Spark 2.4.x y Spark 3.0.x, además de otras características. Para obtener una lista completa de todas las características, mejoras y correcciones de errores, vea las [notas de la versión v1.0.0](https://github.com/dotnet/spark/blob/master/docs/release-notes/1.0.0/release-1.0.0.md) oficiales.
Otro aspecto importante que se debe tener en cuenta es que esta versión **no** es compatible con versiones anteriores de `Microsoft.Spark` y `Microsoft.Spark.Worker`. Vea la [guía de migración](https://github.com/dotnet/spark/blob/master/docs/migration-guide.md#upgrading-from-microsoftspark-0x-to-10) si planea actualizar una aplicación de .NET para Apache Spark para que sea compatible con v1.0.0.

## <a name="using-net-for-apache-spark-v10-in-hdinsight"></a>Uso de .NET para Apache Spark v1.0 en HDInsight

Aunque los clústeres actuales de HDI no se van a ver afectados (es decir, siguen teniendo la misma versión que antes), los clústeres de HDI recién creados llevan esta versión v1.0.0 más reciente de .NET para Apache Spark. Significado si:

- **Tiene un clúster de HDI anterior**: si quiere actualizar la aplicación de .NET para Spark a v1.0.0 (recomendado), tiene que actualizar la versión de `Microsoft.Spark.Worker` en el clúster de HDI. Para obtener más información, vea la sección [Cambio de la versión de .NET para Apache Spark en un clúster de HDI](#changing-net-for-apache-spark-version-on-hdinsight).
Si no quiere actualizar la versión actual de .NET para Apache Spark de la aplicación, no es necesario realizar ningún paso más.  

- **Tiene un nuevo clúster de HDI**: si quiere actualizar la aplicación de .NET para Spark a v1.0.0 (recomendado), no se necesita ningún paso para cambiar el trabajo en HDI, aunque tiene que ver la [guía de migración](https://github.com/dotnet/spark/blob/master/docs/migration-guide.md#upgrading-from-microsoftspark-0x-to-10) para entender los pasos necesarios para actualizar el código y las canalizaciones.
Si no quiere cambiar la versión actual de .NET para Apache Spark de la aplicación, tiene que cambiar la versión del clúster de HDI de v1.0 (valor predeterminado en clústeres nuevos) a cualquier versión que esté usando. Para obtener más información, vea la sección [Cambio de la versión de .NET para Apache Spark en un clúster de HDI](spark-dotnet-version-update.md#changing-net-for-apache-spark-version-on-hdinsight).  

## <a name="changing-net-for-apache-spark-version-on-hdinsight"></a>Cambio de la versión de .NET para Apache Spark en HDInsight

### <a name="deploy-microsoftsparkworker"></a>Implementación de Microsoft.Spark.Worker

`Microsoft.Spark.Worker` es un componente back-end que reside en los nodos de trabajo individuales del clúster de Spark. Si quiere ejecutar una función definida por el usuario (UDF) de C#, Spark debe entender cómo iniciar el CLR de .NET para ejecutar esta UDF. `Microsoft.Spark.Worker` pone a disposición de Spark una colección de clases que habilitan esta funcionalidad. Seleccione la versión del trabajo en función de la versión de .NET para Apache Spark que quiere implementar en el clúster de HDI.

1. Descargue la versión Linux de Microsoft.Spark.Worker de la versión en cuestión. Por ejemplo, si quiere `.NET for Apache Spark v1.0.0`, descargue [Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-1.0.0.tar.gz](https://github.com/dotnet/spark/releases/tag/v1.0.0).  

2. Descargue el script [install-worker.sh](https://github.com/dotnet/spark/blob/master/deployment/install-worker.sh) para instalar los archivos binarios del trabajo descargados en el paso 1 en todos los nodos de trabajo del clúster de HDI.  

3. Cargue los archivos mencionados anteriormente en la cuenta de Azure Storage a la que tiene acceso el clúster. Puede ver [el artículo sobre la implementación de .NET para Apache Spark en HDI](/dotnet/spark/tutorials/hdinsight-deployment#upload-files-to-azure) para obtener más detalles.

4. Ejecute el script `install-worker.sh` en todos los nodos de trabajo del clúster mediante acciones Script. Puede ver [el artículo sobre la implementación de .NET para Apache Spark en HDI](/dotnet/spark/tutorials/hdinsight-deployment#run-the-hdinsight-script-action) para obtener más información.

### <a name="update-your-application-to-use-specific-version"></a>Actualización de la aplicación para que use una versión concreta

Puede actualizar la aplicación de .NET para Apache Spark para usar una versión concreta si elige la versión necesaria del [paquete NuGet Microsoft.Spark](https://www.nuget.org/packages/Microsoft.Spark/) del proyecto. Si decide actualizar la aplicación a v1.0.0., asegúrese de ver las notas de la versión en cuestión y la [guía de migración](https://github.com/dotnet/spark/blob/master/docs/migration-guide.md#upgrading-from-microsoftspark-0x-to-10), como se ha mencionado anteriormente.

## <a name="faqs"></a>Preguntas más frecuentes

### <a name="will-my-existing-hdi-cluster-with-version--100-start-failing-with-the-new-release"></a>¿El clúster de HDI existente con la versión < 1.0.0 va a comenzar a sufrir errores con la nueva versión?

Los clústeres de HDI existentes van a seguir teniendo la misma versión anterior de .NET para Apache Spark y la aplicación existente (con una versión anterior de .NET para Spark) no se va a ver afectada.

## <a name="next-steps"></a>Pasos siguientes

[Implementación de la aplicación de .NET para Apache Spark en HDInsight](/dotnet/spark/tutorials/hdinsight-deployment)