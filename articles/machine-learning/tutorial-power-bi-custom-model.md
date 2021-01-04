---
title: 'Tutorial: Creación del modelo predictivo con un cuaderno (parte 1 de 2)'
titleSuffix: Azure Machine Learning
description: Aprenda a crear e implementar un modelo de aprendizaje automático mediante código en un cuaderno de Jupyter Notebook, de forma que pueda usarlo para predecir los resultados en Microsoft Power BI.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: f8209c0d26cf8c572d10666696231b0468cfcbc6
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2020
ms.locfileid: "97370841"
---
# <a name="tutorial-power-bi-integration---create-the-predictive-model-with-a-notebook-part-1-of-2"></a>Tutorial: Integración de Power BI: creación del modelo predictivo con un cuaderno (parte 1 de 2)

En la primera parte de este tutorial, va a entrenar e implementar un modelo de aprendizaje automático predictivo mediante código en un cuaderno de Jupyter Notebook. En la parte 2, usará el modelo para predecir los resultados en Microsoft Power BI.

En este tutorial ha:

> [!div class="checklist"]
> * Creación de un cuaderno de Jupyter Notebook
> * Creará una instancia de proceso de Azure Machine Learning
> * Entrenará un modelo de regresión mediante scikit-learn
> * Implementará el modelo en un punto de conexión de puntuación en tiempo real

Existen tres formas diferentes de crear e implementar el modelo que se va a usar en Power BI.  En este artículo se describe la Opción A: Entrenamiento e implementación de modelos con cuadernos.  Esta opción muestra una experiencia de creación de tipo "código primero" mediante cuadernos de Jupyter que se hospedan en Azure Machine Learning Studio. 

Como alternativa, podría usar:

* [Opción B: Entrenamiento e implementación de modelos con el diseñador](tutorial-power-bi-designer-model.md): una experiencia de creación con poco código mediante el diseñador (una interfaz de usuario de arrastrar y colocar).
* [Opción C: Entrenamiento e implementación de modelos con el aprendizaje automático automatizado](tutorial-power-bi-automated-model.md): una experiencia de creación sin código que automatiza totalmente la preparación de datos y el entrenamiento del modelo.


## <a name="prerequisites"></a>Prerrequisitos

