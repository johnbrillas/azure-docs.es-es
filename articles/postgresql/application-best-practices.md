---
title: 'Procedimientos recomendados para el desarrollo de aplicaciones: Azure Database for PostgreSQL'
description: Conozca procedimientos recomendados para compilar una aplicación con Azure Database for PostgreSQL.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 12/10/2020
ms.openlocfilehash: 6463f30bc79d937bd5a51a5c8c78fbdd72954b1e
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2020
ms.locfileid: "97364545"
---
# <a name="best-practices-for-building-an-application-with-azure-database-for-postgresql"></a>Procedimientos recomendados para compilar una aplicación con Azure Database for PostgreSQL

Estos son algunos procedimientos recomendados que le van a ayudar a compilar una aplicación lista para la nube mediante Azure Database for PostgreSQL. Estos procedimientos recomendados pueden reducir el tiempo de desarrollo de la aplicación.

## <a name="configuration-of-application-and-database-resources"></a>Configuración de recursos de aplicación y de base de datos

### <a name="keep-the-application-and-database-in-the-same-region"></a>Mantenga la aplicación y la base de datos en la misma región.
Al implementar la aplicación en Azure, asegúrese de que todas las dependencias están en la misma región. La propagación de instancias entre regiones o zonas de disponibilidad crea latencia de red, lo que puede afectar al rendimiento general de la aplicación.

### <a name="keep-your-postgresql-server-secure"></a>Mantenga protegido el servidor PostgreSQL
Configure el servidor PostgreSQL de modo que esté [protegido](./concepts-security.md) y no se pueda acceder a él de forma pública. Use cualquiera de estas opciones para proteger el servidor:
- [Reglas de firewall](./concepts-firewall-rules.md)
- [Redes virtuales](./concepts-data-access-and-security-vnet.md)
- [Azure Private Link](./concepts-data-access-and-security-private-link.md)

Por seguridad, siempre debe conectarse al servidor PostgreSQL a través de SSL y configurar este y la aplicación para que usen TLS 1.2. Vea [Conectividad SSL/TLS](./concepts-ssl-connection-security.md).

### <a name="tune-your-server-parameters"></a>Ajuste de los parámetros del servidor
En el caso de cargas de trabajo con muchas operaciones de lectura, el ajuste de los parámetros `tmp_table_size` y `max_heap_table_size` puede ayudar a optimizar el rendimiento. Para calcular los valores necesarios para estas variables, examine los valores de memoria total por conexión y la memoria base. La suma de los parámetros de memoria por conexión, excluido `tmp_table_size`, junto con la memoria base, da como resultado la memoria total del servidor.

