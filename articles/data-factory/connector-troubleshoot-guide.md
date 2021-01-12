---
title: Solución de problemas de conectores en Azure Data Factory
description: Obtenga información acerca de la solución de problemas relacionados con conectores en Azure Data Factory.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 12/30/2020
ms.author: jingwang
ms.reviewer: craigg
ms.custom: has-adal-ref
ms.openlocfilehash: e6591762ed6a7e2b462a209730276f3198d86ae8
ms.sourcegitcommit: 28c93f364c51774e8fbde9afb5aa62f1299e649e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/30/2020
ms.locfileid: "97821475"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>Solución de problemas de conectores en Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se exploran métodos comunes de solución de problemas de conectores en Azure Data Factory.
  
## <a name="azure-blob-storage"></a>Azure Blob Storage

### <a name="error-code-azurebloboperationfailed"></a>Código de error: AzureBlobOperationFailed

- **Mensaje**: `Blob operation Failed. ContainerName: %containerName;, path: %path;.`

- **Causa**: La operación de almacenamiento de blobs tuvo un problema.

- **Recomendación:**  compruebe el error en los detalles. Consulte el documento de ayuda de blobs: https://docs.microsoft.com/rest/api/storageservices/blob-service-error-codes. Póngase en contacto con el equipo de almacenamiento si necesita ayuda.


### <a name="invalid-property-during-copy-activity"></a>Propiedad no válida durante la actividad de copia

- **Mensaje**: `Copy activity <Activity Name> has an invalid "source" property. The source type is not compatible with the dataset <Dataset Name> and its linked service <Linked Service Name>. Please verify your input against.`

- **Causa**: el tipo definido en el conjunto de datos es incoherente con el tipo de origen o receptor definido en la actividad de copia.

- **Solución:** edite la definición JSON del conjunto de datos o la canalización para que los tipos sean coherentes y vuelva a ejecutar la implementación.


## <a name="azure-cosmos-db"></a>Azure Cosmos DB

### <a name="error-message-request-size-is-too-large"></a>Mensaje de error: Request size is too large (El tamaño de la solicitud es demasiado grande)

- **Síntomas**: Al copiar datos en Azure Cosmos DB con el tamaño de lote de escritura predeterminado, se produce el error *"**El tamaño de la solicitud es demasiado grande**"* .

- **Causa**: Cosmos DB limita el tamaño de una única solicitud a 2 MB. La fórmula es Tamaño de la solicitud = tamaño de documento único x tamaño de lote de escritura. Si el tamaño del documento es grande, el comportamiento predeterminado producirá un tamaño de solicitud demasiado grande. Puede ajustar el tamaño del lote de escritura.

- **Solución:** En el receptor de la actividad de copia, reduzca el valor del tamaño del lote de escritura (el valor predeterminado es 10000).

### <a name="error-message-unique-index-constraint-violation"></a>Mensaje de error: Unique index constraint violation (Infracción de restricción de índice único)

- **Síntomas**: Al copiar datos en Cosmos DB, se produce el siguiente error:

    ```
    Message=Partition range id 0 | Failed to import mini-batch. 
    Exception was Message: {"Errors":["Encountered exception while executing function. Exception = Error: {\"Errors\":[\"Unique index constraint violation.\"]}... 
    ```

- **Causa**: Hay dos causas posibles:

    - Si utiliza **Insert** como comportamiento de escritura, este error indica que los datos de origen tienen filas/objetos con el mismo identificador.
    - Si usa **Upsert** como comportamiento de escritura y establece otra clave única en el contenedor, este error significa que los datos de origen tienen filas/objetos con identificadores diferentes pero con el mismo valor para la clave única definida.

- **Solución:** 

    - Para la primera causa, establezca **Upsert** como comportamiento de escritura.
    - Para la segunda causa, asegúrese de que cada documento tiene un valor diferente para la clave única definida.

### <a name="error-message-request-rate-is-large"></a>Mensaje de error: La tasa de solicitudes es grande

- **Síntomas**: Al copiar datos en Cosmos DB, se produce el siguiente error:

    ```
    Type=Microsoft.Azure.Documents.DocumentClientException,
    Message=Message: {"Errors":["Request rate is large"]}
    ```

