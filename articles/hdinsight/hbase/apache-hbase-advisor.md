---
title: Recomendaciones de optimización para el clúster
titleSuffix: Azure HDInsight
description: Recomendaciones de optimización de Apache HBase para el clúster en Azure HDInsight.
author: ramkrish86
ms.author: ramvasu
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/03/2021
ms.openlocfilehash: 36d40215f759190cc9e6c6e3f4918dcbc384f94f
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97893250"
---
# <a name="apache-hbase-advisories-in-azure-hdinsight"></a>Recomendaciones para Apache HBase en Azure HDInsight

En este artículo se describen varias recomendaciones que ayudan a optimizar el rendimiento de Apache HBase en Azure HDInsight. 

## <a name="optimize-hbase-to-read-most-recently-written-data"></a>Optimización de HBase para leer los datos escritos más recientemente

Al usar Apache HBase en Azure HDInsight, puede optimizar la configuración de HBase para el escenario en el que la aplicación lee los datos escritos más recientemente. Para un mayor rendimiento, lo ideal es que las lecturas de HBase procedan de memstore, en lugar del almacenamiento remoto.

El aviso de consulta indica que para una familia de columnas determinada de una tabla, más del 75 % de las lecturas proceden de memstore. Este indicador sugiere que, aunque memstore se vacíe, es necesario acceder al archivo reciente, que además debe estar en la memoria caché. Los datos se escriben primero en memstore y el sistema accede allí a los datos recientes. Los subprocesos de vaciado de HBase internos podrían detectar que una región determinada ha alcanzado el tamaño de 128 M (predeterminado) y desencadenar un vaciado. Este escenario se produciría incluso con los datos más recientes que se escribieran cuando el tamaño de memstore fuera de unos 128 M. Por lo tanto, para leer después esos registros recientes podría ser necesaria una lectura de archivo en lugar de desde memstore. Por este motivo, es mejor establecer que incluso los datos recientes que se hayan vaciado recientemente puedan residir en la memoria caché.

Para optimizar los datos recientes en la memoria caché, tenga en cuenta las siguientes opciones de configuración:

1. Establezca `hbase.rs.cacheblocksonwrite` en `true`. Esta configuración predeterminada en HDInsight HBase es `true`; compruebe que no se haya restablecido en `false`.

2. Aumente el valor de `hbase.hstore.compactionThreshold` para que evitar la compactación. De forma predeterminada, este valor es `3`. Puede aumentarlo a uno mayor, como `10`.

3. Si sigue el paso 2 y establece compactionThreshold, cambie `hbase.hstore.compaction.max` a un valor mayor, como `100`, y aumente también el valor de la configuración de `hbase.hstore.blockingStoreFiles`, por ejemplo `300`.

4. Si está seguro de que solo necesita leer los datos recientes, establezca la configuración de `hbase.rs.cachecompactedblocksonwrite` en **ON**. Esta configuración indica al sistema que, aunque se produzca la compactación, los datos permanecen en la memoria caché. También se pueden establecer las configuraciones en el nivel de familia. 

   En el shell de HBase, ejecute el comando siguiente:
   
   ```
   alter '<TableName>', {NAME => '<FamilyName>', CONFIGURATION => {'hbase.hstore.blockingStoreFiles' => '300'}}
   ```

