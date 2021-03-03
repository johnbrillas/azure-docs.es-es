---
title: Solución de problemas de los flujos de datos de asignación
description: Obtenga información acerca de la solución de problemas relacionados con flujos de datos en Azure Data Factory.
ms.author: makromer
author: kromerm
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 09/11/2020
ms.openlocfilehash: 4545c3529baf92e2f90d9289ec6828ad9a720e3a
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101738011"
---
# <a name="troubleshoot-mapping-data-flows-in-azure-data-factory"></a>Solución de problemas de los flujos de datos de asignación en Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se exploran métodos comunes de solución de problemas de flujos de datos de asignación en Azure Data Factory.

## <a name="common-error-codes-and-messages"></a>Mensajes y códigos de error comunes 

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Código de error: DF-Executor-SourceInvalidPayload
- **Mensaje**: Data preview, debug, and pipeline data flow execution failed because container does not exist (Error en la ejecución del flujo de datos de la depuración, la canalización y la vista previa de datos porque el contenedor no existe)
- **Causas**: cuando el conjunto de datos contiene un contenedor que no existe en el almacenamiento
- **Recomendación:** asegúrese de que el contenedor al que se hace referencia en el conjunto de datos exista o sea accesible.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Código de error: DF-Executor-SystemImplicitCartesian

- **Mensaje**: Implicit cartesian product for INNER join is not supported, use CROSS JOIN instead. Columns used in join should create a unique key for rows. (No se admite el producto cartesiano implícito para la combinación interna; utilice la combinación cruzada en su lugar. Las columnas utilizadas en la combinación deben crear una clave única para las filas).
- **Causas**: no se admite el producto cartesiano implícito para la combinación interna entre planes lógicos. Si las columnas se usan en la combinación, se requiere la clave única con al menos una columna de ambos lados de la relación.
- **Recomendación:** en el caso de las combinaciones que no se basan en la igualdad, debe optar por la combinación cruzada personalizada.

### <a name="error-code-df-executor-systeminvalidjson"></a>Código de error: DF-Executor-SystemInvalidJson

- **Mensaje**: JSON parsing error, unsupported encoding or multiline (Error de análisis de JSON, codificación no compatible o multilínea)
- **Causas**: posibles problemas con el archivo JSON, como codificación no admitida, bytes dañados o se usa el origen JSON como un único documento en muchas líneas anidadas.
- **Recomendación:** verifique que se admite la codificación del archivo JSON. En la transformación de origen que usa un conjunto de datos JSON, expanda "JSON Settings" (Configuración de JSON) y active "Documento único".
 
### <a name="error-code-df-executor-broadcasttimeout"></a>Código de error: DF-Executor-BroadcastTimeout

- **Mensaje**: Broadcast join timeout error, make sure broadcast stream produces data within 60 secs in debug runs and 300 secs in job runs (Error de tiempo de espera de combinación de difusión, asegúrese de que el flujo de difusión genera datos en un plazo de 60 segundos en ejecuciones de depuración y de 300 segundos en ejecuciones de trabajos).
- **Causas**: la difusión tiene un tiempo de espera predeterminado de 60 segundos en ejecuciones de depuración y de 300 segundos en ejecuciones de trabajos. El flujo elegido para la difusión parece demasiado grande para generar datos dentro de este límite.
- **Recomendación:** active la pestaña Optimizar en las transformaciones de flujo de datos para Join, Exists y Lookup. La opción predeterminada para Broadcast (Difusión) es "Auto". Si se establece "Auto" o si va a configurar manualmente el lado izquierdo o derecho para difundir en "Fixed" (Fijo), puede establecer una configuración mayor de Azure Integration Runtime o desactivar la difusión. El enfoque recomendado para el mejor rendimiento en los flujos de datos es permitir que Spark realice la difusión mediante "Auto" y use una instancia de Azure IR optimizada para memoria. Si ejecuta el flujo de datos en una ejecución de pruebas de depuración desde una ejecución de la canalización de depuración, puede ejecutar en esta condición con mayor frecuencia. Esto se debe a que ADF limita el tiempo de espera de difusión a 60 segundos para mantener una experiencia de depuración más rápida. Si desea ampliarlo al tiempo de espera de 300 segundos de una ejecución desencadenada, puede usar la opción Debug > Use Activity Runtime (Depurar > Usar tiempo de ejecución de la actividad) para utilizar la instancia de Azure IR definida en su actividad de canalización Ejecución de flujo de datos.

- **Mensaje**: Broadcast join timeout error, you can choose 'Off' of broadcast option in join/exists/lookup transformation to avoid this issue. If you intend to broadcast join option to improve performance then make sure broadcast stream can produce data within 60 secs in debug runs and 300 secs in job runs (Error de tiempo de espera de combinación de difusión, puede elegir "Desactivado" en la opción de difusión en la transformación join/exists/lookup para evitar este problema. Si tiene previsto difundir la opción de combinación para mejorar el rendimiento, asegúrese de que el flujo de difusión puede generar datos en un plazo de 60 segundos en ejecuciones de depuración y de 300 segundos en ejecuciones de trabajos).
- **Causas**: la difusión tiene un tiempo de espera predeterminado de 60 segundos en ejecuciones de depuración y de 300 segundos en ejecuciones de trabajos. En la combinación de difusión, el flujo elegido para la difusión parece demasiado grande para generar datos dentro de este límite. Si no se usa una combinación de difusión, la difusión predeterminada realizada por el flujo de entrada puede alcanzar el mismo límite.
- **Recomendación:** Desactive la opción de difusión o evite la difusión de flujos de datos de gran tamaño cuyo procesamiento pueda tardar más de 60 segundos. En su lugar, elija un flujo más pequeño para la difusión. Las tablas SQL/DW y los archivos de código fuente de gran tamaño suelen ser malos candidatos. En ausencia de una combinación de difusión, use un clúster más grande si se presenta el error.

### <a name="error-code-df-executor-conversion"></a>Código de error: DF-Executor-Conversion

- **Mensaje**: Converting to a date or time failed due to an invalid character (Error al realizar la conversión a una fecha o hora porque un carácter no es válido).
- **Causas**: los datos no tienen el formato esperado.
- **Recomendación:** use el tipo de datos correcto.

### <a name="error-code-df-executor-invalidcolumn"></a>Código de error: DF-Executor-InvalidColumn
- **Mensaje**: Column name needs to be specified in the query, set an alias if using a SQL function (Es necesario especificar el nombre de la columna en la consulta; establezca un alias si usa una función SQL).
- **Causas**: no se ha especificado ningún nombre de columna.
- **Recomendación:** establezca un alias si usa una función SQL, como min()/max(), etc.

### <a name="error-code-df-executor-drivererror"></a>Código de error: DF-Executor-DriverError
- **Mensaje**: INT96 es un tipo de marca de tiempo heredado que no es compatible con el flujo de datos de ADF. Considere la posibilidad de actualizar el tipo de columna a los tipos más recientes.
- **Causas**: error del controlador
- **Recomendación:** INT96 es un tipo de marca de tiempo heredado que no es compatible con el flujo de datos de ADF. Considere la posibilidad de actualizar el tipo de columna a los tipos más recientes.

### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Código de error: DF-Executor-BlockCountExceedsLimitError
- **Mensaje**: el recuento de bloques sin confirmar no puede superar el límite máximo de 100 000 bloques. Compruebe la configuración de blobs.
- **Causas**: puede haber un máximo de 100 000 bloques sin confirmar en un blob.
- **Recomendación:** póngase en contacto con el equipo de productos de Microsoft en relación con este problema para obtener más detalles.

