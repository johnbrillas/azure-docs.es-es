---
title: Solución de problemas relacionados con orquestaciones y desencadenadores de canalizaciones en Azure Data Factory
description: Use distintos métodos para solucionar problemas de desencadenadores de canalizaciones en Azure Data Factory.
author: ssabat
ms.service: data-factory
ms.date: 12/15/2020
ms.topic: troubleshooting
ms.author: susabat
ms.reviewer: susabat
ms.openlocfilehash: 0e67a316b012eda61607c84edfd8e10d6aa3318d
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97589175"
---
# <a name="troubleshoot-pipeline-orchestration-and-triggers-in-azure-data-factory"></a>Solución de problemas relacionados con orquestaciones y desencadenadores de canalizaciones en Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Una ejecución de canalización en Azure Data Factory define una instancia de dicha ejecución. Por ejemplo, supongamos que tiene una canalización que se ejecuta a las 8:00 a. m., 9:00 a. m. y 10:00 a. m. En este caso, hay tres ejecuciones independientes de la canalización o ejecuciones de canalización. Cada canalización ejecutar tiene un identificador de canalización único. Un identificador de ejecución es un GUID (identificador único global) que define esa ejecución de canalización concreta.

Normalmente las instancias de ejecuciones de canalización se crean al pasar argumentos a parámetros que se definen en las canalizaciones. Puede ejecutar una canalización manualmente o mediante un desencadenador. Consulte [Ejecución y desencadenadores de canalización en Azure Data Factory](concepts-pipeline-execution-triggers.md) para más información.

## <a name="common-issues-causes-and-solutions"></a>Problemas comunes, causas y soluciones

### <a name="pipeline-with-azure-function-throws-error-with-private-end-point-connectivity"></a>La canalización con Azure Function produce un error con la conectividad de puntos de conexión privados
 
#### <a name="issue"></a>Problema
En algunos contextos, Data Factory y una aplicación de funciones de Azure se ejecutan en un punto de conexión privado. Está intentando obtener una canalización que interactúa con la aplicación de funciones de Azure para trabajar. Ha probado tres métodos diferentes, pero uno devuelve el error `Bad Request`, los otros dos métodos devuelven `103 Error Forbidden`.

#### <a name="cause"></a>Causa 
Data Factory no admite actualmente un conector de punto de conexión privado para la aplicación de funciones de Azure. Este debe ser el motivo por el que la aplicación de funciones de Azure está rechazando las llamadas, ya que estaría configurado para permitir solo las conexiones desde una instancia de Private Link.

#### <a name="resolution"></a>Solución
Puede crear un punto de conexión privado del tipo **PrivateLinkService** y proporcionar el DNS de su aplicación de funciones y, con eso, la conexión debería funcionar.

### <a name="pipeline-run-is-killed-but-the-monitor-still-shows-progress-status"></a>La ejecución de la canalización se ha terminado, pero el monitor sigue mostrando el estado En curso

#### <a name="issue"></a>Problema
A menudo, cuando se termina una ejecución de canalización, la supervisión de la canalización sigue mostrando el estado En curso. Esto sucede debido al problema de la memoria caché en el explorador y al hecho de no contar con los filtros adecuados para la supervisión.

#### <a name="resolution"></a>Solución
Actualice el explorador y aplique los filtros adecuados para la supervisión.
 
### <a name="copy-pipeline-failure--found-more-columns-than-expected-column-count-delimitedtextmorecolumnsthandefined"></a>Error en la canalización de copia: se encontraron más columnas que el número de columnas esperado (DelimitedTextMoreColumnsThanDefined)

#### <a name="issue"></a>Problema  
Si el contenido de una carpeta determinada que está copiando contiene archivos con esquemas diferentes, como un número variable de columnas, delimitadores diferentes, valores de carácter de comillas o algún problema de datos, la canalización de Data Factory terminará ejecutándose con este error:

`
Operation on target Copy_sks  failed: Failure happened on 'Sink' side.
ErrorCode=DelimitedTextMoreColumnsThanDefined,
'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
Message=Error found when processing 'Csv/Tsv Format Text' source '0_2020_11_09_11_43_32.avro' with row number 53: found more columns than expected column count 27.,
Source=Microsoft.DataTransfer.Common,'
`

