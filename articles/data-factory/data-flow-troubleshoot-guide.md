---
title: Solución de problemas de los flujos de datos de asignación
description: Obtenga información sobre la solución de problemas relacionados con flujos de datos en Azure Data Factory.
ms.author: makromer
author: kromerm
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 09/11/2020
ms.openlocfilehash: f8a852a8c4197169061a9c7633f4f363ad057337
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102505807"
---
# <a name="troubleshoot-mapping-data-flows-in-azure-data-factory"></a>Solución de problemas de los flujos de datos de asignación en Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se exploran métodos comunes de solución de problemas de flujos de datos de asignación en Azure Data Factory.

## <a name="common-error-codes-and-messages"></a>Mensajes y códigos de error comunes 

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Código de error: DF-Executor-SourceInvalidPayload
- **Mensaje**: Data preview, debug, and pipeline data flow execution failed because container does not exist (Error en la ejecución del flujo de datos de la depuración, la canalización y la vista previa de datos porque el contenedor no existe)
- **Causa**: Un conjunto de datos contiene un contenedor que no existe en el almacenamiento.
- **Recomendación**: Asegúrese de que el contenedor al que se hace referencia en el conjunto de datos exista o se pueda acceder.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Código de error: DF-Executor-SystemImplicitCartesian

- **Mensaje**: Implicit cartesian product for INNER join is not supported, use CROSS JOIN instead. Columns used in join should create a unique key for rows. (No se admite el producto cartesiano implícito para la combinación interna; utilice la combinación cruzada en su lugar. Las columnas utilizadas en la combinación deben crear una clave única para las filas).
- **Causa**: No se admiten los productos cartesianos implícitos para combinaciones internas entre planes lógicos. Si usa columnas de la combinación, cree una clave única con al menos una columna de ambos lados de la relación.
- **Recomendación**: En el caso de las combinaciones que se basan en la no igualdad, use la combinación cruzada personalizada.

### <a name="error-code-df-executor-systeminvalidjson"></a>Código de error: DF-Executor-SystemInvalidJson

- **Mensaje**: JSON parsing error, unsupported encoding or multiline (Error de análisis de JSON, codificación no compatible o multilínea)
- **Causa**: Posibles problemas con el archivo JSON, como codificación no admitida, bytes dañados o uso del origen JSON como un único documento en muchas líneas anidadas.
- **Recomendación**: Verifique que se admite la codificación del archivo JSON. En la transformación de origen que usa un conjunto de datos JSON, expanda **JSON Settings** (Configuración de JSON) y active **Documento único**.
 
### <a name="error-code-df-executor-broadcasttimeout"></a>Código de error: DF-Executor-BroadcastTimeout

- **Mensaje**: Broadcast join timeout error, make sure broadcast stream produces data within 60 secs in debug runs and 300 secs in job runs (Error de tiempo de espera de combinación de difusión, asegúrese de que el flujo de difusión genera datos en un plazo de 60 segundos en ejecuciones de depuración y de 300 segundos en ejecuciones de trabajos).
- **Causa**: La difusión tiene un tiempo de espera predeterminado de 60 segundos en ejecuciones de depuración y de 300 segundos en ejecuciones de trabajos. El flujo elegido para la difusión es demasiado grande para generar datos dentro de este límite.
- **Recomendación**: Marque la pestaña **Optimizar** en las transformaciones de flujo de datos para combinación, existe y búsqueda. La opción predeterminada para la difusión es **Auto**. Si se establece en **Auto** o si configura manualmente el lado izquierdo o derecho para difundir en **Fijo**, puede establecer una configuración mayor de Azure Integration Runtime (IR) o desactivar la difusión. Para el mejor rendimiento en los flujos de datos, se recomienda permitir que Spark realice la difusión con **Auto** y use una instancia de Azure IR optimizada para memoria. 
 
  Si ejecuta el flujo de datos en una ejecución de prueba de depuración desde una ejecución de canalización de depuración, puede ejecutar en esta condición con mayor frecuencia. Esto se debe a que Azure Data Factory limita el tiempo de espera de la difusión a 60 segundos para mantener una experiencia de depuración más rápida. Puede ampliar el tiempo de espera hasta el tiempo de espera de 300 segundos de una ejecución desencadenada. Para ello, puede usar la opción **Depurar** > **Use Activity Runtime** (Usar tiempo de ejecución de actividad) para usar la instancia de Azure IR definida en la actividad de ejecutar canalización de flujo de datos.