- Una suscripción a Azure ([existe una evaluación gratuita](https://aka.ms/AMLFree)). 
- Un área de trabajo de Azure Machine Learning. Si aún no tiene un área de trabajo, siga los pasos sobre [cómo crear un área de trabajo de Azure Machine Learning](./how-to-manage-workspace.md#create-a-workspace).
- Conocimiento introductorio del lenguaje Python y los flujos de trabajo de aprendizaje automático.

## <a name="create-a-notebook-and-compute"></a>Creación de un cuaderno y un recurso de proceso

En la página principal de [Azure Machine Learning Studio](https://ml.azure.com), seleccione **Crear nuevo** y, luego, **Cuaderno**:

:::image type="content" source="media/tutorial-power-bi/create-new-notebook.png" alt-text="Captura de pantalla que muestra cómo crear un cuaderno":::
 
Se muestra el cuadro de diálogo **Create a new file** (Crear un archivo nuevo).

1. Escriba un nombre de archivo para el cuaderno (por ejemplo `my_model_notebook`).
1. Cambie **Tipo de archivo** a **Cuaderno**.

Seleccione **Crear**. A continuación, debe crear algún proceso y asociarlo al cuaderno para ejecutar celdas de código. Para ello, seleccione el icono de signo más situado en la parte superior del cuaderno:

:::image type="content" source="media/tutorial-power-bi/create-compute.png" alt-text="Captura de pantalla que muestra cómo crear una instancia de proceso":::

A continuación, en la página **Create compute instance** (Crear instancia de proceso), siga estos pasos:

1. Elija un tamaño de máquina virtual de CPU (para los fines de este tutorial, un tamaño **Standard_D11_v2** [dos núcleos, 14 GB de RAM] servirá).
1. Seleccione **Next** (Siguiente). 
1. En la página **Parámetros de configuración**, proporcione un **nombre de proceso** válido (los caracteres admitidos son mayúsculas y minúsculas, dígitos y guiones).
1. Seleccione **Crear**.

Puede que observe en el cuaderno que el círculo situado junto a **Proceso** se ha vuelto de color cian, lo que indica que se está creando la instancia de proceso:

:::image type="content" source="media/tutorial-power-bi/creating.png" alt-text="Captura de pantalla que muestra el proceso que se está creando":::

> [!NOTE]
> El proceso puede tardar entre 2 y 4 minutos en aprovisionarse.

Una vez aprovisionado el proceso, puede usar el cuaderno para ejecutar las celdas de código. Por ejemplo, escriba en la celda:

```python
import numpy as np

np.sin(3)
```

Seguido de **Mayús+Entrar** (o **Control+Entrar** o seleccione el botón de reproducción situado junto a la celda). Debería ver la siguiente salida:

:::image type="content" source="media/tutorial-power-bi/simple-sin.png" alt-text="Captura de pantalla que muestra la ejecución de la celda":::

Ya está listo para crear un modelo de Machine Learning.

## <a name="build-a-model-using-scikit-learn"></a>Creación de un modelo con scikit-learn

En este tutorial, usará el [conjunto de datos Diabetes](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html), que está disponible en [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/). 


### <a name="import-data"></a>Importar datos

Para importar los datos, copie y pegue el código siguiente en una nueva **celda de código** en el cuaderno:

```python
from azureml.opendatasets import Diabetes

diabetes = Diabetes.get_tabular_dataset()
X = diabetes.drop_columns("Y")
y = diabetes.keep_columns("Y")
X_df = X.to_pandas_dataframe()
y_df = y.to_pandas_dataframe()
X_df.info()
```

La trama de datos de Pandas `X_df` contiene 10 variables de entrada de base de referencia (como la edad, el sexo, el índice de masa corporal, la presión arterial media y seis mediciones del suero en sangre). La trama de datos de Pandas `y_df` es la variable de destino que contiene una medida cuantitativa de la progresión de la enfermedad un año después de la base de referencia. Hay un total de 442 registros.

### <a name="train-model"></a>Entrenamiento de un modelo

Cree una **celda de código** en el cuaderno y copie y pegue el fragmento de código siguiente, que construye un modelo de regresión contraída y serializa el modelo con el formato pickle de Python:

```python
import joblib
from sklearn.linear_model import Ridge

model = Ridge().fit(X,y)
joblib.dump(model, 'sklearn_regression_model.pkl')
```

### <a name="register-the-model"></a>Registro del modelo

Además del contenido del propio archivo del modelo, el modelo registrado también almacenará sus metadatos, como la descripción, las etiquetas y la información del marco, lo que será útil al administrar e implementar modelos en el área de trabajo. Con las etiquetas, por ejemplo, puede clasificar los modelos y aplicar filtros al enumerar los modelos del área de trabajo. Además, al marcar este modelo con el marco scikit-learn, será más sencillo implementarlo como un servicio web, como veremos más adelante.

Copie y pegue el código siguiente en una nueva **celda de código** del cuaderno:

```python
import sklearn

from azureml.core import Workspace
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

ws = Workspace.from_config()

model = Model.register(workspace=ws,
                       model_name='my-sklearn-model',                # Name of the registered model in your workspace.
                       model_path='./sklearn_regression_model.pkl',  # Local file to upload and register as a model.
                       model_framework=Model.Framework.SCIKITLEARN,  # Framework used to create the model.
                       model_framework_version=sklearn.__version__,  # Version of scikit-learn used to create the model.
                       sample_input_dataset=X,
                       sample_output_dataset=y,
                       resource_configuration=ResourceConfiguration(cpu=2, memory_in_gb=4),
                       description='Ridge regression model to predict diabetes progression.',
                       tags={'area': 'diabetes', 'type': 'regression'})

print('Name:', model.name)
print('Version:', model.version)
```

También puede ver el modelo en Azure Machine Learning Studio desplazándose hasta **Puntos de conexión** en el menú de la izquierda:

:::image type="content" source="media/tutorial-power-bi/model.png" alt-text="Captura de pantalla que muestra el modelo":::

### <a name="define-the-scoring-script"></a>Definición del script de puntuación

Al implementar un modelo que se va a integrar en Microsoft Power BI, debe definir un *script de puntuación* de Python y un entorno personalizado. El script de puntuación contiene dos funciones:

- `init()` : esta función se ejecuta una vez que se inicia el servicio. Esta función carga el modelo (tenga en cuenta que el modelo se descarga automáticamente del registro de modelos) y lo deserializa.
- `run(data)`: esta función se ejecuta cuando se realiza una llamada al servicio con algunos datos de entrada que necesitan puntuación. 

>[!NOTE]
> Usamos elementos decoradores de Python para definir el esquema de los datos de entrada y salida, que es importante para que funcione la integración de Microsoft Power BI.

Copie y pegue el código siguiente en una nueva **celda de código** del cuaderno. El siguiente fragmento de código tiene una instrucción mágica de celda que escribirá el código en un archivo llamado score.py.

```python
%%writefile score.py

import json
import pickle
import numpy as np
import pandas as pd
import os
import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType


def init():
    global model
    # Replace filename if needed.
    path = os.getenv('AZUREML_MODEL_DIR') 
    model_path = os.path.join(path, 'sklearn_regression_model.pkl')
    # Deserialize the model file back into a sklearn model.
    model = joblib.load(model_path)


input_sample = pd.DataFrame(data=[{
    "AGE": 5,
    "SEX": 2,
    "BMI": 3.1,
    "BP": 3.1,
    "S1": 3.1,
    "S2": 3.1,
    "S3": 3.1,
    "S4": 3.1,
    "S5": 3.1,
    "S6": 3.1
}])

# This is an integer type sample. Use the data type that reflects the expected result.
output_sample = np.array([0])

# To indicate that we support a variable length of data input,
# set enforce_shape=False
@input_schema('data', PandasParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        print("input_data....")
        print(data.columns)
        print(type(data))
        result = model.predict(data)
        print("result.....")
        print(result)
    # You can return any data type, as long as it is JSON serializable.
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

### <a name="define-the-custom-environment"></a>Definición del entorno personalizado

A continuación, debemos definir el entorno para puntuar el modelo: es necesario definir en este entorno los paquetes de Python requeridos por el script de puntuación (score.py) definido anteriormente, como pandas, scikit-learn, etc.

Para definir el entorno, copie y pegue el código siguiente en una nueva **celda de código** del cuaderno:

```python
from azureml.core.model import InferenceConfig
from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies

environment = Environment('my-sklearn-environment')
environment.python.conda_dependencies = CondaDependencies.create(pip_packages=[
    'azureml-defaults',
    'inference-schema[numpy-support]',
    'joblib',
    'numpy',
    'pandas',
    'scikit-learn=={}'.format(sklearn.__version__)
])

inference_config = InferenceConfig(entry_script='./score.py',environment=environment)
```

### <a name="deploy-the-model"></a>Implementación del modelo

Para implementar el modelo, copie y pegue el código siguiente en una nueva **celda de código** del cuaderno:

```python
service_name = 'my-diabetes-model'

service = Model.deploy(ws, service_name, [model], inference_config, overwrite=True)
service.wait_for_deployment(show_output=True)
```

>[!NOTE]
> El servicio tarda ente 2 y 4 minutos en implementarse.

Si la operación se ha realizado correctamente, debería ver la siguiente salida:

```txt
Tips: You can try get_logs(): https://aka.ms/debugimage#dockerlog or local deployment: https://aka.ms/debugimage#debug-locally to debug if deployment takes longer than 10 minutes.
Running......................................................................................
Succeeded
ACI service creation operation finished, operation "Succeeded"
```

También puede ver el servicio en Azure Machine Learning Studio desplazándose hasta **Puntos de conexión** en el menú de la izquierda:

:::image type="content" source="media/tutorial-power-bi/endpoint.png" alt-text="Captura de pantalla que muestra el punto de conexión":::

Se recomienda probar el servicio web para asegurarse de que funciona según lo previsto. Vuelva al cuaderno seleccionando **Cuadernos** en el menú de la izquierda de Azure Machine Learning Studio. Copie y pegue el código siguiente en una nueva **celda de código** del cuaderno para probar el servicio:

```python
import json


input_payload = json.dumps({
    'data': X_df[0:2].values.tolist(),
    'method': 'predict'  # If you have a classification model, you can get probabilities by changing this to 'predict_proba'.
})

output = service.run(input_payload)

print(output)
```

La salida debe parecerse a la siguiente estructura JSON: `{'predict': [[205.59], [68.84]]}`.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha visto cómo crear e implementar un modelo de manera que Microsoft Power BI pueda utilizarlo. En la siguiente parte, aprenderá a consumir este modelo desde un informe de Power BI.

> [!div class="nextstepaction"]
> [Tutorial: Consumo del modelo en Power BI](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
