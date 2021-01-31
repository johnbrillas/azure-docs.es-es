---
title: Ejecución de la exportación mediante la invocación del comando $export en Azure API for FHIR
description: En este artículo se describe cómo exportar datos de FHIR mediante $export
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 1/21/2021
ms.author: cavoeg
ms.openlocfilehash: 48dbd0892c9ec02f203edba55d1104f1ab0118a8
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98737615"
---
# <a name="how-to-export-fhir-data"></a>Exportación de datos de FHIR


La característica de exportación masiva permite exportar datos desde el servidor de FHIR según la [especificación FHIR](https://hl7.org/fhir/uv/bulkdata/export/index.html). 

Antes de usar el comando $export, puede asegurarse de que Azure API for FHIR esté configurado para usarlo. Para configurar las opciones de exportación y crear una cuenta de almacenamiento de Azure, consulte la [página de configuración de los datos de exportación](configure-export-data.md).

## <a name="using-export-command"></a>Uso del comando $export

Después de configurar Azure API for FHIR para la exportación, puede usar el comando $export para exportar los datos fuera del servicio. Los datos se almacenarán en la cuenta de almacenamiento que especificó al configurar la exportación. Para más información sobre cómo invocar el comando $export en el servidor de FHIR, lea la documentación sobre la [especificación de $export del estándar FHIR de HL7](https://hl7.org/Fhir/uv/bulkdata/export/index.html). 

Azure API for FHIR admite $export en los siguientes niveles:
* [Sistema](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---system-level-export): `GET https://<<FHIR service base URL>>/$export>>`
* [Paciente](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---all-patients): `GET https://<<FHIR service base URL>>/Patient/$export>>`
* [Grupo de pacientes *](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---group-of-patients): Azure API for FHIR exporta todos los recursos relacionados, pero no exporta las características del grupo: `GET https://<<FHIR service base URL>>/Group/[ID]/$export>>`



> [!Note] 
> `Patient/$export` y `Group/[ID]/$export` pueden exportar recursos duplicados si el recurso está en un compartimiento de más de un recurso o se encuentra en varios grupos.

Además, se admite la comprobación del estado de exportación a través de la dirección URL devuelta por el encabezado de ubicación durante la puesta en cola, junto con la cancelación del trabajo de exportación real.

## <a name="settings-and-parameters"></a>Configuración y parámetros

### <a name="headers"></a>encabezados
Hay dos parámetros de encabezado necesarios que se deben establecer para los trabajos de $export. Los valores se definen mediante la [especificación de $export](https://hl7.org/Fhir/uv/bulkdata/export/index.html#headers) actual.
* **Accept** (Aceptar): application/fhir+json.
* **Prefer** (Preferir): respond-async.

### <a name="query-parameters"></a>Parámetros de consulta
Azure API for FHIR admite los siguientes parámetros de consulta. Todos estos parámetros son opcionales:
|Parámetro de consulta        | ¿Se define mediante la especificación de FHIR?    |  Descripción|
|------------------------|---|------------|
| \_outputFormat | Sí | Actualmente admite tres valores que se alinean con la especificación de FHIR: application/fhir+ndjson, application/ndjson o just ndjson. Todos los trabajos de exportación devolverán `ndjson` y el valor pasado no tiene ningún efecto sobre el comportamiento del código. |
| \_since | Sí | Permite exportar solo los recursos que se han modificado desde el momento indicado. |
| \_type | Sí | Permite especificar los tipos de recursos que se van a incluir. Por ejemplo, \_type=Patient devolvería solo los recursos de pacientes.|
| \_typefilter | Sí | Para solicitar un filtrado más preciso, puede usar \_typefilter junto con el parámetro \_type. El valor del parámetro _typeFilter es una lista separada por comas de consultas de FHIR que restringen aún más los resultados. |
| \_container | No |  Especifica el contenedor en la cuenta de almacenamiento configurada donde se deben exportar los datos. Si se especifica un contenedor, los datos se exportarán a este en una nueva carpeta con el nombre. Si no se especifica, se exportarán a un nuevo contenedor con la marca de tiempo y el identificador de trabajo. |


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a exportar recursos de FHIR mediante el comando $export. A continuación, aprenda a exportar datos sin identificación:
 
>[!div class="nextstepaction"]
>[Exportación de datos sin identificación](de-identified-export.md)
