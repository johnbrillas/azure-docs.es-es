---
title: 'Tutorial: Entrenamiento de su primer modelo de Machine Learning: Python'
titleSuffix: Azure Machine Learning
description: En la parte 3 de la serie de introducción de Azure Machine Learning se muestra cómo entrenar un modelo de Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: 17bf7b3f457ff6046d92012ffd679ed4b9315530
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96574129"
---
# <a name="tutorial-train-your-first-machine-learning-model-part-3-of-4"></a>Tutorial: Entrenamiento de su primer modelo de Machine Learning (parte 3 de 4)

En este tutorial se muestra cómo entrenar un modelo de Machine Learning en Azure Machine Learning.

Este tutorial es la *parte 3 de una serie de tutoriales de cuatro partes* en la que aprenderá los aspectos básicos de Azure Machine Learning y completará tareas de aprendizaje automático basadas en trabajos en Azure. Este tutorial se basa en el trabajo completado en [Parte 1: Configuración](tutorial-1st-experiment-sdk-setup-local.md) y [Parte 2: Ejecución de "Hola mundo"](tutorial-1st-experiment-hello-world.md) de la serie.

En este tutorial, realizará el siguiente paso mediante el envío de un script que entrena un modelo de Machine Learning. Este ejemplo le ayudará a comprender cómo Azure Machine Learning facilita el comportamiento coherente entre la depuración local y las ejecuciones remotas.

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Cree un script de entrenamiento.
> * Usar Conda para definir un entorno de Azure Machine Learning.
> * Crear un script de control.
> * Comprender las clases de Azure Machine Learning (`Environment`, `Run`, `Metrics`).
> * Enviar y ejecutar el script de entrenamiento.
> * Ver la salida del código en la nube.
> * Registrar métricas en Azure Machine Learning.
> * Ver las métricas en la nube.

## <a name="prerequisites"></a>Requisitos previos

* Completar la [parte 2](tutorial-1st-experiment-hello-world.md) de la serie.
* Conocimiento introductorio del lenguaje Python y los flujos de trabajo de aprendizaje automático.
* Entorno de desarrollo local, como Visual Studio Code, Jupyter o PyCharm.
* Python (versión 3.5 a 3.7)

## <a name="create-training-scripts"></a>Creación de scripts de entrenamiento

En primer lugar, defina la arquitectura de red neuronal en un archivo de `model.py`. Todo el código de entrenamiento entrará en el subdirectorio `src`, incluido `model.py`.

