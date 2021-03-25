---
title: Migración de metastore de Hive predeterminado a una metastore externa en Azure HDInsight
description: Migración de metastore de Hive predeterminado a una metastore externa en Azure HDInsight
author: kevxmsft
ms.author: kevx
ms.reviewer: ''
ms.service: hdinsight
ms.topic: how-to
ms.date: 11/4/2020
ms.openlocfilehash: 825204fe40125a65e8e6f27c6973417813700a9e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743694"
---
# <a name="migrate-default-hive-metastore-db-to-external-metastore-db"></a>Migración de la base de datos de metastore de Hive predeterminada a una base de datos de metastore externa

En este artículo se muestra cómo migrar los metadatos de una [base de datos de metastore predeterminada](../hdinsight-use-external-metadata-stores.md#default-metastore) para Hive a una base de datos SQL externa en HDInsight. 

## <a name="why-migrate-to-external-metastore-db"></a>Por qué migrar a una base de datos de metastore externa

* La base de datos de metastore predeterminada está limitada a la SKU básica y no puede controlar cargas de trabajo de escala de producción.

* La base de metastore externa permite a los clientes escalar horizontalmente los recursos de proceso de Hive mediante la incorporación de nuevos clústeres de HDInsight que compartan la misma base de datos de metastore.

* Para la migración de HDInsight 3.6 a 4.0, es obligatorio migrar los metadatos a una base de datos de metastore externa antes de actualizar la versión del esquema de Hive. Consulte [Migración de cargas de trabajo de Hive de Azure HDInsight 3.6 a HDInsight 4.0](./apache-hive-migrate-workloads.md).

Dado que la base de datos de metastore predeterminada tiene una capacidad de proceso limitada, se recomienda un uso moderado de otros trabajos del clúster durante la migración de metadatos.

Las bases de datos de origen y destino deben usar la misma versión de HDInsight y las mismas cuentas de almacenamiento. Si va a actualizar las versiones de HDInsight de la 3.6 a la 4.0, complete los pasos de este artículo en primer lugar. Después, siga los pasos de actualización oficiales [aquí](./apache-hive-migrate-workloads.md).

## <a name="prerequisites"></a>Requisitos previos

Si usa [Azure Data Lake Storage Gen1](../overview-data-lake-storage-gen1.md), es probable que las ubicaciones de las tablas de Hive dependan de las configuraciones de HDFS del clúster para Azure Data Lake Storage Gen1. Ejecute la siguiente acción de script para que estas ubicaciones se puedan llevar a otros clústeres. Consulte [Acción de script a un clúster en ejecución](../hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster).

La acción es similar a reemplazar los vínculos simbólicos por sus rutas de acceso completas.

|Propiedad | Value |
|---|---|
|URI de script de Bash|`https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/hive-adl-expand-location-v01.sh`|
|Tipos de nodo|Head|
|Parámetros|""|

## <a name="migrate-with-exportimport-using-sqlpackage"></a>Migración con exportación e importación mediante sqlpackage

Solo los clústeres de HDInsight creados después de 2020-10-15 admiten la exportación o importación de SQL para la base de datos de metastore predeterminada de Hive mediante `sqlpackage`.

1. Instale [sqlpackage](https://docs.microsoft.com/sql/tools/sqlpackage-download#get-sqlpackage-net-core-for-linux) en el clúster.

2. Exporte la base de datos de metastore predeterminada a un archivo BACPAC mediante la ejecución del siguiente comando.

    ```bash
    wget "https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/hive_metastore_tool.py"
    SQLPACKAGE_FILE='/home/sshuser/sqlpackage/sqlpackage'  # replace with sqlpackage location
    TARGET_FILE='hive.bacpac'
    sudo python hive_metastore_tool.py --sqlpackagefile $SQLPACKAGE_FILE --targetfile $TARGET_FILE
    ```

3. Guarde el archivo BACPAC. A continuación se muestra una opción.

    ```bash
    hdfs dfs -mkdir -p /bacpacs
    hdfs dfs -put $TARGET_FILE /bacpacs/
    ```

4. Importe el archivo BACPAC a una nueva base de datos con los pasos que se enumeran [aquí](../../azure-sql/database/database-import.md).

5. La nueva base de datos está lista para su [configuración como una base de datos de metastore externa en un nuevo clúster de HDInsight](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation).

## <a name="migrate-using-hive-script"></a>Migración mediante un script de Hive

Los clústeres creados antes del 2020-10-15 no admiten la exportación e importación de la base de datos de metastore predeterminada.

En el caso de estos clústeres, siga la guía [Migración de cargas de trabajo de Hive a una cuenta nueva en Azure Storage](./hive-migration-across-storage-accounts.md) mediante un segundo clúster con una [base de datos de metastore de Hive externa](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation). El segundo clúster puede usar la misma cuenta de almacenamiento, pero debe usar un nuevo sistema de archivos predeterminado.

### <a name="option-to-shallow-copy"></a>Opción de copia "superficial"
El consumo de almacenamiento sería doble cuando las tablas se copian "en profundidad" mediante la guía anterior. Debe limpiar manualmente los datos en el contenedor de almacenamiento de origen.
En su lugar, podemos hacer una copia "superficial" de las tablas si no son transaccionales. Todas las tablas de Hive en HDInsight 3.6 son no transaccionales de forma predeterminada, pero solo las tablas externas son no transaccionales en HDInsight 4.0. Las tablas transaccionales se deben copiar en profundidad. Siga estos pasos para realizar una copia superficial de las tablas no transaccionales:

1. Ejecute el script [hive-ddls.sh](https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/hive-ddls.sh) en el nodo principal del clúster de origen para generar el DDL para cada tabla de Hive.
2. El DDL se escribe en un script de Hive local llamado `/tmp/hdi_hive_ddls.hql`. Ejecútelo en el clúster de destino que usa una base de datos de metastore de Hive externa.

## <a name="verify-that-all-hive-tables-are-imported"></a>Comprobación de la importación de todas las tablas de Hive

El siguiente comando usa una consulta SQL en la base de datos de metastore para imprimir todas las tablas de Hive y sus ubicaciones de datos. Compare la salida entre el clúster nuevo y el anterior para comprobar que no faltan tablas en la nueva base de datos de metastore.

```bash
SCRIPT_FNAME='hive_metastore_tool.py'
SCRIPT="/tmp/$SCRIPT_FNAME"
wget -O "$SCRIPT" "https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/$SCRIPT_FNAME"
OUTPUT_FILE='/tmp/hivetables.csv'
QUERY="SELECT DBS.NAME, TBLS.TBL_NAME, SDS.LOCATION FROM SDS, TBLS, DBS WHERE TBLS.SD_ID = SDS.SD_ID AND TBLS.DB_ID = DBS.DB_ID ORDER BY DBS.NAME, TBLS.TBL_NAME ASC;"
sudo python "$SCRIPT" --query "$QUERY" > $OUTPUT_FILE
```

## <a name="further-reading"></a>Lecturas adicionales

* [Migración de cargas de trabajo de Hive de Azure HDInsight 3.6 a HDInsight 4.0](./apache-hive-migrate-workloads.md)
* [Migración de cargas de trabajo de Hive a una cuenta nueva en Azure Storage](./hive-migration-across-storage-accounts.md)
* [Conexión a Apache Beeline en HDInsight o instalación local](../hadoop/connect-install-beeline.md)
* [Escenario: Error de permiso denegado al intentar crear una tabla de Apache Hive en Azure HDInsight](./interactive-query-troubleshoot-permission-error-create-table.md)
