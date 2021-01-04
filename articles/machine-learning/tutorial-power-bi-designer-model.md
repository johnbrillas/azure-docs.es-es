---
title: 'Tutorial: Arrastrar y colocar para crear el modelo predictivo (parte 1 de 2)'
titleSuffix: Azure Machine Learning
description: Aprenda a crear e implementar un modelo de aprendizaje automático predictivo con el diseñador, de forma que pueda usarlo para predecir los resultados en Microsoft Power BI.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: f0e1ffe60069a2379f8eddab1aae74db2b4eac50
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2020
ms.locfileid: "97370790"
---
# <a name="tutorial--power-bi-integration---drag-and-drop-to-create-the-predictive-model-part-1-of-2"></a>Tutorial:  Integración de Power BI: arrastrar y colocar para crear el modelo predictivo (parte 1 de 2)

En la primera parte de este tutorial, va a entrenar e implementar un modelo de aprendizaje automático predictivo mediante el diseñador de Azure Machine Learning, una interfaz de usuario de arrastrar y colocar con poco uso de código. En la parte 2, usará el modelo para predecir los resultados en Microsoft Power BI.

En este tutorial ha:

> [!div class="checklist"]
> * Creará una instancia de proceso de Azure Machine Learning
> * Creación de un clúster de inferencia de Azure Machine Learning
> * Crear un conjunto de datos
> * Entrenamiento de un modelo de regresión
> * Implementará el modelo en un punto de conexión de puntuación en tiempo real


Existen tres formas diferentes de crear e implementar el modelo que se va a usar en Power BI.  En este artículo se describe la Opción B: Entrenamiento e implementación de modelos mediante el diseñador.  Esta opción muestra una experiencia de creación con poco código mediante el diseñador (una interfaz de usuario de arrastrar y colocar).  

Como alternativa, podría usar:

* [Opción A: Entrenamiento e implementación de modelos mediante cuadernos](tutorial-power-bi-custom-model.md): una experiencia de creación de tipo "código primero" con cuadernos de Jupyter que se hospedan en Azure Machine Learning Studio.
* [Opción C: Entrenamiento e implementación de modelos con el aprendizaje automático automatizado](tutorial-power-bi-automated-model.md): una experiencia de creación sin código que automatiza totalmente la preparación de datos y el entrenamiento del modelo.

## <a name="prerequisites"></a>Prerrequisitos

