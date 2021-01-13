---
title: Límites de servicio
titleSuffix: Azure Machine Learning
description: Límites de servicio usados en el planeamiento de la capacidad y los límites máximos de solicitudes y respuestas de Azure Machine Learning.
services: machine-learning
author: andscho
ms.author: andscho
ms.reviewer: mldocs
ms.topic: reference
ms.service: machine-learning
ms.subservice: core
ms.date: 12/21/2020
ms.openlocfilehash: 132c4b223b99f7110cd4553bb0dd93f1e58f4cdd
ms.sourcegitcommit: a89a517622a3886b3a44ed42839d41a301c786e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2020
ms.locfileid: "97733233"
---
# <a name="service-limits-in-azure-machine-learning"></a>Límites de servicio en Azure Machine Learning

En esta sección se muestra una lista de las cuotas básicas y los umbrales de limitación en Azure Machine Learning.

## <a name="workspaces"></a>Áreas de trabajo
| Límite | Value |
| --- | --- |
| Nombre del área de trabajo | 2-32 caracteres |

## <a name="runs"></a>Ejecuciones
| Límite | Value |
| --- | --- |
| Ejecuciones por área de trabajo | 10 millones |
| RunId/ParentRunId | 256 caracteres |
| DataContainerId | 261 caracteres |
| DisplayName |256 caracteres|
| Descripción |5000 caracteres|
| Número de propiedades |50 |
| Longitud de la clave de propiedad |100 caracteres |
| Longitud del valor de propiedad |1000 caracteres |
| Número de etiquetas |50 |
| Longitud de la clave de etiqueta |100 |
| Longitud del valor de etiqueta |1000 caracteres |
| CancelUri / CompleteUri / DiagnosticsUri |1000 caracteres |
| Longitud del mensaje de error |3000 caracteres |
| Longitud del mensaje de advertencia |300 caracteres |
| Número de conjuntos de datos de entrada |200 |
| Número de conjuntos de datos de salida |20 |


## <a name="metrics"></a>Métricas
| Límite | Value |
| --- | --- |
| Nombres de métricas por ejecución |50|
| Filas de métricas por nombre de métrica |10 millones|
| Columnas por fila de métrica |15|
| Longitud del nombre de la columna de métrica |255 caracteres |
| Longitud del valor de la columna de métrica |255 caracteres |
| Filas de métricas por lote cargado | 250 |

> [!NOTE]
> Si alcanza el límite de nombres de métricas por ejecución porque está aplicando formato a las variables en el nombre de la métrica, considere la posibilidad de usar una métrica de fila en la que una columna sea el valor de la variable y la segunda columna el valor de la métrica.

## <a name="artifacts"></a>Artifacts

| Límite | Value |
| --- | --- |
| Número de artefactos por ejecución |10 millones|
| Longitud máxima de la ruta de acceso del artefacto |5000 caracteres |

## <a name="limit-increases"></a>Aumento de límites
Algunos límites se pueden aumentar en áreas de trabajo individuales; para ello, debe [ponerse en contacto con el servicio de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). 

## <a name="next-steps"></a>Pasos siguientes

- [Configurar el entorno de Azure Machine Learning](how-to-configure-environment.md)