### <a name="use-environment-variables-for-connection-information"></a>Use variables de entorno para la información de conexión
No guarde las credenciales de base de datos en el código de la aplicación. En función de la aplicación front-end, siga las instrucciones para configurar variables de entorno. En el caso de App Service, vea [Configuración de aplicaciones](../app-service/configure-common.md#configure-app-settings) y, en el de Azure Kuberentes Service, vea [Uso de secretos de Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/).

## <a name="performance-and-resiliency"></a>Rendimiento y resistencia
Estas son algunas herramientas y prácticas que puede usar para depurar problemas de rendimiento con la aplicación.

### <a name="use-connection-pooling"></a>Usar agrupación de conexiones
Con la agrupación de conexiones, un conjunto fijo de conexiones se establece en el momento del inicio y se mantiene. Esto también ayuda a reducir la fragmentación de memoria en el servidor causada por las nuevas conexiones dinámicas establecidas en el servidor de bases de datos. La agrupación de conexiones se puede configurar en el lado de la aplicación si el marco de esta o el controlador de base de datos lo admiten. Si no es así, la otra opción recomendada es aprovechar un servicio de agrupación de conexiones proxy como [PgBouncer](https://pgbouncer.github.io/) o [Pgpool](https://pgpool.net/mediawiki/index.php/Main_Page), que se ejecutan fuera de la aplicación y se conectan al servidor de bases de datos. Tanto PgBouncer como Pgpool son herramientas basadas en la comunidad que funcionan con Azure Database for PostgreSQL.

### <a name="retry-logic-to-handle-transient-errors"></a>Lógica de reintento para controlar errores transitorios
La aplicación podría experimentar errores transitorios en los que las conexiones a la base de datos se rechazan o se pierden de manera intermitente. En tales situaciones, el servidor se pone en funcionamiento al cabo de uno o dos reintentos en 5 o 10 segundos. Una buena práctica es esperar 5 segundos antes del primer reintento. Después, aumente gradualmente la espera entre cada reintento, hasta 60 segundos. Para investigar el problema más a fondo, limite el número máximo de reintentos en cuyo momento la aplicación considera que se produjo un error en la operación. Consulte [Solución de problemas de conexión](./concepts-connectivity.md) para más información.

### <a name="enable-read-replication-to-mitigate-failovers"></a>Habilitar la replicación de lectura para mitigar las conmutaciones por error
Se puede usar la [Replicación de datos de entrada](./concepts-read-replicas.md) en los escenarios de conmutación por error. Cuando se usan réplicas de lectura, no se produce conmutación por error automatizada entre los servidores de origen y réplica. Observará un retraso entre el origen y la réplica porque la replicación es asincrónica. El retraso de red puede verse afectado por muchos factores, por ejemplo, el tamaño de la carga de trabajo que se ejecuta en el servidor de origen y la latencia entre los centros de datos. En la mayoría de los casos, el retraso de la réplica oscila entre unos segundos y un par de minutos.


## <a name="database-deployment"></a>Implementación de bases de datos

### <a name="configure-cicd-deployment-pipeline"></a>Configure la canalización de implementación de CI/CD
En ocasiones, es necesario implementar cambios en la base de datos. En estos casos, puede usar la integración continua (CI) por medio de [acciones de GitHub](https://github.com/Azure/postgresql/blob/master/README.md) para que el servidor PostgreSQL actualice la base de datos mediante la ejecución de un script personalizado en ella.

### <a name="define-manual-database-deployment-process"></a>Defina un proceso de implementación manual de bases de datos
Durante la implementación manual de bases de datos, siga estos pasos para minimizar el tiempo de inactividad o reducir el riesgo de implementaciones con errores:

- Cree una copia de una base de datos de producción en una nueva base de datos mediante pg_dump.
- Actualice la nueva base de datos con los nuevos cambios de esquema o las actualizaciones necesarias de la base de datos.
- Ponga la base de datos de producción en estado de solo lectura. No debe tener operaciones de escritura en la base de datos de producción hasta que se complete la implementación.
- Pruebe la aplicación con la base de datos recién actualizada del paso 1.
- Implemente los cambios de la aplicación y asegúrese de que la aplicación usa ahora la nueva base de datos que tiene las actualizaciones más recientes.
- Mantenga la antigua base de datos de producción para poder revertir los cambios. Después, puede valorar eliminar la base de datos de producción antigua o exportarla en Azure Storage, si es necesario.

>  [!NOTE]
> Si la aplicación es como una aplicación de comercio electrónico y no puede ponerla en estado de solo lectura, implemente los cambios directamente en la base de datos de producción después de realizar una copia de seguridad. Estos cambios deben realizarse durante las horas de poca actividad y poco tráfico a la aplicación, para que tengan el menor efecto posible, ya que algunos usuarios pueden experimentar solicitudes con error. Asegúrese de que el código de aplicación también controla las solicitudes con error.

## <a name="database-schema-and-queries"></a>Consultas y esquema de base de datos
Estos son algunos consejos que hay que tener en cuenta al compilar el esquema de base de datos y las consultas.

### <a name="use-bigint-or-uuid-for-primary-keys"></a>Use BIGINT o UUID en las claves principales
Al compilar aplicaciones personalizadas o algunos marcos, puede que usen `INT` en lugar de `BIGINT` en las claves principales. Cuando se usa ```INT```, se asume el riesgo de que el valor de la base de datos pueda superar la capacidad de almacenamiento del tipo de datos ```INT```. La realización de este cambio en una aplicación de producción existente puede llevar mucho tiempo, con más tiempo de desarrollo. Otra opción es usar [UUID](https://www.postgresql.org/docs/current/datatype-uuid.html) en las claves principales. Este identificador emplea una cadena de 128 bits generada automáticamente, por ejemplo, ```a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11```. Obtenga más información sobre los [tipos de datos de PostgreSQL](https://www.postgresql.org/docs/8.1/datatype.html).

### <a name="use-indexes"></a>Uso de índices

Hay muchos tipos de [índices](https://www.postgresql.org/docs/9.1/indexes.html) de Postgres que se pueden usar de maneras diferentes. El uso de un índice ayuda al servidor a buscar y recuperar filas específicas mucho más rápido de lo que lo haría sin un índice. Pero los índices también agregan sobrecarga al servidor de bases de datos, por lo que se evita tener demasiados.

### <a name="use-autovacuum"></a>Use el vaciado automático
Puede optimizar el servidor con el vaciado automático en un servidor Azure Database for PostgreSQL. PostgreSQL permite una mayor simultaneidad de bases de datos, aunque cada actualización da lugar a una operación de inserción y eliminación. En el caso de la eliminación, los registros se marcan temporalmente para purgarse más adelante. Para llevar a cabo estas tareas, PostgreSQL ejecuta un trabajo de vaciado. Si no vacía de vez en cuando, las tuplas inactivas que se acumulan en pueden convertirse en:

- Sobredimensionamiento de datos, como bases de datos y tablas aún más grandes.
- Índices de mayor tamaño poco óptimos.
- Aumento de E/S.

Obtenga más información sobre [cómo optimizar con el vaciado automático](howto-optimize-autovacuum.md).

### <a name="use-pg_stats_statements"></a>Uso de pg_stats_statements
Pg_stat_statements es una extensión de PostgreSQL habilitada de manera predeterminada en Azure Database for PostgreSQL. La extensión proporciona un medio para realizar el seguimiento de las estadísticas de ejecución para todas las instrucciones SQL ejecutadas por un servidor. Vea [cómo usar pg_statement](howto-optimize-query-stats-collection.md).


### <a name="use-the-query-store"></a>Uso del Almacén de consultas
La característica [Almacén de consultas](./concepts-query-store.md) de Azure Database for PostgreSQL proporciona una manera más eficaz de realizar el seguimiento de las estadísticas de consulta. Se recomienda esta característica como alternativa al uso de pg_stats_statements.

### <a name="optimize-bulk-inserts-and-use-transient-data"></a>Optimice las inserciones masivas y use datos transitorios
Si tiene operaciones de carga de trabajo que impliquen datos transitorios o que inserten grandes conjuntos de datos de forma masiva, debe considerar la posibilidad de usar tablas sin registrar. que proporciona atomicidad y durabilidad de forma predeterminada. Las propiedades ACID incluyen la Atomicidad, Coherencia, aIslamiento y Durabilidad. Vea [Optimización de las inserciones masivas](howto-optimize-bulk-inserts.md).

## <a name="next-steps"></a>Pasos siguientes
[Guía de Postgres](http://postgresguide.com/)