- Una suscripción a Azure ([existe una evaluación gratuita](https://aka.ms/AMLFree)). 
- Un área de trabajo de Azure Machine Learning. Si aún no tiene un área de trabajo, siga los pasos sobre [cómo crear un área de trabajo de Azure Machine Learning](./how-to-manage-workspace.md#create-a-workspace).
- Conocimiento básico de los flujos de trabajo de aprendizaje automático.


## <a name="create-compute-for-training-and-scoring"></a>Creación de proceso para entrenamiento y puntuación

En esta sección, creará una *instancia de proceso*, que se usa para entrenar modelos de aprendizaje automático. Además, creará un *clúster de inferencia* que se usará para hospedar el modelo implementado para la puntuación en tiempo real.

Inicie sesión en [Azure Machine Learning Studio](https://ml.azure.com) y seleccione **Proceso** en el menú de la izquierda y, luego, **Nuevo**.

:::image type="content" source="media/tutorial-power-bi/create-new-compute.png" alt-text="Captura de pantalla que muestra cómo crear una instancia de proceso":::

En la pantalla **Create compute instance** (Crear instancia de proceso) resultante, seleccione un tamaño de máquina virtual (para este tutorial, seleccione `Standard_D11_v2`) y, luego, **Siguiente**. En la página Configuración, proporcione un nombre válido para la instancia de proceso y, luego, seleccione **Crear**. 

>[!TIP]
> La instancia de proceso también se puede usar para crear y ejecutar cuadernos.

Ahora puede ver que el **estado** de la instancia de proceso es **Creando**; el aprovisionamiento de la máquina tardará unos 4 minutos. Mientras espera, seleccione la pestaña **Inference Clusters** (Clústeres de inferencia) en la página de proceso y, luego, elija **Nuevo**:

:::image type="content" source="media/tutorial-power-bi/create-cluster.png" alt-text="Captura de pantalla que muestra cómo crear un clúster de inferencia":::

En la página **Create inference cluster** (Crear un clúster de inferencia) resultante, seleccione una región y, luego, un tamaño de máquina virtual (para los fines de este tutorial, seleccione `Standard_D11_v2`) y, después, **Siguiente**. En la página **Parámetros de configuración**, haga lo siguiente:

1. Proporcione un nombre de proceso válido.
1. Seleccione **Dev-test** (Dearrollo-prueba) como finalidad del clúster (crea un solo nodo para hospedar el modelo implementado)
1. Seleccione **Crear**

Ahora puede ver que el **estado** del clúster de inferencia es **Creando**; la implementación del clúster con un solo nodo tardará unos 4 minutos.

## <a name="create-a-dataset"></a>Crear un conjunto de datos

En este tutorial, usará el [conjunto de datos Diabetes](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html), que está disponible en [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/).

Para crear el conjunto de datos, en el menú de la izquierda seleccione **Conjuntos de datos** y, luego, **Crear conjunto de datos**; verá las siguientes opciones:

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

## <a name="create-a-machine-learning-model-using-designer"></a>Creación de un modelo de Machine Learning mediante el diseñador

Cuando haya creado el proceso y los conjuntos de datos, puede continuar con la creación del modelo de aprendizaje automático mediante el diseñador. En el Azure Machine Learning Studio, seleccione **Diseñador** y, luego, **Nueva canalización**:

:::image type="content" source="media/tutorial-power-bi/create-designer.png" alt-text="Captura de pantalla que muestra cómo crear una canalización":::

Verá un *lienzo* en blanco, donde puede observar también un **menú de configuración**:

:::image type="content" source="media/tutorial-power-bi/select-compute.png" alt-text="Captura de pantalla que muestra cómo seleccionar un destino de proceso":::

En el **menú de configuración**, **seleccione un destino de proceso** y, luego, la instancia de proceso que creó anteriormente seguido de **Guardar**. Cambie el nombre del **borrador** por algo que sea más fácil de recordar (por ejemplo, *diabetes-model*) y escriba una descripción.

A continuación, en los recursos enumerados, expanda **Conjuntos de datos** y busque el que se llama **diabetes**; arrastre y coloque este módulo en el lienzo:

:::image type="content" source="media/tutorial-power-bi/drag-component.png" alt-text="Captura de pantalla que muestra cómo arrastrar un componente":::

A continuación, arrastre y coloque los siguientes componentes en el lienzo:

1. Linear regression (Regresión lineal) (ubicado en **Machine Learning Algorithms** [Algoritmos de aprendizaje automático]).
1. Train model (Entrenar modelo) (ubicado en **Model Training** [Entrenamiento de modelos]).

El lienzo debe parecerse a este (observe a continuación que encima y debajo de los componentes hay pequeños círculos denominados puertos, resaltados en rojo):

:::image type="content" source="media/tutorial-power-bi/connections.png" alt-text="Captura de pantalla que muestra los componentes desconectados":::
 
A continuación, debe *conectar* estos componentes. Seleccione el puerto situado debajo del conjunto de datos **diabetes** y arrástrelo al puerto de la derecha encima del componente **Train model** (Entrenar modelo). Seleccione el puerto situado debajo del componente **Linear regression** (Regresión lineal) y arrástrelo al puerto de la derecha encima del componente **Train model** (Entrenar modelo).

Elija la columna del conjunto de datos que se va a usar como variable de etiqueta (de destino) que se predecirá Seleccione el componente **Train model** (Entrenar modelo) seguido de **Editar columna**. En el cuadro de diálogo, seleccione **Enter Column name** (Escribir nombre de columna) seguido de **Y** en la lista desplegable:

:::image type="content" source="media/tutorial-power-bi/label-columns.png" alt-text="Captura de pantalla de selección de columna de etiqueta":::

Seleccione **Guardar**. El *flujo de trabajo* de aprendizaje automático debe parecerse a este:

:::image type="content" source="media/tutorial-power-bi/connected-diagram.png" alt-text="Captura de pantalla que muestra los componentes conectados":::

Seleccione **Enviar** y, luego, **Crear nuevo** debajo del experimento. Proporcione un nombre para el experimento y seleccione **Enviar**.

>[!NOTE]
> La primera ejecución del experimento tardará unos 5 minutos en completarse. Las ejecuciones posteriores son mucho más rápidas: el diseñador almacena en caché los componentes ya ejecutados para reducir la latencia.

Una vez finalizado el experimento, verá:

:::image type="content" source="media/tutorial-power-bi/completed-run.png" alt-text="Captura de pantalla que muestra la ejecución finalizada":::

Puede inspeccionar los registros del experimento seleccionando **Train model** (Entrenar modelo) seguido de **Outputs + logs** (Salidas y registros).

## <a name="deploy-the-model"></a>Implementación del modelo

Para implementar el modelo, seleccione **Create Inference Pipeline** (Crear canalización de inferencia) (que se encuentra en la parte superior del lienzo) seguido de **Real-time inference pipeline** (Canalización de inferencia en tiempo real):

:::image type="content" source="media/tutorial-power-bi/pipeline.png" alt-text="Captura de pantalla que muestra la canalización de inferencia en tiempo real":::
 
La canalización se simplifica a solo los componentes necesarios para realizar la puntuación del modelo. Al puntuar los datos, no conocerá los valores de las variables de destino, por lo que podemos quitar **Y** del conjunto de datos. Para ello, agregue al lienzo un componente **Select columns in Dataset** (Seleccionar columnas del conjunto de datos). Conecte el componente para que el conjunto de datos diabetes sea la entrada y los resultados sean la salida en el componente **Score Model** (Puntuar modelo):

:::image type="content" source="media/tutorial-power-bi/remove-column.png" alt-text="Captura de pantalla que muestra la eliminación de una columna":::

Seleccione el componente **Select Columns in Dataset** (Selección de columnas del conjunto de datos) seguido de **Edit Columns** (Editar columnas). En el cuadro de diálogo Seleccionar columnas, elija **Por nombre** y asegúrese de que todas las variables de entrada estén seleccionadas, pero **no** el destino:

:::image type="content" source="media/tutorial-power-bi/removal-settings.png" alt-text="Captura de pantalla que muestra la eliminación de una configuración de columna":::

Seleccione **Guardar**. Por último, seleccione el componente **Score Model** (Puntuar modelo) y asegúrese de que la casilla **Append score columns to output** (Anexar columnas de puntuación a la salida) esté desactivada (en lugar de las entradas *y* las predicciones, solo se devuelven las segundas, lo que reduce la latencia):

:::image type="content" source="media/tutorial-power-bi/score-settings.png" alt-text="Captura de pantalla que muestra la configuración del componente Score Model (Puntuar modelo)":::

Seleccione **Enviar** en la parte superior del lienzo.

Cuando haya ejecutado correctamente la canalización de inferencia, puede implementar el modelo en el clúster de inferencia. Seleccione **Deploy new real-time endpoint** (Implementar nuevo punto de conexión en tiempo real); se mostrará el cuadro de diálogo **Set-up real-time endpoint** (Configurar punto de conexión en tiempo real). Seleccione **Deploy new real-time endpoint** (Implementar nuevo punto de conexión en tiempo real), asigne al punto de conexión el nombre **my-diabetes-model**, seleccione la inferencia que creó anteriormente y, luego, elija **Implementar**:

:::image type="content" source="media/tutorial-power-bi/endpoint-settings.png" alt-text="Captura de pantalla que muestra la configuración del punto de conexión en tiempo real":::
## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha visto cómo entrenar e implementar un modelo del diseñador. En la siguiente parte, aprenderá a consumir (puntuar) este modelo desde Power BI.

> [!div class="nextstepaction"]
> [Tutorial: Consumo del modelo en Power BI](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
