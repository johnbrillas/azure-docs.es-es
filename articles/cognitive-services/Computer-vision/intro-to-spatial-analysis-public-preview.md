---
title: Introducción al análisis espacial de Computer Vision
titleSuffix: Azure Cognitive Services
description: En este documento se explican los conceptos básicos y las características de un contenedor de análisis espacial de Computer Vision.
services: cognitive-services
author: tchristiani
manager: nitinme
ms.author: terrychr
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: 402ee6d5efdd489914cb7d283c7c46d4f7d175f6
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97968065"
---
# <a name="introduction-to-computer-vision-spatial-analysis"></a>Introducción al análisis espacial de Computer Vision

El análisis espacial de Computer Vision es una nueva característica de Azure Cognitive Services Computer Vision, que ayuda a las organizaciones a maximizar el valor de sus espacios físicos mediante la comprensión de los movimientos y la presencia de personas dentro de un área determinada. Le permite ingerir vídeos de cámaras de CCTV o de vigilancia, ejecutar aptitudes de inteligencia artificial para extraer información de las secuencias de vídeo y generar eventos para que los usen otros sistemas. Con la entrada de una transmisión de cámara, una aptitud de inteligencia artificial puede hacer cosas como contar el número de personas que entran en un sitio o medir el cumplimiento de las directrices de distanciamiento social.

## <a name="the-basics-of-spatial-analysis"></a>Aspectos básicos del análisis espacial

En la actualidad, las operaciones principales del análisis espacial se basan en una canalización que ingiere el vídeo, detecta personas en él, realiza un seguimiento de esas personas a medida que aparecen en el vídeo y genera eventos a medida que la gente interactúa con regiones de interés.

## <a name="spatial-analysis-terms"></a>Términos de análisis espaciales

| Término | Definición |
|------|------------|
| Detección de personas | Este componente responde a la pregunta "¿Dónde están las personas de esta imagen?" Se encarga de buscar personas en una imagen y pasa un cuadro de límite que indica la ubicación de cada persona al componente de seguimiento. |
| Seguimiento de personas | Este componente conecta las detecciones de personas a lo largo de la reproducción, a medida que se mueven delante de una cámara. Para ello, usa una lógica temporal acerca de cómo suelen moverse las personas, e información básica sobre la apariencia general de la gente. En cambio, no puede realizar el seguimiento de las personas a través de varias cámaras o volver a identificar a alguien que haya desaparecido durante más de un minuto aproximadamente. El seguimiento de personas no usa marcadores biométricos como el reconocimiento facial o el seguimiento de la forma de andar. |
| Región de interés | Se trata de una zona o línea definida en el vídeo de entrada como parte de la configuración. Cuando una persona interactúa con la región del vídeo, el sistema genera un evento. Por ejemplo, para la aptitud PersonCrossingLine, se define una línea en el vídeo. Cuando una persona cruza esa línea, se genera un evento. |
| Evento | Un evento es la salida principal del análisis espacial. Cada aptitud emite un evento específico de forma periódica (por ejemplo, una vez por minuto) o cuando se genera un desencadenador específico. El evento incluye información sobre lo que pasó en el vídeo de entrada, pero no incluye imágenes ni vídeo. Por ejemplo, la aptitud PeopleCount puede emitir un evento que contenga un recuento actualizado cada vez que el número de personas cambia (es decir, el evento se desencadena) o una vez cada minuto (el evento se realiza periódicamente). |

## <a name="example-use-cases-for-spatial-analysis"></a>Casos de uso de ejemplo para el análisis espacial

A continuación, se muestran ejemplos de casos de uso que debemos tener en cuenta al diseñar y probar el análisis espacial.

**Cumplimiento de la distancia social**: en una oficina hay varias cámaras que usan el análisis espacial para supervisar el cumplimiento del distanciamiento social midiendo la distancia entre personas. El administrador de la oficina puede usar mapas térmicos que muestran estadísticas agregadas del cumplimiento de la distancia social a lo largo del tiempo, para así ajustar el área de trabajo y que cumplir con la distancia social sea más fácil.

**Análisis de compradores**: una tienda de comestibles utiliza cámaras que apuntan a los escaparates de productos para medir el impacto que supone para el tráfico de la tienda cambiar la mercancía. Este sistema permite al administrador de la tienda identificar qué productos nuevos van a generar el mayor cambio en la involucración de los consumidores.

