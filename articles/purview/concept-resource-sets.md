---
title: Descripción de los conjuntos de recursos
description: En este artículo se explica qué son los conjuntos de recursos y cómo los crea Azure Purview.
author: yaronyg
ms.author: yarong
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 10/19/2020
ms.openlocfilehash: 55efa9443fd59b66a7677c9c460e473715f201df
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96551130"
---
# <a name="understanding-resource-sets"></a>Descripción de los conjuntos de recursos

Este artículo le ayuda a entender cómo Azure Purview usa conjuntos de recursos para asignar recursos de datos a recursos lógicos.

## <a name="background-info"></a>Información de contexto

Los sistemas de procesamiento de datos a escala normalmente almacenan una sola tabla en un disco en varios archivos. Este concepto se representa en Azure Purview mediante conjuntos de recursos. Un conjunto de recursos es un único objeto del catálogo que representa una gran cantidad de recursos en almacenamiento.

Por ejemplo, supongamos que el clúster de Spark ha conservado un dataframe en un origen de datos de ADLS Gen2. Aunque en Spark la tabla parece un único recurso lógico, en el disco es probable que haya miles de archivos de Parquet, cada uno de los cuales representa una partición del contenido total de un dataframe. Los datos de IoT y los datos de registro web presentan el mismo reto. Imagine que tiene un sensor que genera archivos de registro varias veces por segundo. No tardará mucho en tener cientos de miles de archivos de registro de ese único sensor.

Para abordar el reto de asignar un gran número de recursos de datos a un único recurso lógico, Azure Purview emplea conjuntos de recursos.

## <a name="how-azure-purview-detects-resource-sets"></a>Cómo detecta Azure Purview conjuntos de recursos

Azure Purview admite la detección de conjuntos de recursos solo en blobs de Azure, ADLS Gen1 y ADLS Gen2.

Azure Purview detecta automáticamente los conjuntos de recursos mediante una característica denominada detección automatizada de conjuntos de recursos. Esta característica examina todos los datos que se ingieren a través de un examen y los compara con un conjunto de patrones definidos.

Por ejemplo, supongamos que examina un origen de datos cuya dirección URL es `https://myaccount.blob.core.windows.net/mycontainer/machinesets/23/foo.parquet`. Azure Purview examina los segmentos de trazado y determina si coinciden con algún patrón integrado. Este servicio dispone de patrones integrados para GUID, números, formatos de fecha, códigos de localización (por ejemplo, en-US), etc. En este caso, el patrón de números coincide con *23*. Azure Purview presupone que este archivo forma parte de un conjunto de recursos denominado `https://myaccount.blob.core.windows.net/mycontainer/machinesets/{N}/foo.parquet`.

O bien, para una dirección URL como `https://myaccount.blob.core.windows.net/mycontainer/weblogs/en_au/23.json`, Azure Purview hace coincidir el patrón de localización y el patrón de números, lo que genera un conjunto de recursos denominado `https://myaccount.blob.core.windows.net/mycontainer/weblogs/{LOC}/{N}.json`.

Con esta estrategia, Azure Purview asignaría los siguientes recursos al mismo conjunto de recursos, `https://myaccount.blob.core.windows.net/mycontainer/weblogs/{LOC}/{N}.json`:

- `https://myaccount.blob.core.windows.net/mycontainer/weblogs/cy_gb/1004.json`
- `https://myaccount.blob.core.windows.net/mycontainer/weblogs/cy_gb/234.json`
- `https://myaccount.blob.core.windows.net/mycontainer/weblogs/de_Ch/23434.json`

> [!Note]
> Azure Data Lake Storage Gen2 ya está disponible con carácter general. Se recomienda que empiece a usarlo hoy mismo. Para más información, consulte la [página del producto](https://azure.microsoft.com/en-us/services/storage/data-lake-storage/).

## <a name="file-types-that-azure-purview-will-not-detect-as-resource-sets"></a>Tipos de archivo que Azure Purview no detectará como conjuntos de recursos

Purview no intenta clasificar a propósito la mayoría de los tipos de archivo de documento, como Word, Excel o PDF, como conjuntos de recursos. La excepción son los CSV, ya que es un formato de archivo común con particiones.

## <a name="how-azure-purview-scans-resource-sets"></a>Cómo Azure Purview examina los conjuntos de recursos

Cuando Azure Purview detecta recursos que considera forman parte de un conjunto de recursos, cambia de un examen completo a un examen de muestra. En un examen de muestra, se abre solo un subconjunto de los archivos que considera que están en el conjunto de recursos. Para cada archivo que se abre, usa su esquema y ejecuta sus clasificadores. A continuación, Azure Purview busca el recurso más reciente entre los recursos abiertos y usa el esquema y las clasificaciones de ese recurso con el conjunto de recursos completo del catálogo en la entrada.

## <a name="what-azure-purview-stores-about-resource-sets"></a>Qué almacena Azure Purview sobre los conjuntos de recursos

Además de los esquemas y las clasificaciones, Azure Purview almacena la siguiente información sobre los conjuntos de recursos:

- Datos del recurso de partición más reciente que examinó en profundidad.
- Información agregada sobre los recursos de partición que constituyen el conjunto de recursos.
- Un recuento de particiones que muestra cuántos recursos de partición encontró.
- Un recuento de esquemas que muestra el número de esquemas únicos que encontró en el conjunto de muestra que examinó en profundidad. Este valor es un número entre 1 y 5 o valores mayores que 5, 5+.
- Una lista de tipos de partición cuando se incluye más de un único tipo de partición en el conjunto de recursos. Por ejemplo, un sensor de IoT puede generar archivos XML y JSON, aunque ambos forman parte del mismo conjunto de recursos de manera lógica.

## <a name="built-in-resource-set-patterns"></a>Patrones de conjuntos de recursos integrados

Azure Purview admite los siguientes patrones de conjunto de recursos. Estos patrones pueden aparecer como un nombre en un directorio o como parte de un nombre de archivo.

| Nombre del patrón | Nombre para mostrar | Descripción |
|--------------|--------------|-------------|
| GUID         | {GUID}       | Un identificador único global, como se define en [RFC 4122](https://tools.ietf.org/html/rfc4122). |
| Número       | {N}          | Uno o más dígitos. |
| Formatos de fecha y hora | {N}     | Azure Purview admite distintos tipos de formatos de fecha y hora, pero todos se reducen a una serie de {N}s. |
| 4ByteHex     | {HEX}        | Un número hexadecimal de cuatro dígitos. |
| Localización | {LOC}        | Una etiqueta de idioma, como se define en [BCP 47](https://tools.ietf.org/html/bcp47). Azure Purview admite etiquetas que contienen un guion (-) o un carácter de subrayado (_). Por ejemplo, en_ca y en-ca. |

## <a name="issues-with-resource-sets"></a>Problemas con los conjuntos de recursos

Aunque los conjuntos de recursos funcionan bien en la mayoría de los casos, es posible que se encuentre con los siguientes problemas en los que Azure Purview:

- Marca incorrectamente un recurso como un conjunto de recursos
- Coloca un recurso en un conjunto de recursos equivocado
- Marca incorrectamente un recurso como que no es un conjunto de recursos

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con Data Catalog, consulte [Inicio rápido: Creación de una cuenta de Azure Purview](create-catalog-portal.md).
