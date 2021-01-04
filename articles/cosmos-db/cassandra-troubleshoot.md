---
title: Solución de errores habituales de Cassandra API de Azure Cosmos DB
description: En este documento se describen las formas de solucionar los problemas habituales detectados en Cassandra API de Azure Cosmos DB.
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 12/01/2020
ms.author: thvankra
ms.openlocfilehash: f5f2cb5ac8c354df38310cdcb47b98e1da5b6cfa
ms.sourcegitcommit: 66479d7e55449b78ee587df14babb6321f7d1757
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2020
ms.locfileid: "97517703"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-db-cassandra-api"></a>Solución de problemas habituales de Cassandra API de Azure Cosmos DB
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Cassandra API en Azure Cosmos DB es una capa de compatibilidad, que proporciona [compatibilidad con el protocolo de conexión](cassandra-support.md) para la conocida base de datos de código abierto Apache Cassandra; asimismo, se basa en [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction). Como servicio nativo de nube totalmente administrado, Azure Cosmos DB proporciona y [garantiza la disponibilidad, el rendimiento y la coherencia](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/) de Cassandra API. Tenga en cuenta que estas garantías no son posibles en las implementaciones heredadas de Apache Cassandra. Igualmente, Cassandra API también facilita operaciones de plataforma de mantenimiento cero y una aplicación de revisión sin tiempo de inactividad. Como tal, muchas de sus operaciones de back-end son diferentes de Apache Cassandra, por lo que le recomendamos configuraciones y enfoques determinados para evitar errores comunes. 

En este artículo se describen los errores comunes y sus soluciones para aquellas aplicaciones que usan Cassandra API de Azure Cosmos DB.

## <a name="common-errors-and-solutions"></a>Errores habituales y soluciones

| Error               |  Descripción             | Solución  |
|---------------------|--------------------------|-----------|
| OverloadedException (Java) | El número total de unidades de solicitud consumidas es mayor que las unidades de solicitud aprovisionadas en el espacio o la tabla. Por lo tanto, se limitan las solicitudes. | Considere la posibilidad de escalar el rendimiento asignado a un espacio de claves o una tabla en Azure Portal (consulte [aquí](manage-scale-cassandra.md) las operaciones de escalado en Cassandra API); asimismo, también puede implementar una directiva de reintentos. Para Java, consulte los ejemplos de reintentos para el [controlador V3.x](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) y el [controlador V4.x](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample-v4). Consulte también las [Extensiones de Cassandra de Azure Cosmos para Java](https://github.com/Azure/azure-cosmos-cassandra-extensions). |
| OverloadedException (Java) incluso con suficiente rendimiento | El sistema está realiza solicitudes de limitación a pesar de que se aprovisiona un rendimiento suficiente para el volumen de solicitudes o el costo de las unidades de solicitud consumidas.  | Cassandra API implementa un presupuesto de rendimiento del sistema para las operaciones de nivel de esquema (CREATE TABLE, ALTER TABLE y DROP TABLE). Este presupuesto debe ser suficiente para las operaciones de esquema en un sistema de producción. Sin embargo, si tiene un gran número de operaciones de nivel de esquema, es posible que supere este límite. Como este presupuesto no lo controla el usuario, tendrá que considerar la posibilidad de reducir el número de operaciones de esquema que se estén ejecutando. Si al realizar esta acción no se resuelve el problema o no es factible para la carga de trabajo, [cree una solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md).|
| ClosedConnectionException (Java) | Después de un período de inactividad al realizar las conexiones correctas, la aplicación no se puede conectar.| Este error puede deberse al tiempo de espera de inactividad de Azure LoadBalancers, que es de 4 minutos. Establezca la configuración para mantener la conexión en el controlador (consulte la información que tiene a continuación) y aumente la configuración para mantener la conexión en el sistema operativo; por otro lado, también puede [ajustar el tiempo de espera de inactividad en Azure Load Balancer](../load-balancer/load-balancer-tcp-idle-timeout.md?tabs=tcp-reset-idle-portal). |
| Otros errores de conectividad intermitentes (Java) | La conexión se interrumpe o se agota el tiempo de espera de forma inesperada. | Los controladores de Apache Cassandra para Java proporcionan dos directivas de reconexión nativa: `ExponentialReconnectionPolicy` y `ConstantReconnectionPolicy`. De manera predeterminada, es `ExponentialReconnectionPolicy`. Sin embargo, para Cassandra API de Azure Cosmos DB, se recomienda `ConstantReconnectionPolicy` con un retraso de 2 segundos. Consulte la [documentación del controlador](https://docs.datastax.com/developer/java-driver/4.9/manual/core/reconnection/) correspondiente al controlador de Java v4.x y haga clic [aquí](https://docs.datastax.com/developer/java-driver/3.7/manual/reconnection/) para obtener instrucciones sobre Java 3.x (consulte también los ejemplos siguientes).|

Si el error no aparece en la lista anterior y se produce un error al ejecutar una [operación admitida en Cassandra API](cassandra-support.md) (este error *no se produce al usar la instancia nativa de Apache Cassandra*), [cree una solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md).

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

