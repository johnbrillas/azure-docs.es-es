---
title: 'Mejora del modelo: Servicio Custom Vision'
titleSuffix: Azure Cognitive Services
description: En este artículo aprenderá cómo la cantidad, la calidad y la variedad de datos pueden mejorar la calidad del modelo del servicio Custom Vision.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 02/09/2021
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: ae0112292994fdcf88e80abff8ab52e5971cb0ed
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101731092"
---
# <a name="how-to-improve-your-custom-vision-model"></a>Mejora del modelo de Custom Vision

En esta guía aprenderá a mejorar la calidad del modelo del servicio Custom Vision. La calidad de su [clasificador](./getting-started-build-a-classifier.md) o del [detector de objetos](./get-started-build-detector.md) depende de la cantidad, calidad y variedad de los datos con etiqueta que proporcione, así como del grado de equilibrio del conjunto de datos en general. Un buen modelo tiene un conjunto de datos de aprendizaje equilibrado que es representativo de lo que se enviará al modelo. El proceso de creación de un modelo de este tipo es iterativo; es habitual requerir algunos ciclos de aprendizaje para alcanzar los resultados esperados.

Este es un patrón general que le ayudará a entrenar un modelo más preciso:

1. Primera ronda de entrenamiento
1. Agregar más imágenes y equilibrar datos, reentrenar
1. Agregar imágenes con fondo, luz, tamaño de objeto, ángulo de cámara y estilo diversos, reentrenar
1. Usar nuevas imágenes para probar la predicción
1. Modificar los datos de entrenamiento existentes según los resultados de la predicción

## <a name="prevent-overfitting"></a>Evitación del sobreajuste

En ocasiones, un modelo aprenderá a realizar predicciones en función de las características arbitrarias que las imágenes tienen en común. Por ejemplo, si va a crear un clasificador que distingue las manzanas de los cítricos y se proporcionan imágenes de manzanas en las manos y de cítricos en platos blancos, puede que el clasificador conceda una importancia innecesaria a la diferencia entre manos y platos blancos en vez de entre manzanas y cítricos.

![Imagen de una clasificación inesperada](./media/getting-started-improving-your-classifier/unexpected.png)

Para corregir este problema, proporcione imágenes con distintos ángulos, fondos, tamaños del objeto, grupos y otras variantes. En las secciones siguientes se amplían estos conceptos.

## <a name="data-quantity"></a>Cantidad de datos

El número de imágenes de entrenamiento es el factor más importante para el conjunto de datos. Se recomienda usar al menos cincuenta imágenes por etiqueta como punto de partida. Con menos imágenes, hay un mayor riesgo de sobreajuste y, aunque los números de rendimiento pueden sugerir una buena calidad, el modelo podría tener dificultades con datos reales. 

## <a name="data-balance"></a>Equilibrio de datos

También es importante tener en cuenta las cantidades relativas de los datos de entrenamiento. Por ejemplo, el uso de 500 imágenes para una etiqueta y 50 imágenes para otra etiqueta desequilibra un conjunto de datos de entrenamiento. Esto hará que el modelo sea más preciso para predecir una etiqueta que otra. Probablemente obtendrá mejores resultados si mantiene al menos una relación 1:2 entre la etiqueta con menos imágenes y la etiqueta que tiene la mayoría de las imágenes. Por ejemplo, si la etiqueta con más imágenes tiene 500 imágenes, la etiqueta con menos imágenes necesitará tener al menos 250 imágenes para el entrenamiento.

## <a name="data-variety"></a>Variedad de datos

Asegúrese de proporcionar imágenes que sean representativas de lo que se va a enviar al clasificador durante el uso normal. De lo contrario, el modelo podría obtener información sobre cómo realizar predicciones en función de las características arbitrarias que las imágenes tienen en común. Por ejemplo, si va a crear un clasificador que distingue las manzanas de los cítricos y se proporcionan imágenes de manzanas en las manos y de cítricos en platos blancos, puede que el clasificador conceda una importancia innecesaria a la diferencia entre manos y platos blancos en vez de entre manzanas y cítricos.

