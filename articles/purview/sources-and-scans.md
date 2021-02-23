---
title: Tipos de archivo y orígenes de datos admitidos
description: En este artículo se proporcionan detalles conceptuales sobre los orígenes de datos y los tipos de archivo admitidos en Purview.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/24/2020
ms.custom: references_regions
ms.openlocfilehash: 9a73f9b734d5404d07e05dd37d5ad8571c1aab2e
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100383898"
---
# <a name="supported-data-sources-and-file-types-in-azure-purview"></a>Tipos de archivo y orígenes de datos admitidos en Azure Purview

En este artículo se describen los orígenes de datos, los tipos de archivo y los conceptos de examen admitidos en Purview.

## <a name="supported-data-sources"></a>Orígenes de datos admitidos

Azure Purview admite los siguientes orígenes:

| Tipo de almacén | Tipo de autenticación admitido | Configuración de exámenes mediante experiencia de usuario/PowerShell |
| ---------- | ------------------- | ------------------------------ |
| SQL Server local                   | Autenticación de SQL                        | Experiencia de usuario                                |
| Azure Synapse Analytics (anteriormente SQL DW)            | Autenticación de SQL, entidad de servicio, MSI               | Experiencia de usuario                             |
| Azure SQL Database (DB)                  | Autenticación de SQL, entidad de servicio, MSI               | Experiencia de usuario |
| Instancia administrada de Azure SQL Database      | Autenticación de SQL, entidad de servicio, MSI               | Experiencia de usuario    |
| Azure Blob Storage                       | Clave de cuenta, entidad de servicio, MSI | Experiencia de usuario            |
| Explorador de datos de Azure                      | Entidad de servicio                              | Experiencia de usuario            |
| Azure Data Lake Storage Gen1 (ADLS Gen1) | Entidad de servicio, MSI                              | Experiencia de usuario            |
| Azure Data Lake Storage Gen2 (ADLS Gen2) | Clave de cuenta, entidad de servicio, MSI            | Experiencia de usuario            |
| Azure Cosmos DB                          | Clave de cuenta                                    | Experiencia de usuario            |


> [!Note]
> Azure Data Lake Storage Gen2 ya está disponible con carácter general. Se recomienda que empiece a usarlo hoy mismo. Para más información, consulte la [página del producto](https://azure.microsoft.com/en-us/services/storage/data-lake-storage/).

## <a name="file-types-supported-for-scanning"></a>Tipos de archivo admitidos para examen

Los siguientes tipos de archivo se admiten para los exámenes y para la extracción y clasificación de esquemas, si procede:

- Formatos de archivo estructurados admitidos por extensión: AVRO, ORC, PARQUET, CSV, JSON, PSV, SSV, TSV, TXT, XML
- Formatos de archivo de documento admitidos por extensión: DOC, DOCM, DOCX, DOT, ODP, ODS, ODT, PDF, POT, PPS, PPSX, PPT, PPTM, PPTX, XLC, XLS, XLSB, XLSM, XLSX, XLT
- Purview también admite extensiones de archivo personalizadas y analizadores personalizados.

## <a name="sampling-within-a-file"></a>Muestreo en un archivo

En la terminología de Purview:
- Examen L1: extrae información básica y metadatos como el nombre de archivo, el tamaño y el nombre completo
- Examen L2: extrae el esquema de los tipos de archivo estructurados y las tablas de base de datos
- Examen L3: extrae el esquema cuando proceda y somete el archivo muestreado a reglas de clasificación personalizadas y del sistema.

En todos los formatos de archivo estructurados, el examen de Purview muestrea los archivos de la siguiente manera:

- En el caso de los tipos de archivo estructurados, muestrea 128 filas de cada columna o 1 MB, lo que sea menor.
- En el caso de los formatos de archivo de documento, muestrea 20 MB de cada archivo.
    - Si un archivo de documento es mayor de 20 MB, no estará sujeto a un examen profundo (sujeto a clasificación). En tal caso, Purview captura solo metadatos básicos como el nombre de archivo y el nombre completo.

## <a name="resource-set-file-sampling"></a>Muestreo de archivos del conjunto de recursos

En Purview, una carpeta o grupo de archivos de partición se detecta como un *conjunto de recursos* si coincide con una directiva de conjuntos de recursos del sistema o con una directiva de conjuntos de recursos definida por el cliente. Si se detecta un conjunto de recursos, Purview muestreará cada carpeta que contenga. Consulte [aquí](concept-resource-sets.md) más información sobre los conjuntos de recursos.

Muestreo de archivos para conjuntos de recursos por tipos de archivo:

- **Archivos delimitados (CSV, PSV, SSV, TSV)** : se muestrea uno de cada 100 archivos (examen L3) de una carpeta o grupo de archivos de partición que se considere un grupo de recursos.
- **Tipos de archivo de Data Lake (Parquet, Avro, Orc)** : se muestrea uno de cada 18446744073709551615 archivos (longitud máxima) (examen L3) de una carpeta o grupo de archivos de partición que se considere *grupo de recursos*.
- **Otros tipos de archivo estructurados (JSON, XML, TXT)** : se muestrea uno de cada 100 archivos (examen L3) de una carpeta o grupo de archivos de partición que se considere un grupo de recursos.
- **Objetos SQL y entidades de CosmosDB**: cada archivo se somete a un examen L3.
- **Tipos de archivo de documento**: cada archivo se somete a un examen L3. Los patrones de conjuntos de recursos no se aplican a estos tipos de archivo.

## <a name="scan-regions"></a>Regiones de la exploración
A continuación se muestra una lista de todas las regiones de origen de datos de Azure (centro de datos) donde se ejecuta el explorador de Purview. Si el origen de datos de Azure se encuentra en una región que no está en esta lista, el explorador se ejecutará en la región de la instancia de Purview.
 
### <a name="purview-scanner-regions"></a>Regiones del explorador de Purview

- EastUs
- EastUs2 
- SouthCentralUS
- WestUs
- WestUs2
- SoutheastAsia
- Oeste de Europa
- Norte de Europa
- UkSouth
- AustraliaEast
- CanadaCentral
- BrazilSouth
- CentralIndia
- JapanEast
- SouthAfricaNorth
- FranceCentral

## <a name="classification"></a>clasificación

Las 105 reglas de clasificación del sistema se aplican a los formatos de archivo estructurados. Solo las reglas de clasificación de MCE se aplican a los tipos de archivo de documento (no los patrones de expresiones regulares nativos del examen de datos, detección basada en filtros de Bloom). Para más información sobre las clasificaciones admitidas, consulte [Clasificaciones admitidas en Azure Purview](supported-classifications.md).

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: ejecución de Starter Kit y examen de datos](tutorial-scan-data.md)
- [Administración de orígenes de datos en Azure Purview (versión preliminar)](manage-data-sources.md)