### <a name="error-code-df-executor-partitiondirectoryerror"></a>Código de error: DF-Executor-PartitionDirectoryError
- **Mensaje**: la ruta de origen especificada tiene varios directorios con particiones (por ejemplo, <Source Path>/<Directorio raíz de la partición 1>/a=10/b=20, <Source Path>/<Directorio raíz de la partición 2>/c=10/d=30) o un directorio con particiones con otro archivo o directorio sin particiones (por ejemplo, <Source Path>/<Directorio raíz con particiones 1>/a=10/b=20, <Source Path>/Directorio 2/archivo1). Quite el directorio raíz con particiones de la ruta de origen y léalo a través de una transformación de origen independiente.
- **Causas**: la ruta de origen tiene varios directorios con particiones o un directorio con particiones con otro archivo o directorio sin particiones.
- **Recomendación:** quite el directorio raíz con particiones de la ruta de origen y léalo a través de una transformación de origen independiente.

### <a name="error-code-df-executor-outofmemoryerror"></a>Código de error: DF-Executor-OutOfMemoryError
- **Mensaje**: el clúster ha tenido un problema de memoria insuficiente durante la ejecución. Vuelva a intentar usar un entorno de ejecución de integración con un mayor recuento de núcleos o un tipo de proceso optimizado para memoria
- **Causas**: el clúster se está quedando sin memoria
- **Recomendación:** los clústeres de depuración están diseñados con fines de desarrollo. Aproveche el muestreo de datos, el tipo de proceso adecuado y el tamaño para ejecutar la carga. Consulte la [guía de rendimiento del flujo de datos de asignación](concepts-data-flow-performance.md) para llevar a cabo una optimización y, de este modo, lograr el mejor rendimiento.

### <a name="error-code-df-executor-invalidtype"></a>Código de error: DF-Executor-InvalidType
- **Mensaje**: asegúrese de que el tipo de parámetro coincide con el tipo de valor pasado. Actualmente no se admite el paso de parámetros float desde canalizaciones.
- **Causas**: tipos de datos incompatibles entre el tipo declarado y el valor de parámetro real
- **Recomendación:** compruebe que los valores de parámetro pasados a un flujo de datos coinciden con el tipo declarado.

### <a name="error-code-df-executor-columnunavailable"></a>Código de error: DF-Executor-ColumnUnavailable
- **Mensaje**: el nombre de columna usado en la expresión no está disponible o no es válido
- **Causas**: nombre de columna no válido o no disponible usado en expresiones
- **Recomendación:** compruebe los nombres de columna usados en las expresiones.

### <a name="error-code-df-executor-parseerror"></a>Código de error: DF-Executor-ParseError
- **Mensaje**: no puede analizarse la expresión
- **Causas**: la expresión tiene errores de análisis debido al formato
- **Recomendación:** compruebe el formato de la expresión.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Código de error: DF-Executor-SystemImplicitCartesian
- **Mensaje**: Implicit cartesian product for INNER join is not supported, use CROSS JOIN instead. Columns used in join should create a unique key for rows. (No se admite el producto cartesiano implícito para la combinación interna; utilice la combinación cruzada en su lugar. Las columnas utilizadas en la combinación deben crear una clave única para las filas).
- **Causas**: no se admite el producto cartesiano implícito para la combinación interna entre planes lógicos. Si las columnas usadas en la combinación crean la clave única.
- **Recomendación:** en el caso de las combinaciones que no se basen en la igualdad, debe optar por la combinación cruzada.

### <a name="error-code-df-executor-systeminvalidjson"></a>Código de error: DF-Executor-SystemInvalidJson
- **Mensaje**: JSON parsing error, unsupported encoding or multiline (Error de análisis de JSON, codificación no compatible o multilínea)
- **Causas**: posibles problemas con el archivo JSON, como codificación no admitida, bytes dañados o se usa el origen JSON como un único documento en muchas líneas anidadas.
- **Recomendación:** verifique que se admite la codificación del archivo JSON. En la transformación de origen que usa un conjunto de datos JSON, expanda "JSON Settings" (Configuración de JSON) y active "Documento único".



### <a name="error-code-df-executor-conversion"></a>Código de error: DF-Executor-Conversion
- **Mensaje**: Converting to a date or time failed due to an invalid character (Error al realizar la conversión a una fecha o hora porque un carácter no es válido).
- **Causas**: los datos no tienen el formato esperado.
- **Recomendación:**  use el tipo de datos correcto.


### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Código de error: DF-Executor-BlockCountExceedsLimitError
- **Mensaje**: el recuento de bloques sin confirmar no puede superar el límite máximo de 100 000 bloques. Compruebe la configuración de blobs.
- **Causas**: puede haber un máximo de 100 000 bloques sin confirmar en un blob.
- **Recomendación:** Póngase en contacto con el equipo de productos de Microsoft en relación con este problema para obtener más detalles.

### <a name="error-code-df-executor-partitiondirectoryerror"></a>Código de error: DF-Executor-PartitionDirectoryError
- **Mensaje**: La ruta de origen especificada tiene varios directorios con particiones (por ejemplo, *<Source Path>/<Directorio raíz de la partición 1>/a=10/b=20, <Source Path>/<Directorio raíz de la partición 2>/c=10/d=30*) o un directorio con particiones con otro archivo o directorio sin particiones (por ejemplo, *<Source Path>/<Directorio raíz con particiones 1>/a=10/b=20, <Source Path>/Directorio 2/archivo1*). Quite el directorio raíz con particiones de la ruta de origen y léalo a través de una transformación de origen independiente.
- **Causas**: la ruta de origen tiene varios directorios con particiones o un directorio con particiones con otro archivo o directorio sin particiones.
- **Recomendación:** quite el directorio raíz con particiones de la ruta de origen y léalo a través de una transformación de origen independiente.

### <a name="error-code-getcommand-outputasync-failed"></a>Código de error: Error de GetCommand OutputAsync
- **Mensaje**: Durante la depuración de Data Flow y la vista previa de datos: Error de GetCommand OutputAsync con...
- **Causas**: Se trata de un error del servicio de back-end. Puede volver a intentar la operación y reiniciar la sesión de depuración.
- **Recomendación:** Si reintentar y reiniciar no resuelven el problema, póngase en contacto con el soporte al cliente. 

### <a name="error-code-df-executor-outofmemoryerror"></a>Código de error: DF-Executor-OutOfMemoryError
 
- **Mensaje**: el clúster ha tenido un problema de memoria insuficiente durante la ejecución. Vuelva a intentar usar un entorno de ejecución de integración con un mayor recuento de núcleos o un tipo de proceso optimizado para memoria
- **Causas**: El clúster se está quedando sin memoria.
- **Recomendación:** los clústeres de depuración están diseñados con fines de desarrollo. Aproveche el muestreo de datos, el tipo de proceso adecuado y el tamaño para ejecutar la carga. Consulte la [Guía de rendimiento de flujo de datos](./concepts-data-flow-performance.md) para optimizar los flujos de datos para obtener el mejor rendimiento.

### <a name="error-code-df-executor-illegalargument"></a>Código de error: DF-Executor-illegalArgument
- **Mensaje**: Asegúrese de que la clave de acceso del servicio vinculado es correcta.
- **Causas**: El nombre de cuenta o la clave de acceso son incorrectos.
- **Recomendación:** Proporcione el nombre de cuenta o la clave de acceso correctos.

- **Mensaje**: asegúrese de que la clave de acceso del servicio vinculado es correcta
- **Causas**: nombre de cuenta o clave de acceso incorrectos
- **Recomendación:** asegúrese de que el nombre de cuenta o la clave de acceso especificados en el servicio vinculado son correctos. 

### <a name="error-code-df-executor-invalidtype"></a>Código de error: DF-Executor-InvalidType
- **Mensaje**: asegúrese de que el tipo de parámetro coincide con el tipo de valor pasado. Actualmente no se admite el paso de parámetros float desde canalizaciones.
- **Causas**: tipos de datos incompatibles entre el tipo declarado y el valor de parámetro real
- **Recomendación:** Especifique los tipos de datos correctos.

### <a name="error-code-df-executor-columnunavailable"></a>Código de error: DF-Executor-ColumnUnavailable
- **Mensaje**: El nombre de columna usado en la expresión no está disponible o no es válido.
- **Causas**: Un nombre de columna no válido o no disponible se usa en las expresiones.
- **Recomendación:** compruebe los nombres de columna usados en las expresiones.


