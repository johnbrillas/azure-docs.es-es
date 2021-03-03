---
title: Solución de errores habituales de Cassandra API de Azure Cosmos DB
description: En este documento se describen las formas de solucionar los problemas habituales detectados en Cassandra API de Azure Cosmos DB.
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 03/02/2021
ms.author: thvankra
ms.openlocfilehash: f9b6e586879b8697660ced7aa6f1e75083e3ee29
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658578"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-db-cassandra-api"></a>Solución de problemas habituales de Cassandra API de Azure Cosmos DB
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Cassandra API en Azure Cosmos DB es una capa de compatibilidad, que proporciona [compatibilidad con el protocolo de conexión](cassandra-support.md) para la conocida base de datos de código abierto Apache Cassandra; asimismo, se basa en [Azure Cosmos DB](./introduction.md). Como servicio nativo de nube totalmente administrado, Azure Cosmos DB proporciona y [garantiza la disponibilidad, el rendimiento y la coherencia](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/) de Cassandra API. Tenga en cuenta que estas garantías no son posibles en las implementaciones heredadas de Apache Cassandra. Igualmente, Cassandra API también facilita operaciones de plataforma de mantenimiento cero y una aplicación de revisión sin tiempo de inactividad. Como tal, muchas de sus operaciones de back-end son diferentes de Apache Cassandra, por lo que le recomendamos configuraciones y enfoques determinados para evitar errores comunes. 

En este artículo se describen los errores comunes y sus soluciones para aquellas aplicaciones que usan Cassandra API de Azure Cosmos DB. Si el error no aparece en la lista siguiente y se produce un error al ejecutar una [operación admitida en Cassandra API](cassandra-support.md) (este error *no se produce al usar la instancia nativa de Apache Cassandra*), [cree una solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md).