![Imagen de una clasificación inesperada](./media/getting-started-improving-your-classifier/unexpected.png)

Para corregir este problema, incluya varias imágenes para asegurarse de que el clasificador pueda generalizar bien. A continuación se indican algunas formas de diversificar más el conjunto de entrenamiento:

* __Información previa:__ Proporcione imágenes de su objeto delante distintos fondos. Las fotos en contexto natural son mejores que las fotos delante de fondos neutros, ya que ofrecen más información para el clasificador.

    ![Ejemplos de imágenes de fondos](./media/getting-started-improving-your-classifier/background.png)

* __Luz:__ Proporcione imágenes con una luz distinta (es decir, fotos realizadas con flash, con alta exposición, etc.), sobre todo si las imágenes utilizadas para predicciones presentan una iluminación distinta. También resulta útil usar imágenes con niveles diferentes de saturación, matiz y brillo.

    ![Ejemplos de imágenes con luz](./media/getting-started-improving-your-classifier/lighting.png)

* __Tamaño de objeto:__ Proporcione imágenes en las que los objetos varíen en tamaño y número (por ejemplo, una foto de un racimo de plátanos y un primer plano de un solo plátano). Los diferentes tamaños ayudan al clasificador a generalizar mejor.

    ![Ejemplos de tamaños en imágenes](./media/getting-started-improving-your-classifier/size.png)

* __Ángulo de cámara:__ proporcione imágenes realizadas con ángulos de cámara diferentes. Alternativamente, si todas las fotos se toman con cámaras fijas (como cámaras de vigilancia), asegúrese de asignar una etiqueta diferente a cada objeto que se capture periódicamente para evitar sobreajustes o la interpretación de objetos no relacionados (como las farolas) como la característica principal.

    ![Ejemplo de imágenes con ángulo](./media/getting-started-improving-your-classifier/angle.png)

* __Estilo:__ Proporcione imágenes de diferentes estilos de la misma clase (por ejemplo, diferentes variedades de la fruta misma). Sin embargo, si tiene objetos de estilos radicalmente distintos (por ejemplo, Mickey Mouse frente a una ratón real), se recomienda etiquetarlas como clases independientes para representar mejor sus características distintivas.

    ![Ejemplos de imágenes de estilos](./media/getting-started-improving-your-classifier/style.png)

## <a name="negative-images-classifiers-only"></a>Imágenes negativas (solo clasificadores)

Si va a usar un clasificador de imágenes, puede que tenga que agregar _ejemplos negativos_ para ayudar a que el clasificador sea más preciso. Ejemplos negativos son las imágenes que no coinciden con ninguna de las otras etiquetas. Cuando cargue estas imágenes, aplíqueles la etiqueta especial **Negative** (Negativo).

Los detectores de objetos controlan los ejemplos negativos automáticamente, ya que las áreas de imágenes fuera de los rectángulos de selección dibujados se consideran negativos.

> [!NOTE]
> El servicio Custom Vision Service admite cierto control negativo automático de la imagen. Por ejemplo, si va a crear un clasificador que distingue uvas de plátanos y envía una imagen de un zapato para la predicción, el clasificador puntuaría esa imagen con un valor cercano al 0 % para uva y plátano.
> 
> Por otro lado, en los casos en que las imágenes negativas son solo una variación de las imágenes utilizadas en el entrenamiento, es probable que el modelo clasifique las imágenes negativas como una clase etiquetada debido a las grandes similitudes que existen. Por ejemplo, si tiene un clasificador que distingue naranjas de pomelos y envía una imagen de una mandarina, puede que se clasifique la mandarina como una naranja porque muchas características de las mandarinas recuerdan a las de las naranjas. Si las imágenes negativas son de esta naturaleza, se recomienda crear una o varias etiquetas adicionales (como **Otros**) y etiquetar las imágenes negativas con esta etiqueta durante el entrenamiento para permitir que el modelo diferencie mejor entre estas clases.

