---
title: Uso del módulo Puntuación del modelo de imagen
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Puntuación del modelo de imagen en Azure Machine Learning para generar predicciones mediante un modelo de imagen entrenado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: fe57a9e8ce9b14f7d1346d819965576770afef3b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "93324884"
---
# <a name="score-image-model"></a>Puntuación del modelo de imagen

En este artículo se describe un módulo del diseñador de Azure Machine Learning.

Use este módulo para generar predicciones con un modelo de imagen entrenado sobre los datos de la imagen de entrada.

## <a name="how-to-configure-score-image-model"></a>Configuración de Puntuación del modelo de imagen

1. Agregue el módulo **Puntuación del modelo de imagen** a la canalización.

2. Adjunte un modelo de imagen entrenado y un conjunto de datos que contenga los datos de la imagen de entrada. 

    Los datos deben ser de tipo ImageDirectory. Para más información sobre cómo obtener un directorio de imagen, consulte [Conversión a directorio de imagen](convert-to-image-directory.md). El esquema del conjunto de datos de entrada también debe coincidir generalmente con el esquema de los datos utilizados para entrenar el modelo.

3. Envíe la canalización.

## <a name="results"></a>Results

Después de haber generado un conjunto de puntuaciones mediante [Puntuación del modelo de imagen](score-image-model.md), para generar un conjunto de métricas usadas para evaluar la exactitud del modelo (rendimiento), puede conectar este módulo y el conjunto de datos puntuado a [Evaluación del modelo](evaluate-model.md). 

### <a name="publish-scores-as-a-web-service"></a>Publicar puntuaciones como servicio web

Un uso común de la puntuación es devolver la salida como parte de un servicio web predictivo. Para obtener más información, vea [este tutorial](../tutorial-designer-automobile-price-deploy.md) sobre cómo implementar un punto de conexión en tiempo real basado en una canalización en el diseñador de Azure Machine Learning.

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning.