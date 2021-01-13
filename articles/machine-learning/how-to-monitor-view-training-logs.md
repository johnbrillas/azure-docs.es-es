---
title: Supervisión y visualización de métricas y registros de ejecución de ML
titleSuffix: Azure Machine Learning
description: Supervise los experimentos de ML y consulte las métricas de ejecución con widgets de Jupyter y con Estudio de Azure Machine Learning.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 07/30/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: ea96e1056e6157cfddbdc2f0b6451ed55a74d1de
ms.sourcegitcommit: 90caa05809d85382c5a50a6804b9a4d8b39ee31e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97756065"
---
# <a name="monitor-and-view-ml-run-logs-and-metrics"></a>Supervisión y visualización de métricas y registros de ejecución de ML

Aprenda a supervisar las ejecuciones de Azure Machine Learning y a ver sus registros. 

Al ejecutar un experimento, los registros y las métricas se transmiten automáticamente.  Además, puede agregar los suyos propios.  Para averiguar cómo hacerlo, consulte [Habilitación del registro en ejecuciones de entrenamiento de Azure ML](how-to-track-experiments.md).

Los registros pueden ayudarle a diagnosticar errores y advertencias para la ejecución. Las métricas de rendimiento como los parámetros y la precisión del modelo ayudan a realizar el seguimiento y la supervisión de las ejecuciones.

En este artículo, aprenderá a ver los registros con los métodos siguientes:

> [!div class="checklist"]
> * Supervisar las ejecuciones de Studio
> * Supervisar las ejecuciones con el widget de Jupyter Notebook
> * Supervisar las ejecuciones automatizadas de aprendizaje automático
> * Ver registros de salida al finalizar
> * Ver registros de salida en Studio

Para obtener información general sobre cómo administrar los experimentos, consulte [Inicio, supervisión y cancelación de las ejecuciones de entrenamiento](how-to-manage-runs.md).

## <a name="monitor-runs-using-the-jupyter-notebook-widget"></a>Supervisar las ejecuciones con el widget de Jupyter Notebooks

Cuando se usa el método **ScriptRunConfig** para enviar ejecuciones, se puede ver el progreso de la ejecución mediante el [widget de Jupyter](/python/api/azureml-widgets/azureml.widgets?preserve-view=true&view=azure-ml-py). Al igual que el envío de ejecución, el widget es asincrónico y proporciona las actualizaciones directas cada 10 a 15 segundos hasta que se completa el trabajo.

Vea el widget de Jupyter mientras espera a que finalice la ejecución.
    
```python
from azureml.widgets import RunDetails
RunDetails(run).show()
```

![Captura de pantalla del widget de cuaderno de Jupyter](./media/how-to-track-experiments/run-details-widget.png)

También puede obtener un vínculo a la misma pantalla en el área de trabajo.

```python
print(run.get_portal_url())
```

## <a name="monitor-automated-machine-learning-runs"></a>Supervisar las ejecuciones automatizadas de aprendizaje automático

En el caso de las ejecuciones automatizadas de aprendizaje automático, para acceder a los gráficos de una ejecución anterior, sustituya `<<experiment_name>>` con el nombre del experimento correspondiente.

```python
from azureml.widgets import RunDetails
from azureml.core.run import Run

experiment = Experiment (workspace, <<experiment_name>>)
run_id = 'autoML_my_runID' #replace with run_ID
run = Run(experiment, run_id)
RunDetails(run).show()
```

![Widget de cuaderno de Jupyter Notebooks para aprendizaje automático automatizado](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)

## <a name="show-output-upon-completion"></a>Visualización de la salida al finalizar

Cuando se usa **ScriptRunConfig**, se puede usar ```run.wait_for_completion(show_output = True)``` para mostrar cuándo finaliza el entrenamiento del modelo. La marca ```show_output``` le ofrece una salida detallada. Para obtener más información, consulte la sección ScriptRunConfig en [Cómo habilitar el registro](how-to-track-experiments.md#scriptrun-logs).

<a id="queryrunmetrics"></a>

## <a name="query-run-metrics"></a>Métricas de ejecución de consulta

Puede ver las métricas de un modelo entrenado con ```run.get_metrics()```. Podría usar esto con el ejemplo anterior para determinar el mejor modelo buscando el modelo con el menor valor de error cuadrático medio (MSE).

<a name="view-the-experiment-in-the-web-portal"></a>

## <a name="view-run-records-in-the-studio"></a>Visualización de los registros de ejecución en Studio

Puede examinar los registros de ejecución completados, incluidas las métricas registradas, en [Azure Machine Learning Studio](https://ml.azure.com).

Vaya a la sección **Experimentos**. Para ver todas las ejecuciones en el área de trabajo de todos los experimentos, seleccione la pestaña **Todas las ejecuciones**. Puede explorar en profundidad las ejecuciones de Experimentos concretos si aplica el filtro de experimentos en la barra de menús de la parte superior.

Para obtener la vista de experimentos individuales, seleccione la pestaña **Todos los experimentos**. En el panel de ejecución del experimento, puede ver las métricas y los registros del seguimiento de cada ejecución. 

También puede editar la tabla de la lista de ejecución para seleccionar varias ejecuciones y mostrar el último valor registrado, el mínimo o el máximo para las ejecuciones. Personalice los gráficos para comparar los valores de las métricas registradas y los agregados en varias ejecuciones. 

![Detalles de la ejecución en Azure Machine Learning Studio](media/how-to-track-experiments/experimentation-tab.gif)

### <a name="format-charts"></a>Formato de los gráficos 

Use los métodos siguientes en las API de registro para influir en las visualizaciones de las métricas.

|Valor registrado|Ejemplo de código| Formato en el portal|
|----|----|----|
|Registrar una matriz de valores numéricos| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Gráfico de líneas de una sola variable|
|Registrar un único valor numérico con el mismo nombre de métrica usado repetidamente (como desde dentro de un bucle)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Gráfico de líneas de una sola variable|
|Registrar una fila con dos columnas numéricas repetidamente|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|Gráfico de líneas de dos variables|
|Registrar tabla con dos columnas numéricas|`run.log_table(name='Sine Wave', value=sines)`|Gráfico de líneas de dos variables|


### <a name="view-log-files-for-a-run"></a>Visualización de los archivos de registro de una ejecución 

Los archivos de registro son un recurso esencial para depurar las cargas de trabajo de Azure Machine Learning. Explore en profundidad hasta llegar a una ejecución específica para ver sus registros y salidas:  

1. Vaya a la sección **Experimentos**.
1. Seleccione el identificador de la ejecución para ver una ejecución específica.
1. Seleccione **Resultados y registros** en la parte superior de la página.

:::image type="content" source="media/how-to-monitor-view-training-logs/view-logs.png" alt-text="Captura de pantalla de la sección Resultados y registros de una ejecución":::

En las tablas siguientes se muestra el contenido de los archivos de registro de las carpetas que verá en esta sección.

> [!NOTE]
> No verá necesariamente todos los archivos de cada ejecución. Por ejemplo, el archivo 20_image_build_log*.txt solo aparece cuando se compila una nueva imagen (por ejemplo, cuando se cambia el entorno).

#### <a name="azureml-logs-folder"></a>`azureml-logs`

|Archivo  |Descripción  |
|---------|---------|
|20_image_build_log.txt     | Registro de compilación de imagen de Docker para el entorno de entrenamiento, opcional, uno por cada ejecución. Solo se aplica cuando se actualiza el entorno. De lo contrario, Azure Machine Learning volverá a usar la imagen almacenada en caché. Si es correcto, contiene los detalles del registro de la imagen correspondiente.         |
|55_azureml-execution-<node_id>.txt     | Registro de stdout/stderr de la herramienta de host, uno por nodo. Extrae la imagen en el destino de proceso. Tenga en cuenta que este registro solo aparece una vez que se han protegido los recursos de proceso.         |
|65_job_prep-<node_id>.txt     |   Registro de stdout/stderr del script de preparación del trabajo, uno por nodo. Descargue el código en el destino de proceso y los almacenes de datos (si se solicita).       |
|70_driver_log(_x).txt      |  Registro de stdout/stderr del script de control de Azure Machine Learning y el script de entrenamiento del cliente, uno por proceso. **Esta es la salida estándar del script. Aquí es donde se muestran los registros del código (por ejemplo, las instrucciones PRINT).** En la mayoría de los casos, supervisará los registros aquí.       |
|70_mpi_log.txt     |   Registro de la plataforma DE MPI, opcional, uno por cada ejecución. Solo para la ejecución de MPI.   |
|75_job_post-<node_id>.txt     |  Registro de stdout/stderr del script de liberación del trabajo, uno por nodo. Envíe registros y vuelva a liberar los recursos de proceso en Azure.        |
|process_info.json      |   Muestra los procesos que se están ejecutando en cada nodo.  |
|process_status.json      | Muestra el estado del proceso, es decir, si un proceso está no iniciado, en ejecución o completo.         |

#### <a name="logs--azureml-folder"></a>`logs > azureml`

|Archivo  |Descripción  |
|---------|---------|
|110_azureml.log      |         |
|job_prep_azureml.log     |   Registro del sistema para la preparación del trabajo        |
|job_release_azureml.log     | Registro del sistema para la liberación del trabajo        |

#### <a name="logs--azureml--sidecar--node_id-folder"></a>`logs > azureml > sidecar > node_id`

Cuando se habilita sidecar, los scripts de preparación y liberación de trabajos se ejecutarán dentro del contenedor sidecar.  Hay una carpeta para cada nodo. 

|Archivo  |Descripción  |
|---------|---------|
|start_cms.txt     |  Registro del proceso que se inicia cuando se inicia el contenedor sidecar       |
|prep_cmd.txt      |   Registro de ContextManagers especificado al ejecutar `job_prep.py` (algunos de estos se transmitirán a `azureml-logs/65-job_prep`)       |
|release_cmd.txt     |  Registro de ComtextManagers de los que se ha salido al ejecutar `job_release.py`        |

#### <a name="other-folders"></a>Otras carpetas

Para el entrenamiento de trabajos en clústeres con varios procesos, los registros están presentes para cada IP de nodo. La estructura de cada nodo es igual que la de los trabajos de un solo nodo. Hay una carpeta de registros adicional para los registros de ejecución general, stderr y stdout.

Azure Machine Learning registra información de varios orígenes durante el entrenamiento, como AutoML o el contenedor de Docker que ejecuta el trabajo de entrenamiento. Muchos de estos registros no están documentados. Si encuentra problemas y se pone en contacto con el Soporte técnico de Microsoft, es posible que puedan usar estos registros durante la resolución de problemas.

## <a name="monitor-a-compute-cluster"></a>Supervisión de un clúster de proceso

Para supervisar las ejecuciones de un destino de proceso específico desde el explorador, siga estos pasos:

1. En [Azure Machine Learning Studio](https://ml.azure.com/), seleccione el área de trabajo y, a continuación, seleccione __Proceso__ en el lado izquierdo de la página.

1. Seleccione __Training Clusters__ (Clústeres de entrenamiento) para mostrar una lista de los destinos de proceso usados para el entrenamiento. Después, seleccione el clúster.

    ![Selección del clúster de entrenamiento](./media/how-to-track-experiments/select-training-compute.png)

1. Seleccione __Runs__ (Ejecuciones). Se muestra la lista de ejecuciones que usan este clúster. Para ver los detalles de una ejecución específica, use el vínculo de la columna __Run__ (Ejecución). Para ver los detalles del experimento, use el vínculo de la columna __Experiment__ (Experimento).

    ![Selección de ejecuciones para el clúster de entrenamiento](./media/how-to-track-experiments/show-runs-for-compute.png)
    
    > [!TIP]
    > Dado que los destinos de proceso de entrenamiento son un recurso compartido, pueden tener varias ejecuciones en cola o activas en un momento dado.
    > 
    > Una ejecución puede contener ejecuciones secundarias, por lo que un trabajo de entrenamiento puede dar lugar a varias entradas.

Una vez finalizada la ejecución, ya no se muestra en esta página. Para ver información sobre las ejecuciones completadas, visite la sección de __experimentos__ de Studio, seleccione el experimento y ejecútelo. Para obtener más información, consulte la sección [Visualización de métricas para las ejecuciones completadas](#view-the-experiment-in-the-web-portal).


## <a name="next-steps"></a>Pasos siguientes

Pruebe estos pasos para aprender a usar Azure Machine Learning:

* Obtenga información sobre cómo [realizar un seguimiento de los experimentos y habilitar los registros en el diseñador de Azure Machine Learning](how-to-track-designer-experiments.md).

* Vea un ejemplo de cómo registrar el mejor modelo e implementarlo en el tutorial, [Entrenar un modelo de clasificación de imágenes con Azure Machine Learning](tutorial-train-models-with-aml.md).