- **Mensaje**: Broadcast join timeout error, you can choose 'Off' of broadcast option in join/exists/lookup transformation to avoid this issue. If you intend to broadcast join option to improve performance then make sure broadcast stream can produce data within 60 secs in debug runs and 300 secs in job runs (Error de tiempo de espera de combinación de difusión, puede elegir "Desactivado" en la opción de difusión en la transformación join/exists/lookup para evitar este problema. Si tiene previsto difundir la opción de combinación para mejorar el rendimiento, asegúrese de que el flujo de difusión puede generar datos en un plazo de 60 segundos en ejecuciones de depuración y de 300 segundos en ejecuciones de trabajos).
- **Causa**: La difusión tiene un tiempo de espera predeterminado de 60 segundos en ejecuciones de depuración y de 300 segundos en ejecuciones de trabajos. En la combinación de difusión, el flujo elegido para la difusión es demasiado grande para generar datos dentro de este límite. Si no se usa una combinación de difusión, la difusión predeterminada realizada por el flujo de datos puede alcanzar el mismo límite.
- **Recomendación**: Desactive la opción de difusión o evite la difusión de flujos de datos de gran tamaño para los que el procesamiento pueda tardar más de 60 segundos. Elija un flujo más pequeño para la difusión. Los archivos de código fuente y las tablas de Azure SQL Data Warehouse grandes no suelen ser opciones adecuadas. En ausencia de una combinación de difusión, use un clúster mayor si se produce el error.

### <a name="error-code-df-executor-conversion"></a>Código de error: DF-Executor-Conversion

- **Mensaje**: Converting to a date or time failed due to an invalid character (Error al realizar la conversión a una fecha o hora porque un carácter no es válido).
- **Causa**: Los datos no tienen el formato esperado.
- **Recomendación:** Use el tipo de datos correcto.

### <a name="error-code-df-executor-invalidcolumn"></a>Código de error: DF-Executor-InvalidColumn
- **Mensaje**: Column name needs to be specified in the query, set an alias if using a SQL function (Es necesario especificar el nombre de la columna en la consulta; establezca un alias si usa una función SQL).
- **Causa**: No se ha especificado ningún nombre de columna.
- **Recomendación**: Establezca un alias si usa una función SQL como min () o max().

### <a name="error-code-df-executor-drivererror"></a>Código de error: DF-Executor-DriverError
- **Mensaje**: INT96 es un tipo de marca de tiempo heredado que no es compatible con el flujo de datos de ADF. Considere la posibilidad de actualizar el tipo de columna a los tipos más recientes.
- **Causa**: Error del controlador.
- **Recomendación**: INT96 es un tipo de marca de tiempo heredado que no es compatible con el flujo de datos de Azure Data Factory. Considere la posibilidad de actualizar el tipo de columna al tipo más reciente.

### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Código de error: DF-Executor-BlockCountExceedsLimitError
- **Mensaje**: el recuento de bloques sin confirmar no puede superar el límite máximo de 100 000 bloques. Compruebe la configuración de blobs.
- **Causa**: El número máximo de bloques sin confirmar en un blob es de 100 000.
- **Recomendación**: Póngase en contacto con el equipo del producto de Microsoft para obtener más detalles sobre este problema.