El código siguiente se toma de [este ejemplo de introducción](https://pytorch.org/tutorials/beginner/blitz/cifar10_tutorial.html) de PyTorch. Tenga en cuenta que los conceptos de Azure Machine Learning se aplican a cualquier código de aprendizaje automático, no solo PyTorch.

:::code language="python" source="~/MachineLearningNotebooks/tutorials/get-started-day1/IDE-users/src/model.py":::

A continuación, defina el script de entrenamiento. Este script descarga el conjunto de datos de CIFAR10 con las API `torchvision.dataset` de PyTorch, configura la red que se define en `model.py` y realiza el entrenamiento en dos tiempos mediante el estándar SGD y la pérdida de entropía cruzada.

Cree un script de `train.py` en el subdirectorio `src`:

:::code language="python" source="~/MachineLearningNotebooks/tutorials/get-started-day1/IDE-users/src/train.py":::

Ahora tiene la estructura de directorios siguiente:

```txt
tutorial
└──.azureml
|  └──config.json
└──src
|  └──hello.py
|  └──model.py
|  └──train.py
└──01-create-workspace.py
└──02-create-compute.py
└──03-run-hello.py
```

> [!div class="nextstepaction"]
> [He creado los scripts de entrenamiento](?success=create-scripts#environment) [He tenido un problema](https://www.research.net/r/7CTJQQN?issue=create-scripts)

## <a name="create-a-python-environment"></a><a name="environment"></a> Creación de un entorno de Python

Para fines de demostración, vamos a usar un entorno de Conda (Los pasos para un entorno virtual de PIP son casi idénticos).

En el directorio oculto `.azureml`, cree un archivo denominado `pytorch-env.yml`:

:::code language="yml" source="~/MachineLearningNotebooks/tutorials/get-started-day1/IDE-users/environments/pytorch-env.yml":::

Este entorno tiene todas las dependencias necesarias para el modelo y el script de entrenamiento. Tenga en cuenta que no hay ninguna dependencia en el SDK de Azure Machine Learning para Python.

> [!div class="nextstepaction"]
> [He creado el archivo de entorno](?success=create-env-file#test-local) [He tenido un problema](https://www.research.net/r/7CTJQQN?issue=create-env-file)

## <a name="test-locally"></a><a name="test-local"></a> Prueba local

Use el código siguiente para probar las ejecuciones de scripts localmente en este entorno:

```bash
conda env create -f .azureml/pytorch-env.yml    # create conda environment
conda activate pytorch-env                      # activate conda environment
python src/train.py                             # train model
```

Después de ejecutar este script, verá los datos descargados en un directorio llamado `tutorial/data`.

> [!div class="nextstepaction"]
> [He creado el archivo de entorno](?success=test-local#create-local) [He tenido un problema](https://www.research.net/r/7CTJQQN?issue=test-local)

## <a name="create-the-control-script"></a><a name="create-local"></a> Creación del script de control

La diferencia entre el siguiente script de control y el que usó para enviar "Hola mundo" es que se agregan un par de líneas adicionales para establecer el entorno.

Cree un nuevo archivo de Python en el directorio `tutorial` denominado `04-run-pytorch.py`:

:::code language="python" source="~/MachineLearningNotebooks/tutorials/get-started-day1/IDE-users/04-run-pytorch.py":::

### <a name="understand-the-code-changes"></a>Comprensión de los cambios de código

:::row:::
   :::column span="":::
      `env = ...`
   :::column-end:::
   :::column span="2":::
      Azure Machine Learning proporciona el concepto de un [entorno](/python/api/azureml-core/azureml.core.environment.environment?preserve-view=true&view=azure-ml-py) para representar un entorno de Python reproducible y con control de versiones para ejecutar experimentos. Es fácil crear un entorno desde un entorno de PIP o Conda local.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `config.run_config.environment = env`
   :::column-end:::
   :::column span="2":::
      Agrega el entorno a [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py).
   :::column-end:::
:::row-end:::

> [!div class="nextstepaction"]
> [He creado el script de control](?success=control-script#submit) [He tenido un problema](https://www.research.net/r/7CTJQQ?issue=control-script)


## <a name="submit-the-run-to-azure-machine-learning"></a><a name="submit"></a> Envío de la ejecución a Azure Machine Learning

En caso de que haya cambiado los entornos locales, asegúrese de volver a un entorno que tenga el SDK de Azure Machine Learning para Python instalado.

A continuación, ejecute:

```bash
python 04-run-pytorch.py
```

>[!NOTE] 
> La primera vez que ejecute este script, Azure Machine Learning compilará una nueva imagen de Docker desde el entorno de PyTorch. La ejecución completa podría tardar de 5 a 10 minutos en finalizar. 
>
> Puede ver los registros de compilación de Docker en Estudio de Azure Machine Learning. Siga el vínculo a Estudio de Azure Machine Learning, seleccione la pestaña **Resultados y registros** y, después, `20_image_build_log.txt`.
>
> Esta imagen se reutilizará en ejecuciones futuras, lo que hará que se ejecuten mucho más rápido.

Una vez compilada la imagen, seleccione `70_driver_log.txt` para ver el resultado del script de entrenamiento.

```txt
Downloading https://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz to ./data/cifar-10-python.tar.gz
...
Files already downloaded and verified
epoch=1, batch= 2000: loss 2.19
epoch=1, batch= 4000: loss 1.82
epoch=1, batch= 6000: loss 1.66
epoch=1, batch= 8000: loss 1.58
epoch=1, batch=10000: loss 1.52
epoch=1, batch=12000: loss 1.47
epoch=2, batch= 2000: loss 1.39
epoch=2, batch= 4000: loss 1.38
epoch=2, batch= 6000: loss 1.37
epoch=2, batch= 8000: loss 1.33
epoch=2, batch=10000: loss 1.31
epoch=2, batch=12000: loss 1.27
Finished Training
```

> [!WARNING]
> Si ve un error `Your total snapshot size exceeds the limit`, el directorio `data` se encuentra en el valor `source_directory` que se usa en `ScriptRunConfig`.
>
> Mueva `data` fuera de `src`.

Los entornos se pueden registrar en un área de trabajo con `env.register(ws)`. Así, es posible compartirlos, volver a usarlos y crear versiones fácilmente. Los entornos facilitan la reproducción de resultados anteriores y la colaboración con el equipo.

Azure Machine Learning también mantiene una colección de entornos mantenidos. Estos entornos cubren escenarios de aprendizaje automático comunes y están respaldados por imágenes de Docker almacenadas en caché. Las imágenes de Docker almacenadas en caché hacen que la primera ejecución remota sea más rápida.

En resumen, el uso de entornos registrados le permite ahorrar tiempo. Lea [Cómo usar entornos](./how-to-use-environments.md) para más información.

> [!div class="nextstepaction"]
> [He enviado la ejecución](?success=test-w-environment#log) [He tenido un problema](https://www.research.net/r/7CTJQQ?issue=test-w-environment)

## <a name="log-training-metrics"></a><a name="log"></a> Registro de métricas de entrenamiento

Ahora que tiene un entrenamiento del modelo en Azure Machine Learning, empiece a realizar un seguimiento de algunas métricas de rendimiento.

El script de entrenamiento actual imprime las métricas en el terminal. Azure Machine Learning proporciona un mecanismo para registrar métricas con más funcionalidad. Al agregar algunas líneas de código, puede ver las métricas en Studio y comparar las métricas entre varias ejecuciones.

### <a name="modify-trainpy-to-include-logging"></a>Modificación de `train.py` para incluir el registro

Modifique el script de `train.py` para que incluya dos líneas de código más:

:::code language="python" source="~/MachineLearningNotebooks/tutorials/get-started-day1/code/pytorch-cifar10-train-with-logging/train.py":::


#### <a name="understand-the-additional-two-lines-of-code"></a>Comprensión de las dos líneas de código adicionales

En `train.py`, puede acceder al objeto de ejecución desde _dentro_ del script de entrenamiento en sí mediante el método `Run.get_context()` y usarlo para registrar las métricas:

```python
# in train.py
run = Run.get_context()

...

run.log('loss', loss)
```

Las métricas de Azure Machine Learning están:

- Organizadas por experimento y ejecución para que sea fácil realizar un seguimiento de las métricas y compararlas.
- Equipadas con una interfaz de usuario para que pueda visualizar el rendimiento del entrenamiento en Studio.
- Diseñadas para escalar, con el fin de mantener estas ventajas incluso cuando se ejecutan cientos de experimentos.

> [!div class="nextstepaction"]
> [He modificado train.py ](?success=modify-train#log) [He tenido un problema](https://www.research.net/r/7CTJQQ?issue=modify-train)

### <a name="update-the-conda-environment-file"></a>Actualización del archivo de entorno de Conda

El script de `train.py` ha tomado una dependencia nueva en `azureml.core`. Actualice `pytorch-env.yml` para reflejar este cambio:

:::code language="python" source="~/MachineLearningNotebooks/tutorials/get-started-day1/configuration/pytorch-aml-env.yml":::

> [!div class="nextstepaction"]
> [He actualizado el archivo de entorno](?success=update-environment#submit-again) [He tenido un problema](https://www.research.net/r/7CTJQQ?issue=update-environment)

### <a name="submit-the-run-to-azure-machine-learning"></a><a name="submit-again"></a> Envío de la ejecución a Azure Machine Learning
Envíe este script una vez más:

```bash
python 04-run-pytorch.py
```

Esta vez, cuando visite Estudio de Azure Machine Learning, vaya a la pestaña **Métricas**, donde ya puede ver actualizaciones directas sobre la pérdida de entrenamiento del modelo.

:::image type="content" source="media/tutorial-1st-experiment-sdk-train/logging-metrics.png" alt-text="Gráfico de pérdida de entrenamiento en la pestaña Métricas.":::

> [!div class="nextstepaction"]
> [He reenviado la ejecución](?success=resubmit-with-logging#next-steps) [He tenido un problema](https://www.research.net/r/7CTJQQ?issue=resubmit-with-logging)

## <a name="next-steps"></a>Pasos siguientes

En esta sesión, ha realizado la actualización desde un script básico de "Hola mundo" a un script de entrenamiento más realista que requirió la ejecución de un entorno de Python específico. Ha visto cómo llevar un entorno de Conda local a la nube con entornos de Azure Machine Learning. Por último, vio cómo en algunas líneas de código puede registrar métricas en Azure Machine Learning.

Hay otras maneras de crear entornos de Azure Machine Learning, como [desde un archivo requirements.txt de PIP](/python/api/azureml-core/azureml.core.environment.environment?preserve-view=true&view=azure-ml-py#from-pip-requirements-name--file-path-) o [desde un entorno de Conda local existente](/python/api/azureml-core/azureml.core.environment.environment?preserve-view=true&view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-).

En la siguiente sesión, verá cómo trabajar con datos en Azure Machine Learning mediante la carga del conjunto de datos de CIFAR10 en Azure.

> [!div class="nextstepaction"]
> [Tutorial: Traiga sus propios datos](tutorial-1st-experiment-bring-data.md)

>[!NOTE] 
> Si quiere finalizar la serie de tutoriales aquí y no avanzar al paso siguiente, recuerde [limpiar los recursos](tutorial-1st-experiment-bring-data.md#clean-up-resources).