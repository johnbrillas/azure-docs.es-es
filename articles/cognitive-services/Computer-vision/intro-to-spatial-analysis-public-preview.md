---
title: Introducción al análisis espacial
titleSuffix: Azure Cognitive Services
description: En este documento se explican los conceptos básicos y las características de un contenedor de análisis espacial de Computer Vision.
services: cognitive-services
author: nitinme
manager: nitinme
ms.author: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/01/2021
ms.openlocfilehash: ad05dd59c925242baf5c2b0e36c1f51bc4fec5d4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100575357"
---
# <a name="overview-of-computer-vision-spatial-analysis"></a>Introducción al análisis espacial de Computer Vision

El análisis espacial de Computer Vision es una nueva característica de Azure Cognitive Services Computer Vision, que ayuda a las organizaciones a maximizar el valor de sus espacios físicos mediante la comprensión de los movimientos y la presencia de personas dentro de un área determinada. Le permite ingerir vídeos de cámaras de CCTV o de vigilancia, ejecutar operaciones de inteligencia artificial para extraer información de las secuencias de vídeo y generar eventos para que los usen otros sistemas. Con la entrada de una transmisión de cámara, una operación de inteligencia artificial puede hacer cosas como contar el número de personas que entran en un sitio o medir el cumplimiento de las directrices de uso de máscaras faciales y distanciamiento físico.

## <a name="the-basics-of-spatial-analysis"></a>Aspectos básicos del análisis espacial

En la actualidad, las operaciones principales del análisis espacial se basan en una canalización que ingiere el vídeo, detecta personas en él, realiza un seguimiento de esas personas a medida que aparecen en el vídeo y genera eventos a medida que la gente interactúa con regiones de interés.

## <a name="spatial-analysis-terms"></a>Términos del análisis espacial

| Término | Definición |
|------|------------|
| Detección de personas | Este componente responde a la pregunta "¿Dónde están las personas de esta imagen?" Se encarga de buscar personas en una imagen y pasa un cuadro de límite que indica la ubicación de cada persona al componente de seguimiento. |
| Seguimiento de personas | Este componente conecta las detecciones de personas a lo largo de la reproducción, a medida que se mueven delante de una cámara. Para ello, usa una lógica temporal acerca de cómo suelen moverse las personas, e información básica sobre la apariencia general de la gente. No realiza un seguimiento de las personas a lo largo de varias cámaras. Si una persona sale del campo de visión de una cámara durante más de aproximadamente un minuto y luego vuelve a entrar en la visión de la cámara, el sistema la percibirá como una nueva persona. El seguimiento de personas no identifica de forma exclusiva a las personas entre las cámaras. No usa el reconocimiento facial ni el seguimiento de la forma de caminar. |
| Detección de máscaras faciales | Este componente detecta la ubicación de la cara de una persona en el campo de visión de la cámara e identifica la presencia de una máscara facial. Para ello, la operación de inteligencia artificial examina las imágenes del vídeo: cuando se detecta una cara, el servicio proporciona un cuadro de límite alrededor de la cara. Con las funcionalidades de detección de objetos, identifica la presencia de máscaras faciales dentro del cuadro de límite. La detección de máscaras faciales no implica distinguir una cara de otra, predecir ni clasificar atributos faciales ni realizar el reconocimiento facial. |
| Región de interés | Se trata de una zona o línea definida en el vídeo de entrada como parte de la configuración. Cuando una persona interactúa con la región del vídeo, el sistema genera un evento. Por ejemplo, para la operación PersonCrossingLine, se define una línea en el vídeo. Cuando una persona cruza esa línea, se genera un evento. |
| Evento | Un evento es la salida principal del análisis espacial. Cada operación emite un evento específico de forma periódica (por ejemplo, una vez por minuto) o cuando se genera un desencadenador específico. El evento incluye información sobre lo que pasó en el vídeo de entrada, pero no incluye imágenes ni vídeo. Por ejemplo, la operación PeopleCount puede emitir un evento que contenga un recuento actualizado cada vez que el número de personas cambia (es decir, el evento se desencadena) o una vez cada minuto (el evento se realiza periódicamente). |

## <a name="responsible-use-of-spatial-analysis-technology"></a>Uso responsable de la tecnología de análisis espacial

Para aprender a usar la tecnología de análisis espacial de forma responsable, consulte la [nota de transparencia](/legal/cognitive-services/computer-vision/transparency-note-spatial-analysis?context=%2fazure%2fcognitive-services%2fComputer-vision%2fcontext%2fcontext). Las notas de transparencia de Microsoft están pensadas para ayudarle a entender cómo funciona nuestra tecnología de inteligencia artificial, las elecciones que los propietarios del sistema pueden hacer que influyan en el rendimiento y el comportamiento del sistema y la importancia de pensar en todo el sistema, incluida la tecnología, las personas y el entorno.

## <a name="spatial-analysis-gating-for-public-preview"></a>Restricción de acceso al análisis espacial en la versión preliminar pública

Para garantizar que el análisis espacial se usa en los escenarios para los que se diseñó, esta tecnología está a disposición de aquellos clientes que pasen por un proceso de solicitud. Para obtener acceso al análisis espacial, deberá rellenar el formulario de admisión en línea. [Realice aquí la solicitud](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u).

El acceso a la versión preliminar pública del análisis espacial está sujeto a la exclusiva discreción de Microsoft en función de los criterios de idoneidad, el proceso de investigación y la disponibilidad para admitir un número limitado de clientes durante esta versión preliminar controlada. Para la versión preliminar pública, buscamos clientes que tengan una relación significativa con Microsoft, y que estén interesados en trabajar con nosotros en los casos de uso recomendados y en escenarios adicionales que estén en consonancia con nuestros compromisos de inteligencia artificial.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción al contenedor de análisis espacial](spatial-analysis-container.md)