## <a name="consider-occlusion-and-truncation-object-detectors-only"></a>Tener en cuenta la oclusión y el truncamiento (solo detectores de objetos)

Si desea que el detector de objetos detecte objetos truncados (el objeto aparece parcialmente cortado en la imagen) o los objetos ocluidos (un objeto está bloqueado parcialmente por otro objeto de la imagen), deberá incluir imágenes de entrenamiento que incluyan esos casos.

> [!NOTE]
> El problema de los objetos que están ocluidos por otros objetos no se debe confundir con el **umbral de superposición**, un parámetro para el rendimiento del modelo de clasificación. El control deslizante **Overlap Threshold** (Umbral de superposición) del [sitio web de Custom Vision](https://customvision.ai) permite seleccionar el grado en que un rectángulo delimitador de predicción debe superponerse al rectángulo delimitador verdadero que se considera correcto.

## <a name="use-prediction-images-for-further-training"></a>Uso de imágenes de predicción para entrenamiento adicional

Al usar o probar el modelo mediante el envío de imágenes al punto de conexión de predicción, el servicio Custom Vision almacena esas imágenes. A continuación, puede usarlas para mejorar el modelo.

1. Para ver las imágenes enviadas al modelo, abra la [página web de Custom Vision](https://customvision.ai), vaya al proyecto y seleccione la pestaña __Predictions__ (Predicciones). La vista predeterminada muestra imágenes de la iteración actual. Puede usar el menú desplegable __Iteración__ para ver las imágenes enviadas durante las iteraciones anteriores.

    ![captura de pantalla de la pestaña de predicciones, con imágenes en la vista](./media/getting-started-improving-your-classifier/predictions.png)

2. Mantenga el mouse sobre una imagen para ver las etiquetas previstas por el modelo. Las imágenes se ordenan de manera que aquellas que pueden aportar más mejoras al modelo ocupan los primeros lugares. Para usar un método de ordenación diferente, realice una selección en la sección __Ordenar__. 

    Para agregar una imagen a los datos de entrenamiento existentes, seleccione la imagen, seleccione la etiqueta correcta y después haga clic en __Guardar y cerrar__. La imagen se quitará de las __predicciones__ y se agrega a las imágenes de entrenamiento. Para verla, seleccione la pestaña __Training Images__ (Imágenes de entrenamiento).

    ![Imagen de la página de etiquetado](./media/getting-started-improving-your-classifier/tag.png)

3. Luego, use el botón __Entrenar__ para reentrenar el modelo.

## <a name="visually-inspect-predictions"></a>Inspección visual de predicciones

Para inspeccionar las predicciones de la imagen, vaya a la pestaña de __imágenes de entrenamiento__, seleccione la iteración de entrenamiento anterior en el menú desplegable **Iteración** y compruebe una o varias etiquetas en la sección **Etiquetas**. La vista ahora debe mostrar un cuadro rojo alrededor de cada una de las imágenes para las que el modelo no pudo predecir correctamente la etiqueta determinada.

![Imagen del historial de iteración](./media/getting-started-improving-your-classifier/iteration.png)

En ocasiones, la inspección visual puede identificar patrones que puede corregir agregando datos de entrenamiento adicionales o modificando los datos de entrenamiento existentes. Por ejemplo, un clasificador que distingue manzanas de limas puede etiquetar incorrectamente las manzanas verdes como limas. Puede corregir este problema agregando y proporcionando datos de entrenamiento que contengan imágenes etiquetadas de manzanas verdes.

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido varias técnicas para hacer más preciso el modelo de clasificación de imágenes personalizadas o el modelo del detector de objetos. A continuación, aprenda a probar imágenes mediante programación enviándolas a Prediction API.

> [!div class="nextstepaction"]
> [Uso de la API de predicción](use-prediction-api.md)