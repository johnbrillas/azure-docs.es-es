---
title: ¿Qué es la traducción de documentos?
description: Información general sobre el proceso y el servicio de traducción de documentos por lotes basados en la nube.
ms.topic: overview
manager: nitinme
ms.author: lajanuar
author: laujan
ms.date: 02/11/2021
ms.openlocfilehash: fc75bfc3b79cd6aed51a44fc47787532a9d61e8b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101713021"
---
# <a name="what-is-document-translation-preview"></a>¿Qué es la traducción de documentos (versión preliminar)?

La traducción de documentos es una característica basada en la nube del servicio [Azure Translator](../translator-info-overview.md) y forma parte de la familia de Azure Cognitive Services de las API de REST. La API de traducción de documentos traduce documentos a y desde 90 idiomas y dialectos manteniendo la estructura del documento y el formato de los datos.

## <a name="document-translation-key-features"></a>Características clave de la traducción de documentos

| Característica | Descripción |
| ---------| -------------|
| **Traducir archivos grandes**| Traducir documentos completos de forma asincrónica.|
|**Traducir numerosos archivos**|Traducir varios archivos a y desde 90 idiomas y dialectos.|
|**Conservar la presentación del archivo de origen**| Traducir archivos conservando el diseño y el formato originales.|
|**Aplicar traducción personalizada**| Traducir documentos con modelos de [traducción personalizada](../customization.md#custom-translator) y general.|
|**Aplicar glosarios personalizados**|Traducir documentos mediante glosarios personalizados.|

## <a name="how-to-get-started"></a>Cómo empezar

En nuestra guía de paso a paso, aprenderá a empezar a trabajar rápidamente con el traductor de documentos. Para empezar, necesitará una [cuenta de Azure](https://azure.microsoft.com/free/cognitive-services/) activa.  En caso de no tener ninguna, puede [crear una gratis](https://azure.microsoft.com/free).

> [!div class="nextstepaction"]
> [Introducción](get-started-with-document-translation.md)

## <a name="supported-document-formats"></a>Formatos de documento admitidos

Los siguientes tipos de archivo de documento son compatibles con la traducción de documentos:

| Tipo de archivo| Extensión de archivo|Descripción|
|---|---|--|
|PDF de Adobe|.pdf|Portable Document Format de Adobe Acrobat|
|HTML|.html|Lenguaje de marcado de hipertexto|
|Formato de archivo de intercambio de localización|.xlf. , xliff| Formato de documento paralelo que se exporta desde los sistemas de memoria de traducción. Los idiomas utilizados se definen dentro del archivo.|
|Microsoft Excel|.xlsx|Archivo de hoja de cálculo para el análisis de datos y la documentación|
|Microsoft Outlook|.msg|Mensaje de correo electrónico creado o guardado en Microsoft Outlook|
|Microsoft PowerPoint|.pptx| Archivo de presentación utilizado para mostrar contenido en formato de presentación|
|Microsoft Word|.docx| Archivo de documento de texto|
|Valores separados por tabulaciones/TAB|.tsv/.tab| Archivo de datos sin formato delimitado por tabulaciones que usan los programas de hojas de cálculo|
|Texto|.txt| Documento de texto sin formato|
|Intercambio de memoria de traducción|.tmx|Estándar Open XML que se usa para intercambiar los datos de la memoria de traducción (MT) creados mediante aplicaciones de traducción asistida por ordenador (CAT) y localización|

## <a name="supported-glossary-formats"></a>Formatos de glosario compatibles

Los siguientes tipos de archivo de glosario son compatibles con la traducción de documentos:

| Tipo de archivo| Extensión de archivo|Descripción|
|---|---|--|
|Formato de archivo de intercambio de localización|.xlf. , xliff| Formato de documento paralelo que se exporta desde los sistemas de memoria de traducción. Los idiomas utilizados se definen dentro del archivo.|
|Valores separados por tabulaciones/TAB|.tsv/.tab| Archivo de datos sin formato delimitado por tabulaciones que usan los programas de hojas de cálculo|

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción a la traducción de documentos](get-started-with-document-translation.md)
>
>