**Administración de colas**: las cámaras que apuntan a las colas para pagar proporcionan alertas a los administradores cuando el tiempo de espera es demasiado largo, lo que les permite abrir más cajas. Los datos históricos sobre el abandono de la cola proporcionan información sobre el comportamiento del consumidor.

**Análisis y ocupación de edificios**: un edificio de oficinas usa cámaras que se centran en las entradas a espacios clave para medir la cantidad de clientes y cómo se usa el área de trabajo. Esta información permite al administrador del edificio ajustar el servicio y el diseño para ofrecer un mejor servicio a sus ocupantes.

**Detección mínima de personal**: en un centro de datos, las cámaras supervisan la actividad alrededor de los servidores. Cuando los empleados corrigen físicamente el equipo confidencial, siempre deben estar presentes dos personas durante la reparación por motivos de seguridad. Las cámaras se usan para comprobar que se sigue esta directriz.

**Optimización del área de trabajo**: en un restaurante informal, las cámaras de la cocina se usan para generar información agregada sobre el flujo de trabajo de los empleados. Esto puede ser de utilidad para que los administradores mejoren los procesos y el aprendizaje del equipo.

## <a name="considerations-when-choosing-a-use-case"></a>Consideraciones al elegir un caso de uso

**Evite las alertas de seguridad críticas**: el análisis espacial no se diseñó para generar alertas críticas de seguridad en tiempo real. No debe basarse en escenarios en los que se necesiten alertas en tiempo real para desencadenar una intervención que evite lesiones, como la desactivación de una pieza pesada de maquinaria cuando una persona está presente. En cambio, se puede usar para reducir el riesgo mediante el uso de estadísticas e intervenciones para reducir comportamientos arriesgados, como cuando la gente entra en un área restringida o prohibida.

**Evite el uso de decisiones relacionadas con el empleo**: el análisis espacial proporciona métricas probabilísticas respecto a la ubicación y el movimiento de personas dentro de un espacio. Aunque estos datos pueden ser útiles para la mejora de procesos agregados, los datos no son un buen indicador del rendimiento individual del trabajador y no deben usarse para tomar decisiones relacionadas con el empleo.

**Evite su uso para tomar decisiones relacionadas con la atención sanitaria**: el análisis espacial proporciona datos probabilísticos y parciales relacionados con los movimientos de personas. Estos datos no son adecuados para tomar decisiones relacionadas con la salud.

**Evite su uso en espacios protegidos**: debe proteger la privacidad de los individuos mediante la evaluación de las ubicaciones y las posiciones de la cámara; para ello, debe ajustar los ángulos y las regiones de interés, para no pasar por alto áreas protegidas como los aseos.

**Considere detenidamente su uso en escuelas o instalaciones de atención a la tercera edad**: el análisis espacial no se ha sometido a pruebas exhaustivas con datos de menores de 18 años o adultos con una edad superior a los 65 años. Es recomendable que los clientes evalúen detenidamente las tasas de error para su escenario en entornos en los que estas edades son predominantes.

**Considere cuidadosamente la posibilidad de su uso en espacios públicos**: debe evaluar las ubicaciones y las posiciones de la cámara; asimismo, debe ajustar los ángulos y la región de interés para minimizar la recopilación de datos de los espacios públicos. La iluminación y el tiempo en espacios públicos como calles y parques influirán significativamente en el rendimiento del sistema de análisis espacial, por lo que es muy difícil proporcionar una divulgación efectiva en espacios públicos.

## <a name="spatial-analysis-gating-for-public-preview"></a>Restricción de acceso al análisis espacial en la versión preliminar pública

Para garantizar que el análisis espacial se usa en los escenarios para los que se diseñó, esta tecnología está a disposición de aquellos clientes que pasen por un proceso de solicitud. Para obtener acceso al análisis espacial, deberá rellenar el formulario de admisión en línea. [Realice aquí la solicitud](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u).

El acceso a la versión preliminar pública del análisis espacial está sujeto a la exclusiva discreción de Microsoft en función de los criterios de idoneidad, el proceso de investigación y la disponibilidad para admitir un número limitado de clientes durante esta versión preliminar controlada. Para la versión preliminar pública, buscamos clientes que tengan una relación significativa con Microsoft, y que estén interesados en trabajar con nosotros en los casos de uso recomendados y en escenarios adicionales que estén en consonancia con nuestros compromisos de inteligencia artificial.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Características y limitaciones del análisis espacial](https://docs.microsoft.com/legal/cognitive-services/computer-vision/accuracy-and-limitations?context=%2fazure%2fcognitive-services%2fComputer-vision%2fcontext%2fcontext)