5. La caché de bloques se puede desactivar para una familia determinada de una tabla. Asegúrese de que está en **ON** para las familias con las lecturas de datos más recientes. De forma predeterminada, la memoria caché de bloques está activada (ON) para todas las familias de una tabla. Si ha deshabilitado la memoria caché de bloques para una familia y necesita activarla, use el comando alter del shell de HBase.

   Estas configuraciones ayudan a garantizar que los datos están en la memoria caché y que los recientes no se compactan. Si su escenario admite TTL, considere la posibilidad de usar la compactación con niveles de fecha. Para más información, consulte la [Guía de referencia de Apache HBase: compactación con niveles de fecha](https://hbase.apache.org/book.html#ops.date.tiered).  

## <a name="optimize-the-flush-queue"></a>Optimización de la cola de vaciado

El aviso para optimizar la cola de vaciado indica que el vaciado de HBase podría necesitar ajustes. Los controladores de vaciado podrían no ser lo suficientemente altos como están configurados.

En la interfaz de usuario del servidor de regiones, observe si la cola de vaciado supera los 100. Este umbral indica que los vaciados son lentos y es posible que tenga que ajustar la configuración de `hbase.hstore.flusher.count`. De forma predeterminada, el valor es 2. Asegúrese de que el número máximo de subprocesos de vaciado no supera los 6.

Además, consulte las recomendaciones de ajuste del número de regiones. En primer lugar, pruebe a ajustar la región para ver si esto ayuda a agilizar el vaciado. Optimizar los subprocesos de vaciado puede ayudar de varias maneras. 

## <a name="region-count-tuning"></a>Optimización del número de regiones

El aviso de optimización del número de regiones indica que HBase ha bloqueado las actualizaciones y que el número de regiones puede ser mayor que el montón óptimo admitido. Puede ajustar el tamaño del montón, el tamaño de memstore y el número de regiones.

Escenario de ejemplo:

- Supongamos que el tamaño del montón del servidor de regiones es 10 GB. De forma predeterminada, `hbase.hregion.memstore.flush.size` es `128M`. El valor predeterminado de `hbase.regionserver.global.memstore.size` es `0.4`. Esto significa que, a partir de los 10 GB, se asignan 4 GB para memstore (globalmente).

- Demos por hecho que hay una distribución uniforme de la carga de escritura en todas las regiones y, suponiendo que cada región crece hasta 128 MB, el número máximo de regiones de esta configuración es `32`. Si un servidor determinado está configurado para tener 32 regiones, el sistema evita mejor el bloqueo de las actualizaciones.

- Con esta configuración vigente, el número de regiones es 100. Los 4 GB globales de memstore ahora se dividen entre 100 regiones. Por lo tanto, efectivamente, cada región obtiene solo 40 MB de memstore. Cuando las escrituras son uniformes, el sistema realiza vaciados frecuentes y reduce el tamaño al orden de < 40 MB. Tener muchos subprocesos de vaciado podría aumentar la velocidad de vaciado de `hbase.hstore.flusher.count`.

El aviso significa que sería conveniente reevaluar el número de regiones por servidor, el tamaño del montón y la configuración global del tamaño de memstore junto con la optimización de los subprocesos de vaciado para evitar el bloqueo de estas actualizaciones.

## <a name="compaction-queue-tuning"></a>Optimización de la cola de compactación

Si la cola de compactación de HBase supera los 2000 y esto se produce periódicamente, puede aumentar los subprocesos de compactación a un valor mayor.

Un número de archivos para la compactación excesivo puede provocar mayor uso del montón en cuanto al modo en que los archivos interactúan con el sistema de archivos de Azure. Por lo tanto, es mejor completar la compactación lo antes posible. A veces, en clústeres anteriores, las configuraciones de compactación relacionadas con la limitación pueden suponer una velocidad de compactación más lenta.

Compruebe las configuraciones `hbase.hstore.compaction.throughput.lower.bound` y `hbase.hstore.compaction.throughput.higher.bound`. Si ya se han establecido en 50 M y 100 M, deje los valores como están. Sin embargo, si las configuraciones tienen un valor menor (que era el caso de los clústeres anteriores), cambie los límites a 50 M y 100 M, respectivamente.

Las configuraciones son `hbase.regionserver.thread.compaction.small` y `hbase.regionserver.thread.compaction.large` (los valores predeterminados son 1 cada una).
Limite el valor máximo de esta configuración a menos de 3.

## <a name="full-table-scan"></a>Análisis de tabla completa

El aviso de recorrido de tabla completo indica que más del 75 % de los exámenes emitidos son exámenes completos de la tabla o región. Puede reconsiderar el modo en que el código llama a los exámenes para mejorar el rendimiento de las consultas. Observe los procedimientos siguientes:

* Establezca la fila de inicio y detención adecuadas para cada examen.

* Use la API **MultiRowRangeFilter** para poder consultar distintos intervalos en una llamada de examen. Para más información, consulte la [documentación de la API MultiRowRangeFilter](https://hbase.apache.org/2.1/apidocs/org/apache/hadoop/hbase/filter/MultiRowRangeFilter.html).

* Cuando necesite un examen completo de la tabla o la región, compruebe si existe la posibilidad de evitar el uso de la memoria caché para esas consultas, de modo que otras consultas que usen la memoria caché no expulsen los bloques activos. Para asegurarse de que los exámenes no utilizan la memoria caché, use la API de **examen** con la opción **setCaching(false)** en el código: 

   ```
   scan#setCaching(false)
   ```
   
## <a name="next-steps"></a>Pasos siguientes

[Optimización de Apache HBase con Ambari](../optimize-hbase-ambari.md)