- **Causa**: Las unidades de solicitud utilizadas son mayores que la RU disponible configurada en Cosmos DB. Obtenga información sobre cómo Cosmos DB calcula la RU [aquí](../cosmos-db/request-units.md#request-unit-considerations).

- **Solución:** Hay dos soluciones:

    - **Aumente la RU del contenedor** a un valor mayor en Cosmos DB, lo que mejorará el rendimiento de la actividad de copia, aunque acarreará un mayor costo en Cosmos DB. 
    - Reduzca **writeBatchSize** a un valor más pequeño (por ejemplo, 1000) y establezca **parallelCopies** en un valor inferior, como 1, lo que hará que el rendimiento de la ejecución de copia sea peor que el actual, pero no acarreará más costos en Cosmos DB.

### <a name="column-missing-in-column-mapping"></a>Falta una columna en la asignación de columnas

- **Síntomas**: Cuando se importa un esquema para Cosmos DB para la asignación de columnas, faltan algunas columnas. 

- **Causa**: ADF infiere el esquema de los 10 primeros documentos de Cosmos DB. Si algunas columnas o propiedades no tienen valor en esos documentos, ADF no las detectará, por lo que no se mostrarán.

- **Solución:** Puede ajustar la consulta como se muestra a continuación para hacer que la columna aparezca en el conjunto de resultados con un valor vacío (suponga que la columna "impossible" falta en los 10 primeros documentos). Como alternativa, puede agregar manualmente la columna para la asignación.

    ```sql
    select c.company, c.category, c.comments, (c.impossible??'') as impossible from c
    ```

### <a name="error-message-the-guidrepresentation-for-the-reader-is-csharplegacy"></a>Mensaje de error: The GuidRepresentation for the reader is CSharpLegacy (El valor de GuidRepresentation del lector es CSharpLegacy)

- **Síntomas**: Al copiar datos desde Cosmos DB MongoAPI/MongoDB con el campo de UUID, se produce el siguiente error:

    ```
    Failed to read data via MongoDB client.,
    Source=Microsoft.DataTransfer.Runtime.MongoDbV2Connector,Type=System.FormatException,
    Message=The GuidRepresentation for the reader is CSharpLegacy which requires the binary sub type to be UuidLegacy not UuidStandard.,Source=MongoDB.Bson,’“,
    ```

- **Causa**: Hay dos maneras de representar el UUID en BSON: UuidStardard y UuidLegacy. De forma predeterminada, UuidLegacy se usa para leer datos. Se producirá un error si los datos de UUID en MongoDB son UuidStandard.

- **Solución:** En la cadena de conexión de MongoDB, agregue la opción "**uuidRepresentation=standard**". Para más información, consulte [Cadena de conexión de MongoDB](connector-mongodb.md#linked-service-properties).
            
## <a name="azure-cosmos-db-sql-api"></a>Azure Cosmos DB (API de SQL)

### <a name="error-code--cosmosdbsqlapioperationfailed"></a>Código de error:  CosmosDbSqlApiOperationFailed

- **Mensaje**: `CosmosDbSqlApi operation Failed. ErrorMessage: %msg;.`

- **Causa**: La operación CosmosDbSqlApi ha encontrado un problema.

- **Recomendación:**  compruebe el error en los detalles. Consulte el [documento de ayuda de CosmosDb](https://docs.microsoft.com/azure/cosmos-db/troubleshoot-dot-net-sdk). Póngase en contacto con el equipo de CosmosDb si necesita ayuda.


## <a name="azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1

### <a name="error-message-the-underlying-connection-was-closed-could-not-establish-trust-relationship-for-the-ssltls-secure-channel"></a>Mensaje de error: Se ha cerrado la conexión subyacente: No se ha podido establecer la relación de confianza para el canal seguro SSL/TLS.

- **Síntomas**: Se produce el siguiente error en la actividad de copia: 

    ```
    Message: ErrorCode = `UserErrorFailedFileOperation`, Error Message = `The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel`.
    ```

- **Causa**: Error de validación de certificado durante el protocolo de enlace TLS.

- **Solución:** Solución alternativa: Use una copia almacenada provisionalmente para omitir la validación TLS para ADLS Gen1. Debe reproducir este problema y recopilar el seguimiento de netmon y, a continuación, implicar al equipo de red en la comprobación de la configuración de red local.

    ![Solución de problemas de ADLS Gen1](./media/connector-troubleshoot-guide/adls-troubleshoot.png)


### <a name="error-message-the-remote-server-returned-an-error-403-forbidden"></a>Mensaje de error: Error en el servidor remoto: (403) Prohibido

- **Síntomas**: Se produce el siguiente error en la actividad de copia: 

    ```
    Message: The remote server returned an error: (403) Forbidden.. 
    Response details: {"RemoteException":{"exception":"AccessControlException""message":"CREATE failed with error 0x83090aa2 (Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.)....
    ```

- **Causa**: Una posible causa es que la entidad de servicio o la identidad administrada que usa no tiene permiso para acceder a una carpeta o un archivo determinados.

- **Solución:** Conceda los permisos correspondientes en todas las carpetas y subcarpetas que necesita copiar. Consulte [este documento](connector-azure-data-lake-store.md#linked-service-properties).

### <a name="error-message-failed-to-get-access-token-by-using-service-principal-adal-error-service_unavailable"></a>Mensaje de error: No se pudo obtener el token de acceso mediante la entidad de servicio. Error de ADAL: service_unavailable

- **Síntomas**: Se produce el siguiente error en la actividad de copia:

    ```
    Failed to get access token by using service principal. 
    ADAL Error: service_unavailable, The remote server returned an error: (503) Server Unavailable.
    ```

- **Causa**: Cuando el servicio de token de seguridad (STS) propiedad de Azure Active Directory no está disponible, es decir, cuando está demasiado ocupado para administrar las solicitudes, devuelve un error HTTP 503. 

- **Solución:** Vuelva a ejecutar la actividad de copia después de unos minutos.


## <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

### <a name="error-code-adlsgen2operationfailed"></a>Código de error: ADLSGen2OperationFailed

- **Mensaje**: `ADLS Gen2 operation failed for: %adlsGen2Message;.%exceptionData;.`

- **Causa**: ADLS Gen2 muestra un error que indica que no se pudo realizar la operación.

- **Recomendación:**  compruebe el mensaje de error detallado de ADLS Gen2. Si se debe a un error transitorio, vuelva a intentarlo. Si necesita más ayuda, póngase en contacto con el Soporte técnico de Azure Storage y proporcione el id. de solicitud en el mensaje de error.

- **Causa**: Cuando el mensaje de error contiene el valor "Forbidden", es posible que la entidad de servicio o la identidad administrada que está usando no tenga permisos suficientes para obtener acceso a ADLS Gen2.

- **Recomendación:**  consulte el documento de ayuda: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication.

- **Causa**: Cuando el mensaje de error contiene el valor "InternalServerError", el error lo devuelve ADLS Gen2.

- **Recomendación:**  Si se debe a un error transitorio, vuelva a intentarlo. Si el problema persiste, póngase en contacto con el Soporte técnico de Azure Storage y proporcione el id. de solicitud en el mensaje de error.

### <a name="request-to-adls-gen2-account-met-timeout-error"></a>La solicitud para la cuenta ADLS Gen2 encontró un error de tiempo de espera agotado

- **Mensaje**: código de error = `UserErrorFailedBlobFSOperation`, mensaje de error = `BlobFS operation failed for: A task was canceled`.

- **Causa**: el problema se debe al error de tiempo de espera del receptor de ADLS Gen2, que suele ocurrir en la máquina del entorno de ejecución de integración autohospedado.

- **Recomendación:** 

    - Coloque la máquina del entorno de ejecución de integración autohospedado y la cuenta de ADLS Gen2 de destino en la misma región, si es posible. Esto puede evitar errores de tiempo de espera aleatorios y mejorar el rendimiento.

    - Compruebe si hay una configuración de red especial como ExpressRoute y asegúrese de que la red tiene suficiente ancho de banda. Se recomienda reducir el valor del número de trabajos simultáneos del entorno de ejecución de integración autohospedado si el ancho de banda total es bajo, lo cual puede evitar la competencia de los recursos de red que se produce entre varios trabajos simultáneos.

    - Use un tamaño de bloque menor para la copia no binaria para mitigar este error de tiempo de espera si el tamaño del archivo es moderado o pequeño. Consulte [Put Block de Blob Storage](https://docs.microsoft.com/rest/api/storageservices/put-block).

       Para especificar el tamaño de bloque personalizado, puede editar la propiedad en el editor .json:
        ```
        "sink": {
            "type": "DelimitedTextSink",
            "storeSettings": {
                "type": "AzureBlobFSWriteSettings",
                "blockSizeInMB": 8
            }
        }
        ```

                  
## <a name="azure-file-storage"></a>Azure File Storage

### <a name="error-code--azurefileoperationfailed"></a>Código de error:  AzureFileOperationFailed

- **Mensaje**: `Azure File operation Failed. Path: %path;. ErrorMessage: %msg;.`

- **Causa**: La operación de almacenamiento de Archivo de Azure ha encontrado un problema.

- **Recomendación:**  compruebe el error en los detalles. Consulte el documento de ayuda de Archivo de Azure: https://docs.microsoft.com/rest/api/storageservices/file-service-error-codes. Póngase en contacto con el equipo de almacenamiento si necesita ayuda.


## <a name="azure-synapse-analyticsazure-sql-databasesql-server"></a>Azure Synapse Analytics/Azure SQL Database/SQL Server

### <a name="error-code--sqlfailedtoconnect"></a>Código de error:  SqlFailedToConnect

- **Mensaje**: `Cannot connect to SQL Database: '%server;', Database: '%database;', User: '%user;'. Check the linked service configuration is correct, and make sure the SQL Database firewall allows the integration runtime to access.`

- **Causa**: Azure SQL: Si el mensaje de error contiene "SqlErrorNumber=47073", significa que se deniega el acceso a la red pública en la configuración de conectividad.

- **Recomendación:** En el firewall de Azure SQL, establezca la opción "Denegación del acceso a una red pública" en "No". Puede obtener más información sobre esto https://docs.microsoft.com/azure/azure-sql/database/connectivity-settings#deny-public-network-access.

- **Causa**: Azure SQL: Si el mensaje de error contiene código de error de SQL como "NúmeroDeErrorDeSQL=[código_de_error]", consulte la guía de solución de problemas de Azure SQL.

- **Recomendación:** Obtenga más información en https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-errors-issues.

- **Causa**: Compruebe si el puerto 1433 está en la lista de permitidos del firewall.

- **Recomendación:** Siga con esta documentación de referencia: https://docs.microsoft.com/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access#ports-used-by-.

- **Causa**: si el mensaje de error contiene el valor "SqlException", SQL Database genera el error que indica que se produjo un problema en una operación específica.

- **Recomendación:**  busque en este documento de referencia el código de error de SQL para obtener más detalles: https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors. Si necesita más ayuda, póngase en contacto con el soporte técnico de Azure SQL.

- **Causa**: Si se trata de un problema transitorio (por ejemplo, una conexión de red inestable), agregue reintento en la directiva de actividad para mitigarlo.

- **Recomendación:**  Siga esta documentación de referencia: https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities#activity-policy.

- **Causa**: si el mensaje de error es "El cliente con la dirección IP '...' no tiene permiso para acceder al servidor" y está intentando conectarse a Azure SQL Database, normalmente se debe a un problema con el firewall de Azure SQL Database.

- **Recomendación:**  En la configuración del firewall de Azure SQL Server, habilite la opción "Permitir que los servicios y recursos de Azure tengan acceso a este servidor". Documento de referencia: https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure.


### <a name="error-code--sqloperationfailed"></a>Código de error:  SqlOperationFailed

- **Mensaje**: `A database operation failed. Please search error to get more details.`

- **Causa**: si el mensaje de error contiene el valor "SqlException", SQL Database genera el error que indica que se produjo un problema en una operación específica.

- **Recomendación:**  si el error de SQL no está claro, intente modificar la base de datos al nivel de compatibilidad más reciente de "150". Puede producir errores de SQL de la versión más reciente. Consulte el [documento de detalles](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level#backwardCompat).

    Para resolver problemas relacionados con SQL, busque en este documento de referencia el código de error de SQL para obtener más detalles: https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors. Si necesita más ayuda, póngase en contacto con el soporte técnico de Azure SQL.

- **Causa**: Si el mensaje de error contiene el valor "PdwManagedToNativeInteropException", normalmente se debe a una falta de coincidencia entre los tamaños de columna de origen y receptor.

- **Recomendación:**  compruebe el tamaño de las columnas de origen y receptor. Si necesita más ayuda, póngase en contacto con el soporte técnico de Azure SQL.

- **Causa**: Si el mensaje de error contiene el valor "InvalidOperationException", normalmente se debe a que los datos de entrada no son válidos.

- **Recomendación:**  para saber en qué fila encuentra el problema, habilite la característica de tolerancia a errores en la actividad de copia, que puede redirigir las filas problemáticas al almacenamiento para poder realizar una investigación más detallada. Documento de referencia: https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance.


### <a name="error-code--sqlunauthorizedaccess"></a>Código de error:  SqlUnauthorizedAccess

- **Mensaje**: `Cannot connect to '%connectorName;'. Detail Message: '%message;'`

- **Causa**: la credencial es incorrecta o la cuenta de inicio de sesión no puede acceder a SQL Database.

- **Recomendación:**  compruebe que la cuenta de inicio de sesión tiene permisos suficientes para acceder a SQL Database.


### <a name="error-code--sqlopenconnectiontimeout"></a>Código de error:  SqlOpenConnectionTimeout

- **Mensaje**: `Open connection to database timeout after '%timeoutValue;' seconds.`

- **Causa**: podría ser un error transitorio de SQL Database.

- **Recomendación:**  vuelva a intentar actualizar la cadena de conexión del servicio vinculado mediante un valor de tiempo de espera de conexión mayor.


### <a name="error-code--sqlautocreatetabletypemapfailed"></a>Código de error:  SqlAutoCreateTableTypeMapFailed

- **Mensaje**: `Type '%dataType;' in source side cannot be mapped to a type that supported by sink side(column name:'%columnName;') in autocreate table.`

- **Causa**: La tabla de creación automática no puede cumplir el requisito de origen.

- **Recomendación:**  actualice el tipo de columna en "mappings" o cree manualmente la tabla del receptor en el servidor de destino.


### <a name="error-code--sqldatatypenotsupported"></a>Código de error:  SqlDataTypeNotSupported

- **Mensaje**: `A database operation failed. Check the SQL errors.`

- **Causa**: Si el problema se produce en el origen de SQL y el error está relacionado con el desbordamiento de SqlDateTime, el valor de datos está por encima del intervalo de tipo de lógica (1/1/1753 12:00:00 AM-12/31/9999 11:59:59 PM).

- **Recomendación:**  convierta el tipo en una cadena en la consulta SQL de origen o, en la asignación de columnas de la actividad de copia, cambie el tipo de columna a "String".

- **Causa**: Si el problema se produce en el receptor de SQL y el error está relacionado con el desbordamiento de SqlDateTime, el valor de los datos estará por encima del intervalo permitido en la tabla del receptor.

- **Recomendación:**  Actualice el tipo de columna correspondiente al tipo "datetime2" en la tabla del receptor.


### <a name="error-code--sqlinvaliddbstoredprocedure"></a>Código de error:  SqlInvalidDbStoredProcedure

- **Mensaje**: `The specified Stored Procedure is not valid. It could be caused by that the stored procedure doesn't return any data. Invalid Stored Procedure script: '%scriptName;'.`

- **Causa**: El procedimiento almacenado especificado no es válido. Esto podría deberse a que el procedimiento almacenado no devuelve ningún dato.

- **Recomendación:**  valide el procedimiento almacenado con las herramientas de SQL. Asimismo, asegúrese de que el procedimiento almacenado puede devolver datos.


### <a name="error-code--sqlinvaliddbquerystring"></a>Código de error:  SqlInvalidDbQueryString

- **Mensaje**: `The specified SQL Query is not valid. It could be caused by that the query doesn't return any data. Invalid query: '%query;'`

- **Causa**: La consulta SQL especificada no es válida. Esto podría deberse a que la consulta no devuelve ningún dato.

- **Recomendación:**  Valide la consulta SQL con las herramientas de SQL. Asimismo, asegúrese de que la consulta pueda devolver datos.


### <a name="error-code--sqlinvalidcolumnname"></a>Código de error:  SqlInvalidColumnName

- **Mensaje**: `Column '%column;' does not exist in the table '%tableName;', ServerName: '%serverName;', DatabaseName: '%dbName;'.`

- **Causa**: No se puede encontrar la columna. Es posible que la configuración no sea correcta.

- **Recomendación:**  compruebe la columna de la consulta, el valor "structure" en el conjunto de datos y el valor "mappings" en la actividad.


### <a name="error-code--sqlbatchwritetimeout"></a>Código de error:  SqlBatchWriteTimeout

- **Mensaje**: `Timeouts in SQL write operation.`

- **Causa**: podría ser un error transitorio de SQL Database.

- **Recomendación:**  vuelva a intentarlo. Si el problema persiste, póngase en contacto con el soporte técnico de Azure SQL.


### <a name="error-code--sqlbatchwritetransactionfailed"></a>Código de error:  SqlBatchWriteTransactionFailed

- **Mensaje**: `SQL transaction commits failed`

- **Causa**: si los detalles de la excepción indican constantemente un tiempo de espera de la transacción, la latencia de red entre el entorno de ejecución de integración y la base de datos supera el umbral predeterminado 30 segundos.

- **Recomendación:**  actualice la cadena de conexión del servicio vinculado SQL con el valor de "tiempo de espera de la conexión" igual a 120 o superior y vuelva a ejecutar la actividad.

- **Causa**: si los detalles de la excepción indican de forma intermitente una interrupción de la conexión de SQL, podría tratarse de un error de red transitorio o de un problema de SQL Database.

- **Recomendación:**  vuelva a intentar la actividad y revise las métricas de SQL Database.


### <a name="error-code--sqlbulkcopyinvalidcolumnlength"></a>Código de error:  SqlBulkCopyInvalidColumnLength

- **Mensaje**: `SQL Bulk Copy failed due to receive an invalid column length from the bcp client.`

- **Causa**: error en la copia masiva de SQL, ya que se ha recibido una longitud de columna del cliente bcp que no es válida.

- **Recomendación:**  para saber en qué fila encuentra el problema, habilite la característica de tolerancia a errores en la actividad de copia, que puede redirigir las filas problemáticas al almacenamiento para poder realizar una investigación más detallada. Documento de referencia: https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance.


### <a name="error-code--sqlconnectionisclosed"></a>Código de error:  SqlConnectionIsClosed

- **Mensaje**: `The connection is closed by SQL Database.`

- **Causa**: SQL Database cierra la conexión SQL cuando la ejecución simultánea excesiva y el servidor cierran la conexión.

- **Recomendación:**  el servidor remoto cerró la conexión SQL. Reintentar. Si el problema persiste, póngase en contacto con el soporte técnico de Azure SQL.


### <a name="error-message-conversion-failed-when-converting-from-a-character-string-to-uniqueidentifier"></a>Mensaje de error: Error de conversión al convertir de una cadena de caracteres a uniqueidentifier

- **Síntomas**: al copiar datos desde un origen de datos tabulares (como SQL Server) en Azure Synapse Analytics mediante copias almacenadas provisionalmente y PolyBase, se produce el siguiente error:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into Azure Synapse Analytics.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Conversion failed when converting from a character string to uniqueidentifier...
    ```

- **Causa**: PolyBase de Azure Synapse Analytics no puede convertir una cadena vacía en GUID.

- **Solución:** En el receptor de la actividad de copia, en la configuración de PolyBase, establezca la opción **Use Type default** (Usar tipo predeterminado) en false.


### <a name="error-message-expected-data-type-decimalxx-offending-value"></a>Mensaje de error: Expected data type: DECIMAL(x,x), Offending value (Tipo de datos esperado: DECIMAL(x,x), valor incorrecto)

- **Síntomas**: al copiar datos desde un origen de datos tabulares (como SQL Server) en Azure Synapse Analytics mediante copias almacenadas provisionalmente y PolyBase, se produce el siguiente error:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into Azure Synapse Analytics.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Query aborted-- the maximum reject threshold (0 rows) was reached while reading from an external source: 1 rows rejected out of total 415 rows processed. (/file_name.txt) 
    Column ordinal: 18, Expected data type: DECIMAL(x,x), Offending value:..
    ```

- **Causa**: PolyBase de Azure Synapse Analytics no puede insertar una cadena vacía (valor null) en la columna decimal.

- **Solución:** En el receptor de la actividad de copia, en la configuración de PolyBase, establezca la opción **Use Type default** (Usar tipo predeterminado) en false.


### <a name="error-message-java-exception-message-hdfsbridgecreaterecordreader"></a>Mensaje de error: Mensaje de excepción de Java: HdfsBridge::CreateRecordReader

- **Síntomas**: al copiar datos en Azure Synapse Analytics mediante PolyBase se produce el siguiente error:

    ```
    Message=110802;An internal DMS error occurred that caused this operation to fail. 
    Details: Exception: Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException, 
    Message: Java exception raised on call to HdfsBridge_CreateRecordReader. 
    Java exception message:HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.: Error [HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.] occurred while accessing external file.....
    ```

- **Causa**: La posible causa es que el esquema (ancho total de columna) es demasiado grande (más de 1 MB). Compruebe el esquema de la tabla de Azure Synapse Analytics de destino sumando el tamaño de todas las columnas:

    - Int -> 4 bytes
    - Bigint -> 8 bytes
    - Varchar(n),char(n),binary(n), varbinary(n) -> n bytes
    - Nvarchar(n), nchar(n) -> n*2 bytes
    - Date -> 6 bytes
    - Datetime/(2), smalldatetime -> 16 bytes
    - Datetimeoffset -> 20 bytes
    - Decimal -> 19 bytes
    - Float -> 8 bytes
    - Money -> 8 bytes
    - Smallmoney -> 4 bytes
    - Real -> 4 bytes
    - Smallint -> 2 bytes
    - Time -> 12 bytes
    - Tinyint -> 1 byte

- **Solución:** 
    - Reduzca el ancho de columna a menos de 1 MB.
    - También puede deshabilitar PolyBase para usar un enfoque de inserción masiva.


### <a name="error-message-the-condition-specified-using-http-conditional-headers-is-not-met"></a>Mensaje de error: The condition specified using HTTP conditional header(s) is not met (No se cumple la condición especificada mediante encabezados condicionales HTTP)

- **Síntomas**: al utilizar una consulta SQL para extraer datos de Azure Synapse Analytics, se produce el siguiente error:

    ```
    ...StorageException: The condition specified using HTTP conditional header(s) is not met...
    ```

- **Causa**: se produce un problema en Azure Synapse Analytics al consultar la tabla externa en Azure Storage.

- **Solución:** Ejecute la misma consulta en SSMS y compruebe si ve el mismo resultado. En caso afirmativo, abra una incidencia de soporte técnico en Azure Synapse Analytics y proporcione el nombre del servidor y de la base de datos de Azure Synapse Analytics para solucionar los problemas.
            

### <a name="low-performance-when-load-data-into-azure-sql"></a>Bajo rendimiento al cargar datos en Azure SQL

- **Síntomas**: la copia de datos en Azure SQL es lenta.

- **Causa**: la causa principal del problema suele ser un cuello de botella en el lado de Azure SQL. Las posibles causas son las siguientes:

    - El nivel de la base de datos de Azure no es suficientemente alto.

    - El uso de DTU de la base de datos de Azure está cerca del 100%. Puede [supervisar el rendimiento](https://docs.microsoft.com/azure/azure-sql/database/monitor-tune-overview) y considerar la posibilidad de actualizar el nivel de la base de datos.

    - Los índices no están configurados correctamente. Quite todos los índices antes de la carga de datos y vuelva a crearlos después de completar la carga.

    - WriteBatchSize no es lo suficientemente grande como para ajustarse al tamaño de fila del esquema. Intente aumentar la propiedad para solucionar el problema.

    - En lugar de Bulk insert, se usa el procedimiento almacenado, cuyo rendimiento es previsiblemente menor. 

- **Solución:** Consulte el TSG para obtener el [rendimiento de la actividad de copia](https://docs.microsoft.com/azure/data-factory/copy-activity-performance-troubleshooting).


### <a name="performance-tier-is-low-and-leads-to-copy-failure"></a>El nivel de rendimiento es bajo y genera un error de copia

- **Síntomas**: apareció el siguiente mensaje de error al copiar datos en Azure SQL: `Database operation failed. Error message from database execution : ExecuteNonQuery requires an open and available Connection. The connection's current state is closed.`

- **Causa**: se está utilizando Azure SQL s1, el cual hace que se alcance el límite de E/S en este caso.

- **Solución:** Actualice el nivel de rendimiento de Azure SQL para corregir el problema. 


### <a name="sql-table-cannot-be-found"></a>No se encuentra la tabla SQL. 

- **Síntomas**: se produjo un error al copiar datos de Hybrid en una tabla de SQL Server local: `Cannot find the object "dbo.Contoso" because it does not exist or you do not have permissions.`

- **Causa**: la cuenta de SQL actual no tiene permisos suficientes para ejecutar las solicitudes emitidas por SqlBulkCopy.WriteToServer de .NET.

- **Solución:** Cambie a una cuenta de SQL con más privilegios.


### <a name="error-message-string-or-binary-data-would-be-truncated"></a>Mensaje de error: Los datos binarios o de tipo cadena se truncarían.

- **Síntomas**: se produjo un error al copiar datos en la tabla local o en la tabla de Azure SQL Server: 

- **Causa**: la definición del esquema de la tabla Cx Sql tiene una o más columnas con menos longitud que la esperada.

- **Solución:** Siga estos pasos para solucionar el problema:

    1. Aplique [tolerancia a errores](https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance) al receptor SQL, especialmente "redirectIncompatibleRowSettings" para detectar las filas que tienen el problema.

        > [!NOTE]
        > Tenga en cuenta que es posible que la tolerancia a errores introduzca tiempo de ejecución adicional, lo que puede provocar un costo mayor.

    2. Vuelva a comprobar los datos redirigidos con la longitud de la columna del esquema de la tabla SQL para ver qué columnas deben actualizarse.

    3. Actualice el esquema de tabla como corresponde.


## <a name="azure-table-storage"></a>Azure Table Storage

### <a name="error-code--azuretableduplicatecolumnsfromsource"></a>Código de error:  AzureTableDuplicateColumnsFromSource

- **Mensaje**: `Duplicate columns with same name '%name;' are detected from source. This is NOT supported by Azure Table Storage sink`

- **Causa**: Podría ser común para consultas SQL con join, o bien archivos CSV no estructurados.

- **Recomendación**: Compruebe las columnas de origen y corrija en consecuencia.


## <a name="db2"></a>DB2

### <a name="error-code--db2driverrunfailed"></a>Código de error:  DB2DriverRunFailed

- **Mensaje**: `Error thrown from driver. Sql code: '%code;'`

- **Causa**: Si el mensaje de error contiene "SQLSTATE=51002 SQLCODE=-805", consulte la sugerencia de este documento: https://docs.microsoft.com/azure/data-factory/connector-db2#linked-service-properties

- **Recomendación:**  Intente establecer "NULLID" en la propiedad "packageCollection".


## <a name="delimited-text-format"></a>Formato de texto delimitado

### <a name="error-code--delimitedtextcolumnnamenotallownull"></a>Código de error:  DelimitedTextColumnNameNotAllowNull

- **Mensaje**: `The name of column index %index; is empty. Make sure column name is properly specified in the header row.`

- **Causa**: cuando establezca "firstRowAsHeader" en la actividad, se usará la primera fila como nombre de columna. Este error significa que la primera fila contiene un valor vacío. Por ejemplo: "ColumnA, ColumnB".

- **Recomendación:**  compruebe la primera fila y corrija el valor si está vacío.


### <a name="error-code--delimitedtextmorecolumnsthandefined"></a>Código de error:  DelimitedTextMoreColumnsThanDefined

- **Mensaje**: `Error found when processing '%function;' source '%name;' with row number %rowCount;: found more columns than expected column count: %expectedColumnCount;.`

- **Causa**: el número de columnas de la fila problemática es mayor que el recuento de columnas de la primera fila. Puede deberse a un problema de datos o a una configuración incorrecta de los caracteres de la oferta o el delimitador de columna.

- **Recomendación:**  obtenga el número de filas en el mensaje de error, compruebe la columna de la fila y corrija los datos.

- **Causa**: Si el número de columnas esperado es "1" en el mensaje de error, es posible que haya especificado una configuración de compresión o de formato incorrecta. Por lo tanto, ADF analizó los archivos de forma incorrecta.

- **Recomendación:**  compruebe la configuración de formato para asegurarse de que coincide con los archivos de origen.

- **Causa**: si el origen es una carpeta, es posible que los archivos de la carpeta especificada tengan un esquema diferente.

- **Recomendación:**  asegúrese de que los archivos de la carpeta especificada tienen un esquema idéntico.


## <a name="dynamics-365common-data-servicedynamics-crm"></a>Dynamics 365, Common Data Service, Dynamics CRM

### <a name="error-code--dynamicscreateserviceclienterror"></a>Código de error:  DynamicsCreateServiceClientError

- **Mensaje**: `This is a transient issue on dynamics server side. Try to rerun the pipeline.`

- **Causa**: se trata de un problema transitorio en el servidor de Dynamics.

- **Recomendación:**  vuelva a ejecutar la canalización. Si sigue sin funcionar, intente reducir el paralelismo. Si esto no funciona, póngase en contacto con el soporte técnico de Dynamics.


### <a name="columns-are-missing-when-previewingimporting-schema"></a>Faltan columnas al obtener una vista previa o importar el esquema

- **Síntomas**: faltan algunas de las columnas al importar el esquema u obtener una vista previa de los datos. Mensaje de error: `The valid structure information (column name and type) are required for Dynamics source.`

- **Causa**: este problema se produce básicamente por diseño, ya que ADF no puede mostrar columnas que no tengan ningún valor en los 10 primeros registros. Asegúrese de que las columnas que ha agregado tengan el formato correcto. 

- **Recomendación:** agregue manualmente las columnas en la pestaña Asignación.


### <a name="error-code--dynamicsmissingtargetformultitargetlookupfield"></a>Código de error:  DynamicsMissingTargetForMultiTargetLookupField

- **Mensaje**: `Cannot find the target column for multi-target lookup field: '%fieldName;'.`

- **Causa**: La columna de destino no existe en el origen o en la asignación de columnas.

- **Recomendación:**  1. Asegúrese de que el origen contiene la columna de destino. 2. Agregue la columna de destino en la asignación de columnas. Asegúrese de que la columna de receptor tiene el patrón "{nombreDeCampo}@EntityReference".


### <a name="error-code--dynamicsinvalidtargetformultitargetlookupfield"></a>Código de error:  DynamicsInvalidTargetForMultiTargetLookupField

- **Mensaje**: `The provided target: '%targetName;' is not a valid target of field: '%fieldName;'. Valid targets are: '%validTargetNames;"`

- **Causa**: Se ha proporcionado un nombre de entidad incorrecto como entidad de destino de un campo de búsqueda de varios destinos.

- **Recomendación:**  Proporcione un nombre de entidad válido para el campo de búsqueda de varios destinos.


### <a name="error-code--dynamicsinvalidtypeformultitargetlookupfield"></a>Código de error:  DynamicsInvalidTypeForMultiTargetLookupField

- **Mensaje**: `The provided target type is not a valid string. Field: '%fieldName;'.`

- **Causa**: El valor de la columna de destino no es una cadena.

- **Recomendación:**  Proporcione una cadena válida en la columna de destino de búsqueda de varios destinos.


### <a name="error-code--dynamicsfailedtorequetserver"></a>Código de error:  DynamicsFailedToRequetServer

- **Mensaje**: `The dynamics server or the network is experiencing issues. Check network connectivity or check dynamics server log for more details.`

- **Causa**: El servidor de Dynamics es inestable o no es accesible, o bien en la red se experimentan problemas.

- **Recomendación:**  Compruebe la conectividad de red o compruebe el registro del servidor de Dynamics para obtener más detalles. Póngase en contacto con el soporte técnico de Dynamics para obtener más ayuda.


## <a name="excel-format"></a>Formato Excel

### <a name="timeout-or-slow-performance-when-parsing-large-excel-file"></a>Tiempo de espera o rendimiento lento al analizar un archivo de Excel de gran tamaño

- **Síntomas**:

    - Al crear un conjunto de datos de Excel e importar el esquema de la conexión o el almacenamiento, obtener una vista previa de los datos, enumerar o actualizar las hojas de cálculo, es posible que se produzca un error de tiempo de espera si el tamaño del archivo de Excel es grande.

    - Cuando use la actividad de copia para copiar datos de un archivo de Excel de gran tamaño (> = 100 MB) en otro almacén de datos, es posible que experimente un rendimiento lento o un problema de memoria insuficiente.

- **Causa**: 

    - En operaciones como la importación de esquemas, la vista previa de datos y la enumeración de hojas de cálculo en un conjunto de datos de Excel, el tiempo de espera es de 100 s y es estático. Para un archivo de Excel de gran tamaño, es posible que estas operaciones no finalicen dentro del valor del tiempo de espera.

    - La actividad de copia de ADF lee el archivo de Excel completo en la memoria y, luego, busca la hoja de cálculo y las celdas especificadas para leer los datos. Este comportamiento se debe a que usa ADF del SDK subyacente.

- **Solución:** 

    - Para importar el esquema, puede generar un archivo de ejemplo más pequeño que sea un subconjunto del archivo original y elegir "Importar esquema de archivo de ejemplo" en lugar de "Importar esquema desde conexión/almacén".

    - Para mostrar una hoja de cálculo, en la lista desplegable de hoja de cálculo, puede hacer clic en "Editar" e introducir en su lugar el nombre/índice de la hoja.

    - Para copiar un archivo de Excel de gran tamaño (> 100 MB) en otro almacén, puede usar el origen de Excel de Data Flow cuyo streaming de Sport lee y funciona mejor.
    

## <a name="ftp"></a>FTP

### <a name="error-code--ftpfailedtoconnecttoftpserver"></a>Código de error:  FtpFailedToConnectToFtpServer

- **Mensaje**: `Failed to connect to FTP server. Please make sure the provided server informantion is correct, and try again.`

- **Causa**: Es posible que se haya usado el tipo de servicio vinculado incorrecto para el servidor FTP como, por ejemplo, cuando se utiliza el servicio vinculado de SFTP para conectarse a un servidor FTP.

- **Recomendación:**  Compruebe el puerto del servidor de destino. De forma predeterminada, FTP usa el puerto 21.


## <a name="http"></a>Http

### <a name="error-code--httpfilefailedtoread"></a>Código de error:  HttpFileFailedToRead

- **Mensaje**: `Failed to read data from http server. Check the error from http server：%message;`

- **Causa**: Este error se produce cuando Azure Data Factory se comunica con el servidor HTTP, pero se produce un error en la operación de solicitud HTTP.

- **Recomendación:**  Compruebe el código de estado HTTP o el mensaje en el mensaje de error y solucione el problema del servidor remoto.


## <a name="oracle"></a>Oracle

### <a name="error-code-argumentoutofrangeexception"></a>Código de error: ArgumentOutOfRangeException

- **Mensaje**: `Hour, Minute, and Second parameters describe an un-representable DateTime.`

- **Causa**: en ADF, los valores de fecha y hora admitidos se encuentran en el intervalo de 0001-01-01 00:00:00 a 9999-12-31 23:59:59. Sin embargo, Oracle admite un intervalo más amplio de valores de fecha y hora (como siglos AC o min/seg > 59), lo que da lugar a un error en ADF.

- **Recomendación:** 

    Ejecute `select dump(<column name>)` para comprobar si el valor de Oracle se encuentra dentro del rango de ADF. 

    Si desea conocer la secuencia de bytes del resultado, consulte https://stackoverflow.com/questions/13568193/how-are-dates-stored-in-oracle.


## <a name="orc-format"></a>Formato ORC

### <a name="error-code--orcjavainvocationexception"></a>Código de error:  OrcJavaInvocationException

- **Mensaje**: `An error occurred when invoking java, message: %javaException;.`

- **Causa**: si el mensaje de error contiene "java.lang.OutOfMemory", "Java heap space" y "doubleCapacity", suele ser un problema de administración de memoria en la versión anterior de Integration Runtime.

- **Recomendación:**  Si usa el entorno de ejecución de integración autohospedado, sugiera actualizar a la versión más reciente.

- **Causa**: si el mensaje de error contiene " java.lang.OutOfMemory", Integration Runtime no tiene suficientes recursos para procesar los archivos.

- **Recomendación:**  limite las ejecuciones simultáneas en Integration Runtime. En el caso de Integration Runtime autohospedado, escale verticalmente a una máquina eficaz con una memoria igual o superior a 8 GB.

- **Causa**: si el mensaje de error contiene "NullPointerReference", es posible que se trata de un error transitorio.

- **Recomendación:**  vuelva a intentarlo. Si el problema persiste, póngase en contacto con el soporte técnico.

- **Causa**: Si el mensaje de error contiene "BufferOverflowException", es posible que sea un error transitorio.

- **Recomendación:**  vuelva a intentarlo. Si el problema persiste, póngase en contacto con el soporte técnico.

- **Causa**: Cuando el mensaje de error contiene "java.lang.ClassCastException:org.apache.hadoop.hive.serde2.io.HiveCharWritable no se puede convertir a org.apache.hadoop.io.Text", este problema de conversión de tipos es interno de Java Runtime. Normalmente, se debe a que los datos de origen no se pueden administrar correctamente en Java Runtime.

- **Recomendación:**  Se trata de un problema de datos. Intente usar una cadena en lugar de char/varchar en los datos de formato ORC.

### <a name="error-code--orcdatetimeexceedlimit"></a>Código de error:  OrcDateTimeExceedLimit

- **Mensaje**: `The Ticks value '%ticks;' for the datetime column must be between valid datetime ticks range -621355968000000000 and 2534022144000000000.`

- **Causa**: Si el valor datetime es "0001-01-01 00:00:00"', podría deberse a la diferencia entre el calendario Juliano y el calendario Gregoriano. https://en.wikipedia.org/wiki/Proleptic_Gregorian_calendar#Difference_between_Julian_and_proleptic_Gregorian_calendar_dates.

- **Recomendación:**  Compruebe el valor de tics y evite usar el valor datetime "0001-01-01 00:00:00".


## <a name="parquet-format"></a>Formato Parquet

### <a name="error-code--parquetjavainvocationexception"></a>Código de error:  ParquetJavaInvocationException

- **Mensaje**: `An error occurred when invoking java, message: %javaException;.`

- **Causa**: si el mensaje de error contiene "java.lang.OutOfMemory", "Java heap space" y "doubleCapacity", suele ser un problema de administración de memoria en la versión anterior de Integration Runtime.

- **Recomendación:**  si usa Integration Runtime autohospedado y la versión es anterior a 3.20.7159.1, se sugiere actualizar a la versión más reciente.

- **Causa**: si el mensaje de error contiene " java.lang.OutOfMemory", Integration Runtime no tiene suficientes recursos para procesar los archivos.

- **Recomendación:**  limite las ejecuciones simultáneas en Integration Runtime. En el caso de Integration Runtime autohospedado, escale verticalmente a una máquina eficaz con una memoria igual o superior a 8 GB.

- **Causa**: si el mensaje de error contiene "NullPointerReference", es posible que se trata de un error transitorio.

- **Recomendación:**  vuelva a intentarlo. Si el problema persiste, póngase en contacto con el soporte técnico.


### <a name="error-code--parquetinvalidfile"></a>Código de error:  ParquetInvalidFile

- **Mensaje**: `File is not a valid Parquet file.`

- **Causa**: problema del archivo Parquet.

- **Recomendación:**  compruebe que la entrada es un archivo Parquet válido.


### <a name="error-code--parquetnotsupportedtype"></a>Código de error:  ParquetNotSupportedType

- **Mensaje**: `Unsupported Parquet type. PrimitiveType: %primitiveType; OriginalType: %originalType;.`

- **Causa**: el formato Parquet no se admite en Azure Data Factory.

- **Recomendación:**  compruebe los datos de origen. Consulte el documento: https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs.


### <a name="error-code--parquetmisseddecimalprecisionscale"></a>Código de error:  ParquetMissedDecimalPrecisionScale

- **Mensaje**: `Decimal Precision or Scale information is not found in schema for column: %column;.`

- **Causa**: intenta analizar la precisión y la escala del número, pero no se proporciona esa información.

- **Recomendación:**  "Source" no devuelve la precisión y la escala correctas. Compruebe la precisión y la escala de la columna problemática.


### <a name="error-code--parquetinvaliddecimalprecisionscale"></a>Código de error:  ParquetInvalidDecimalPrecisionScale

- **Mensaje**: `Invalid Decimal Precision or Scale. Precision: %precision; Scale: %scale;.`

- **Causa**: el esquema no es válido.

- **Recomendación:**  Compruebe la precisión y la escala de la columna problemática.


### <a name="error-code--parquetcolumnnotfound"></a>Código de error:  ParquetColumnNotFound

- **Mensaje**: `Column %column; does not exist in Parquet file.`

- **Causa**: el esquema de origen no coincide con el esquema de receptor.

- **Recomendación:**  compruebe el valor "mappings" en "activity". Asegúrese de que la columna de origen se puede asignar a la columna de receptor correcta.


### <a name="error-code--parquetinvaliddataformat"></a>Código de error:  ParquetInvalidDataFormat

- **Mensaje**: `Incorrect format of %srcValue; for converting to %dstType;.`

- **Causa**: los datos no se pueden convertir al tipo especificado en mappings.source.

- **Recomendación:**  vuelva a comprobar los datos de origen o especifique el tipo de datos correcto para esta columna en la asignación de columnas de la actividad de copia. Consulte el documento: https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs.


### <a name="error-code--parquetdatacountnotmatchcolumncount"></a>Código de error:  ParquetDataCountNotMatchColumnCount

- **Mensaje**: `The data count in a row '%sourceColumnCount;' does not match the column count '%sinkColumnCount;' in given schema.`

- **Causa**: el número de columnas de origen y el número de columnas de receptor no coinciden.

- **Recomendación:**  vuelva a comprobar que el número de columnas de origen sea igual al número de columnas de receptor en "mapping".


### <a name="error-code--parquetdatatypenotmatchcolumntype"></a>Código de error:  ParquetDataTypeNotMatchColumnType

- **Mensaje**: el tipo de datos %srcType; no coincide con el tipo de columna especificada %dstType; en la columna "%columnIndex;".

- **Causa**: los datos del origen no se pueden convertir al tipo definido en el receptor.

- **Recomendación:**  especifique un tipo correcto en mapping.sink.


### <a name="error-code--parquetbridgeinvaliddata"></a>Código de error:  ParquetBridgeInvalidData

- **Mensaje**: `%message;`

- **Causa**: el valor de datos supera la limitación.

- **Recomendación:**  vuelva a intentarlo. Si el problema persiste, póngase en contacto con Microsoft.


### <a name="error-code--parquetunsupportedinterpretation"></a>Código de error:  ParquetUnsupportedInterpretation

- **Mensaje**: `The given interpretation '%interpretation;' of Parquet format is not supported.`

- **Causa**: escenario no admitido.

- **Recomendación:**  el valor de "ParquetInterpretFor" no debe ser "sparkSql".


### <a name="error-code--parquetunsupportfilelevelcompressionoption"></a>Código de error:  ParquetUnsupportFileLevelCompressionOption

- **Mensaje**: `File level compression is not supported for Parquet.`

- **Causa**: escenario no admitido.

- **Recomendación:**  Quite "CompressionType" de la carga útil.


### <a name="error-code--usererrorjniexception"></a>Código de error:  UserErrorJniException

- **Mensaje**: `Cannot create JVM: JNI return code [-6][JNI call failed: Invalid arguments.]`

- **Causa**: JVM no se puede crear porque se han establecido algunos argumentos no válidos (globales).

- **Recomendación:**  inicie sesión en la máquina que hospeda **cada nodo** del entorno de ejecución de integración autohospedado. Compruebe si la variable del sistema se ha establecido correctamente de la siguiente manera: `_JAVA_OPTIONS "-Xms256m -Xmx16g" with memory bigger than 8 G`. Reinicie todos los nodos de IR y vuelva a ejecutar la canalización.


### <a name="arithmetic-overflow"></a>Desbordamiento aritmético

- **Síntomas**: ha aparecido un mensaje de error al copiar los archivos de Parquet: `Message = Arithmetic Overflow., Source = Microsoft.DataTransfer.Common`

- **Causa**: actualmente solo se admite un número de posiciones decimales <= 38 y una longitud de la parte entera <= 20 al copiar archivos de Oracle a Parquet. 

- **Solución:** puede convertir las columnas con este problema en VARCHAR2 como solución alternativa.


### <a name="no-enum-constant"></a>Ninguna constante de enumeración

- **Síntomas**: ha aparecido un mensaje de error al copiar datos en formato Parquet: `java.lang.IllegalArgumentException:field ended by &apos;;&apos;` o `java.lang.IllegalArgumentException:No enum constant org.apache.parquet.schema.OriginalType.test`.

- **Causa**: 

    El problema puede deberse a espacios en blanco o caracteres no admitidos (como ,;{}()\n\t=) en el nombre de la columna, ya que Parquet no admite este formato. 

    Por ejemplo, un nombre de columna como *contoso(test)* analizará el tipo entre corchetes a partir del [código](https://github.com/apache/parquet-mr/blob/master/parquet-column/src/main/java/org/apache/parquet/schema/MessageTypeParser.java) `Tokenizer st = new Tokenizer(schemaString, " ;{}()\n\t");`. El error se generará, ya que no hay ningún tipo "test".

    Si desea comprobar los tipos admitidos, puede hacerlo [aquí](https://github.com/apache/parquet-mr/blob/master/parquet-column/src/main/java/org/apache/parquet/schema/OriginalType.java).

- **Solución:** 

    - Compruebe si hay espacios en blanco en el nombre de la columna de receptor.

    - Compruebe si la primera fila con espacios en blanco se usa como nombre de la columna.

    - Compruebe si se admite el tipo OriginalType o no. Intente evitar estos símbolos especiales `,;{}()\n\t=`. 


## <a name="rest"></a>REST

### <a name="error-code--restsinkcallfailed"></a>Código de error:  RestSinkCallFailed

- **Mensaje**: `Rest Endpoint responded with Failure from server. Check the error from server:%message;`

- **Causa**: Este error se produce cuando Azure Data Factory se comunica con el punto de conexión REST sobre el protocolo HTTP, y se produce un error en la operación de solicitud.

- **Recomendación:**  Compruebe el código de estado HTTP o el mensaje en el mensaje de error y solucione el problema del servidor remoto.

### <a name="unexpected-network-response-from-rest-connector"></a>Respuesta de red inesperada del conector REST

- **Síntomas**: A veces, el punto de conexión recibe una respuesta inesperada (400/401/403/500) del conector REST.

- **Causa**: El conector de origen REST utiliza la dirección URL y el método HTTP/encabezado/cuerpo del servicio vinculado/origen de copia como parámetros al crear una solicitud HTTP. Lo más probable es que el problema se deba a errores en uno o varios parámetros especificados.

- **Solución:** 
    - Utilice "curl" en la ventana cmd para comprobar si el parámetro es la causa o no (los encabezados **Accept** y **User-Agent** de usuario se deben incluir siempre):
        ```
        curl -i -X <HTTP method> -H <HTTP header1> -H <HTTP header2> -H "Accept: application/json" -H "User-Agent: azure-data-factory/2.0" -d '<HTTP body>' <URL>
        ```
      Si el comando devuelve la misma respuesta inesperada, corrija los parámetros anteriores con "curl" hasta que devuelva la respuesta esperada. 

      También puede usar "curl--help" para obtener un uso más avanzado del comando.

    - Si solo el conector REST de ADF devuelve una respuesta inesperada, póngase en contacto con el servicio de soporte técnico de Microsoft para solucionar el problema.
    
    - Tenga en cuenta que "curl" puede no ser adecuado para reproducir el problema de validación de certificados SSL. En algunos escenarios, el comando "curl" se ha ejecutado correctamente sin generar ningún problema de validación de certificados SSL. Pero cuando se ejecuta la misma dirección URL en el explorador, en realidad no se devuelve ningún certificado SSL en primer lugar para que el cliente establezca una relación de confianza con el servidor.

      En el caso anterior, se recomiendan herramientas como **Postman** y **Fiddler**.


## <a name="sftp"></a>SFTP

#### <a name="error-code--sftpoperationfail"></a>Código de error:  SftpOperationFail

- **Mensaje**: `Failed to '%operation;'. Check detailed error from SFTP.`

- **Causa**: Se ha producido un problema en la operación SFTP.

- **Recomendación:**  Compruebe el error detallado de SFTP.


### <a name="error-code--sftprenameoperationfail"></a>Código de error:  SftpRenameOperationFail

- **Mensaje**: `Failed to rename the temp file. Your SFTP server doesn't support renaming temp file, please set "useTempFileRename" as false in copy sink to disable uploading to temp file.`

- **Causa**: El servidor SFTP no admite el cambio de nombre del archivo temporal.

- **Recomendación:**  Establezca "useTempFileRename" como false en el receptor de copia para deshabilitar la carga en el archivo temporal.


### <a name="error-code--sftpinvalidsftpcredential"></a>Código de error:  SftpInvalidSftpCredential

- **Mensaje**: `Invalid Sftp credential provided for '%type;' authentication type.`

- **Causa**: El contenido de la clave privada se ha capturado de AKV/SDK pero no se ha codificado correctamente.

- **Recomendación:**  

    Si el contenido de la clave privada es de AKV y el archivo de la clave original puede funcionar si el cliente lo carga directamente en el servicio vinculado de SFTP,

    consulte https://docs.microsoft.com/azure/data-factory/connector-sftp#using-ssh-public-key-authentication, el contenido de privateKey es un contenido de clave privada SSH codificado en Base64.

    Codifique **todo el contenido del archivo de clave privada original** con codificación Base64 y almacene la cadena codificada en AKV. El archivo de clave privada original es el que puede funcionar en el servicio vinculado de SFTP si hace clic en Cargar desde archivo.

    Estos son algunos ejemplos que se usan para generar la cadena:

    - Mediante código en C#:
    ```
    byte[] keyContentBytes = File.ReadAllBytes(Private Key Path);
    string keyContent = Convert.ToBase64String(keyContentBytes, Base64FormattingOptions.None);
    ```

    - Mediante código de Python:
    ```
    import base64
    rfd = open(r'{Private Key Path}', 'rb')
    keyContent = rfd.read()
    rfd.close()
    print base64.b64encode(Key Content)
    ```

    - Mediante la herramienta de conversión Base64 de terceros

        Se recomiendan herramientas como https://www.base64encode.org/.

- **Causa**: Se ha elegido un formato de contenido de clave incorrecto.

- **Recomendación:**  

    La clave privada de SSH con el formato PKCS#8 (que empieza por "-----BEGIN ENCRYPTED PRIVATE KEY-----") no se admite actualmente para acceder al servidor de SFTP en ADF. 

    Ejecute los siguientes comandos para convertir la clave al formato tradicional de clave SSH (que empieza por "-----BEGIN RSA PRIVATE KEY-----"):

    ```
    openssl pkcs8 -in pkcs8_format_key_file -out traditional_format_key_file
    chmod 600 traditional_format_key_file
    ssh-keygen -f traditional_format_key_file -p
    ```

- **Causa**: Contenido de clave privada o credencial no válido.

- **Recomendación:**  Vuelva a comprobar con herramientas como WinSCP para ver si el archivo de clave o la contraseña son correctos.

### <a name="sftp-copy-activity-failed"></a>Error en la actividad de copia de SFTP.

- **Síntomas**: Código de error: UserErrorInvalidColumnMappingColumnNotFound. Mensaje de error: `Column &apos;AccMngr&apos; specified in column mapping cannot be found in source data.`

- **Causa**: el origen no incluye una columna denominada "AccMngr".

- **Solución:** compruebe cómo se ha configurado el conjunto de datos asignando la columna de conjunto de datos de destino para confirmar si existe dicha columna "AccMngr".


### <a name="error-code--sftpfailedtoconnecttosftpserver"></a>Código de error:  SftpFailedToConnectToSftpServer

- **Mensaje**: `Failed to connect to Sftp server '%server;'.`

- **Causa**: Si el mensaje de error contiene "La operación de lectura del socket ha agotado el tiempo de espera después de 30 000 milisegundos", una posible causa es que se use un tipo de servicio vinculado incorrecto para el servidor SFTP, como el uso del servicio vinculado FTP para conectarse a un servidor SFTP.

- **Recomendación:**  Compruebe el puerto del servidor de destino. De forma predeterminada, SFTP usa el puerto 22.

- **Causa**: Si el mensaje de error contiene "La respuesta del servidor no contiene la identificación del protocolo SSH", una posible causa es que el servidor SFTP haya limitado la conexión. ADF crea varias conexiones para realizar descargas desde el servidor SFTP en paralelo y, a veces, se alcanza la limitación del servidor SFTP. En la práctica, otro servidor devolverá otro error al alcanzar la limitación.

- **Recomendación:**  

    Establezca en 1 el número máximo de conexiones simultáneas del conjunto de datos de SFTP y vuelva a ejecutar la copia. Si se realiza correctamente, podemos estar seguros de que la limitación era la causa.

    Si desea promocionar el bajo rendimiento, póngase en contacto con el administrador de SFTP para aumentar el límite de conexiones simultáneas o agregue la siguiente dirección IP a la lista de permitidas:

    - Si usa un entorno de ejecución de integración administrado, agregue [intervalos de direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653).
      O bien, puede instalar un entorno de ejecución de integración autohospedado si no desea agregar una gran lista de intervalos IP a la lista de permitidos del servidor SFTP.

    - Si va a usar un entorno de ejecución de integración autohospedado, agregue la dirección IP de la máquina que instaló SHIR a la lista de permitidas.


## <a name="sharepoint-online-list"></a>Lista de SharePoint Online

### <a name="error-code--sharepointonlineauthfailed"></a>Código de error:  SharePointOnlineAuthFailed

- **Mensaje**: `The access token generated failed, status code: %code;, error message: %message;.`

- **Causa**: Es posible que el identificador y la clave de la entidad de servicio no se hayan establecido correctamente.

- **Recomendación:**  Compruebe que la aplicación registrada (el id. de entidad de servicio) y la clave se han establecido correctamente.


## <a name="xml-format"></a>Formato XML

### <a name="error-code--xmlsinknotsupported"></a>Código de error:  XmlSinkNotSupported

- **Mensaje**: `Write data in xml format is not supported yet, please choose a different format!`

- **Causa**: Se ha usado un conjunto de datos XML como conjunto de datos de receptor en la actividad de copia

- **Recomendación:**  Use un conjunto de datos con otro formato como receptor de copia.


### <a name="error-code--xmlattributecolumnnameconflict"></a>Código de error:  XmlAttributeColumnNameConflict

- **Mensaje**: `Column names %attrNames;' for attributes of element '%element;' conflict with that for corresponding child elements, and the attribute prefix used is '%prefix;'.`

- **Causa**: Se ha usado un prefijo de atributo, que ha provocado el conflicto.

- **Recomendación:**  Establezca otro valor para la propiedad "attributePrefix".


### <a name="error-code--xmlvaluecolumnnameconflict"></a>Código de error:  XmlValueColumnNameConflict

- **Mensaje**: `Column name for the value of element '%element;' is '%columnName;' and it conflicts with the child element having the same name.`

- **Causa**: Se ha usado uno de los nombres de elemento secundario como nombre de columna para el valor del elemento.

- **Recomendación:**  Establezca otro valor para la propiedad "valueColumn".


### <a name="error-code--xmlinvalid"></a>Código de error:  XmlInvalid

- **Mensaje**: `Input XML file '%file;' is invalid with parsing error '%error;'.`

- **Causa**: El archivo XML de entrada no tiene el formato correcto.

- **Recomendación:**  Corrija el archivo XML para que esté bien formado.


## <a name="general-copy-activity-error"></a>Error de actividad de copia general

### <a name="error-code--jrenotfound"></a>Código de error:  JreNotFound

- **Mensaje**: `Java Runtime Environment cannot be found on the Self-hosted Integration Runtime machine. It is required for parsing or writing to Parquet/ORC files. Make sure Java Runtime Environment has been installed on the Self-hosted Integration Runtime machine.`

- **Causa**: Integration Runtime autohospedado no puede encontrar Java Runtime. Se requiere Java Runtime para leer el origen específico.

- **Recomendación:**  compruebe el entorno de Integration Runtime y el documento de referencia: https://docs.microsoft.com/azure/data-factory/format-parquet#using-self-hosted-integration-runtime.


### <a name="error-code--wildcardpathsinknotsupported"></a>Código de error:  WildcardPathSinkNotSupported

- **Mensaje**: `Wildcard in path is not supported in sink dataset. Fix the path: '%setting;'.`

- **Causa**: el conjunto de datos del receptor no admite caracteres comodín.

- **Recomendación:**  compruebe el conjunto de valores del receptor y corrija la ruta de acceso sin el valor del carácter comodín.


### <a name="fips-issue"></a>Problema de PFIPS

- **Síntomas**: Se produce un error en la actividad de copia en el equipo del entorno de ejecución de integración autohospedado habilitado para FIPS con el mensaje de error `This implementation is not part of the Windows Platform FIPS validated cryptographic algorithms.`. Esto puede ocurrir cuando se copian datos con conectores como Azure Blob, SFTP, etc.

- **Causa**: FIPS (Estándar federal de procesamiento de información) define un determinado conjunto de algoritmos criptográficos que se pueden usar. Cuando se habilita el modo FIPS en el equipo, algunas clases criptográficas de las que depende la actividad se bloquean en algunos escenarios.

- **Solución:** En [este artículo](https://techcommunity.microsoft.com/t5/microsoft-security-baselines/why-we-8217-re-not-recommending-8220-fips-mode-8221-anymore/ba-p/701037) puede obtener información sobre la situación actual del modo FIPS en Windows y evaluar si puede deshabilitar FIPS en el equipo de entorno de ejecución de integración autohospedado.

    Por otro lado, si solo quiere permitir que Azure Data Factory omita FIPS y que las ejecuciones de actividad sean correctas, puede seguir estos pasos:

    1. Abra la carpeta donde está instalado el entorno de ejecución de integración autohospedado, normalmente en `C:\Program Files\Microsoft Integration Runtime\<IR version>\Shared`.

    2. Abra "diawp.exe.config" y agregue `<enforceFIPSPolicy enabled="false"/>` a la sección `<runtime>` como se muestra a continuación siguiente.

        ![Deshabilitar FIPS](./media/connector-troubleshoot-guide/disable-fips-policy.png)

    3. Reinicie el equipo del entorno de ejecución de integración autohospedado.


## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda para solucionar problemas, pruebe estos recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Página de preguntas y respuestas de Microsoft](/answers/topics/azure-data-factory.html)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)