## <a name="nonodeavailableexception"></a>NoNodeAvailableException
Se trata de una excepción de contenedor de nivel superior con un gran número de posibles causas y excepciones internas, muchas de las cuales pueden estar relacionadas con el cliente. 
### <a name="solution"></a>Solución
Algunas de las causas y soluciones más populares son las siguientes: 
- Tiempo de espera de inactividad de instancias de Azure Load Balancer: También puede manifestarse como `ClosedConnectionException`. Para resolverlo, establezca la configuración para mantener la conexión en el controlador (consulte la información que tiene [a continuación](#enable-keep-alive-for-java-driver)) y aumente la configuración para mantener la conexión en el sistema operativo; por otro lado, también puede [ajustar el tiempo de espera de inactividad en Azure Load Balancer](../load-balancer/load-balancer-tcp-idle-timeout.md?tabs=tcp-reset-idle-portal). 
- **Agotamiento de recursos de la aplicación cliente:** asegúrese de que los equipos cliente tengan suficientes recursos para completar la solicitud. 

## <a name="cannot-connect-to-host"></a>No se puede establecer conexión con el host
Puede aparecer este error: `Cannot connect to any host, scheduling retry in 600000 milliseconds`. 

### <a name="solution"></a>Solución
Esto podría deberse al agotamiento de SNAT en el lado del cliente. Siga los pasos descritos en [Uso de SNAT para conexiones salientes](../load-balancer/load-balancer-outbound-connections.md) para descartar este problema. También puede tratarse de un problema de tiempo de espera de inactividad en el que Azure Load Balancer tiene 4 minutos de tiempo de espera de inactividad de manera predeterminada. Consulte la documentación en [Tiempo de espera de inactividad de Load Balancer](../load-balancer/load-balancer-tcp-idle-timeout.md?tabs=tcp-reset-idle-portal). Habilite TCP-keep alive desde la configuración del controlador (consulte la información [siguiente](#enable-keep-alive-for-java-driver)) y establezca el intervalo de `keepAlive` en el sistema operativo en menos de 4 minutos.

 

## <a name="overloadedexception-java"></a>OverloadedException (Java)
El número total de unidades de solicitud consumidas es mayor que las unidades de solicitud aprovisionadas en el espacio o la tabla. Por lo tanto, se limitan las solicitudes.
### <a name="solution"></a>Solución
Considere la posibilidad de escalar el rendimiento asignado a un espacio de claves o una tabla en Azure Portal (consulte [aquí](manage-scale-cassandra.md) las operaciones de escalado en Cassandra API); asimismo, también puede implementar una directiva de reintentos. Para Java, consulte los ejemplos de reintentos para el [controlador V3.x](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) y el [controlador V4.x](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample-v4). Consulte también las [Extensiones de Cassandra de Azure Cosmos para Java](https://github.com/Azure/azure-cosmos-cassandra-extensions).

### <a name="overloadedexception-even-with-sufficient-throughput"></a>OverloadedException incluso con suficiente rendimiento 
El sistema realiza solicitudes de limitación a pesar de que se aprovisiona un rendimiento suficiente para el volumen de solicitudes o el costo de las unidades de solicitud consumidas. Hay dos causas posibles para una limitación de frecuencia inesperada:
- **Operaciones de nivel de esquema:** Cassandra API implementa un presupuesto de rendimiento del sistema para las operaciones de nivel de esquema (CREATE TABLE, ALTER TABLE y DROP TABLE). Este presupuesto debe ser suficiente para las operaciones de esquema en un sistema de producción. Sin embargo, si tiene un gran número de operaciones de nivel de esquema, es posible que supere este límite. Como este presupuesto no lo controla el usuario, tendrá que considerar la posibilidad de reducir el número de operaciones de esquema que se estén ejecutando. Si al realizar esta acción no se resuelve el problema o no es factible para la carga de trabajo, [cree una solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md).
- **Asimetría de datos:** cuando el rendimiento se aprovisiona en Cassandra API, se divide equitativamente entre las particiones físicas y cada partición física tiene un límite superior. Si tiene una gran cantidad de datos que se insertan o consultan desde una partición concreta, es posible que se limite a la velocidad a pesar de que se aprovisiona una gran cantidad de rendimiento general (unidades de solicitud) para esa tabla. Revise el modelo de datos y asegúrese de que no tiene una asimetría excesiva que pueda estar causando particiones frecuentes. 

## <a name="intermittent-connectivity-errors-java"></a>Errores de conectividad intermitentes (Java) 
La conexión se interrumpe o se agota el tiempo de espera de forma inesperada.

### <a name="solution"></a>Solución 
Los controladores de Apache Cassandra para Java proporcionan dos directivas de reconexión nativa: `ExponentialReconnectionPolicy` y `ConstantReconnectionPolicy`. De manera predeterminada, es `ExponentialReconnectionPolicy`. Sin embargo, para Cassandra API de Azure Cosmos DB, se recomienda `ConstantReconnectionPolicy` con un retraso de 2 segundos. Consulte la [documentación del controlador](https://docs.datastax.com/en/developer/java-driver/4.9/manual/core/reconnection/) correspondiente al controlador de Java v4.x y haga clic [aquí](https://docs.datastax.com/en/developer/java-driver/3.7/manual/reconnection/) para obtener instrucciones sobre Java 3.x (consulte también los ejemplos siguientes que aparecen en [Configuración de ReconnectionPolicy para el controlador de Java](#configuring-reconnectionpolicy-for-java-driver)).

## <a name="error-with-load-balancing-policy"></a>Error con la directiva de equilibrio de carga

Si ha implementado una directiva de equilibrio de carga en v3.x del controlador Datastax de Java, con código similar al siguiente:

```java
cluster = Cluster.builder()
        .addContactPoint(cassandraHost)
        .withPort(cassandraPort)
        .withCredentials(cassandraUsername, cassandraPassword)
        .withPoolingOptions(new PoolingOptions() .setConnectionsPerHost(HostDistance.LOCAL, 1, 2)
                .setMaxRequestsPerConnection(HostDistance.LOCAL, 32000).setMaxQueueSize(Integer.MAX_VALUE))
        .withSSL(sslOptions)
        .withLoadBalancingPolicy(DCAwareRoundRobinPolicy.builder().withLocalDc("West US").build())
        .withQueryOptions(new QueryOptions().setConsistencyLevel(ConsistencyLevel.LOCAL_QUORUM))
        .withSocketOptions(getSocketOptions())
        .build();
```

Si el valor de `withLocalDc()` no coincide con el centro de datos del punto de contacto, es posible que se produzca un error muy intermitente: `com.datastax.driver.core.exceptions.NoHostAvailableException: All host(s) tried for query failed (no host was tried)`. 

### <a name="solution"></a>Solución 
Implemente [CosmosLoadBalancingPolicy](https://github.com/Azure/azure-cosmos-cassandra-extensions/blob/master/package/src/main/java/com/microsoft/azure/cosmos/cassandra/CosmosLoadBalancingPolicy.java) (puede que tenga que actualizar la versión secundaria de Datastax para que funcione):

```java
LoadBalancingPolicy loadBalancingPolicy = new CosmosLoadBalancingPolicy.Builder().withWriteDC("West US").withReadDC("West US").build();
```

## <a name="count-fails-on-large-table"></a>Error de recuento en una tabla grande
Al ejecutar `select count(*) from table` o similar para un gran número de filas, el servidor agota el tiempo de espera.

### <a name="solution"></a>Solución 
Si usa un cliente de CQLSH local, puede intentar cambiar la configuración de `--connect-timeout` o `--request-timeout` (consulte más detalles [aquí](https://cassandra.apache.org/doc/latest/tools/cqlsh.html)). Si esto no es suficiente y el recuento sigue agotando el tiempo de espera, puede obtener un recuento de los registros de la telemetría de back-end de Azure Cosmos DB. Para ello, vaya a la pestaña de métricas en Azure Portal, seleccione la métrica `document count` y, a continuación, agregue un filtro para la base de datos o colección (la analogía de la tabla en Azure Cosmos DB). Después, puede mantener el puntero sobre el gráfico resultante para el momento dado en el que desea un recuento del número de registros.

:::image type="content" source="./media/cassandra-troubleshoot/metrics.png" alt-text="vista de métricas":::


## <a name="configuring-reconnectionpolicy-for-java-driver"></a>Configuración de ReconnectionPolicy para el controlador de Java

### <a name="version-3x"></a>Versión 3.x

Para la versión 3.x del controlador de Java, configure la directiva de reconexión al crear un objeto de clúster:

```java
import com.datastax.driver.core.policies.ConstantReconnectionPolicy;

Cluster.builder()
  .withReconnectionPolicy(new ConstantReconnectionPolicy(2000))
  .build();
```

### <a name="version-4x"></a>Version 4.x

Para la versión 4.x del controlador de Java, configure la directiva de reconexión mediante la invalidación de la configuración en el archivo `reference.conf`:

```xml
datastax-java-driver {
  advanced {
    reconnection-policy{
      # The driver provides two implementations out of the box: ExponentialReconnectionPolicy and
      # ConstantReconnectionPolicy. We recommend ConstantReconnectionPolicy for Cassandra API, with 
      # base-delay of 2 seconds.
      class = ConstantReconnectionPolicy
      base-delay = 2 second
    }
}
```

## <a name="enable-keep-alive-for-java-driver"></a>Habilitar la opción para mantener la conexión en el controlador de Java

### <a name="version-3x"></a>Versión 3.x

En el caso de la versión 3.x del controlador de Java, establezca la opción para mantener la conexión; para ello, cree un objeto de clúster y asegúrese de que la opción para mantener la conexión está [habilitada en el sistema operativo](https://knowledgebase.progress.com/articles/Article/configure-OS-TCP-KEEPALIVE-000080089):

```java
import java.net.SocketOptions;
    
SocketOptions options = new SocketOptions();
options.setKeepAlive(true);
cluster = Cluster.builder().addContactPoints(contactPoints).withPort(port)
  .withCredentials(cassandraUsername, cassandraPassword)
  .withSocketOptions(options)
  .build();
```

### <a name="version-4x"></a>Version 4.x

En el caso de la versión 4.x del controlador de Java, establezca la opción para mantener la conexión; para ello, invalide la configuración en `reference.conf` y asegúrese de que la opción para mantener la conexión está [habilitada en el sistema operativo](https://knowledgebase.progress.com/articles/Article/configure-OS-TCP-KEEPALIVE-000080089):

```xml
datastax-java-driver {
  advanced {
    socket{
      keep-alive = true
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las [características admitidas](cassandra-support.md) en Cassandra API de Azure Cosmos DB.
- Obtenga información sobre cómo [migrar de la instancia nativa de Apache Cassandra a Cassandra API de Azure Cosmos DB](cassandra-migrate-cosmos-db-databricks.md).