### <a name="error-code-df-executor-partitiondirectoryerror"></a>Código de error: DF-Executor-PartitionDirectoryError
- **Mensaje**: la ruta de origen especificada tiene varios directorios con particiones (por ejemplo, <Source Path>/<Directorio raíz de la partición 1>/a=10/b=20, <Source Path>/<Directorio raíz de la partición 2>/c=10/d=30) o un directorio con particiones con otro archivo o directorio sin particiones (por ejemplo, <Source Path>/<Directorio raíz con particiones 1>/a=10/b=20, <Source Path>/Directorio 2/archivo1). Quite el directorio raíz con particiones de la ruta de origen y léalo a través de una transformación de origen independiente.
- **Causa**: La ruta de origen tiene varios directorios con particiones o un directorio con particiones que tiene otro archivo o directorio sin particiones.
- **Recomendación**: Quite el directorio raíz con particiones de la ruta de origen y léalo a través de una transformación de origen independiente.

### <a name="error-code-df-executor-invalidtype"></a>Código de error: DF-Executor-InvalidType
- **Mensaje**: asegúrese de que el tipo de parámetro coincide con el tipo de valor pasado. Actualmente no se admite el paso de parámetros float desde canalizaciones.
- **Causa**: El tipo de datos para el tipo declarado no es compatible con el valor de parámetro real.
- **Recomendación**: Compruebe que los valores de parámetro pasados al flujo de datos coinciden con el tipo declarado.

### <a name="error-code-df-executor-columnunavailable"></a>Código de error: DF-Executor-ColumnUnavailable
- **Mensaje**: el nombre de columna usado en la expresión no está disponible o no es válido
- **Causa**: Nombre de columna no válido o no disponible usado en una expresión.
- **Recomendación**: Compruebe los nombres de columna de las expresiones.

### <a name="error-code-df-executor-parseerror"></a>Código de error: DF-Executor-ParseError
- **Mensaje**: no puede analizarse la expresión
- **Causa**: Una expresión generó errores de análisis debido a un formato incorrecto.
- **Recomendación**: Compruebe el formato de la expresión.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Código de error: DF-Executor-SystemImplicitCartesian
- **Mensaje**: Implicit cartesian product for INNER join is not supported, use CROSS JOIN instead. Columns used in join should create a unique key for rows. (No se admite el producto cartesiano implícito para la combinación interna; utilice la combinación cruzada en su lugar. Las columnas utilizadas en la combinación deben crear una clave única para las filas).
- **Causa**: No se admiten los productos cartesianos implícitos para combinaciones internas entre planes lógicos. Si utiliza columnas en la combinación, cree una clave única.
- **Recomendación**: En el caso de las combinaciones que se basan en la no igualdad, use la combinación cruzada.

### <a name="error-code-df-executor-systeminvalidjson"></a>Código de error: DF-Executor-SystemInvalidJson
- **Mensaje**: JSON parsing error, unsupported encoding or multiline (Error de análisis de JSON, codificación no compatible o multilínea)
- **Causa**: Posibles problemas con el archivo JSON, como codificación no admitida, bytes dañados o uso del origen JSON como un único documento en muchas líneas anidadas.
- **Recomendación**: Verifique que se admite la codificación del archivo JSON. En la transformación de origen que usa un conjunto de datos JSON, expanda **JSON Settings** (Configuración de JSON) y active **Documento único**.



### <a name="error-code-df-executor-conversion"></a>Código de error: DF-Executor-Conversion
- **Mensaje**: Converting to a date or time failed due to an invalid character (Error al realizar la conversión a una fecha o hora porque un carácter no es válido).
- **Causa**: Los datos no tienen el formato esperado.
- **Recomendación:** Use el tipo de datos correcto.


### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Código de error: DF-Executor-BlockCountExceedsLimitError
- **Mensaje**: el recuento de bloques sin confirmar no puede superar el límite máximo de 100 000 bloques. Compruebe la configuración de blobs.
- **Causa**: El número máximo de bloques sin confirmar en un blob es de 100 000.
- **Recomendación**: Póngase en contacto con el equipo del producto de Microsoft para obtener más detalles sobre este problema.

### <a name="error-code-df-executor-partitiondirectoryerror"></a>Código de error: DF-Executor-PartitionDirectoryError
- **Mensaje**: La ruta de origen especificada tiene varios directorios con particiones (por ejemplo, *<Source Path>/<Directorio raíz de la partición 1>/a=10/b=20, <Source Path>/<Directorio raíz de la partición 2>/c=10/d=30*) o un directorio con particiones con otro archivo o directorio sin particiones (por ejemplo, *<Source Path>/<Directorio raíz con particiones 1>/a=10/b=20, <Source Path>/Directorio 2/archivo1*). Quite el directorio raíz con particiones de la ruta de origen y léalo a través de una transformación de origen independiente.
- **Causa**: La ruta de origen tiene varios directorios con particiones o un directorio con particiones que tiene otro archivo o directorio sin particiones. 
- **Recomendación**: Quite el directorio raíz con particiones de la ruta de origen y léalo a través de una transformación de origen independiente.

### <a name="error-code-getcommand-outputasync-failed"></a>Código de error: Error de GetCommand OutputAsync
- **Mensaje**: Durante la depuración de Data Flow y la vista previa de datos: Error de GetCommand OutputAsync con...
- **Causa**: Se trata de un error del servicio de back-end. 
- **Recomendación**: Vuelva a intentar la operación y reinicie la sesión de depuración. Si con ello no se resuelve el problema, póngase en contacto con el servicio de soporte al cliente. 

### <a name="error-code-df-executor-outofmemoryerror"></a>Código de error: DF-Executor-OutOfMemoryError
 
- **Mensaje**: el clúster ha tenido un problema de memoria insuficiente durante la ejecución. Vuelva a intentar usar un entorno de ejecución de integración con un mayor recuento de núcleos o un tipo de proceso optimizado para memoria
- **Causa**: El clúster se está quedando sin memoria.
- **Recomendación**: Los clústeres de depuración están diseñados para el desarrollo. Use el muestreo de datos y un tipo y tamaño de proceso adecuados para ejecutar la carga. Para obtener sugerencias de rendimiento, vea [Guía del rendimiento del flujo de datos de asignación](concepts-data-flow-performance.md).

### <a name="error-code-df-executor-illegalargument"></a>Código de error: DF-Executor-illegalArgument

- **Mensaje**: asegúrese de que la clave de acceso del servicio vinculado es correcta
- **Causa**: El nombre de cuenta o la clave de acceso son incorrectos.
- **Recomendación:** asegúrese de que el nombre de cuenta o la clave de acceso especificados en el servicio vinculado son correctos. 

### <a name="error-code-df-executor-invalidtype"></a>Código de error: DF-Executor-InvalidType
- **Mensaje**: asegúrese de que el tipo de parámetro coincide con el tipo de valor pasado. Actualmente no se admite el paso de parámetros float desde canalizaciones.
- **Causa**: El tipo de datos para el tipo declarado no es compatible con el valor de parámetro real. 
- **Recomendación**: Proporcione los tipos de datos correctos.

### <a name="error-code-df-executor-columnunavailable"></a>Código de error: DF-Executor-ColumnUnavailable
- **Mensaje**: El nombre de columna usado en la expresión no está disponible o no es válido.
- **Causa**: Se usa un nombre de columna no válido o no disponible en una expresión.
- **Recomendación**: Compruebe los nombres de columna usados en las expresiones.


### <a name="error-code-df-executor-parseerror"></a>Código de error: DF-Executor-ParseError
- **Mensaje**: No puede analizarse la expresión.
- **Causa**: Una expresión generó errores de análisis debido a un formato incorrecto. 
- **Recomendación**: Compruebe el formato de la expresión.


 ### <a name="error-code-df-executor-outofdiskspaceerror"></a>Código de error: DF-Executor-OutOfDiskSpaceError
