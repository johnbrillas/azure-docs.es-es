---
title: 'Tutorial: Creación del modelo predictivo con el aprendizaje automático automatizado (parte 1 de 2)'
titleSuffix: Azure Machine Learning
description: Aprenda a crear e implementar modelos de aprendizaje automático automatizado, de modo que pueda usar el mejor modelo para predecir los resultados en Microsoft Power BI.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: 897f493edf6ccdebb25c201e8e4f9babfb0754c5
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2020
ms.locfileid: "97370878"
---
# <a name="tutorial-power-bi-integration---create-the-predictive-model-using-automated-machine-learning-part-1-of-2"></a>Tutorial: Integración de Power BI: creación del modelo predictivo mediante el aprendizaje automático automatizado (parte 1 de 2)

En la primera parte de este tutorial, va a entrenar e implementar un modelo de aprendizaje automático predictivo mediante el aprendizaje automático automatizado en Azure Machine Learning Studio.  En la parte 2, usará el modelo con el mejor rendimiento para predecir los resultados en Microsoft Power BI.

En este tutorial ha:

> [!div class="checklist"]
> * Creación de un clúster de proceso de Azure Machine Learning
> * Crear un conjunto de datos
> * Creará una ejecución de aprendizaje automático automatizado
> * Implementará el mejor modelo en un punto de conexión de puntuación en tiempo real


Existen tres formas diferentes de crear e implementar el modelo que se va a usar en Power BI.  En este artículo se describe la Opción C: Entrenamiento e implementación de modelos con el aprendizaje automático automatizado en Studio.  Esta opción muestra una experiencia de creación sin código que automatiza totalmente la preparación de los datos y el entrenamiento del modelo. 

Como alternativa, podría usar:

* [Opción A: Entrenamiento e implementación de modelos mediante cuadernos](tutorial-power-bi-custom-model.md): una experiencia de creación de tipo "código primero" con cuadernos de Jupyter que se hospedan en Azure Machine Learning Studio.
* [Opción B: Entrenamiento e implementación de modelos con el diseñador](tutorial-power-bi-designer-model.md): una experiencia de creación con poco código mediante el diseñador (una interfaz de usuario de arrastrar y colocar).

## <a name="prerequisites"></a>Prerrequisitos

