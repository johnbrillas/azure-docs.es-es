---
title: Preguntas más frecuentes sobre diferentes API en Azure Cosmos DB
description: Obtenga respuestas a las preguntas más frecuentes sobre Azure Cosmos DB, un servicio de base de datos con varios modelos distribuido globalmente. Obtenga más información sobre capacidad, niveles de rendimiento y escalado.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 81c959467c4bb3700120917cf150773fb18e866d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97359836"
---
# <a name="frequently-asked-questions-about-different-apis-in-azure-cosmos-db"></a>Preguntas más frecuentes sobre diferentes API en Azure Cosmos DB
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>¿Cuáles son los casos de uso típicos de Azure Cosmos DB?

Azure Cosmos DB es una buena elección para las nuevas aplicaciones web, móviles, de juegos y de IoT donde la escala automática, el rendimiento predecible, la orden rápida de tiempos de respuesta en milisegundos y la capacidad para consultar datos sin esquemas son importantes. Se presta a un desarrollo rápido y admite la iteración continua de modelos de datos de aplicaciones. Las aplicaciones que administran contenido y datos generados por el usuario son [casos de uso comunes de Azure Cosmos DB](use-cases.md).

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>¿Cómo ofrece Azure Cosmos DB un rendimiento predecible?

Una [unidad de solicitud](request-units.md) (RU) es la medida de procesamiento de Azure Cosmos DB. 1 RU corresponde al rendimiento de una solicitud GET de un documento de 1 KB. Todas las operaciones de Azure Cosmos DB, incluidas las lecturas, escrituras, consultas SQL y ejecuciones de procedimiento almacenado, tienen un valor de unidad de solicitud determinista en función del rendimiento necesario para completar la operación. En lugar de pensar en la CPU, las E/S y la memoria, y en cómo cada uno de estos aspectos afecta al rendimiento de la aplicación, es mejor que se centre en una unidad única de unidad de solicitud.

