---
title: 'Tutorial: Ejecución de scripts de Python con Data Factory'
description: Aprenda a ejecutar scripts de Python como parte de una canalización que pasa por Azure Data Factory mediante Azure Batch.
author: pkshultz
ms.devlang: python
ms.topic: tutorial
ms.date: 03/12/2021
ms.author: peshultz
ms.custom: mvc, devx-track-python
ms.openlocfilehash: 241a47ccf9021c6065fea907b4d9914744a64972
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103461698"
---
# <a name="tutorial-run-python-scripts-through-azure-data-factory-using-azure-batch"></a>Tutorial: Ejecución de scripts de Python mediante Azure Data Factory con Azure Batch

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Autenticarse en las cuentas de Batch y Storage
> * Desarrollo y ejecución de un script en Python
> * Crear un grupo de nodos de proceso para ejecutar una aplicación
> * Programación de las cargas de trabajo de Python
> * Supervisión de la canalización de análisis
> * Acceso a los archivos de registro

En el ejemplo siguiente se ejecuta un script de Python que recibe la entrada CSV de un contenedor de almacenamiento de blobs, realiza un proceso de manipulación de datos y escribe el resultado en un contenedor independiente de almacenamiento de blobs.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* Una distribución de [Python](https://www.python.org/downloads/) instalada, para las pruebas locales.
* El paquete [azure-storage-blob](https://pypi.org/project/azure-storage-blob/) `pip`.
* El [conjunto de datos iris.csv](https://github.com/Azure-Samples/batch-adf-pipeline-tutorial/blob/master/iris.csv).
* Una cuenta de Azure Batch y una cuenta de Azure Storage vinculada. Consulte [Creación de una cuenta de Batch](quick-create-portal.md#create-a-batch-account) para más información sobre cómo crear y vincular cuentas de Batch para almacenar cuentas.
* Una cuenta de Azure Data Factory. Consulte [Creación de una factoría de datos](../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory) para más información sobre cómo crear una factoría de datos con Azure Portal.
* [Batch Explorer](https://azure.github.io/BatchExplorer/).
* [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

[!INCLUDE [batch-common-credentials](../../includes/batch-common-credentials.md)]

## <a name="create-a-batch-pool-using-batch-explorer"></a>Creación de un grupo de Batch con Batch Explorer

En esta sección, se va a utilizar Batch Explorer para crear el grupo de Batch que la canalización de la factoría de Azure Data usará. 

1. Inicie sesión en Batch Explorer con sus credenciales de Azure.
1. Selección de la cuenta de Batch
1. Para crear un grupo, seleccione **Grupos** en la barra lateral izquierda y, después, el botón **Agregar** situado sobre el formulario de búsqueda. 
    1. Elija un identificador y el nombre para mostrar. Usaremos `custom-activity-pool` para este ejemplo.
    1. Establezca el tipo de escala en **Tamaño fijo** y establezca el número de nodos dedicados en 2.
    1. En **Ciencia de datos**, seleccione **Dsvm Windows** como sistema operativo.
    1. Elija `Standard_f2s_v2` como tamaño de la máquina virtual.
    1. Habilite la tarea de inicio y agregue el comando `cmd /c "pip install azure-storage-blob pandas"`. La identidad del usuario puede permanecer como la del **usuario del grupo** predeterminado.
    1. Seleccione **Aceptar**.

## <a name="create-blob-containers"></a>Creación de contenedores de blobs

Aquí podrá crear contenedores de blobs que almacenarán la entrada y salida de archivos para el trabajo de Batch OCR.

1. Inicie sesión en Explorador de Storage con sus credenciales de Azure.
1. Con la cuenta de almacenamiento vinculada a su cuenta de Batch, cree dos contenedores de blobs (uno para los archivos de entrada y otro para los de salida); para ello, siga los pasos descritos en [Creación de un contenedor de blobs](../vs-azure-tools-storage-explorer-blobs.md#create-a-blob-container).
    * En este ejemplo, llamaremos a nuestro contenedor de entrada `input` y al contenedor de salida, `output`.
1. Cargue [`iris.csv`](https://github.com/Azure-Samples/batch-adf-pipeline-tutorial/blob/master/iris.csv)en el contenedor de entrada `input` con el Explorador de Storage; para ello, siga los pasos que se indican en [Administración de blobs de un contenedor de blobs](../vs-azure-tools-storage-explorer-blobs.md#managing-blobs-in-a-blob-container).

## <a name="develop-a-script-in-python"></a>Desarrollo de un script en Python

El siguiente script de Python carga el conjunto de datos `iris.csv` desde el contenedor `input`, realiza un proceso de manipulación de datos y guarda los resultados de nuevo en el contenedor `output`.

``` python
# Load libraries
from azure.storage.blob import BlobClient
import pandas as pd

# Define parameters
connectionString = "<storage-account-connection-string>"
containerName = "output"
outputBlobName  = "iris_setosa.csv"

# Establish connection with the blob storage account
blob = BlobClient.from_connection_string(conn_str=connectionString, container_name=containerName, blob_name=outputBlobName)

# Load iris dataset from the task node
df = pd.read_csv("iris.csv")

# Take a subset of the records
df = df[df['Species'] == "setosa"]

# Save the subset of the iris dataframe locally in task node
df.to_csv(outputBlobName, index = False)

with open(outputBlobName, "rb") as data:
    blob.upload_blob(data)
```

Guarde el script como `main.py` y cárguelo en el contenedor de **Azure Storage** `input`. Asegúrese de probar y asegurar su funcionalidad localmente antes de cargarlo en el contenedor de blobs:

``` bash
python main.py
```

## <a name="set-up-an-azure-data-factory-pipeline"></a>Configuración de una canalización de Azure Data Factory

En esta sección, creará una canalización con el script de Python y, posteriormente, la validará.

1. Siga los pasos para crear una factoría de datos en la sección "Creación de una factoría de datos" de [este artículo](../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory).
1. En el cuadro **Factory Resources** (Recursos de fábrica), seleccione el botón + (más) y, después, seleccione **Pipeline** (Canalización).
1. En la pestaña **General**, establezca el nombre de la canalización como "Ejecutar Python".

    ![En la pestaña General, establezca el nombre de la canalización como "Ejecutar Python".](./media/run-python-batch-azure-data-factory/create-pipeline.png)

1. En el cuadro **Activities** (Actividades), expanda **Batch Service** (Servicio Batch). Arrastre la actividad personalizada desde el cuadro de herramientas **Activities** (Actividades) a la superficie del diseñador de canalizaciones. Rellene las siguientes pestañas para la actividad personalizada:
    1. En la pestaña **General**, especifique **testPipeline** en Nombre. ![En la pestaña general, especifique testPipeline en Nombre.](./media/run-python-batch-azure-data-factory/create-custom-task.png)
    1. En la pestaña **Azure Batch**, agregue la **Cuenta de Batch** que se creó en los pasos anteriores y **pruebe la conexión** para asegurarse de que es correcta.
    ![En la pestaña Azure Batch, agregue la cuenta de Batch que se creó en los pasos anteriores y pruebe la conexión.](./media/run-python-batch-azure-data-factory/integrate-pipeline-with-azure-batch.png)
    1. En la pestaña **Configuración**:
        1. Establezca el **comando** como `python main.py`.
        1. Como **Resource Linked Service** (Servicio vinculado de recursos), agregue la cuenta de almacenamiento que se creó en los pasos anteriores. Pruebe la conexión para asegurarse de que se ha realizado correctamente.
        1. En **Folder Path** (Ruta de acceso a la carpeta), seleccione el nombre del contenedor **Azure Blob Storage**  que contiene el script de Python y las entradas asociadas. De esta forma, descargará los archivos seleccionados del contenedor en las instancias de nodo de grupo antes de la ejecución del script de Python.

        ![En la ruta de acceso de la carpeta, seleccione el nombre del contenedor de Azure Blob Storage.](./media/run-python-batch-azure-data-factory/create-custom-task-py-script-command.png)

1. Haga clic en **Validar** en la barra de herramientas de la canalización situada en la parte superior del lienzo para validar la configuración de la canalización. Confirme que la canalización se ha validado correctamente. Para cerrar la salida de la validación, seleccione el botón &gt;&gt; (fecha derecha).
1. Haga clic en **Debug** (Depurar) para probar la canalización y asegurarse de que funciona correctamente.
1. Haga clic en **Publicar** para publicar la canalización.
1. Haga clic en **Desencadenador** para ejecutar el script de Python como parte de un proceso por lotes.

    ![Haga clic en Trigger (Desencadenador) para ejecutar el script de Python como parte de un proceso por lotes.](./media/run-python-batch-azure-data-factory/create-custom-task-py-success-run.png)

### <a name="monitor-the-log-files"></a>Supervise los archivos de registro

En caso de que se produzcan advertencias o errores mediante la ejecución del script, puede extraer del repositorio `stdout.txt` o `stderr.txt` para obtener más información de la salida que se registró.

1. Seleccione **Jobs** (Trabajos) en el lado izquierdo de Batch Explorer.
1. Elija el trabajo creado por la factoría de datos. Suponiendo que ha llamado al grupo `custom-activity-pool`, seleccione `adfv2-custom-activity-pool`.
1. Haga clic en la tarea que tenía un código de salida de error.
1. Vea `stdout.txt` y `stderr.txt` para investigar y diagnosticar el problema.

## <a name="clean-up-resources"></a>Limpieza de recursos

Aunque no se cobran los trabajos y las tareas, sí se cobran los nodos de proceso. Por consiguiente, se recomienda asignar solo los grupos necesarios. Al eliminar el grupo, las salidas de tarea de los nodos también se eliminan. Sin embargo, los archivos de entrada y salida permanecen en la cuenta de Storage. Cuando ya no las necesite, también puede eliminar la cuenta de Batch y la cuenta de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Autenticarse en las cuentas de Batch y Storage
> * Desarrollo y ejecución de un script en Python
> * Crear un grupo de nodos de proceso para ejecutar una aplicación
> * Programación de las cargas de trabajo de Python
> * Supervisión de la canalización de análisis
> * Acceso a los archivos de registro

Para más información acerca de Azure Data Factory, consulte:

> [!div class="nextstepaction"]
> [Introducción a Azure Data Factory](../data-factory/introduction.md)
