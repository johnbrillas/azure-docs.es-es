---
title: Introducción al desarrollo avanzado de Azure Percept localmente
description: Introducción al desarrollo del aprendizaje automático local mediante VS Code y cuadernos de Jupyter Notebook en AzureML
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: tutorial
ms.date: 02/10/2021
ms.custom: template-tutorial
ms.openlocfilehash: 88e71ac21177a13d30176212e97442c63272eca6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658970"
---
# <a name="getting-started-with-machine-learning-development-using-vs-code--jupyter-notebooks-on-azureml"></a>Introducción al desarrollo del aprendizaje automático mediante VS Code y cuadernos de Jupyter Notebook en AzureML

Este artículo le guiará a través del proceso de configuración de un área de trabajo de Azure Machine Learning, la creación de una instancia de proceso, la configuración de un entorno de desarrollo de Visual Studio Code en la máquina local y la ejecución de las celdas de un cuaderno preconfigurado de Jupyter Notebook de ejemplo en VS Code.

El [cuaderno](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) realiza el aprendizaje por transferencia mediante un modelo de TensorFlow preentrenado en AzureML (MobileNetSSDV2Lite) en Python con un conjunto de datos personalizado para detectar cuencos.

El cuaderno muestra cómo empezar a partir del [conjunto de datos COCO](https://cocodataset.org/#home), filtrarlo para que aparezca solo la clase de interés (cuencos) y, a continuación, convertirlo al formato adecuado. Como alternativa, puede usar la herramienta de etiquetado [VoTT 2](https://github.com/microsoft/VoTT) de código abierto para crear y etiquetar los rectángulos de selección en el formato Pascal VOC.

Después de volver a entrenar el modelo en el conjunto de datos personalizado, el modelo se puede implementar en Azure Percept DK mediante el método de actualización de módulos gemelos. Después, puede comprobar la inferencia de modelos mediante la visualización de la secuencia RTSP en directo desde el módulo de sistema Azure Percept Vision. Tanto el reentrenamiento como la implementación del modelo se realizan dentro del cuaderno a través de la instancia de proceso remota.

## <a name="prerequisites"></a>Requisitos previos

- [Suscripción de Azure Machine Learning](https://azure.microsoft.com/free/services/machine-learning/)
- [Azure Percept DK con el módulo de sistema Azure Percept Vision conectado](./overview-azure-percept-dk.md)
- Haber completado la [experiencia de incorporación de Azure Percept DK](./quickstart-percept-dk-set-up.md)

## <a name="download-azure-percept-github-repository"></a>Descarga del repositorio de GitHub de Azure Percept

1. Vaya al [repositorio de GitHub de Azure Percept DK](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview).
1. Clone el repositorio o descargue el archivo ZIP. Cerca de la parte superior de la pantalla, haga clic en **Código** -> **Descargar archivo ZIP**.

    :::image type="content" source="./media/advanced-development-local/download-zip.png" alt-text="Pantalla de descarga de GitHub.":::

## <a name="create-an-azure-machine-learning-workspace-and-remote-compute-instance"></a>Creación de un área de trabajo y una instancia de proceso remota de Azure Machine Learning

1. Vaya al [portal de Azure Machine Learning](https://ml.azure.com).
1. Seleccione el directorio, la suscripción de Azure y el área de trabajo de Machine Learning en los menús desplegables y haga clic en **Introducción**.

    :::image type="content" source="./media/advanced-development-local/machine-learning-portal-get-started.png" alt-text="Pantalla de introducción de Azure Machine Learning.":::

    Si aún no tiene un área de trabajo de Azure Machine Learning, haga clic en **Crear un área de trabajo nueva**. En la nueva pestaña del explorador, haga lo siguiente:

    1. Seleccione la suscripción de Azure.
    1. Seleccione el grupo de recursos que necesite.
    1. Escriba un nombre del área de trabajo.
    1. Seleccione la región.
    1. Seleccione la edición del área de trabajo.
    1. Haga clic en **Revisar y crear**.
    1. En la página siguiente, revise sus selecciones y haga clic en **Crear**.

        :::image type="content" source="./media/advanced-development-local/workspace-review-and-create.png" alt-text="Pantalla de creación del área de trabajo en Azure Machine Learning.":::

    Espere unos minutos para la creación del área de trabajo. Una vez completada la creación del área de trabajo, verá marcas de verificación verdes junto a los recursos y el mensaje **Se completó la implementación** en la parte superior de la página de información general de Machine Learning Services.

    :::image type="content" source="./media/advanced-development-local/workspace-creation-complete-inline.png" alt-text="Confirmación de creación del área de trabajo." lightbox= "./media/advanced-development-local/workspace-creation-complete.png":::

    Una vez finalizada la creación del área de trabajo, vuelva a la pestaña del portal de Machine Learning y haga clic en **Empezar**.

1. En la página principal del área de trabajo de Machine Learning, haga clic en **Proceso** en el panel izquierdo.

1. Si no hay ninguna instancia de proceso existente, haga clic en **Nuevo** para crear un nuevo proceso de CPU o GPU. En la ventana **New compute instance** (Nueva instancia de proceso), escriba un **nombre de proceso**, elija un **tipo de máquina virtual** y seleccione un **tamaño de máquina virtual**. Haga clic en **Crear**.

    :::image type="content" source="./media/advanced-development-local/new-compute-instance.png" alt-text="Pantalla de creación de nueva instancia de proceso.":::

    Una vez que haga clic en **Crear**, se tardará unos minutos en crear la instancia de proceso. El estado de **Proceso** mostrará un círculo verde y **\<your compute name> - En ejecución** una vez completada la creación del proceso. Esta instancia de proceso ejecuta el servidor de Jupyter y se aprovechará en este tutorial.

## <a name="visual-studio-code-development-environment-setup"></a>Configuración del entorno de desarrollo de Visual Studio Code

1. Instale las herramientas necesarias.

    1. Opción 1:

        Use el [instalador del paquete de herramientas de desarrollo](./dev-tools-installer.md) para instalar los siguientes paquetes:

        - Visual Studio Code
        - Python 3.5, 3.6 o 3.7
        - Miniconda3
        - Intel OpenVino Toolkit 2020.2

        Nota: Intel OpenVino Toolkit aparece como un paquete opcional en el instalador del paquete de herramientas de desarrollo, pero es necesario para trabajar con el módulo de sistema Azure Percept Vision del kit de desarrollo Azure Percept DK.

    1. Opción 2:

        Si prefiere no usar el instalador del paquete de herramientas de desarrollo, instale manualmente los siguientes paquetes; para ello, haga clic en los vínculos siguientes y siga las instrucciones de descarga e instalación especificadas:

        - [Visual Studio Code](https://code.visualstudio.com/)
        - [Python 3.5, 3.6 o 3.7](https://www.python.org/)
        - [Miniconda3](https://docs.conda.io/en/latest/miniconda.html)
        - [Intel OpenVino Toolkit 2020.2](https://docs.openvinotoolkit.org/)

    Nota: Si ya tiene instalada la distribución de Anaconda completa, no es necesario instalar Miniconda. Como alternativa, si prefiere no usar Anaconda ni Miniconda, puede crear un entorno virtual de Python e instalar los paquetes necesarios para ejecutar el desarrollo del modelo de inteligencia artificial mediante PIP.

1. Inicie Visual Studio Code.
1. Configure un entorno de ciencia de datos:

    - Opción 1: conéctese a una instancia de proceso remota existente o nueva de Machine Learning que ya tenga los paquetes de Machine Learning mantenidos. **Esta es la opción que se va a usar en este tutorial**.

    - Opción 2: configure un entorno de Conda en una máquina local.
        1. Abra un símbolo del sistema de Anaconda o Miniconda y ejecute el siguiente comando para crear un entorno denominado **myenv** con los paquetes especificados:

            `conda create -n myenv python=3.7 pandas jupyter seaborn scikit-learn keras tensorflow=1.15`

            Para obtener información adicional sobre la creación y administración de entornos de Anaconda, consulte la [documentación de Anaconda](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

1. Cree un área de trabajo de VS Code:

    1. Cree una carpeta en una ubicación adecuada para que sirva como área de trabajo de Visual Studio Code. Llámela **myworkspace**.
    1. Abra **myworkspace** en Visual Studio Code haciendo clic en **Archivo** > **Abrir carpeta** > **Seleccionar carpeta**.
    1. En el explorador de archivos, vaya y seleccione el [archivo Transferlearningusing_SSDLiteV2 Model.ipynbb](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) de la copia local del repositorio de GitHub de Azure Percept DK. Copie este archivo de cuaderno en la carpeta myworkspace.

## <a name="azure-integration-with-visual-studio-code"></a>Integración de Azure con Visual Studio Code

1. Si no lo está, regístrese en la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

1. Instale las siguientes extensiones de Azure para VS Code:
    - [Extensión de Azure Machine Learning](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).
    - Extensión de Python Insiders. En VS Code, vaya a **Ver** -> **Paleta de comandos**. En la paleta de comandos, escriba y seleccione **Python: Switch to Insiders Daily Channel**.
    - [Extensión de la cuenta de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account). Vuelva a cargar la ventana en VS Code cuando se le solicite.
    - [Extensión de Azure IoT Hub Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).
    - [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

1. Inicie sesión en su cuenta de Azure en Visual Studio Code para aprovisionar recursos y ejecutar cargas de trabajo en Azure.
    1. En Visual Studio Code, abra la paleta de comandos; para ello, seleccione **Ver** > **Paleta de comandos** en la barra de menús.
    1. Escriba **Azure: Sign-In** en la paleta de comandos para empezar el proceso de inicio de sesión.

        :::image type="content" source="./media/advanced-development-local/transfer-learning-azure-sign-in-inline.png" alt-text="Pantalla de inicio de sesión de Azure." lightbox= "./media/advanced-development-local/transfer-learning-azure-sign-in.png":::

    1. Active la extensión de Python y la cuenta de Azure haciendo clic en el icono de Azure en el lado izquierdo de VS Code.

        :::image type="content" source="./media/advanced-development-local/azure-icon.png" alt-text="Icono de Azure en VS Code.":::

    1. Abra el cuaderno Transferlearningusing_SSDLiteV2 Notebook Model_VSCode.ipynb en la carpeta **myworkspace**.
    1. Abra la paleta de comandos. Escriba y seleccione **Python: specify local or remote Jupyter server for connections**.
    1. Debería aparecer una lista de las opciones de conexión entre las que puede elegir. Seleccione **Instancias de proceso de Azure ML**.

        :::image type="content" source="./media/advanced-development-local/azure-machine-learning-compute-instances.png" alt-text="Opciones de instancias de proceso de Azure Machine Learning en VS Code.":::

    1. Siga las indicaciones guiadas para elegir una suscripción, un área de trabajo y una instancia de proceso remota. Seleccione el área de trabajo y la instancia de proceso remota creada anteriormente en este tutorial. También tiene la opción de crear una nueva instancia de proceso.
    1. Después de seleccionar una instancia de proceso, se le pedirá que vuelva a cargar la ventana de VS Code. Una vez que la vuelva a cargar, seleccione el kernel **Python 3.6 - AzureML**. Ahora puede ejecutar cualquiera de las celdas del cuaderno. La ejecución de una celda del cuaderno creará una instancia de la conexión entre el cuaderno y la instancia de proceso. La barra de herramientas del cuaderno se actualizará para reflejar la instancia de proceso en la que está trabajando.

        :::image type="content" source="./media/advanced-development-local/kernel-inline.png" alt-text="Selección de kernel en VS Code." lightbox= "./media/advanced-development-local/kernel.png":::

## <a name="working-with-the-notebook"></a>Uso del cuaderno

Ya está listo para ejecutar el cuaderno para entrenar su detector personalizado de cuencos en VS Code e implementarlo en su kit de desarrollo. Asegúrese de que ejecuta cada celda del cuaderno individualmente, ya que algunas de las celdas requieren parámetros de entrada antes de ejecutar el script. Las celdas que requieren parámetros de entrada se pueden editar directamente en el cuaderno. Para ejecutar una celda, haga clic en el icono de reproducción en el lado izquierdo de la celda:

:::image type="content" source="./media/advanced-development-local/run-cell-1.png" alt-text="Icono de celda resaltado del cuaderno.":::

## <a name="next-steps"></a>Pasos siguientes

Para obtener más cuadernos de ejemplo de Azure Machine Learning Service, visite este [repositorio](https://github.com/Azure/MachineLearningNotebooks/tree/2aa7c53b0ce84e67565d77e484987714fdaed36e/how-to-use-azureml).