- Una suscripción a Azure ([existe una evaluación gratuita](https://aka.ms/AMLFree)). 
- Un área de trabajo de Azure Machine Learning. Si aún no tiene un área de trabajo, siga los pasos sobre [cómo crear un área de trabajo de Azure Machine Learning](./how-to-manage-workspace.md#create-a-workspace).

## <a name="create-compute-cluster"></a>Creación de un clúster de proceso

Con el aprendizaje automático automatizado, se entrenan numerosos modelos de aprendizaje automático diferentes hasta encontrar el algoritmo y los parámetros "mejores". Azure Machine Learning realiza en paralelo la ejecución del entrenamiento del modelo a través de un clúster de proceso.

En [Azure Machine Learning Studio](https://ml.azure.com), seleccione **Proceso** en el menú de la izquierda y, luego, la pestaña **Clústeres de proceso**. Seleccione **Nuevo**:

:::image type="content" source="media/tutorial-power-bi/create-compute-cluster.png" alt-text="Captura de pantalla que muestra cómo crear un clúster de proceso":::

En la pantalla **Create compute cluster** (Crear clúster de proceso), siga estos pasos:

1. Seleccione un tamaño de máquina virtual (para los fines de este tutorial, una máquina `Standard_D11_v2` servirá).
1. Seleccione **Siguiente**.
1. Proporcione un nombre de proceso válido.
1. Mantenga **Minimum number of nodes** (Número mínimo de nodo) en 0.
1. Cambie **Maximum number of nodes** (Número máximo de nodos) a 4.
1. Seleccione **Crear**

Puede ver que el estado del clúster ha cambiado a **Creando**.

>[!NOTE]
> Cuando se crea el clúster, tendrá 0 nodos, lo que significa que no se generan costos de proceso. Solo se generan costos cuando se ejecuta el trabajo de aprendizaje automático automatizado. El clúster se escalará de nuevo a 0 automáticamente al cabo de 120 segundos de tiempo de inactividad.


## <a name="create-dataset"></a>Creación de un conjunto de datos

En este tutorial, usará el [conjunto de datos Diabetes](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html), que está disponible en [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/).

Para crear el conjunto de datos, seleccione el menú izquierdo **Conjuntos de datos** y, luego, **Crear conjunto de datos**; verá las siguientes opciones:

:::image type="content" source="media/tutorial-power-bi/create-dataset.png" alt-text="Captura de pantalla que muestra cómo crear un conjunto de datos":::

Seleccione **From Open Datasets** (Desde Open Datasets) y, luego, en la pantalla **Create dataset from Open Datasets** (Crear conjunto de datos desde Open Datasets):

1. Busque *diabetes* en la barra de búsqueda.
1. Seleccione **Muestra: Diabetes**.
1. Seleccione **Siguiente**.
1. Proporcione un nombre para el conjunto de datos (en este caso, *diabetes*).
1. Seleccione **Crear**

Puede explorar los datos seleccionando el conjunto de datos seguido de **Explorar**:

:::image type="content" source="media/tutorial-power-bi/explore-dataset.png" alt-text="Captura de pantalla que muestra cómo explorar el conjunto de datos":::

Los datos tienen 10 variables de entrada de base de referencia (como la edad, el sexo, el índice de masa corporal, la presión arterial media y seis mediciones del suero en sangre) y una variable de destino denominada **Y** (una medida cuantitativa de la progresión de la diabetes un año después de la base de referencia).

## <a name="create-automated-ml-run"></a>Creación de una ejecución de aprendizaje automático automatizado

En [Azure Machine Learning Studio](https://ml.azure.com), seleccione **Automated ML** (ML automatizado) en el menú izquierdo seguido de **New Automated ML Run** (Nueva ejecución de ML automatizado):

:::image type="content" source="media/tutorial-power-bi/create-new-run.png" alt-text="Captura de pantalla que muestra cómo crear una ejecución de aprendizaje automático automatizado":::

A continuación, seleccione el conjunto de datos **diabetes** que creó anteriormente y elija **Siguiente**:

:::image type="content" source="media/tutorial-power-bi/select-dataset.png" alt-text="Captura de pantalla que muestra cómo seleccionar un conjunto de datos":::
 
En la pantalla **Configure run** (Configurar ejecución), haga lo siguiente:

1. En **Nombre del experimento**, seleccione **Crear nuevo**.
1. Proporcione un nombre para el experimento.
1. En el campo Columna de destino, seleccione **Y**.
1. En el campo **Select compute cluster** (Seleccionar clúster de proceso), seleccione el clúster de proceso que creó anteriormente. 

El formulario completado debe ser similar a este:

:::image type="content" source="media/tutorial-power-bi/configure-automated.png" alt-text="Captura de pantalla que muestra cómo configurar el aprendizaje automático automatizado":::

Por último, debe seleccionar la tarea de aprendizaje automático que se va a realizar, que es **Regresión**:

:::image type="content" source="media/tutorial-power-bi/configure-task.png" alt-text="Captura de pantalla que muestra cómo configurar la tarea":::

Seleccione **Finalizar**.

> [!IMPORTANT]
> El aprendizaje automático automatizado tardará unos 30 minutos en finalizar el entrenamiento de los 100 modelos diferentes.

## <a name="deploy-the-best-model"></a>Implementación del mejor modelo

Una vez completada la ejecución de aprendizaje automático automatizado, puede seleccionar la pestaña **Modelos** para ver la lista de todos los diferentes modelos de aprendizaje automático que se han probado. Los modelos se ordenan según el rendimiento, con el de mejor rendimiento en primer lugar. Al seleccionar el mejor modelo, se habilitará el botón **Implementar**:

:::image type="content" source="media/tutorial-power-bi/list-models.png" alt-text="Captura de pantalla que muestra la lista de modelos":::

Al seleccionar **Implementar**, se presentará una pantalla **Deploy a model** (Implementar un modelo):

1. Proporcione un nombre para el servicio de modelo (use **diabetes-model)** .
1. Seleccione **Azure Container Service**
1. Seleccione **Implementar**.

Debería ver un mensaje que indica que el modelo se ha implementado correctamente.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha visto cómo entrenar e implementar un modelo de aprendizaje automático mediante el aprendizaje automático automatizado. En el siguiente tutorial se muestra cómo consumir (puntuar) este modelo desde Power BI.

> [!div class="nextstepaction"]
> [Tutorial: Consumo del modelo en Power BI](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
