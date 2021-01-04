---
title: 'Tutorial: Entrenamiento de un modelo mediante el aprendizaje automático automatizado'
description: Tutorial para entrenar un modelo de Machine Learning sin código en Azure Synapse mediante Apache Spark y el aprendizaje automático automatizado.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 4967d5305b4b438f3baa6fca078d7b3169612590
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97093407"
---
# <a name="tutorial-train-a-machine-learning-model-code-free-in-azure-synapse-with-apache-spark-and-automated-ml"></a>Tutorial: Entrenamiento de un modelo de aprendizaje automático sin código en Azure Synapse mediante Apache Spark y el aprendizaje automático automatizado

Aprenda a enriquecer fácilmente los datos de las tablas de Spark con los nuevos modelos de Machine Learning que puede entrenar mediante el [aprendizaje automático automatizado en Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml).  Los usuarios de Synapse pueden seleccionar una tabla de Spark en el área de trabajo de Azure Synapse para utilizarla como conjunto de datos de entrenamiento para crear modelos de Machine Learning sin código.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> - Entrenar modelos de Machine Learning sin código en Azure Synapse Studio que utiliza Machine Learning automatizado en Azure Machine Learning. El tipo de modelo que entrena depende del problema que esté intentando resolver.

Si no tiene una suscripción a Azure, [cree una cuenta gratuita antes de empezar](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

- Un [área de trabajo de Synapse Analytics](../get-started-create-workspace.md) con una cuenta de almacenamiento de ADLS Gen2 configurada como el almacenamiento predeterminado. Debe ser el **Colaborador de datos de blobs de almacenamiento** del sistema de archivos de ADLS Gen2 con el que quiere trabajar.
- Grupo de Spark en el área de trabajo de Azure Synapse Analytics. Para más información, consulte el artículo sobre [creación de un grupo de Spark en Azure Synapse](../quickstart-create-sql-pool-studio.md).
- Servicio vinculado de Azure Machine Learning en el área de trabajo de Azure Synapse Analytics. Para más información, consulte [Creación de un servicio vinculado de Azure Machine Learning en Azure Synapse](quickstart-integrate-azure-machine-learning.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

## <a name="create-a-spark-table-for-training-dataset"></a>Creación de una tabla de Spark para conjuntos de datos de entrenamiento

Necesitará una tabla de Spark para este tutorial. El siguiente cuaderno creará una tabla de Spark.

1. Descargue el cuaderno [Create-Spark-Table-NYCTaxi- Data.ipynb](https://go.microsoft.com/fwlink/?linkid=2149229).

1. Importe el cuaderno a Azure Synapse Studio.
![Importar cuaderno](media/tutorial-automl-wizard/tutorial-automl-wizard-00a.png)

1. Seleccione el grupo de Spark que quiere utilizar y haga clic en `Run all`. Al ejecutar este cuaderno, obtendrá los datos de los taxis de Nueva York desde el conjunto de datos abierto y los guardará en la base de datos de Spark predeterminada.
![Ejecutar todo](media/tutorial-automl-wizard/tutorial-automl-wizard-00b.png)

1. Una vez finalizada la ejecución del cuaderno, se creará una nueva tabla de Spark en la base de datos de Spark predeterminada. Vaya al centro de datos y busque la tabla con el nombre `nyc_taxi`.
![Tabla de Spark](media/tutorial-automl-wizard/tutorial-automl-wizard-00c.png)

## <a name="launch-automated-ml-wizard-to-train-a-model"></a>Inicio del Asistente para ML automatizado para entrenar un modelo

Haga clic con el botón derecho en la tabla de Spark creada en el paso anterior. Seleccione "Machine Learning -> Enrich with new model" (Enriquecer con un nuevo modelo) para abrir el asistente.
![Iniciar el Asistente para ML automatizado](media/tutorial-automl-wizard/tutorial-automl-wizard-00d.png)

Aparecerá un panel de configuración, y se le pedirá que especifique detalles de configuración para crear una ejecución de un experimento de aprendizaje automático automatizado en Azure Machine Learning. En esta ejecución se entrenarán varios modelos, y el mejor modelo de una ejecución correcta se registrará en el registro de modelos de Azure Machine Learning:

![Configurar paso 1 de la ejecución](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00a.png)

- **Un área de trabajo de Azure Machine Learning**: se requiere un área de trabajo de Azure Machine Learning para crear la ejecución del experimento de aprendizaje automático automatizado. También debe vincular el área de trabajo de Azure Synapse con el área de trabajo de Azure Machine Learning mediante un [servicio vinculado](quickstart-integrate-azure-machine-learning.md). Una vez que tenga todos los requisitos previos, puede especificar el área de trabajo de Azure Machine Learning que quiere utilizar para esta ejecución de ML automatizado.

- **Nombre del experimento**: especifique el nombre del experimento. Cuando se envía una ejecución de ML automatizado, se especifica un nombre de experimento. La información de la ejecución se almacena en ese experimento en el área de trabajo de Azure Machine Learning. Esta experiencia creará un nuevo experimento de forma predeterminada y generará un nombre propuesto, pero también puede proporcionar un nombre de un experimento existente.

- **El mejor modelo**: especifique el nombre del mejor modelo de una ejecución de ML automatizado. Al mejor modelo se le asignará este nombre y se guardará en el registro de modelos de Azure Machine Learning automáticamente después de esta ejecución. Una ejecución de ML automatizado creará muchos modelos de Machine Learning. En función de la métrica primaria que seleccione en un paso posterior, estos modelos se pueden comparar, y es posible seleccionar el mejor modelo.

- **Columna de destino**: esto es lo que tendrá que predecir el modelo entrenado. Seleccione la columna que desee predecir.

- **Grupo de Spark**: el grupo de Spark que quiere utilizar para la ejecución del experimento de ML automatizado. Los cálculos se ejecutarán en el grupo que especifique.

- **Detalles de configuración de Spark**: además del grupo de Spark, también tiene la opción de proporcionar detalles de la configuración de sesión.

En este tutorial, seleccionamos la columna numérica `fareAmount` como columna de destino.

Haga clic en "Continuar".

## <a name="choose-task-type"></a>Elección del tipo de tarea

Seleccione el tipo de modelo de Machine Learning para el experimento en función de la pregunta a la que intenta responder. Como hemos seleccionado `fareAmount` como columna de destino, y es un valor numérico, seleccionaremos *Regresión*.

Haga clic en "Continuar" para configurar más opciones.

![Selección del tipo de tarea](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00b.png)

## <a name="additional-configurations"></a>Configuraciones adicionales

Si selecciona el tipo *Clasificación* o *Regresión*, las opciones adicionales son las siguientes:

- **Métrica primaria**: métrica que se utiliza para medir el grado de funcionamiento del modelo. Es la métrica que se utilizará para comparar los diferentes modelos creados en la ejecución de ML automatizado y determinar qué modelo ha funcionado mejor.

- **Tiempo de trabajo de entrenamiento (horas)** : cantidad máxima de tiempo, en horas, de un experimento para ejecutar y entrenar modelos. Tenga en cuenta que también puede proporcionar valores inferiores a 1. Por ejemplo, `0.5`.

- **Número máximo de iteraciones simultáneas**: representa el número máximo de iteraciones que se ejecutarán en paralelo.

- **Compatibilidad con el modelo de ONNX**: si está habilitado, los modelos entrenados por ML automatizado se convertirán al formato ONNX. Es especialmente pertinente si quiere utilizar el modelo para la puntuación en grupos de SQL de Azure Synapse.

Todas estas opciones tienen un valor predeterminado que se puede personalizar.
![Configuraciones adicionales](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00c.png)

> Tenga en cuenta que si selecciona "Time series forecasting" (Previsión de serie temporal), se necesitan más configuraciones. La previsión tampoco admite compatibilidad con modelos ONNX.

Una vez que se hayan realizado todas las configuraciones necesarias, puede iniciar la ejecución de ML automatizado.

Hay dos maneras de iniciar una ejecución de ML automatizado en Azure Synapse. Para tener una experiencia sin código, puede optar por **crear una ejecución** directamente. Si prefiere el código, puede seleccionar **Abrir en Notebook**, que le permite ver el código que crea la ejecución y ejecutar el cuaderno.

### <a name="create-run-directly"></a>Creación de ejecución directamente

Haga clic en "Iniciar ejecución" para comenzar la ejecución de ML automatizado directamente. Aparecerá una notificación que indica que se está iniciando la ejecución de ML automatizado.

Una vez que la ejecución de ML automatizado se inicie correctamente, verá otra notificación de que se ha realizado correctamente. También puede hacer clic en el botón de notificación para comprobar el estado del envío de la ejecución.
Aparece Azure Machine Learning si hace clic en el vínculo de la notificación correcta.
![Notificación correcta](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00d.png)

### <a name="create-run-with-notebook"></a>Creación de ejecución con cuaderno

Seleccione *Abrir en Notebook* para generar un cuaderno. Haga clic en *Ejecutar todo* para ejecutar el cuaderno.
Esto también le ofrece la oportunidad de agregar una configuración adicional a la ejecución de ML automatizado.

![Abrir cuaderno](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00e.png)

Una vez que la ejecución del cuaderno se haya enviado correctamente, aparecerá un vínculo a la ejecución del experimento en el área de trabajo de Azure Machine Learning en la salida del cuaderno. Puede hacer clic en el vínculo para supervisar la ejecución de ML automatizado en Azure Machine Learning.
![El cuaderno ejecuta todo](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00f.png)

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Puntuación del modelo de Machine Learning para grupos de SQL dedicados en Azure Synapse](tutorial-sql-pool-model-scoring-wizard.md).
- [Inicio rápido: Creación de un servicio vinculado de Azure Machine Learning en Azure Synapse](quickstart-integrate-azure-machine-learning.md)
- [Funcionalidades de aprendizaje automático en Azure Synapse Analytics](what-is-machine-learning.md)
