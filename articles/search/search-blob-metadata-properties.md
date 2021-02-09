---
title: Propiedades de metadatos de contenido
titleSuffix: Azure Cognitive Search
description: Las propiedades de los metadatos de los blobs pueden proporcionar contenido a los campos de un índice de búsqueda, o bien datos que informan del comportamiento de la indexación en tiempo de ejecución. En este artículo se enumeran las propiedades de metadatos que se admiten en Azure Cognitive Search.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/03/2021
ms.openlocfilehash: 79443785dbd8619e22358631c9c36a3da4ef2e84
ms.sourcegitcommit: 445ecb22233b75a829d0fcf1c9501ada2a4bdfa3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99477290"
---
# <a name="content-metadata-properties-used-in-blob-indexing-in-azure-cognitive-search"></a>Propiedades de los metadatos de contenido que se usan en la indexación de blobs en Azure Cognitive Search

Los blobs pueden tener contenidos muy diversos y muchos de ellos tienen propiedades de metadatos que pueden ser útiles para la indexación de blobs. Del mismo modo que se pueden crear campos de búsqueda para las propiedades de estándar de los blobs, como **`metadata_storage_name`** , se puede crear para las propiedades de los metadatos que son específicas de un formato de documento.

## <a name="supported-document-formats"></a>Formatos de documento admitidos

Cognitive Search admite la indexación de blobs para los siguientes formatos de documento:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="properties-by-document-format"></a>Propiedades por formato de documento

La tabla siguiente resume el procesamiento que se realiza para cada formato de documento y describe las propiedades de metadatos que extrae un indexador de blobs.

| Formato de documento/Tipo de contenido | Metadatos extraídos | Detalles de procesamiento |
| --- | --- | --- |
| HTML (text/html) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Seccionar el marcado HTML y extraer texto |
| PDF (application/pdf) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Extraer texto, incluyendo los documentos insertados (excepto las imágenes) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extraer texto, incluyendo los documentos insertados |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extraer texto, incluyendo los documentos insertados |
| DOCM (application/vnd.ms-word.document.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extraer texto, incluyendo los documentos insertados |
| WORD XML (application/vnd.ms-word2006ml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Seccionar el marcado XML y extraer texto |
| WORD 2003 XML (application/vnd.ms-wordml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date` |Seccionar el marcado XML y extraer texto |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extraer texto, incluyendo los documentos insertados |
| XLS (application/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extraer texto, incluyendo los documentos insertados |
| XLSM (application/vnd.ms-excel.sheet.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extraer texto, incluyendo los documentos insertados |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extraer texto, incluyendo los documentos insertados |
| PPT (application/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extraer texto, incluyendo los documentos insertados |
| PPTM (application/vnd.ms-powerpoint.presentation.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extraer texto, incluyendo los documentos insertados |
| MSG (application/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_from_email`<br/>`metadata_message_to`<br/>`metadata_message_to_email`<br/>`metadata_message_cc`<br/>`metadata_message_cc_email`<br/>`metadata_message_bcc`<br/>`metadata_message_bcc_email`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Extraer texto, incluyendo el texto extraído de los datos adjuntos. `metadata_message_to_email`, `metadata_message_cc_email` y `metadata_message_bcc_email` son colecciones de cadenas, el resto de los campos son cadenas.|
| ODT (application/vnd.oasis.opendocument.text) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extraer texto, incluyendo los documentos insertados |
| ODS (application/vnd.oasis.opendocument.spreadsheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extraer texto, incluyendo los documentos insertados |
| ODP (application/vnd.oasis.opendocument.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`title` |Extraer texto, incluyendo los documentos insertados |
| ZIP (application/zip) |`metadata_content_type` |Extraer el texto de todos los documentos en el archivo |
| GZ (application/gzip) |`metadata_content_type` |Extraer el texto de todos los documentos en el archivo |
| EPUB (application/epub+zip) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_title`<br/>`metadata_description`<br/>`metadata_language`<br/>`metadata_keywords`<br/>`metadata_identifier`<br/>`metadata_publisher` |Extraer el texto de todos los documentos en el archivo |
| XML (application/xml) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> |Seccionar el marcado XML y extraer texto |
| JSON (application/json) |`metadata_content_type`<br/>`metadata_content_encoding` |Extraer texto<br/>NOTA:  Si necesita extraer varios campos de documentos de un blob JSON, consulte [Indexación de blobs JSON](search-howto-index-json-blobs.md) para detalles. |
| EML (message/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Extraer texto, incluidos los datos adjuntos |
| RTF (aplicación/rtf) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_page_count`<br/>`metadata_word_count`<br/> | Extraer texto|
| Plain text (text/plain) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> | Extraer texto|

## <a name="see-also"></a>Consulta también

* [Indexadores de Azure Cognitive Search](search-indexer-overview.md)
* [Uso de la inteligencia artificial para comprender los datos de Blob Storage](search-blob-ai-integration.md)
* [Introducción a la indexación de blobs](search-blob-storage-integration.md)