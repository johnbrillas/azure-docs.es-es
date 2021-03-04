---
title: Búsqueda semántica
titleSuffix: Azure Cognitive Search
description: Conozca cómo Cognitive Search usa modelos de búsqueda semántica de aprendizaje profundo de Bing para crear resultados de búsqueda más intuitivos.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: eedb3dfeafbd378cfff7afb04fcc2b9aa6e791d6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678920"
---
# <a name="semantic-search-in-azure-cognitive-search"></a>Búsqueda semántica en Azure Cognitive Search

> [!IMPORTANT]
> Las características de búsqueda semántica se encuentran en versión preliminar pública, y solo están disponibles mediante la API REST en versión preliminar. Las características en vista previa (GB) se ofrecen tal cual, en [Términos de uso complementarios](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La búsqueda semántica es una colección de características relacionadas con las consultas que admiten una experiencia de consulta más natural y de más calidad. Las características incluyen la reclasificación semántica de los resultados de búsqueda, así como la generación de subtítulos y respuestas con resaltado semántico. Los 50 primeros resultados del [motor de búsqueda de texto completo](search-lucene-query-architecture.md) se vuelven a clasificar para encontrar las coincidencias más pertinentes.

La tecnología subyacente aprovecha las inversiones de Bing y Microsoft Research, y se integra con la infraestructura de Cognitive Search. Para usarla, deberá realizar pequeñas modificaciones en la solicitud de búsqueda, pero no se necesitan más configuraciones ni reindexaciones.

Las características de la versión preliminar pública son:

+ Algoritmo de clasificación semántica que recupera documentos pertinentes según el contexto o el significado semántico de los términos de la consulta de búsqueda.
+ Subtítulos semánticos que resaltan los pasajes importantes
+ Respuestas semánticas a la consulta, también formuladas a partir de los resultados
+ Corrección ortográfica que corrige los errores tipográficos antes de que los términos de consulta lleguen al motor de búsqueda

## <a name="semantic-search-architecture"></a>Arquitectura de la búsqueda semántica

Los componentes de la búsqueda semántica se superponen sobre la canalización de ejecución de la consulta existente. La corrección ortográfica (no se muestra en el diagrama) mejora las coincidencias, ya que subsana los errores tipográficos en términos de consultas individuales. Una vez completados todos los análisis, el motor de búsqueda recupera los documentos que coinciden con la consulta y los puntúa mediante el [algoritmo de puntuación predeterminado](index-similarity-and-scoring.md#similarity-ranking-algorithms), ya sea BM25 o el clásico, según cuándo se haya creado el servicio. Los perfiles de puntuación también se aplican en esta fase. 

Tras recibir las 50 coincidencias principales, el [algoritmo de clasificación semántica](semantic-how-to-query-response.md) vuelve a evaluar el corpus de documentos. Los resultados pueden incluir más de 50 coincidencias, pero solo se volverán a clasificar las 50 primeras. Para la clasificación, el algoritmo usa el aprendizaje automático y el aprendizaje por transferencia para volver a puntuar los documentos en función del nivel de coincidencia de cada uno con la intención de la consulta.

Para crear subtítulos y respuestas, usa modelos de representación del lenguaje. Dentro de un subtítulo, el algoritmo usa un modelo de lenguaje desarrollado por Bing para extraer los subtítulos con los aspectos destacados que mejor resuman los resultados de una consulta. Si la consulta de búsqueda es una pregunta y se solicitan respuestas, un modelo de lenguaje similar identifica los pasajes de texto que mejor responden a la pregunta según se expresa en la consulta de búsqueda.

:::image type="content" source="media/semantic-search-overview/semantic-query-architecture.png" alt-text="Componentes semánticos de una canalización de consulta" border="true":::

## <a name="availability-and-pricing"></a>Disponibilidad y precios

La clasificación semántica está disponible mediante el [registro de inicio de sesión](https://aka.ms/SemanticSearchPreviewSignup), en los servicios de búsqueda creados en un nivel Estándar (S1, S2, S3), ubicados en una de estas regiones: Centro-norte de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2, Este de EE. UU. 2, Europa del norte y Oeste de Europa. La corrección ortográfica está disponible en las mismas regiones, pero no tiene restricciones de nivel. Si tiene un servicio existente que cumpla los criterios de nivel y región, solo es necesario que se registre.

Entre el lanzamiento de la versión preliminar del 2 de marzo al 1 de abril, la corrección ortográfica y la clasificación semántica se ofrecen de forma gratuita. Después del 1 de abril, los costos computacionales de ejecutar esta funcionalidad pasarán a ser un evento facturable. El costo esperado es de aproximadamente 500 USD/mes por 250 000 consultas. Puede encontrar información detallada de los costos documentada en la [página de precios de Cognitive Search](https://azure.microsoft.com/pricing/details/search/) y en [Estimación y administración de los costos](search-sku-manage-costs.md).

## <a name="next-steps"></a>Pasos siguientes

Un nuevo tipo de consulta permite la clasificación de relevancia y las estructuras de respuesta de la búsqueda semántica. Para comenzar, [cree una consulta semántica](semantic-how-to-query-request.md). O bien, revise cualquiera de los siguientes artículos para encontrar información relacionada.

+ [Adición de la corrección ortográfica a los términos de consulta](speller-how-to-add.md)
+ [Clasificación y respuestas semánticas](semantic-how-to-query-response.md)