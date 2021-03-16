---
title: Ejecución de la exportación mediante la invocación del comando $export en Azure API for FHIR
description: En este artículo se describe cómo exportar datos de FHIR mediante $export
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 2/19/2021
ms.author: cavoeg
ms.openlocfilehash: 9ed78baed35312b9a33c71a3e49b7e9dca22eb9f
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102487226"
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

Cuando se exportan los datos, se crea un archivo independiente para cada tipo de recurso. Para asegurarse de que los archivos exportados no sean demasiado grandes, se crea un nuevo archivo cuando el tamaño de un único archivo exportado supera los 64 MB. El resultado es que puede obtener varios archivos para cada tipo de recurso, los cuales se enumerarán (es decir, Patient-1.ndjson, Patient-2.ndjson). 


> [!Note] 
> `Patient/$export` y `Group/[ID]/$export` pueden exportar recursos duplicados si el recurso está en un compartimiento de más de un recurso o se encuentra en varios grupos.

Además, se admite la comprobación del estado de exportación a través de la dirección URL devuelta por el encabezado de ubicación durante la puesta en cola, junto con la cancelación del trabajo de exportación real.

### <a name="exporting-fhir-data-to-adls-gen2"></a>Exportación de datos de FHIR a ADLS Gen2

Actualmente se admiten $export para las cuentas de almacenamiento habilitadas para ADLS Gen2, con la siguiente limitación:

- El usuario no puede aprovechar todavía los [espacios de nombres jerárquicos](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-namespace); no hay ninguna manera de destinar la exportación a un subdirectorio específico dentro del contenedor. Solo se ofrece la posibilidad de destinar a un contenedor específico (donde crearemos una carpeta para cada exportación).

- Una vez completada la exportación, nunca se exporta nada de nuevo a esa carpeta, ya que las exportaciones posteriores al mismo contenedor estarán dentro de una carpeta recién creada.


## <a name="settings-and-parameters"></a>Configuración y parámetros

### <a name="headers"></a>Encabezados
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

## <a name="secure-export-to-azure-storage"></a>Exportación segura a Azure Storage

Azure API for FHIR admite una operación de exportación segura. Una opción para ejecutar una exportación segura consiste en permitir que las direcciones IP específicas asociadas a Azure API for FHIR accedan a la cuenta de almacenamiento de Azure. Dependiendo de si la cuenta de almacenamiento está en la misma ubicación o en otra diferente de Azure API for FHIR, las configuraciones son diferentes.

### <a name="when-the-azure-storage-account-is-in-a-different-region"></a>Si la cuenta de almacenamiento de Azure se encuentra en una región diferente

Seleccione la hoja Redes de la cuenta de almacenamiento de Azure en el portal. 

   :::image type="content" source="media/export-data/storage-networking.png" alt-text="Configuración de redes de Azure Storage." lightbox="media/export-data/storage-networking.png":::
   
Seleccione "Redes seleccionadas" y especifique la dirección IP en el cuadro **Intervalo de direcciones** en la sección del firewall. Agregue intervalos IP para permitir el acceso desde Internet o las redes locales. Puede encontrar la dirección IP en la tabla siguiente para la región de Azure donde se aprovisiona el servicio Azure API for FHIR.

|**Región de Azure**         |**Dirección IP pública** |
|:----------------------|:-------------------|
| Este de Australia       | 20.53.44.80       |
| Centro de Canadá       | 20.48.192.84      |
| Centro de EE. UU.           | 52.182.208.31     |
| Este de EE. UU.              | 20.62.128.148     |
| Este de EE. UU. 2            | 20.49.102.228     |
| EUAP de Este de EE. UU. 2       | 20.39.26.254      |
| Norte de Alemania        | 51.116.51.33      |
| Centro-oeste de Alemania | 51.116.146.216    |
| Japón Oriental           | 20.191.160.26     |
| Centro de Corea del Sur        | 20.41.69.51       |
| Centro-Norte de EE. UU     | 20.49.114.188     |
| Norte de Europa         | 52.146.131.52     |
| Norte de Sudáfrica   | 102.133.220.197   |
| Centro-sur de EE. UU.     | 13.73.254.220     |
| Sudeste de Asia       | 23.98.108.42      |
| Norte de Suiza    | 51.107.60.95      |
| Sur de Reino Unido             | 51.104.30.170     |
| Oeste de Reino Unido              | 51.137.164.94     |
| Centro-Oeste de EE. UU.      | 52.150.156.44     |
| Oeste de Europa          | 20.61.98.66       |
| Oeste de EE. UU. 2            | 40.64.135.77      |

### <a name="when-the-azure-storage-account-is-in-the-same-region"></a>Si la cuenta de almacenamiento de Azure se encuentra en la misma región

El proceso de configuración es el mismo que el anterior, salvo que en su lugar se usa un intervalo de direcciones IP específico en formato CIDR, 100.64.0.0/10. La razón por la que se debe especificar el intervalo de direcciones IP, que incluye 100.64.0.0 a 100.127.255.255, es que la dirección IP real utilizada por el servicio varía, aunque lo hará dentro del intervalo, para cada solicitud de $export.

> [!Note] 
> En su lugar, es posible que se use una dirección IP privada dentro del intervalo 10.0.2.0/24. En ese caso, la operación de $export no se realizará correctamente. Puede volver a intentar la solicitud de $export, pero no hay ninguna garantía de que se usará la próxima vez una dirección IP dentro del intervalo 100.64.0.0/10. Ese es el comportamiento de red conocido por diseño. La alternativa es configurar la cuenta de almacenamiento en una región diferente.
    
## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a exportar recursos de FHIR mediante el comando $export. A continuación, aprenda a exportar datos sin identificación:
 
>[!div class="nextstepaction"]
>[Exportación de datos sin identificación](de-identified-export.md)
