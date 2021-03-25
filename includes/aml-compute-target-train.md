---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 09/17/2020
ms.openlocfilehash: 081eea7842652884363603bc5ba742ba48539f85
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102503630"
---
**Los destinos de proceso se pueden reutilizar de un trabajo de entrenamiento al siguiente**. Por ejemplo, después de adjuntar una VM remota al área de trabajo, puede reutilizarla para varios trabajos. En el caso de las canalizaciones de aprendizaje automático, use el [paso de canalización](/python/api/azureml-pipeline-steps/azureml.pipeline.steps) adecuado para cada destino de proceso.

Puede usar cualquiera de los siguientes recursos para un destino de proceso de entrenamiento para la mayoría de los trabajos. No todos los recursos se pueden usar para el aprendizaje automático automatizado, canalizaciones de aprendizaje automático o el diseñador.

|&nbsp;Objetivos del aprendizaje|[Aprendizaje automático automatizado](../articles/machine-learning/concept-automated-ml.md) | [Canalizaciones de aprendizaje automático](../articles/machine-learning/concept-ml-pipelines.md) | [Diseñador de Azure Machine Learning](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Equipo local](../articles/machine-learning/how-to-attach-compute-targets.md#local)| Sí | &nbsp; | &nbsp; |
|[Clúster de proceso de Azure Machine Learning](../articles/machine-learning/how-to-create-attach-compute-cluster.md)| Sí | Sí | Sí |
|[Instancia de proceso de Azure Machine Learning](../articles/machine-learning/how-to-create-manage-compute-instance.md) | Sí (mediante el SDK)  | Sí |  |
|[Máquina virtual remota](../articles/machine-learning/how-to-attach-compute-targets.md#vm) | Sí  | Sí | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/how-to-attach-compute-targets.md#databricks)| Sí (solo en el modo local del SDK) | Sí | &nbsp; |
|[Análisis con Azure Data Lake](../articles/machine-learning/how-to-attach-compute-targets.md#adla) | &nbsp; | Sí | &nbsp; |
|[HDInsight de Azure](../articles/machine-learning/how-to-attach-compute-targets.md#hdinsight) | &nbsp; | Sí | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-attach-compute-targets.md#azbatch) | &nbsp; | Sí | &nbsp; |