### <a name="error-code-df-executor-parseerror"></a>Código de error: DF-Executor-ParseError
- **Mensaje**: No puede analizarse la expresión.
- **Causas**: La expresión tiene errores de análisis debido al formato.
- **Recomendación:** compruebe el formato de la expresión.


 ### <a name="error-code-df-executor-outofdiskspaceerror"></a>Código de error: DF-Executor-OutOfDiskSpaceError
- **Mensaje**: Error interno del servidor
- **Causas**: El clúster se están quedando sin espacio en disco.
- **Recomendación:** Vuelva a intentar realizar la canalización. Si el problema continúa, póngase en contacto con soporte al cliente.


 ### <a name="error-code-df-executor-storeisnotdefined"></a>Código de error: DF-Executor-StoreIsNotDefined
- **Mensaje**: The store configuration is not defined. This error is potentially caused by invalid parameter assignment in the pipeline (La configuración del almacén no está definida. Este error se puede deber a una asignación de parámetros no válida en la canalización).
- **Causas**: Indeterminada
- **Recomendación:** Compruebe la asignación de valores de parámetros en la canalización. La expresión de parámetros puede contener caracteres no válidos.

### <a name="error-code-df-excel-invalidconfiguration"></a>Código de error: DF-Excel-InvalidConfiguration
- **Mensaje**: Excel sheet name or index is required (El nombre o el índice de la hoja de Excel es obligatorio).
- **Causas**: Indeterminada
- **Recomendación:** Compruebe el valor de los parámetros y especifique el nombre o índice de la hoja para leer los datos de Excel.

- **Mensaje**: Excel sheet name and index cannot exist at the same time (El nombre y el índice de la hoja de Excel no pueden existir al mismo tiempo).
- **Causas**: Indeterminada
- **Recomendación:** Compruebe el valor de los parámetros y especifique el nombre o índice de la hoja para leer los datos de Excel.

