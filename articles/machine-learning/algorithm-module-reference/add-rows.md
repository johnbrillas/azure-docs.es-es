---
title: 'Adición de filas: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo de incorporación de filas en el diseñador de Azure Machine Learning para concatenar dos conjuntos de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 2c2a74ee88e7161ab8dd1701ef4d105df67c0938
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "93421947"
---
# <a name="add-rows-module"></a>Módulo Adición de filas

En este artículo se describe un módulo del diseñador de Azure Machine Learning.

Utilice este módulo para concatenar dos conjuntos de datos. En la concatenación, las filas del segundo conjunto de datos se agregan al final del primer conjunto de datos.  
  
La concatenación de filas es útil en escenarios como los siguientes:  
  
+ Se ha generado una serie de estadísticas de evaluación y desea combinarlas en una sola tabla para crear informes más fácilmente.  
  
+ Ha estado trabajando con diferentes conjuntos de datos y desea combinarlos para crear un conjunto de datos final.  

## <a name="how-to-use-add-rows"></a>Procedimiento para usar Adición de filas  

Para concatenar las filas de dos conjuntos de datos, las filas deben tener exactamente el mismo esquema. Esto significa, el mismo número de columnas y el mismo tipo de datos en las columnas.

1.  Arrastre el módulo **Adición de filas** a la canalización. Puede encontrarlo en **Transformación de datos**.

2. Conecte los conjuntos de datos a los dos puertos de entrada. El conjunto de datos que desea anexar debe estar conectado al segundo puerto (derecho). 
  
3.  Envíe la canalización. El número de filas del conjunto de datos de salida debe ser igual a la suma de las filas de ambos conjuntos de datos de entrada.

    Si agrega el mismo conjunto de datos a las dos entradas del módulo **Adición de filas**, se duplica el conjunto de datos. 

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 