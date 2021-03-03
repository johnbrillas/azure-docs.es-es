---
title: Introducción al desarrollo avanzado de Azure Percept en la nube
description: Introducción al desarrollo avanzado en la nube a través de cuadernos de Jupyter Notebook y Azure Machine Learning
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: tutorial
ms.date: 02/10/2021
ms.custom: template-tutorial
ms.openlocfilehash: 408040ea68273a29ed9be87b60bd0cc6da736a05
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658902"
---
# <a name="getting-started-with-advanced-development-in-the-cloud-via-jupyter-notebooks-and-azure-machine-learning"></a>Introducción al desarrollo avanzado en la nube a través de cuadernos de Jupyter Notebook y Azure Machine Learning

Este artículo le guiará a través del proceso de configuración de un área de trabajo de Azure Machine Learning, la carga de un cuaderno de ejemplo preconfigurado de Jupyter Notebook en el área de trabajo, la creación de una instancia de proceso y la ejecución de las celdas del cuaderno en el área de trabajo.

El [cuaderno](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) realiza el aprendizaje por transferencia mediante un modelo de TensorFlow preentrenado en AzureML (MobileNetSSDV2Lite) en Python con un conjunto de datos personalizado para detectar cuencos.

El cuaderno muestra cómo empezar a partir de [COCO](https://cocodataset.org/#home), filtrar el conjunto de datos para que aparezca solo la clase de interés (cuencos) y, a continuación, convertirlo al formato adecuado. Como alternativa, puede usar la herramienta de etiquetado [VoTT 2](https://github.com/microsoft/VoTT) de código abierto para crear y etiquetar los rectángulos de selección en el formato Pascal VOC.

Después de volver a entrenar el modelo con el conjunto de datos personalizado, el modelo se puede implementar en Azure Percept DK con el método de actualización de módulos gemelos.

Después, puede comprobar la inferencia de modelos mediante la visualización de la secuencia RTSP en directo desde el módulo de sistema Azure Percept Vision. El reentrenamiento y la implementación de los modelos se realiza dentro del cuaderno en la nube.

## <a name="prerequisites"></a>Requisitos previos

- [Suscripción de Azure Machine Learning](https://azure.microsoft.com/free/services/machine-learning/)
- [Azure Percept DK con el módulo de sistema Azure Percept Vision conectado](./overview-azure-percept-dk.md)
- Haber completado la [experiencia de incorporación de Azure Percept DK](./quickstart-percept-dk-set-up.md)

## <a name="download-azure-percept-github-repository"></a>Descarga del repositorio de GitHub de Azure Percept

1. Vaya al [repositorio de GitHub de Azure Percept](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview).

1. Clone el repositorio o descargue el archivo ZIP. Cerca de la parte superior de la pantalla, haga clic en **Código** -> **Descargar archivo ZIP**.

    :::image type="content" source="./media/advanced-development-cloud/download-zip.png" alt-text="Pantalla de descarga de GitHub.":::

## <a name="set-up-azure-machine-learning-portal-and-notebook"></a>Configuración del portal y el cuaderno de Azure Machine Learning

1. Vaya al [portal de Azure Machine Learning](https://ml.azure.com).

1. Seleccione el directorio, la suscripción de Azure y el área de trabajo de Machine Learning en los menús desplegables y haga clic en **Introducción**.

    :::image type="content" source="./media/advanced-development-cloud/machine-learning-portal-get-started.png" alt-text="Pantalla de introducción de Azure Machine Learning.":::

    Si aún no tiene un área de trabajo de Azure Machine Learning, haga clic en **Crear un área de trabajo nueva**. En la nueva pestaña del explorador, haga lo siguiente:

    1. Seleccione la suscripción de Azure.
    1. Seleccione el grupo de recursos que necesite.
    1. Escriba un nombre del área de trabajo.
    1. Seleccione la región.
    1. Seleccione la edición del área de trabajo.
    1. Haga clic en **Revisar y crear**.
    1. En la página siguiente, revise sus selecciones y haga clic en **Crear**.

        :::image type="content" source="./media/advanced-development-cloud/workspace-review-and-create.png" alt-text="Pantalla de creación del área de trabajo en Azure Machine Learning.":::

    Espere unos minutos para la creación del área de trabajo. Una vez completada la creación del área de trabajo, verá marcas de verificación verdes junto a los recursos y el mensaje **Se completó la implementación** en la parte superior de la página de información general de Machine Learning Services.

    :::image type="content" source="./media/advanced-development-cloud/workspace-creation-complete-inline.png" alt-text="Confirmación de creación del área de trabajo." lightbox= "./media/advanced-development-cloud/workspace-creation-complete.png":::

    Una vez finalizada la creación del área de trabajo, vuelva a la pestaña del portal de Machine Learning y haga clic en **Empezar**.

1. En la página principal del área de trabajo de Machine Learning, haga clic en **Cuadernos** en el panel izquierdo.

    :::image type="content" source="./media/advanced-development-cloud/notebook.png" alt-text="Página principal de Azure Machine Learning.":::

1. En la pestaña **Mis archivos**, haga clic en la flecha vertical para cargar el archivo .ipynb.

    :::image type="content" source="./media/advanced-development-cloud/upload-files.png" alt-text="Página principal que resalta el icono Cargar archivo.":::

1. Vaya y seleccione el [archivo Transferlearningusing_SSDLiteV2 Model.ipynbb](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) de la copia local del repositorio de GitHub de Azure Percept. Haga clic en **Abrir**. En la ventana **Cargar archivos**, active la casilla situada junto a **I trust contents from this file** (Confío en el contenido de este archivo) y haga clic en **Cargar**.

    :::image type="content" source="./media/advanced-development-cloud/select-file.png" alt-text="Pantalla de selección de archivos.":::

1. En la barra de menús superior derecha, compruebe el estado del **proceso**. Si no se encuentra ningún proceso, haga clic en el icono **+** para crear un nuevo proceso.

    :::image type="content" source="./media/advanced-development-cloud/no-computes-found-inline.png" alt-text="Estado de proceso con el icono + resaltado." lightbox= "./media/advanced-development-cloud/no-computes-found.png":::

1. En la ventana **New compute instance** (Nueva instancia de proceso), escriba un **nombre de proceso**, elija un **tipo de máquina virtual** y seleccione un **tamaño de máquina virtual**. Haga clic en **Crear**.

    :::image type="content" source="./media/advanced-development-cloud/new-compute-instance.png" alt-text="Pantalla de creación de nueva instancia de proceso.":::

    Una vez que haga clic en **Crear**, el estado de **Proceso** mostrará un círculo azul y **\<your compute name> - En creación**

    :::image type="content" source="./media/advanced-development-cloud/compute-creating.png" alt-text="Estado del proceso cuando su creación está aún en curso.":::

    El estado de **Proceso** mostrará un círculo verde y **\<your compute name> - En ejecución** una vez completada la creación del proceso.

    :::image type="content" source="./media/advanced-development-cloud/compute-running.png" alt-text="Estado de proceso que muestra que la creación del proceso se ha completado.":::

1. Una vez que se esté ejecutando el proceso, seleccione el kernel **Python 3.6 - AzureML** en el menú desplegable situado junto al icono **+** .

## <a name="working-with-the-notebook"></a>Uso del cuaderno

Ya está listo para ejecutar el cuaderno para entrenar su detector personalizado de cuencos e implementarlo en su kit de desarrollo. Asegúrese de que ejecuta cada celda del cuaderno individualmente, ya que algunas de las celdas requieren parámetros de entrada antes de ejecutar el script. Las celdas que requieren parámetros de entrada se pueden editar directamente en el cuaderno. Para ejecutar una celda, haga clic en el icono de reproducción en el lado izquierdo de la celda:

:::image type="content" source="./media/advanced-development-cloud/run-cell-inline.png" alt-text="Icono de celda resaltado del cuaderno." lightbox= "./media/advanced-development-cloud/run-cell.png":::

## <a name="next-steps"></a>Pasos siguientes

Para obtener más cuadernos de ejemplo de Azure Machine Learning Service, visite este [repositorio](https://github.com/Azure/MachineLearningNotebooks/tree/2aa7c53b0ce84e67565d77e484987714fdaed36e/how-to-use-azureml).