Cada contenedor de Azure Cosmos puede configurarse con rendimiento aprovisionado en términos de unidades de solicitud del rendimiento por segundo. Para las aplicaciones de cualquier escala, puede realizar pruebas comparativas de las solicitudes individuales para medir los valores de unidad de solicitud y aprovisionar un contenedor para administrar el total de unidades de solicitud en todas las solicitudes. También puede escalar o reducir verticalmente el rendimiento del contenedor a medida que evolucionen las necesidades de la aplicación. Para obtener más información sobre las unidades de solicitud y ayuda para determinar las necesidades del contenedor, pruebe la [calculadora de rendimiento](https://www.documentdb.com/capacityplanner).

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>¿Por qué Azure Cosmos DB admite varios modelos de datos, como pares clave-valor, columnas, documentos y gráficos?

Los modelos de datos de pares clave-valor (tabla), columnas, documentos y gráficos se admiten de forma nativa porque Azure Cosmos DB se basa en el diseño de ARS (átomos, registros y secuencias). Los átomos, los registros y las secuencias se pueden asignar y proyectar con facilidad en varios modelos de datos. Ya se encuentran disponibles las API para un subconjunto de modelos (SQL, MongoDB, Table y Gremlin), mientras que otras que son específicas para modelos de datos adicionales estarán disponibles en el futuro.

Azure Cosmos DB tiene un motor de indexación independiente del esquema capaz de indexar automáticamente todos los datos ingeridos sin necesidad de que el desarrollador proporcione índices de esquema o secundarios. El motor se basa en un conjunto de diseños de índices lógicos (invertidos, en columnas y en árbol) que desacopla el diseño de almacenamiento del índice y los subsistemas de procesamiento de consultas. Cosmos DB también tiene la capacidad de admitir un conjunto de API y protocolos de conexión de forma extensible y convertirlos de forma eficaz en el modelo de datos principal (1) y los diseños de índices lógicos (2), lo que le permite admitir con exclusividad más de un modelo de datos de forma nativa.

### <a name="can-i-use-multiple-apis-to-access-my-data"></a>¿Puedo usar varias API para acceder a mis datos?

Azure Cosmos DB es un servicio de base de datos con varios modelos distribuido de forma global de Microsoft. Si varios modelos significa que Azure Cosmos DB admite varias API y varios modelos de datos, las distintas API usan formatos de datos diferentes para el protocolo de almacenamiento y conexión. Por ejemplo, SQL usa JSON, MongoDB usa BSON, Table usa EDM, Cassandra usa CQL y Gremlin usa JSON. Como resultado, se recomienda usar la misma API para todo el acceso a los datos de una cuenta determinada.

Cada API funciona de forma independiente, excepto las API de Gremlin y SQL, que son interoperables.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>¿Es compatible Azure Cosmos DB con HIPAA?

Sí, Azure Cosmos DB es compatible con HIPAA. HIPAA establece los requisitos para el uso, la divulgación y la protección de información de mantenimiento identificable individualmente. Para más información, consulte el [Centro de confianza de Microsoft](/compliance/regulatory/offering-hipaa-hitech).

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>¿Cuáles son los límites de almacenamiento de Azure Cosmos DB?

No existe ningún límite en la cantidad total de datos que puede almacenar un contenedor en Azure Cosmos DB.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>¿Cuáles son los límites de rendimiento de Azure Cosmos DB?

No existe ningún límite en la cantidad total de rendimiento que admite un contenedor en Azure Cosmos DB. La idea clave es distribuir la carga de trabajo de manera más o menos uniforme entre una cantidad suficientemente grande de claves partición.

### <a name="are-direct-and-gateway-connectivity-modes-encrypted"></a>¿Se cifran los modos de conectividad directa y por puerta de enlace?

Sí, ambos modos se cifran siempre totalmente.

### <a name="how-much-does-azure-cosmos-db-cost"></a>¿Cuánto cuesta Azure Cosmos DB?

Para más información, vea la página de [precios de Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). Los cargos por uso de Azure Cosmos DB se determinan según la cantidad de contenedores aprovisionados, el número de horas que los contenedores estuvieron conectados y el rendimiento aprovisionado de cada contenedor.

### <a name="is-a-free-account-available"></a>¿Existe una cuenta gratuita disponible?

Sí, puede registrarse para una cuenta de tiempo limitado sin cargo, con ningún compromiso. Para registrarse, visite [Pruebe gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) o consulte la sección de [preguntas más frecuentes sobre Azure Cosmos DB](#try-cosmos-db).

Si es la primera vez que usa Azure, regístrese para obtener una [cuenta gratuita de Azure](https://azure.microsoft.com/free/), que ofrece 30 días y un crédito para que pruebe todos los servicios de Azure. Si tiene una suscripción a Visual Studio, puede recibir [créditos gratis de Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) para usarlos en cualquier servicio de Azure.

También puede usar el [Emulador de Azure Cosmos DB](local-emulator.md) para desarrollar y probar su aplicación localmente, de forma gratuita, sin necesidad de crear una suscripción de Azure. Cuando esté satisfecho con el funcionamiento de la aplicación en el Emulador, puede cambiar a una cuenta de Azure Cosmos DB en la nube.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>¿Cómo puedo obtener ayuda adicional con Azure Cosmos DB?

Para formular una pregunta técnica, puede realizar una publicación en alguno de estos dos foros de preguntas y respuestas:

* [Página de preguntas y respuestas de Microsoft](/answers/topics/azure-cosmos-db.html)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-cosmosdb). Stack Overflow es la mejor opción para preguntas de programación. Asegúrese de que su pregunta es [relevante](https://stackoverflow.com/help/on-topic) y [proporcione tantos detalles como sea posible, formulando la pregunta de forma clara y responsable](https://stackoverflow.com/help/how-to-ask).

Para solicitar nuevas características, cree una nueva solicitud en [UserVoice](https://feedback.azure.com/forums/263030-azure-cosmos-db).

Para corregir un problema con la cuenta, envíe una [solicitud de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) en Azure Portal.

## <a name="try-azure-cosmos-db-subscriptions"></a><a id="try-cosmos-db"></a>Prueba de las suscripciones de Azure Cosmos DB

Ya puede disfrutar de Azure Cosmos DB durante un tiempo limitado sin una suscripción, y sin que le suponga ningún gasto ni compromiso. Para registrarse en una suscripción de prueba de Azure Cosmos DB, vaya a [Pruebe gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) y use cualquier cuenta de Microsoft (MSA) personal. Esta suscripción es independiente de la [evaluación gratuita de Azure](https://azure.microsoft.com/free/) y puede utilizarse junto con una evaluación gratuita o una suscripción de pago de Azure.

Las suscripciones de prueba de Azure Cosmos DB aparecen en Azure Portal junto con otras suscripciones asociadas con el identificador de usuario.

Las condiciones siguientes se aplican a las suscripciones de prueba de Azure Cosmos DB:

* Se puede conceder acceso a la cuenta a cuentas de Microsoft (MSA) personales. Evite el uso de cuentas de Azure Active Directory (Azure AD) o de cuentas que pertenezcan a inquilinos corporativos de Azure AD, ya que pueden tener aplicadas limitaciones que podrían bloquear la concesión de acceso.
* Un [contenedor aprovisionado con rendimiento](./set-throughput.md#set-throughput-on-a-container) por cada suscripción para las cuentas de SQL, Gremlin API y Table.
* Hasta tres [colecciones aprovisionadas con rendimiento](./set-throughput.md#set-throughput-on-a-container) por suscripción para las cuentas de MongoDB.
* Una [base de datos aprovisionada con rendimiento](./set-throughput.md#set-throughput-on-a-database) por suscripción. Las bases de datos aprovisionadas con rendimiento pueden incluir cualquier número de contenedores.
* 10 GB de capacidad de almacenamiento.
* La replicación global está disponible en las [regiones de Azure](https://azure.microsoft.com/regions/) siguientes: Centro de EE. UU., Norte de Europa y Sudeste de Asia
* Rendimiento máximo de 5000 unidades de solicitud por segundo cuando se aprovisionan en el nivel de contenedor.
* Rendimiento máximo de 20 000 unidades de solicitud por segundo cuando se aprovisionan en el nivel de base de datos.
* Las suscripciones expiran después de 30 días y se pueden extender hasta un máximo de 31 días. Después de la expiración, se elimina la información contenida.
* No se pueden crear incidencias de soporte técnico de Azure para las cuentas de prueba de Azure Cosmos DB; sin embargo, se proporciona soporte técnico para aquellos suscriptores con planes de soporte técnico ya existentes.

## <a name="set-up-azure-cosmos-db"></a>Configuración de Azure Cosmos DB

### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>¿Cómo me registro en Azure Cosmos DB?

Azure Cosmos DB está disponible en Azure Portal. En primer lugar, regístrese para obtener una suscripción de Azure. Una vez que se haya registrado, puede agregar una cuenta de Azure Cosmos DB a la suscripción de Azure.

### <a name="what-is-a-primary-key"></a>¿Qué es una clave principal?

Una clave principal es un token de seguridad para acceder a todos los recursos de una cuenta. Los usuarios con esta clave tienen acceso de lectura y escritura a todos los recursos de la cuenta de base de datos. Tenga cuidado cuando distribuya claves principales. Las claves principales primaria y secundaria están disponibles en la hoja **Claves** de [Azure Portal][azure-portal]. Para más información sobre las claves, vea [Visualización, copia y regeneración de las claves de acceso](manage-with-cli.md#list-account-keys).

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>¿En qué regiones se puede establecer PreferredLocations?

El valor PreferredLocations se puede establecer en cualquiera de las regiones de Azure en las que Cosmos DB está disponible. Para obtener una lista de las regiones disponibles, vea [Regiones de Azure](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>¿Hay algo que deba tener en cuenta al distribuir datos en todo el mundo a través de centros de datos de Azure?

Azure Cosmos DB se ofrece en todas las regiones de Azure, tal y como se especifica en la página [Regiones de Azure](https://azure.microsoft.com/regions/). Dado que se trata del servicio principal, cada nuevo centro de datos está presente en Azure Cosmos DB.

Al establecer una región, debe recordar que Cosmos DB respeta las nubes independientes y de administración pública. Es decir, si crea una cuenta en una [región soberana](https://azure.microsoft.com/global-infrastructure/), no puede replicarla fuera de dicha [región](https://azure.microsoft.com/global-infrastructure/). De manera similar, tampoco puede habilitar la replicación en otras ubicaciones soberanas desde una cuenta externa.

### <a name="is-it-possible-to-switch-from-container-level-throughput-provisioning-to-database-level-throughput-provisioning-or-vice-versa"></a>¿Es posible pasar del aprovisionamiento de rendimiento del nivel de contenedor al aprovisionamiento de rendimiento del nivel de base de datos? O viceversa

El aprovisionamiento del rendimiento de nivel de base de datos y de contenedor son ofertas diferentes y cambiar entre cualquiera de estos requiera migrar datos de origen a destino. Lo que significa que deberá crear una nueva base de datos o un contenedor nuevo y luego migrar los datos mediante el uso de [biblioteca de ejecutor en masa](bulk-executor-overview.md) o [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md).

### <a name="does-azure-cosmosdb-support-time-series-analysis"></a>¿Azure Cosmos DB admite el análisis de series temporales?

Sí, Azure Cosmos DB admite el análisis de series temporales, este es un ejemplo de un [patrón de serie temporal](https://github.com/Azure/azure-cosmosdb-dotnet/tree/master/samples/Patterns). En este ejemplo se muestra cómo usar la fuente de cambios para generar vistas agregadas con los datos de las series temporales. Puede ampliar este enfoque mediante el uso de streaming de Spark u otro procesador de datos de streaming.

## <a name="what-are-the-azure-cosmos-db-service-quotas-and-throughput-limits"></a>¿Cuáles son las cuotas de servicio de Azure Cosmos DB y los límites de rendimiento?

Consulte los artículos de Azure Cosmos DB sobre [cuotas de servicio](concepts-limits.md) y [límites de rendimiento por contenedor y base de datos](set-throughput.md#comparison-of-models) para más información.

## <a name="frequently-asked-questions-about-sql-api"></a><a id="sql-api-faq"></a>Preguntas más frecuentes sobre SQL API

### <a name="how-do-i-start-developing-against-the-sql-api"></a>¿Cómo se empieza a desarrollar con SQL API?

En primer lugar, debe registrarse para obtener una suscripción de Azure. Una vez registrado para obtener una suscripción de Azure, puede agregar un contenedor de SQL API a la suscripción de Azure. Para obtener instrucciones sobre cómo agregar una cuenta de Azure Cosmos DB, vea [Crear una cuenta de Azure Cosmos](create-sql-api-dotnet.md#create-account).

[SDK](sql-api-sdk-dotnet.md) están disponibles para NET, Python, Node.js, JavaScript y Java. Los desarrolladores también pueden usar las [API de HTTP RESTful](/rest/api/cosmos-db/) para interactuar con recursos de Azure Cosmos DB en varias plataformas y lenguajes.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>¿Se puede acceder a algunos ejemplos disponibles que me puedan ayudar a empezar?

En GitHub se encuentran disponibles ejemplos de los SDK de [.NET](sql-api-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](sql-api-nodejs-samples.md) y [Python](sql-api-python-samples.md) de SQL API.

### <a name="does-the-sql-api-database-support-schema-free-data"></a>¿Las bases de datos de SQL API admiten datos sin esquemas?

Sí, SQL API permite que las aplicaciones almacenen documentos JSON arbitrarios sin definiciones ni sugerencias de esquemas. Los datos están disponibles inmediatamente para consulta a través de la interfaz de consulta SQL de Azure Cosmos DB.

### <a name="does-the-sql-api-support-acid-transactions"></a>¿SQL API admite transacciones ACID?

Sí, SQL API admite transacciones entre documentos expresadas como procedimientos almacenados y desencadenadores JavaScript. Las transacciones se limitan a una única partición dentro de cada contenedor y se ejecutan con semántica ACID, como "todo o nada", aisladas de otras solicitudes de usuario y código que se ejecutan a la vez. Si se producen excepciones por la ejecución en el servidor de código de aplicación JavaScript, la transacción entera se revierte. 

### <a name="what-is-a-container"></a>¿Qué es un contenedor?

Un contenedor es un grupo de documentos y su lógica de aplicación de JavaScript asociada. Un contenedor es una entidad facturable, en la que el [costo](performance-levels.md) se determina en función del rendimiento y del almacenamiento utilizado. Los contenedores pueden abarcar una o varias particiones o uno o varios servidores y se pueden escalar para administrar volúmenes prácticamente ilimitados de almacenamiento o rendimiento.

* En el caso de la API de SQL, el recurso se denomina contenedor.
* En el caso de la API de Cosmos DB para las cuentas de MongoDB, un contenedor se asigna a un objeto Collection.
* Para las cuentas de Cassandra y Table API, se asigna un contenedor a una tabla.
* Para las cuentas de API de Gremlin, se asigna un contenedor a un gráfico.

Los contenedores también son las entidades de facturación de Azure Cosmos DB. Cada contenedor se factura por hora según el rendimiento aprovisionado y el espacio de almacenamiento usado. Para más información, vea los [precios de Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).

### <a name="how-do-i-create-a-database"></a>¿Cómo se crea una base de datos?

Puede crear bases de datos desde [Azure Portal](https://portal.azure.com), como se describe en [Adición de un contenedor](create-sql-api-java.md#add-a-container), uno de los [SDK de Azure Cosmos DB](sql-api-sdk-dotnet.md) o las [API REST](/rest/api/cosmos-db/).

### <a name="how-do-i-set-up-users-and-permissions"></a>¿Cómo se configuran los usuarios y los permisos?

Puede crear usuarios y permisos mediante uno de los [SDK de la API de Cosmos DB](sql-api-sdk-dotnet.md) o de las [API de REST](/rest/api/cosmos-db/).

### <a name="does-the-sql-api-support-sql"></a>¿SQL API admite SQL?

El lenguaje de consulta SQL que admiten las cuentas de SQL API es un subconjunto mejorado de la funcionalidad de consulta admitida por SQL. El lenguaje de consulta SQL de Azure Cosmos DB ofrece complejos operadores relacionales y jerárquicos y extensibilidad a través de funciones definidas por el usuario (UDF) basadas en JavaScript. La sintaxis JSON permite el modelado de documentos JSON como árboles con nodos etiquetados, algo que usan las técnicas de indexación automática de Azure Cosmos DB y el dialecto de consulta SQL de Azure Cosmos DB. Para obtener información sobre el uso de la gramática de SQL, vea el artículo [SQL Query][query] (Consulta SQL).

### <a name="does-the-sql-api-support-sql-aggregation-functions"></a>¿SQL API admite las funciones de agregación de SQL?

SQL API admite la agregación de baja latencia a cualquier escala a través de las funciones de agregado `COUNT`, `MIN`, `MAX`, `AVG` y `SUM` a través de la gramática de SQL. Para más información, consulte [Funciones de agregado](sql-query-aggregate-functions.md).

### <a name="how-does-the-sql-api-provide-concurrency"></a>¿De qué modo proporciona simultaneidad SQL API?

SQL API admite el control de simultaneidad optimista (OCC) a través de etiquetas de entidad HTTP o ETag. Cada recurso de SQL API tiene una ETag, que se establece en el servidor cada vez que se actualiza un documento. El encabezado de la ETag y el valor actual se incluyen en todos los mensajes de respuesta. Las etiquetas ETag pueden utilizarse con el encabezado If-Match para permitir que el servidor decida si debe actualizarse un recurso. El valor de If-Match es el valor de ETag con el que se va a cotejar. Si el valor de ETag coincide con el valor de ETag del servidor, el recurso se actualiza. Si el valor de ETag ya no es el actual, el servidor rechaza la operación con un código de respuesta "HTTP 412 Precondition failure" (HTTP 412: error de condición previa). Después, el cliente recupera el recurso para obtener el valor de Etag actual del recurso. Además, las etiquetas ETag pueden emplearse con el encabezado If-None-Match para determinar si hay que volver a recuperar un recurso.

Para usar la simultaneidad optimista en .NET, utilice la clase [AccessCondition](/dotnet/api/microsoft.azure.documents.client.accesscondition) . Para ver un ejemplo de .NET, vea la sección [Program.cs](https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/samples/code-samples/DocumentManagement/Program.cs) en el ejemplo del artículo DocumentManagement de GitHub.

### <a name="how-do-i-perform-transactions-in-the-sql-api"></a>¿Cómo se realizan transacciones en SQL API?

SQL API admite transacciones integradas en el lenguaje a través de procedimientos almacenados y desencadenadores JavaScript. Todas las operaciones de base de datos contenidas en scripts se ejecutan con aislamiento de instantánea. Si se trata de una contenedor con una sola partición, el ámbito de ejecución es el contenedor. Si el contenedor tiene varias particiones, el ámbito de la ejecución son documentos con el mismo valor de clave de partición que hay dentro del contenedor. Una instantánea de las versiones de documento (etiquetas ETag) se toma al comienzo de la transacción y solo se confirma si el script se ejecuta correctamente. Si el JavaScript produce un error, la transacción se revierte. Para más información, vea [Programación de JavaScript en el lado del servidor de Azure Cosmos DB](stored-procedures-triggers-udfs.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>¿Cómo se pueden insertar documentos de forma masiva en Cosmos DB?

Hay dos maneras de insertar documentos de forma masiva en Azure Cosmos DB:

* La herramienta de ejecutor en masa, como se describe en [Uso de la biblioteca de BulkExecutor en .NET](bulk-executor-dot-net.md) y [Uso de la biblioteca de BulkExecutor en Java](bulk-executor-java.md).
* La herramienta de migración de datos, como se describe en [Herramienta de migración de bases de datos de Azure Cosmos DB](import-data.md).
* Procedimientos almacenados, como se describe en [Programación de JavaScript en el lado del servidor de Azure Cosmos DB](stored-procedures-triggers-udfs.md).

### <a name="does-the-sql-api-support-resource-link-caching"></a>¿SQL API admite el almacenamiento en caché de vínculos de recursos?

Sí. Como Azure Cosmos DB es un servicio RESTful, los vínculos de recursos son inmutables y se pueden almacenar en caché. Los clientes de SQL API pueden especificar un encabezado "If-None-Match" para lecturas en cualquier documento o contenedor de tipo recurso y actualizar sus copias locales cuando la versión del servidor cambie.

### <a name="is-a-local-instance-of-sql-api-available"></a>¿Hay disponible una instancia local de SQL API?

Sí. El [Emulador de Azure Cosmos DB](local-emulator.md) proporciona una emulación de gran fidelidad del servicio Cosmos DB. Admite una funcionalidad que es idéntica a la de Azure Cosmos DB, incluida la compatibilidad con la creación y la consulta de documentos JSON, el aprovisionamiento y el escalado de colecciones y la ejecución de desencadenadores y procedimientos almacenados. Puede desarrollar y probar aplicaciones mediante el Emulador de Azure Cosmos DB e implementarlas en Azure a escala global con tan solo un cambio de configuración en el punto de conexión de la conexión de Azure Cosmos DB.

### <a name="why-are-long-floating-point-values-in-a-document-rounded-when-viewed-from-data-explorer-in-the-portal"></a>¿Por qué se redondean los valores de punto flotante largos cuando se ven desde el explorador de datos en el portal?

Se trata de una limitación de JavaScript. JavaScript usa números de formato de punto flotante de doble precisión como se especifica en IEEE 754 y solo puede retener de forma segura números entre -(2<sup>53</sup> - 1) y 2<sup>53</sup>-1 (es decir, 9007199254740991).

### <a name="where-are-permissions-allowed-in-the-object-hierarchy"></a>¿Donde se permiten los permisos en la jerarquía de objetos?

La creación de permisos mediante ResourceTokens se permite tanto en el nivel de contenedor como en sus descendientes (por ejemplo, documentos, datos adjuntos). Esto implica que la creación de un permiso en el nivel de base de datos o de cuenta no está permitido actualmente.

[azure-portal]: https://portal.azure.com
[query]: ./sql-query-getting-started.md

## <a name="next-steps"></a>Pasos siguientes

Para ver las preguntas más frecuentes sobre otras API, consulte:

* Preguntas más frecuentes sobre la [API de Azure Cosmos DB para MongoDB](mongodb-api-faq.md)
* Preguntas más frecuentes sobre la [API de Gremlin en Azure Cosmos DB](gremlin-api-faq.md)
* Preguntas más frecuentes sobre [Cassandra API en Azure Cosmos DB](cassandra-faq.md)
* Preguntas más frecuentes acerca de [Table API en Azure Cosmos DB](table-api-faq.md)