- **Mensaje**: Error interno del servidor
- **Causa**: El clúster se está quedando sin espacio en disco.
- **Recomendación**: Vuelva a ejecutar la canalización. Si esto no resuelve el problema, póngase en contacto con el servicio de asistencia al cliente.


 ### <a name="error-code-df-executor-storeisnotdefined"></a>Código de error: DF-Executor-StoreIsNotDefined
- **Mensaje**: The store configuration is not defined. This error is potentially caused by invalid parameter assignment in the pipeline (La configuración del almacén no está definida. Este error se puede deber a una asignación de parámetros no válida en la canalización).
- **Causa**: Sin determinar.
- **Recomendación**: Compruebe la asignación de valores de parámetros en la canalización. Una expresión de parámetros puede contener caracteres no válidos.

### <a name="error-code-df-excel-invalidconfiguration"></a>Código de error: DF-Excel-InvalidConfiguration
- **Mensaje**: Excel sheet name or index is required (El nombre o el índice de la hoja de Excel es obligatorio).
- **Causa**: Sin determinar.
- **Recomendación**: Compruebe el valor del parámetro. Especifique el nombre o el índice de la hoja de cálculo para leer los datos de Excel.

- **Mensaje**: Excel sheet name and index cannot exist at the same time (El nombre y el índice de la hoja de Excel no pueden existir al mismo tiempo).
- **Causa**: Sin determinar.
- **Recomendación**: Compruebe el valor del parámetro. Especifique el nombre o el índice de la hoja de cálculo para leer los datos de Excel.

