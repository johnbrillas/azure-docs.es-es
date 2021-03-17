---
author: laujan
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: lajanuar
ms.openlocfilehash: 89b035397ea2050ae7e61f2a19310b6a7fb4192c
ms.sourcegitcommit: 3ea12ce4f6c142c5a1a2f04d6e329e3456d2bda5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2021
ms.locfileid: "103467239"
---
Examine los valores de `"confidence"` de cada resultado de clave-valor en el nodo `"pageResults"`. También debe examinar las puntuaciones de confianza del nodo `"readResults"`, que se corresponden a la operación de lectura de texto. La confianza de los resultados de la lectura no afecta a la confianza de los resultados de la extracción de los pares clave-valor, por lo que debe comprobar ambos.
* Si las puntuaciones de confianza de la operación de lectura son bajas, intente mejorar la calidad de los documentos de entrada (consulte [Requisitos de entrada](../overview.md#input-requirements)).
* Si las puntuaciones de confianza de la operación de extracción de pares clave-valor son bajas, asegúrese de que los documentos que se están analizando sean del mismo tipo que los documentos usados en el conjunto de entrenamiento. Si los documentos del conjunto de entrenamiento tienen variantes de apariencia, considere la posibilidad de dividirlos en carpetas diferentes y entrenar un modelo para cada variante.

El objetivo de puntuación de confianza dependerá de su caso de uso pero, por lo general, se recomienda establecer una puntuación del 80 % o superior. Para casos más confidenciales, como la lectura de registros médicos o instrucciones de facturación, se recomienda una puntuación del 100 %.