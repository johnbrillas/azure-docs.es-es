---
title: Migración de datos de Apache Cassandra a Cassandra API de Azure Cosmos DB mediante Databricks (Spark)
description: Aprenda a migrar datos de la base de datos de Apache Cassandra a Cassandra API de Azure Cosmos DB mediante Azure Databricks y Spark.
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 11/16/2020
ms.author: thvankra
ms.reviewer: thvankra
ms.openlocfilehash: 3cbcb7eb3695e6f57daef741d4cd4b15577d8f58
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99493284"
---
# <a name="migrate-data-from-cassandra-to-azure-cosmos-db-cassandra-api-account-using-azure-databricks"></a>Migración de los datos de Cassandra a una cuenta de Cassandra API de Azure Cosmos DB mediante Azure Databricks
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Cassandra API en Azure Cosmos DB se ha convertido en una excelente opción para cargas de trabajo empresariales que se ejecutan en Apache Cassandra por diversos motivos como, por ejemplo: 

* **No hay sobrecarga de administración y supervisión**: evita la sobrecarga que supone administrar y supervisar innumerables configuraciones en el sistema operativo, JVM, los archivos yaml y sus interacciones.

* **Importante ahorro de costos**: puede ahorrar costos con Azure Cosmos DB, como en el costo de las máquinas virtuales, del ancho de banda y de las licencias aplicables. Además, no tiene que administrar los centros de datos, los servidores, el almacenamiento en SSD, las redes y los costos de electricidad. 

* **Posibilidad de usar código y herramientas existentes**: Azure Cosmos DB proporciona compatibilidad de nivel de protocolo de conexión con SDK y herramientas existentes de Cassandra. Esta compatibilidad garantiza que pueda usar el código base existente con Cassandra API de Azure Cosmos DB con cambios triviales.