#### <a name="resolution"></a>Solución
Seleccione la opción "Binary Copy" (Copia binaria) al crear la actividad Copiar datos. De esta forma, para la copia masiva o la migración de los datos desde un lago de datos a otro, con la opción **binaria**, Data Factory no abrirá los archivos para leer el esquema, sino que simplemente tratará cada archivo como binario y los copiará en la otra ubicación.

### <a name="pipeline-run-fails-when-capacity-limit-of-integration-runtime-is-reached"></a>Se produce un error de ejecución de canalización cuando se alcanza el límite de capacidad del entorno de ejecución de integración

#### <a name="issue"></a>Problema
Mensaje de error:

`
Type=Microsoft.DataTransfer.Execution.Core.ExecutionException,Message=There are substantial concurrent MappingDataflow executions which is causing failures due to throttling under Integration Runtime 'AutoResolveIntegrationRuntime'.
`

El error indica la limitación por entorno de ejecución de integración, que actualmente es 50. Consulte [Límites](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#version-2) para más información.

Si ejecuta una gran cantidad de flujos de datos mediante el mismo entorno de ejecución de integración de manera simultánea, este podría ser el motivo del error.

#### <a name="resolution"></a>Solución 
- Separe estas canalizaciones para que se ejecute el desencadenador en horas diferentes.
- Cree un nuevo entorno de ejecución de integración y divida estas canalizaciones entre varios entornos de ejecución de integración.

### <a name="how-to-monitor-pipeline-failures-on-regular-interval"></a>Supervisión de errores de canalización a intervalos regulares

#### <a name="issue"></a>Problema
A menudo, es necesario supervisar las canalizaciones de Data Factory a intervalos, por ejemplo, de 5 minutos. Puede consultar y filtrar las ejecuciones de canalización de una factoría de datos mediante el punto de conexión. 

#### <a name="recommendation"></a>Recomendación
1. Configure una aplicación lógica de Azure para consultar todas las canalizaciones con error cada 5 minutos.
2. A continuación, puede notificar los incidentes en nuestro sistema de incidencias según [QueryByFactory](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory).

#### <a name="reference"></a>Referencia
- [Envío de notificaciones externas desde Data Factory](https://www.mssqltips.com/sqlservertip/5962/send-notifications-from-an-azure-data-factory-pipeline--part-2/)

### <a name="how-to-handle-activity-level-errors-and-failures-in-pipelines"></a>Control de errores en el nivel de actividad y en las canalizaciones

#### <a name="issue"></a>Problema
La orquestación de Azure Data Factory permite la lógica condicional y permite al usuario tomar diferentes rutas de acceso en función de los resultados de una actividad anterior. Permite cuatro rutas de acceso condicionales: "Upon Success (ruta predeterminada)" ("En caso de ejecución correcta"), "Upon Failure" ("En caso de error"), "Upon Completion" ("En caso de finalización") y "Upon Skip" ("En caso de omisión"). Se permite el uso de diferentes rutas de acceso.

Azure Data Factory define las ejecuciones de canalización correctas y erróneas como se indica a continuación:

- Evalúe el resultado de todas las actividades del nivel hoja. Si se omitió una actividad del nivel hoja, se evalúa su actividad primaria en su lugar.
- El resultado de la canalización es correcto si y solo si todas las actividades del nivel hoja se realizan correctamente.

#### <a name="recommendation"></a>Recomendación
- Implemente comprobaciones en el nivel de actividad con las indicaciones de [Control de errores de canalización](https://techcommunity.microsoft.com/t5/azure-data-factory/understanding-pipeline-failures-and-error-handling/ba-p/1630459).
- Use Azure Logic Apps para supervisar las canalizaciones a intervalos regulares con las indicaciones de [Consulta por factoría]( https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory).

## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda para solucionar problemas, pruebe estos recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Página de preguntas y respuestas de Microsoft](/answers/topics/azure-data-factory.html)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)