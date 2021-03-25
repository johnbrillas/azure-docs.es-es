---
title: Migración de cargas de trabajo de Hive a una cuenta nueva en Azure Storage
description: Migración de cargas de trabajo de Hive a una cuenta nueva en Azure Storage
author: kevxmsft
ms.author: kevx
ms.reviewer: ''
ms.service: hdinsight
ms.topic: how-to
ms.date: 12/11/2020
ms.openlocfilehash: 0d62bebddb7751c168ba2e487b2391a40bbc6e67
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101744064"
---
# <a name="hive-workload-migration-to-new-account-in-azure-storage"></a>Migración de cargas de trabajo de Hive a una cuenta nueva en Azure Storage

Aprenda a usar acciones de script para copiar tablas de Hive entre cuentas de almacenamiento en HDInsight. Esto puede ser útil al migrar a [`Azure Data Lake Storage Gen2`](../hdinsight-hadoop-use-data-lake-storage-gen2.md).

Para copiar manualmente una tabla de Hive individual en HDInsight 4.0, consulte [Hive export/import](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ImportExport) (Exportación e importación de Hive).

## <a name="prerequisites"></a>Requisitos previos

* Un nuevo clúster de HDInsight con las siguientes configuraciones:
  * Su sistema de archivos predeterminado está en la cuenta de almacenamiento de destino. Consulte [Uso de Azure Storage con clústeres de Azure HDInsight](../hdinsight-hadoop-use-blob-storage.md).
  * La versión del clúster debe coincidir con la del clúster de origen.
  * Usa una nueva base de datos externa de metastore de Hive. Consulte [Uso de almacenes de metadatos externos](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation).