Existen varias maneras de migrar las cargas de trabajo de base de datos de una plataforma a otra. [Azure Databricks](https://azure.microsoft.com/services/databricks/) es una oferta de plataforma como servicio para [Apache Spark](https://spark.apache.org/) que proporciona una forma de realizar migraciones sin conexión a gran escala. En este artículo se describen los pasos necesarios para migrar datos de espacios de claves/tablas nativos de Apache Cassandra a Cassandra API de Azure Cosmos DB mediante Azure Databricks.

## <a name="prerequisites"></a>Requisitos previos

* [Aprovisionar una cuenta de Cassandra API de Azure Cosmos DB](create-cassandra-dotnet.md#create-a-database-account)

* [Revisar los conceptos básicos de la conexión a Cassandra API de Azure Cosmos DB](cassandra-spark-generic.md)

* Revise las [características compatibles en Cassandra API de Azure Cosmos DB](cassandra-support.md) para garantizar la compatibilidad.

* Asegúrese de que ya ha creado un espacio de claves y tablas sin contenido en la cuenta de Cassandra API de Azure Cosmos DB de destino.

* [Uso de cqlsh o shell hospedado para la validación](cassandra-support.md#hosted-cql-shell-preview)

## <a name="provision-an-azure-databricks-cluster"></a>Aprovisionar un clúster de Azure Databricks

Puede seguir las instrucciones para [aprovisionar un clúster de Azure Databricks](/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal). Sin embargo, tenga en cuenta que Apache Spark 3.x no es compatible actualmente con el conector de Apache Cassandra. Tendrá que aprovisionar un entorno de ejecución de Databricks con una versión v2.x compatible de Apache Spark. Se recomienda seleccionar una versión del entorno de ejecución de Databricks que admita la versión más reciente de Spark 2.x, con una versión de Scala que no sea posterior a la 2.11:

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-runtime.png" alt-text="Entorno de ejecución de Databricks":::


## <a name="add-dependencies"></a>Adición de dependencias

Tendrá que agregar la biblioteca de conectores de Cassandra de Apache Spark a su clúster para conectarse a los puntos de conexión nativos y de Cassandra de Cosmos DB. En el clúster, seleccione libraries -> install new -> maven -> search packages (bibliotecas > instalar nueva > maven > buscar paquetes):

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-search-packages.png" alt-text="Buscar paquetes de Databricks":::

Escriba `Cassandra` en el cuadro de búsqueda y seleccione el repositorio `spark-cassandra-connector` de Maven más reciente disponible y, a continuación, seleccione install (instalar):

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-search-packages-2.png" alt-text="Seleccionar paquetes de Databricks":::

> [!NOTE]
> Asegúrese de reiniciar el clúster de Databricks después de que se haya instalado la biblioteca de conectores de Cassandra.

## <a name="create-scala-notebook-for-migration"></a>Creación de un cuaderno de Scala para la migración

Cree un cuaderno de Scala en Databricks con el código siguiente. Reemplace las configuraciones de Cassandra de origen y de destino con las credenciales correspondientes, así como los espacios de claves y las tablas de origen y destino, y ejecute lo siguiente:

```scala
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql._
import org.apache.spark.SparkContext

// source cassandra configs
val nativeCassandra = Map( 
    "spark.cassandra.connection.host" -> "<Source Cassandra Host>",
    "spark.cassandra.connection.port" -> "9042",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "false",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>"
)

//target cassandra configs
val cosmosCassandra = Map( 
    "spark.cassandra.connection.host" -> "<USERNAME>.cassandra.cosmos.azure.com",
    "spark.cassandra.connection.port" -> "10350",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "true",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>",
    //throughput related settings below - tweak these depending on data volumes. 
    "spark.cassandra.output.batch.size.rows"-> "1",
    "spark.cassandra.connection.connections_per_executor_max" -> "10",
    "spark.cassandra.output.concurrent.writes" -> "1000",
    "spark.cassandra.concurrent.reads" -> "512",
    "spark.cassandra.output.batch.grouping.buffer.size" -> "1000",
    "spark.cassandra.connection.keep_alive_ms" -> "600000000"
)

//Read from native Cassandra
val DFfromNativeCassandra = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(nativeCassandra)
  .load
  
//Write to CosmosCassandra
DFfromNativeCassandra
  .write
  .format("org.apache.spark.sql.cassandra")
  .options(cosmosCassandra)
  .save
```

> [!NOTE]
> Las configuraciones de `spark.cassandra.output.batch.size.rows`, `spark.cassandra.output.concurrent.writes` y `connections_per_executor_max` son importantes para evitar la [limitación de frecuencia](/samples/azure-samples/azure-cosmos-cassandra-java-retry-sample/azure-cosmos-db-cassandra-java-retry-sample/), que sucede cuando las solicitudes a Azure Cosmos DB superan el rendimiento aprovisionado ([unidades de solicitud](./request-units.md)). Es posible que tenga que ajustar esta configuración en función del número de ejecutores del clúster de Spark y, potencialmente, del tamaño (y, por consiguiente, el costo de unidad de solicitud) de cada registro que se escribe en las tablas de destino.

## <a name="troubleshooting"></a>Solucionar problemas

### <a name="rate-limiting-429-error"></a>Limitación de frecuencia (error 429)
Puede ver un código de error de 429 o el texto de error `request rate is large`, a pesar de reducir la configuración anterior a sus valores mínimos. A continuación se muestran algunos escenarios:

- **El rendimiento asignado a la tabla es inferior a 6000 [unidades de solicitud](./request-units.md)** . Incluso con una configuración mínima, Spark podrá ejecutar escrituras a una velocidad de alrededor de 6000 unidades de solicitud o más. Si ha aprovisionado una tabla en un espacio de claves con un rendimiento compartido aprovisionado, es posible que esta tabla tenga menos de 6000 RU disponibles en tiempo de ejecución. Asegúrese de que la tabla a la que va a migrar tiene al menos 6000 RU a su disposición al ejecutar la migración y, si es necesario, asigne unidades de solicitud dedicadas a esa tabla. 
- **Asimetría de datos excesiva con grandes volúmenes de datos**. Si tiene una gran cantidad de datos (es decir, filas de tabla) para migrar a una tabla determinada, pero tiene una asimetría significativa en los datos (es decir, un gran número de registros que se escriben para el mismo valor de clave de partición), es posible que experimente una limitación de velocidad aunque tenga una gran cantidad de [unidades de solicitud](./request-units.md) aprovisionadas en la tabla. Esto se debe a que las unidades de solicitud se dividen equitativamente entre las particiones físicas y, una asimetría de datos intensiva puede dar lugar a un cuello de botella de las solicitudes en una única partición, lo que produce una limitación de frecuencia. En este escenario, se recomienda reducir a la configuración de rendimiento mínima en Spark para evitar la limitación de frecuencia y forzar la ejecución lenta de la migración. Este escenario puede ser más común al migrar las tablas de referencia o de control, donde el acceso es menos frecuente, pero la asimetría puede ser alta. Sin embargo, si hay una asimetría significativa en cualquier otro tipo de tabla, también puede ser aconsejable revisar el modelo de datos para evitar problemas de partición frecuentes para la carga de trabajo durante las operaciones de estado estable. 
- **No se puede obtener el recuento en una tabla grande**. Actualmente, no se admite la ejecución de `select count(*) from table` para tablas grandes. Puede obtener el recuento de las métricas en Azure Portal (consulte nuestro [artículo de solución de problemas](cassandra-troubleshoot.md)), pero, si necesita determinar el recuento de una tabla grande desde dentro del contexto de un trabajo de Spark, puede copiar los datos en una tabla temporal y después usar Spark SQL para obtener el recuento, por ejemplo, a continuación (reemplace `<primary key>` con algún campo de la tabla temporal resultante).

  ```scala
  val ReadFromCosmosCassandra = sqlContext
    .read
    .format("org.apache.spark.sql.cassandra")
    .options(cosmosCassandra)
    .load

  ReadFromCosmosCassandra.createOrReplaceTempView("CosmosCassandraResult")
  %sql
  select count(<primary key>) from CosmosCassandraResult
  ```

## <a name="next-steps"></a>Pasos siguientes

* [Aprovisionamiento del rendimiento en contenedores y bases de datos](set-throughput.md) 
* [Procedimientos recomendados de las claves de partición](partitioning-overview.md#choose-partitionkey)
* [Estimación de RU/s mediante Capacity Planner de Azure Cosmos DB](estimate-ru-with-capacity-planner.md)
* [Escala elástica en Cassandra API de Azure Cosmos DB](manage-scale-cassandra.md)
