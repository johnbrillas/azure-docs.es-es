---
title: Adopción de ontologías estándar del sector
titleSuffix: Azure Digital Twins
description: Obtenga información sobre las ontologías del sector existentes que se pueden adoptar para Azure Digital Twins
author: baanders
ms.author: baanders
ms.date: 2/12/2021
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 71795e9dc439d5f634fc4d777aa6b5cdd66e8f5c
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100563383"
---
# <a name="adopting-an-industry-ontology"></a>Adopción de una ontología del sector

Como es más fácil comenzar con una ontología de DTDL en código abierto que empezar de cero, Microsoft se está asociando con expertos de dominio para publicar ontologías que representan convenciones del sector ampliamente aceptadas y admiten diversos casos de uso de clientes. 

El resultado es un conjunto de ontologías de código abierto basadas en DTDL que aprenden de las normas estándar del sector, están basadas en ellas o las utilizan directamente. Las ontologías están diseñadas para satisfacer las necesidades de los desarrolladores que se encuentran en un nivel inferior, con la posibilidad de que el sector las adopte o extienda posteriormente según sus necesidades.

En este momento, Microsoft está trabajando con asociados del sector inmobiliario para desarrollar una ontología para edificios inteligentes que proporciona una base común para el modelado de edificios inteligentes basados en estándares del sector para evitar la reinvención. 

## <a name="realestatecore-smart-building-ontology"></a>Ontología RealEstateCore para edificios inteligentes

Microsoft se ha asociado con [RealEstateCore](https://www.realestatecore.io/), un consorcio sueco de propietarios de bienes inmuebles, proveedores de software e instituciones de investigación, para ofrecer una ontología de DTDL de código abierto para el sector inmobiliario: la [**ontología RealEstateCore para edificios inteligentes basada en DTDL**](https://github.com/Azure/opendigitaltwins-building).

Esta ontología para edificios inteligentes proporciona una base común para el modelado de edificios inteligentes mediante estándares del sector (como el  [esquema de BRICK](https://brickschema.org/ontology/) o la  [ontología de la topología para edificios W3C](https://w3c-lbd-cg.github.io/bot/index.html)) para evitar la reinvención. La ontología también incluye procedimientos recomendados para usarla y extenderla correctamente. 

Para más información sobre las convenciones de modelado y la estructura de la ontología, cómo usarla, extenderla y colaborar, visite el [repositorio de la ontología en GitHub](https://github.com/Azure/opendigitaltwins-building). 

También puede leer más sobre la asociación con RealEstateCore y los objetivos de esta iniciativa en esta entrada de blog y en el vídeo adjunto: [*RealEstateCore, una ontología para edificios inteligentes para gemelos digitales ya está disponible*](https://techcommunity.microsoft.com/t5/internet-of-things/realestatecore-a-smart-building-ontology-for-digital-twins-is/ba-p/1914794).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre cómo extender las ontologías estándar del sector para que satisfagan sus especificaciones: [*Conceptos: Extensión de las ontologías del sector*](concepts-ontologies-extend.md).

* O bien, continúe el proceso de aprendizaje para desarrollar modelos basados en ontologías: [*Uso de estrategias de ontología en un procedimiento de desarrollo de modelos*](concepts-ontologies.md#using-ontology-strategies-in-a-model-development-path).