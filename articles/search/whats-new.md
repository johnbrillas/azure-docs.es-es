---
title: Novedades de Azure Cognitive Search
description: Anuncios de características nuevas y mejoradas, como el cambio de nombre del servicio Azure Search por Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 02/09/2021
ms.custom: references_regions
ms.openlocfilehash: 13cb22c178be29af71b57d0f50fdbd0e95718069
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100104610"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Novedades de Azure Cognitive Search

Conozca las novedades del servicio. Marque esta página para mantenerse actualizado con el servicio. Consulte la [lista de características en versión preliminar](search-api-preview.md) para ver las características de la versión preliminar pública.

## <a name="february-2021"></a>Febrero de 2021

|Característica&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  |  Descripción | Disponibilidad  |
|------------------------------|---------------|---------------|
| [Restablecimiento de documentos (versión preliminar)](search-howto-run-reset-indexers.md) |  Vuelve a procesar documentos de búsqueda seleccionados individualmente en cargas de trabajo de indexador. | [API REST Search 2020-06-30-Preview](/rest/api/searchservice/index-preview) |
| [Zonas de disponibilidad](search-performance-optimization.md#availability-zones)| Los servicios de búsqueda con dos o más réplicas en determinadas regiones, como se indica en [este artículo](search-performance-optimization.md#availability-zones), obtienen resistencia mediante réplicas en dos o más ubicaciones físicas distintas.  | La región y la fecha de creación del servicio de búsqueda determinan la disponibilidad. Consulte el documento de optimización del rendimiento para más información. |

## <a name="january-2021"></a>Enero de 2021

|Característica&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  |  Descripción | Disponibilidad  |
|------------------------------|-------------|---------------|
| [Acelerador de soluciones para Azure Cognitive Search y QnA Maker](https://github.com/Azure-Samples/search-qna-maker-accelerator) | Extrae las preguntas y respuestas del documento y sugiere las respuestas más relevantes. Puede encontrar una aplicación de demostración en directo en [https://aka.ms/qnaWithAzureSearchDemo](https://aka.ms/qnaWithAzureSearchDemo).  | Proyecto de código abierto (sin Acuerdo de Nivel de Servicio) |

## <a name="2020-archive"></a>Archivo de 2020

| Month (Mes) | Característica | Descripción |
|-------|---------|-------------|
| Noviembre | [Cifrado de claves administrado por el cliente (ampliación)](search-security-manage-encryption-keys.md) | Amplía el cifrado administrado por el cliente a toda la gama de recursos creados y administrados por un servicio de búsqueda. Disponible con carácter general.|
| Septiembre | [Extensión de Visual Studio Code para Azure Cognitive Search](search-get-started-vs-code.md) | Agrega un área de trabajo, navegación, IntelliSense y plantillas para crear índices, indexadores, orígenes de datos y habilidades. | Versión preliminar pública |
| Septiembre | [Managed Service Identity (indexadores)](search-howto-managed-identities-data-sources.md) | Disponible con carácter general.  |
| Septiembre | [Solicitudes salientes mediante un vínculo privado](search-indexer-howto-access-private.md) | Disponible con carácter general.  |
| Septiembre | [API REST de administración (2020-08-01)](/rest/api/searchmanagement/management-api-versions) | Disponible con carácter general. |
| Septiembre | [API REST de administración (2020-08-01-Preview)](/rest/api/searchmanagement/management-api-versions) | Agrega un recurso compartido de Private Link para Azure Functions y Azure SQL para bases de datos MySQL. |
| Septiembre | [SDK 4.0 de .NET de administración](/dotnet/api/overview/azure/search/management) |  Actualización del SDK de Azure para el SDK de administración, versión 2020-08-01 de la API REST de destino. Disponible con carácter general.|
| Agosto | [doble cifrado](search-security-overview.md#encryption) | Disponible con carácter general en todos los servicios de búsqueda creados después del 1 de agosto de 2020 en estas regiones: Oeste de EE. UU. 2, este de EE. UU., centro-sur de EE. UU., US Gov Virginia y US Gov Arizona. |
| Julio | [Biblioteca cliente Azure.Search.Documents](/dotnet/api/overview/azure/search.documents-readme) | SDK de Azure para .NET, disponible con carácter general. |
| Julio | [Biblioteca cliente azure.search.documents](/python/api/overview/azure/search-documents-readme)  | SDK de Azure para Python, disponible con carácter general. |
| Julio | [Biblioteca cliente de@azure/search-documents](/javascript/api/overview/azure/search-documents-readme)  | SDK de Azure para JavaScript, disponible con carácter general. |
| Junio | [Almacén de conocimiento](knowledge-store-concept-intro.md) | Disponible con carácter general. |
| Junio | [API REST de búsqueda 2020-06-30](/rest/api/searchservice/) | Disponible con carácter general. |
| Junio | [API REST Search 2020-06-30-Preview](/rest/api/searchservice/) | Agrega el restablecimiento de habilidades para el reprocesamiento selectivo de las habilidades y el enriquecimiento incremental. |
| Junio | [Algoritmo de relevancia Okapi BM25](index-ranking-similarity.md) | Disponible con carácter general. |
| Junio |  **executionEnvironment** (se aplica a los servicios de búsqueda mediante Azure Private Link). | Disponible con carácter general. |
| Junio | [Habilidad de AML (versión preliminar)](cognitive-search-aml-skill.md) | Aptitud cognitiva que amplía el enriquecimiento de IA con un modelo personalizado de Azure Machine Learning (AML). |
| May | [Sesiones de depuración (versión preliminar)](cognitive-search-debug-session.md) | Depurador del conjunto de aptitudes en el portal.  |
| May | [Reglas de IP para la compatibilidad con el firewall de entrada](service-configure-firewall.md) | Disponible con carácter general.  |
| May | [Azure Private Link para un punto de conexión de búsqueda privado](service-create-private-endpoint.md) | Disponible con carácter general.  |
| May | [Managed Service Identity (indexadores) (versión preliminar)](search-howto-managed-identities-data-sources.md) | Conéctese a orígenes de datos de Azure mediante una identidad administrada.  |
| May | [parámetro de consulta sessionId](index-similarity-and-scoring.md), [parámetro scoringStatistics=global](index-similarity-and-scoring.md#scoring-statistics)  | Estadísticas de búsqueda global, útiles para la [relevancia de búsqueda de los modelos de Machine Learning (LearnToRank)](https://github.com/Azure-Samples/search-ranking-tutorial).  |
| May | [Expansion de puntuación de relevancia de featuresMode (versión preliminar)](index-similarity-and-scoring.md#featuresMode-param)  |   |
|Marzo  | [Eliminación temporal de blobs nativos (versión preliminar)](search-howto-index-changed-deleted-blobs.md) | Elimina los documentos de búsqueda si el blob de origen se elimina de forma temporal del almacenamiento de blobs. |
|Marzo  | [API REST de administración (2020-03-13)](/rest/api/searchmanagement/management-api-versions) | Disponible con carácter general. |
|February | [Aptitud para la detección de información de identificación personal (versión preliminar)](cognitive-search-skill-pii-detection.md)  | Habilidad cognitiva que extrae y enmascara información personal. |
|February | [Aptitud de búsqueda de entidades personalizadas (versión preliminar)](cognitive-search-skill-custom-entity-lookup.md) | Habilidad cognitiva que busca palabras y frases de una lista y etiqueta todos los documentos con las entidades coincidentes.  |
|January | [Cifrado de claves administrado por el cliente](search-security-manage-encryption-keys.md) | Disponibilidad general  |
|January | [Reglas de IP para la compatibilidad con el firewall de entrada (versión preliminar)](service-configure-firewall.md) | Propiedades nuevas **IpRule** y **NetworkRuleSet** en [CreateOrUpdate API](/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service).  |
|January | [Creación de un punto de conexión privado (versión preliminar)](service-create-private-endpoint.md) | Configure un vínculo privado para las conexiones seguras con el servicio de búsqueda. Esta característica en versión preliminar usa [Azure Private Link](../private-link/private-link-overview.md) y [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) como parte de la solución. |

## <a name="2019-archive"></a>Archivo de 2019

| Month (Mes) | Característica | Descripción |
|-------|---------|-------------|
|Diciembre | [Creación de una aplicación de demostración (versión preliminar)](search-create-app-portal.md) | Asistente que genera un archivo HTML descargable con acceso de consulta (solo lectura) a un índice, diseñado como herramienta de validación y pruebas en lugar de un atajo a una aplicación cliente completa.|
|Noviembre | [Enriquecimiento incremental (versión preliminar)](cognitive-search-incremental-indexing-conceptual.md) | Almacena en caché el procesamiento del conjunto de aptitudes para su reutilización futura.  |
|Noviembre | [Aptitud de extracción de documentos (versión preliminar)](cognitive-search-skill-document-extraction.md) | Aptitud cognitiva para extraer el contenido de un archivo desde un conjunto de aptitudes.|
|Noviembre | [Aptitud de traducción de texto](cognitive-search-skill-text-translation.md) | Habilidad cognitiva utilizada durante la indexación que analiza y traduce el texto. Disponible con carácter general.|
|Noviembre | [Plantillas de Power BI](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) | Plantilla para visualizar el contenido en el almacén de conocimiento |
|Noviembre | [Azure Data Lake Storage Gen2 (versión preliminar)](search-howto-index-azure-data-lake-storage.md), [Gremlin API de Cosmos DB (versión preliminar)](search-howto-index-cosmosdb.md) y [Cassandra API de Cosmos DB (versión preliminar)](search-howto-index-cosmosdb.md) | Nuevos orígenes de datos de indexador en la versión preliminar pública. |
|Julio | [Compatibilidad con la nube de Azure Government](../azure-government/compare-azure-government-global-azure.md#azure-cognitive-search) | Disponible con carácter general.|

<a name="new-service-name"></a>

## <a name="new-service-name"></a>Nuevo nombre de servicio

Azure Search se llama **Azure Cognitive Search** desde octubre de 2019 para reflejar el uso ampliado (aunque opcional) de las aptitudes cognitivas y el procesamiento de inteligencia artificial en las operaciones principales. Las versiones de API, los paquetes NuGet, los espacios de nombres y los puntos de conexión permanecen sin cambios. Las soluciones de búsqueda nuevas y las existentes no se ven afectadas por el cambio de nombre del servicio.

## <a name="service-updates"></a>Actualizaciones del servicio

Los [anuncios de actualización de servicios](https://azure.microsoft.com/updates/?product=search&status=all) de Azure Cognitive Search se pueden encontrar en el sitio web de Azure.