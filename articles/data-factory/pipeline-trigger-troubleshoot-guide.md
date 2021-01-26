---
title: Solución de problemas relacionados con orquestaciones y desencadenadores de canalizaciones en Azure Data Factory
description: Use distintos métodos para solucionar problemas de desencadenadores de canalizaciones en Azure Data Factory.
author: ssabat
ms.service: data-factory
ms.date: 12/15/2020
ms.topic: troubleshooting
ms.author: susabat
ms.reviewer: susabat
ms.openlocfilehash: 0ceee3c65e8c4df5d843bb441fb6426a0f4eb696
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2021
ms.locfileid: "98220276"
---
# <a name="troubleshoot-pipeline-orchestration-and-triggers-in-azure-data-factory"></a>Solución de problemas relacionados con orquestaciones y desencadenadores de canalizaciones en Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Una ejecución de canalización en Azure Data Factory define una instancia de dicha ejecución. Por ejemplo, supongamos que tiene una canalización que se ejecuta a las 8:00 a. m., 9:00 a. m. y 10:00 a. m. En este caso, hay tres ejecuciones de canalización independientes. Cada canalización ejecutar tiene un identificador de canalización único. Un id. de ejecución es un identificador único global (GUID) que define esa ejecución de canalización concreta.

Normalmente las instancias de ejecuciones de canalización se crean al pasar argumentos a parámetros que se definen en las canalizaciones. Puede ejecutar una canalización manualmente o mediante un desencadenador. Consulte [Ejecución y desencadenadores de canalización en Azure Data Factory](concepts-pipeline-execution-triggers.md) para más información.

## <a name="common-issues-causes-and-solutions"></a>Problemas comunes, causas y soluciones

### <a name="an-azure-functions-app-pipeline-throws-an-error-with-private-endpoint-connectivity"></a>La canalización de una aplicación de Azure Functions produce un error con la conectividad de puntos de conexión privados
 
Data Factory y una aplicación de Azure Functions se ejecutan en un punto de conexión privado. Está intentando ejecutar una canalización que interactúa con la aplicación de funciones. Ha probado tres métodos diferentes, pero uno devuelve el error "solicitud incorrecta" y los otros dos métodos devuelven "error 103 prohibido".

**Causa**: Data Factory no admite actualmente un conector de punto de conexión privado para las aplicaciones de funciones. Azure Functions rechaza las llamadas porque está configurado para permitir solo las conexiones de un vínculo privado.

**Solución:** Cree un punto de conexión de **PrivateLinkService** y especifique el DNS de la aplicación de funciones.

### <a name="a-pipeline-run-is-canceled-but-the-monitor-still-shows-progress-status"></a>Se cancela una ejecución de la canalización, pero el monitor sigue mostrando el mantenimiento en curso

Cuando se cancela una ejecución de canalización, la supervisión de la canalización sigue mostrando el estado En curso. Esto sucede debido a un problema de caché del explorador web. También es posible que no tenga los filtros de supervisión correctos.

**Solución:** Actualice el explorador y aplique los filtros de supervisión adecuados.
 
### <a name="you-see-a-delimitedtextmorecolumnsthandefined-error-when-copying-a-pipeline"></a>Aparece un error "DelimitedTextMoreColumnsThanDefined" al copiar una canalización
 
Si una carpeta que está copiando contiene archivos con esquemas diferentes, como un número variable de columnas, delimitadores diferentes, valores de carácter de comillas o algún problema de datos, la canalización de Data Factory podría devolver este error:

`
Operation on target Copy_sks  failed: Failure happened on 'Sink' side.
ErrorCode=DelimitedTextMoreColumnsThanDefined,
'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
Message=Error found when processing 'Csv/Tsv Format Text' source '0_2020_11_09_11_43_32.avro' with row number 53: found more columns than expected column count 27.,
Source=Microsoft.DataTransfer.Common,'
`

**Solución:** Seleccione la opción **Binary Copy** (Copia binaria) al crear la actividad de copia. De esta manera, para realizar copias masivas o migrar los datos de un lago de datos a otro, Data Factory no abrirá los archivos para leer el esquema. En su lugar, Data Factory tratará cada archivo como un archivo binario y cópielo en la otra ubicación.

### <a name="a-pipeline-run-fails-when-you-reach-the-capacity-limit-of-the-integration-runtime"></a>Se produce un error de ejecución de canalización cuando se alcanza el límite de capacidad del entorno de ejecución de integración

Mensaje de error:

`
Type=Microsoft.DataTransfer.Execution.Core.ExecutionException,Message=There are substantial concurrent MappingDataflow executions which is causing failures due to throttling under Integration Runtime 'AutoResolveIntegrationRuntime'.
`

**Causa**: Ha alcanzado el límite de capacidad del entorno de ejecución de integración. Podría estar ejecutando una gran cantidad de flujos de datos mediante el mismo entorno de ejecución de integración de manera simultánea. Para más información, consulte [Límites, cuotas y restricciones de suscripción y servicios de Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#version-2)).

**Solución:**
 
- Ejecute las canalizaciones en distintos momentos del desencadenador.
- Cree un nuevo entorno de ejecución de integración y divida sus canalizaciones entre varios entornos de ejecución de integración.

### <a name="you-have-activity-level-errors-and-failures-in-pipelines"></a>Hay errores en el nivel de actividad y en las canalizaciones

La orquestación de Azure Data Factory permite la lógica condicional y permite al usuario tomar diferentes rutas de acceso en función del resultado de una actividad anterior. Permite cuatro rutas de acceso condicionales: **Upon Success** (ruta predeterminada) (en caso de ejecución correcta), **Upon Failure** (en caso de error), **Upon Completion** (en caso de finalización) y **Upon Skip** (en caso de omisión). 

Azure Data Factory evalúa el resultado de todas las actividades de nivel de hoja. Los resultados de la canalización son correctos solo si todas las actividades del nivel de hoja se realizan correctamente. Si se omitió una actividad del nivel hoja, se evalúa su actividad primaria en su lugar. 

**Resolución**

1. Implemente comprobaciones en el nivel de actividad con las indicaciones de [Control de errores de canalización](https://techcommunity.microsoft.com/t5/azure-data-factory/understanding-pipeline-failures-and-error-handling/ba-p/1630459).
1. Use Azure Logic Apps para supervisar las canalizaciones a intervalos regulares con las indicaciones de [Consulta por factoría](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory).

## <a name="monitor-pipeline-failures-in-regular-intervals"></a>Supervisión de errores de canalización en intervalos regulares

Quizá necesite supervisar las canalizaciones de Data Factory con errores a intervalos, por ejemplo, de 5 minutos. Puede consultar y filtrar las ejecuciones de canalización de una factoría de datos con el punto de conexión. 

Configure una aplicación lógica de Azure para consultar todas las canalizaciones con error cada 5 minutos como se describe en [Consulta por factoría](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory). A continuación, puede notificar los incidentes en nuestro sistema de incidencias.

Para obtener más información, diríjase a [Envío de notificaciones desde Data Factory, parte 2](https://www.mssqltips.com/sqlservertip/5962/send-notifications-from-an-azure-data-factory-pipeline--part-2/).

## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda para solucionar problemas, pruebe estos recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Página de preguntas y respuestas de Microsoft](/answers/topics/azure-data-factory.html)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)