- **Mensaje**: Invalid range is provided (Se proporcionó un intervalo no válido).
- **Causa**: Sin determinar.
- **Recomendación**: Compruebe el valor del parámetro. Especifique un intervalo válido por referencia. Para más información, consulte [Propiedades de Excel](./format-excel.md#dataset-properties).

- **Mensaje**: Invalid excel file is provided while only .xlsx and .xls are supported (Se proporcionó un archivo de Excel no válido mientras que solo se admiten .xlsx y .xls)
- **Causa**: Sin determinar.
- **Recomendación**: Asegúrese de que la extensión de archivo de Excel sea .xlsx o .xls.


 ### <a name="error-code-df-excel-invaliddata"></a>Código de error: DF-Excel-InvalidData
- **Mensaje**: Excel worksheet does not exist (La hoja de cálculo de Excel no existe).
- **Causa**: Sin determinar.
- **Recomendación**: Compruebe el valor del parámetro. Especifique un nombre o índice válido de la hoja de cálculo para leer los datos de Excel.

- **Mensaje**: Reading excel files with different schema is not supported now (En este momento no se admite la lectura de archivos de Excel con esquemas diferentes).
- **Causa**: Sin determinar.
- **Recomendación**: Use un archivo de Excel compatible.

- **Mensaje**: Tipo de datos no admitido.
- **Causa**: Sin determinar.
- **Recomendación**: Use tipos de datos de archivo de Excel compatibles.

### <a name="error-code-4502"></a>Código de error: 4502
- **Mensaje**: Hay importantes ejecuciones de MappingDataflow simultáneas que están causando errores debido a la limitación en el entorno de ejecución de integración.
- **Causa**: Un gran número de ejecuciones de actividad de flujo de datos se producen simultáneamente en el entorno de ejecución de integración. Para más información, consulte [Límites de Data Factory](../azure-resource-manager/management/azure-subscription-service-limits.md#data-factory-limits).
- **Recomendación**: Si desea ejecutar más actividades de flujo de datos en paralelo, distribúyalas en varios entornos de ejecución de integración.


### <a name="error-code-invalidtemplate"></a>Código de error: InvalidTemplate
- **Mensaje**: no se puede evaluar la expresión de canalización.
- **Causa**: La expresión de canalización pasada en la actividad de flujo de datos no se está procesando correctamente debido a un error de sintaxis.
- **Recomendación**: Compruebe la actividad en la supervisión de actividades para comprobar la expresión.

### <a name="error-code-2011"></a>Código de error: 2011
- **Mensaje**: la actividad se estaba ejecutando en Azure Integration Runtime y no pudo descifrar la credencial del almacén de datos o el proceso conectado a través de un entorno de ejecución de integración autohospedado. Compruebe la configuración de los servicios vinculados asociados con esta actividad y asegúrese de usar el tipo de entorno de ejecución de integración adecuado.
- **Causa**: El flujo de datos no es compatible con los servicios vinculados en los entornos de ejecución de integración autohospedados.
- **Recomendación**: Configure el flujo de datos para que se ejecute en el entorno de ejecución de integración de red virtual administrada.

## <a name="miscellaneous-troubleshooting-tips"></a>Consejos de solución de problemas varios
- **Problema**: Se produjo una excepción inesperada y un error en la ejecución.
    - **Mensaje**: Durante la ejecución de una actividad de Data Flow: Se obtuvo una excepción inesperada y se produjo un error de ejecución.
    - **Causa**: Se trata de un error del servicio de back-end. Vuelva a intentar la operación y reinicie la sesión de depuración.
    - **Recomendación**: Si reintenta y reinicia y no se resuelve el problema, póngase en contacto con el servicio de soporte al cliente.

-  **Problema**: No hay datos de salida en la combinación durante la vista previa de los datos de depuración.
    - **Mensaje**: hay un gran número de valores NULL o valores que faltan, lo que puede deberse a que no se han muestreado suficientes filas. Pruebe a actualizar el límite de filas de depuración y los datos.
    - **Causa**: La condición de combinación no ha coincidido con ninguna fila o dio como resultado un gran número de valores NULL durante la vista previa de los datos.
    - **Recomendación**: En **Configuración de depuración**, aumente el número de filas del límite de filas de origen. Asegúrese de seleccionar una instancia de Azure IR que tenga un clúster de flujo de datos lo suficientemente grande como para controlar más datos.
    
- **Problema**: Error de validación en el origen con archivos CSV de varias líneas. 
    - **Mensaje**: Puede ver uno de estos mensajes de error:
        - Falta la última columna o es NULL.
        - Error en la validación del esquema en el origen.
        - La importación de esquema no se muestra correctamente en la experiencia de usuario y la última columna tiene un carácter de nueva línea en el nombre.
    - **Causa**: En el flujo de datos de asignación, los archivos de origen CSV de varias líneas no funcionan actualmente cuando se usa \r\n como delimitador de filas. A veces, las líneas adicionales de los retornos de carro pueden producir errores. 
    - **Recomendación** Genere el archivo en el origen usando el carácter \n como delimitador de filas, en lugar de \r\n. O bien, use la actividad de copia para convertir el archivo CSV para que use \n como delimitador de filas.

## <a name="general-troubleshooting-guidance"></a>Guía de solución de problemas generales
1. Compruebe el estado de las conexiones del conjunto de datos. En cada transformación de origen y de receptor, vaya al servicio vinculado para cada conjunto de datos que use y pruebe las conexiones.
2. Compruebe el estado de las conexiones de archivos y tablas en el diseñador de flujo de datos. En el modo de depuración, seleccione **Vista previa de datos** en las transformaciones de origen para asegurarse de que puede tener acceso a los datos.
3. Si todo parece correcto en la vista previa de los datos, vaya al diseñador de canalizaciones y coloque el flujo de datos en una actividad de canalización. Depure la canalización para realizar una prueba de un extremo a otro.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más ayuda para solucionar problemas, consulte estos recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)