* Una cuenta de almacenamiento que sea accesible tanto para el clúster original como para el nuevo. Consulte [Adición de más cuentas de almacenamiento a HDInsight](../hdinsight-hadoop-add-storage.md) y [Tipos de almacenamiento y características](../hdinsight-hadoop-compare-storage-options.md#storage-types-and-features) para conocer los tipos de almacenamiento secundario permitidos.

    Estas son algunas opciones:
  * Agregue la cuenta de almacenamiento de destino al clúster original.
  * Agregue la cuenta de almacenamiento original al nuevo clúster.
  * Agregue una cuenta de almacenamiento intermediaria tanto al clúster original como al nuevo.

## <a name="how-it-works"></a>Funcionamiento

Ejecutaremos una acción de script para exportar las tablas de Hive desde el clúster original hasta un directorio de HDFS especificado. Consulte [Acción de script a un clúster en ejecución](../hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster).

Después, ejecutaremos otra acción de script en el nuevo clúster para importar las tablas de Hive desde el directorio de HDFS.

El script volverá a crear las tablas en el sistema de archivos predeterminado del nuevo clúster. Las tablas nativas también copiarán sus datos en el almacenamiento. Las tablas no nativas solo se copiarán por definición. Consulte [Hive Storage Handlers](https://cwiki.apache.org/confluence/display/Hive/StorageHandlers) (Controladores de almacenamiento de Hive) para obtener más información sobre tablas no nativas.

Se conservará la ruta de acceso de las tablas externas que no estén en el directorio de almacenes de Hive. Las otras tablas se copiarán en la ruta de acceso predeterminada de Hive del clúster de destino. Consulte propiedades de Hive `hive.metastore.warehouse.external.dir` y `hive.metastore.warehouse.dir`.

Los scripts no conservarán los permisos de archivo personalizados en el clúster de destino.

> [!NOTE]
>
> Esta guía permite copiar objetos de metadatos relacionados con las bases de datos, tablas y particiones de Hive. Los otros objetos de metadatos deben volver a crearse manualmente.
>
> * Para `Views`, Hive admite el comando `SHOW VIEWS` a partir de Hive 2.2.0 en HDInsight 4.0. Use `SHOW CREATE TABLE` para la definición de vista. Para las versiones de Hive anteriores, consulte la base de datos SQL de metastore para mostrar las vistas.
> * Para `Materialized Views`, use los comandos `SHOW MATERIALIZED VIEWS`, `DESCRIBE FORMATTED` y `CREATE MATERIALIZED VIEW`. Consulte [Materialized views](https://cwiki.apache.org/confluence/display/Hive/Materialized+views) (Vistas materializadas) para obtener más información.
> * Para `Constraints`, compatible a partir de Hive 2.1.0 en HDInsight 4.0, use `DESCRIBE EXTENDED` para enumerar las restricciones de una tabla, y use `ALTER TABLE` para agregar restricciones. Consulte [Alter Table Constraints](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-AlterTableConstraints) (Alterar restricciones de tabla) para obtener más información.

## <a name="copy-hive-tables"></a>Copia de tablas de Hive

1. Aplique la acción de script "export" en el clúster original con los campos siguientes.

    Esto generará y ejecutará scripts de Hive intermediarios. Se guardarán en la ruta `<hdfs-export-path>` especificada.

    De manera opcional, puede usar `--run-script=false` para personalizarlos antes de la ejecución manual.

    |Propiedad | Value |
    |---|---|
    |URI de script de Bash|`https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/export-hive-data-v01.sh`|
    |Tipos de nodo|Head|
    |Parámetros|`<hdfs-export-path>` `--run-script`|

    ```sh
    usage: generate Hive export and import scripts and export Hive data to specified HDFS path
           [--run-script={true,false}]
           hdfs-export-path

    positional arguments:

        hdfs-export-path      remote HDFS directory to write export data to

    optional arguments:
        --run-script={true,false}
                            whether to execute the generated Hive export script
                            (default: true)
    ```

2. Después de completar correctamente la exportación, aplique la acción de script "import" en el nuevo clúster con los campos siguientes.

    |Propiedad | Value |
    |---|---|
    |URI de script de Bash|`https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/import-hive-data-v01.sh`|
    |Tipos de nodo|Head|
    |Parámetros|`<hdfs-export-path>`|

    ```sh
    usage: download Hive import script from specified HDFS path and execute it
           hdfs-export-path

    positional arguments:

      hdfs-export-path      remote HDFS directory to download Hive import script from

    ```

## <a name="verification"></a>Comprobación

Descargue y ejecute el script como usuario raíz [`hive_contents.sh`](https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/hive_contents.sh) en el nodo principal de cada clúster, y compare el contenido del archivo de salida `/tmp/hive_contents.out`. Consulte [Conexión a HDInsight (Apache Hadoop) mediante SSH](../hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="cleanup-additional-storage-usage"></a>Limpieza del uso de almacenamiento adicional

Una vez completada y verificada la migración del almacenamiento, puede eliminar los datos de la ruta de exportación de HDFS especificada.

La opción es usar el comando `hdfs dfs -rm -R` de HDFS.

## <a name="option-reduce-additional-storage-usage"></a>Opción: reducción del uso de almacenamiento adicional

La acción de script export probablemente duplique el uso de almacenamiento debido a Hive. Sin embargo, es posible limitar el uso de almacenamiento adicional mediante la migración manual, una base de datos o una tabla a la vez.

1. Especifique `--run-script=false` para omitir la ejecución del script de Hive generado. Los scripts export e import de Hive todavía se guardarán en la ruta de acceso de exportación.

2. Ejecute fragmentos de código de los scripts export e import de Hive base de datos por base de datos o tabla por tabla; para ello, limpie manualmente la ruta de acceso de exportación después de cada base de datos o tabla que migre.

## <a name="next-steps"></a>Pasos siguientes

* [Azure Data Lake Storage Gen2](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
* [Uso de almacenes de metadatos externos](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation)
* [Tipos de almacenamiento y características](../hdinsight-hadoop-compare-storage-options.md#storage-types-and-features)
* [Acción de script a un clúster en ejecución](../hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster)