- **Mensaje**: Invalid range is provided (Se proporcionó un intervalo no válido).
- **Causas**: Indeterminada
- **Recomendación:** Compruebe el valor de los parámetros y especifique un intervalo válido por referencia: [propiedades de Excel](./format-excel.md#dataset-properties).

- **Mensaje**: Invalid excel file is provided while only .xlsx and .xls are supported (Se proporcionó un archivo de Excel no válido mientras que solo se admiten .xlsx y .xls)
- **Causas**: Indeterminada
- **Recomendación:** Asegúrese de que la extensión de archivo de Excel sea .xlsx o .xls.


 ### <a name="error-code-df-excel-invaliddata"></a>Código de error: DF-Excel-InvalidData
- **Mensaje**: Excel worksheet does not exist (La hoja de cálculo de Excel no existe).
- **Causas**: Indeterminada
- **Recomendación:** Compruebe el valor de los parámetros y especifique el nombre o índice válido de la hoja para leer los datos de Excel.

- **Mensaje**: Reading excel files with different schema is not supported now (En este momento no se admite la lectura de archivos de Excel con esquemas diferentes).
- **Causas**: Indeterminada
- **Recomendación:** Use el archivo de Excel correcto.

- **Mensaje**: Tipo de datos no admitido.
- **Causas**: Indeterminada
- **Recomendación:** use tipos de datos correctos de archivo de Excel.

### <a name="error-code-4502"></a>Código de error: 4502
- **Mensaje**: hay importantes ejecuciones de MappingDataflow simultáneas que están causando errores debido a la limitación en Integration Runtime.
- **Causas**: una gran cantidad de ejecuciones de actividad de flujo de datos se están realizando simultáneamente en Integration Runtime. Obtenga más información sobre los [Límites de Azure Data Factory](../azure-resource-manager/management/azure-subscription-service-limits.md#data-factory-limits).
- **Recomendación:** en caso de que quiera ejecutar más actividades de flujo de datos en paralelo, distribúyalas en varios entornos de ejecución de integración.


### <a name="error-code-invalidtemplate"></a>Código de error: InvalidTemplate
- **Mensaje**: no se puede evaluar la expresión de canalización.
- **Causas**: la expresión de canalización pasada en la actividad de flujo de datos no se está procesando correctamente debido a un error de sintaxis.
- **Recomendación:** compruebe la actividad en la supervisión de la actividad para comprobar la expresión.

### <a name="error-code-2011"></a>Código de error: 2011
- **Mensaje**: la actividad se estaba ejecutando en Azure Integration Runtime y no pudo descifrar la credencial del almacén de datos o el proceso conectado a través de un entorno de ejecución de integración autohospedado. Compruebe la configuración de los servicios vinculados asociados con esta actividad y asegúrese de usar el tipo de entorno de ejecución de integración adecuado.
- **Causas**: el flujo de datos no admite los servicios vinculados con el entorno de ejecución de integración autohospedado.
- **Recomendación:** configure el flujo de datos para que se ejecute en el entorno de ejecución de integración con "Red virtual administrada".

## <a name="miscellaneous-troubleshooting-tips"></a>Consejos de solución de problemas varios
- **Problema:** Se obtuvo una excepción inesperada y se produjo un error de ejecución
    - **Mensaje**: Durante la ejecución de una actividad de Data Flow: Se obtuvo una excepción inesperada y se produjo un error de ejecución.
    - **Causas**: Se trata de un error del servicio de back-end. Puede volver a intentar la operación y reiniciar la sesión de depuración.
    - **Recomendación:** Si reintentar y reiniciar no resuelven el problema, póngase en contacto con el soporte al cliente.

-  **Problema:** Debug data preview No Output Data on Join (Vista previa de datos de depuración: sin datos de salida en la combinación)
    - **Mensaje**: hay un gran número de valores NULL o valores que faltan, lo que puede deberse a que no se han muestreado suficientes filas. Pruebe a actualizar el límite de filas de depuración y los datos.
    - **Causas**: la condición de combinación no coincidía con ninguna fila o dio como resultado un número elevado de valores NULL durante la vista previa de los datos.
    - **Recomendación:** vaya a Configuración de depuración y aumente el número de filas del límite de filas de origen. Asegúrese de que ha seleccionado una instancia de Azure IR con un clúster de flujo de datos lo suficientemente grande como para administrar más datos.
    
- **Problema:** Error de validación en el origen con archivos CSV de varias líneas 
    - **Mensaje**: Puede ver uno de los siguientes mensajes de error:
        - Falta la última columna o es NULL.
        - Error en la validación del esquema en el origen.
        - La importación de esquema no se muestra correctamente en la experiencia de usuario y la última columna tiene un carácter de nueva línea en el nombre.
    - **Causas**: actualmente, en el flujo de datos de asignación, el origen CSV de varias líneas no funciona con los caracteres \r\n como delimitador de filas. A veces, las líneas adicionales en los retornos de carros interrumpen los valores de origen. 
    - **Recomendación:** genere el archivo en el origen con el carácter \n como delimitador de filas, en lugar de \r\n. O bien, use la actividad de copia para convertir el archivo CSV con caracteres \r\n en \n como delimitador de filas.

## <a name="general-troubleshooting-guidance"></a>Guía de solución de problemas generales
1. Compruebe el estado de las conexiones del conjunto de datos. En cada transformación de origen y de receptor, visite el servicio vinculado para cada conjunto de datos que use y pruebe las conexiones.
2. Compruebe el estado de las conexiones de archivos y tablas desde el diseñador de flujo de datos. Cambie al modo de depuración y haga clic en Vista previa de datos en las transformaciones de origen para asegurarse de que puede acceder a los datos.
3. Si todo parece correcto desde la vista previa de los datos, vaya al diseñador de canalizaciones y coloque el flujo de datos en una actividad de canalización. Depure la canalización para realizar una prueba de un extremo a otro.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más ayuda para solucionar problemas, pruebe los siguientes recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)