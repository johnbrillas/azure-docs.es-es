---
title: Uso de un cuaderno de Jupyter Notebook con las ofertas de Microsoft
description: Obtenga información sobre cómo se pueden usar cuadernos de Jupyter Notebook con ofertas de Microsoft.
ms.topic: quickstart
ms.date: 02/01/2021
ms.openlocfilehash: 5679c28d9cc8a4f1893ffad72002b66ad59861e6
ms.sourcegitcommit: b85ce02785edc13d7fb8eba29ea8027e614c52a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99507384"
---
# <a name="use-a-jupyter-notebook-with-microsoft-offerings"></a>Uso de un cuaderno de Jupyter Notebook con las ofertas de Microsoft

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

En esta guía de inicio rápido, aprenderá a importar un cuaderno de Jupyter Notebook para usarlo con diversas ofertas de Microsoft. 

Si ya tiene cuadernos de Jupyter Notebook o desea crear un proyecto nuevo, puede usarlos con muchas de las ofertas de Microsoft. Estas son algunas de las opciones que se describen en las secciones siguientes: 
- [Visual Studio Code](#use-notebooks-in-visual-studio-code)
- [GitHub Codespaces](#use-notebooks-in-github-codespaces)
- [Azure Machine Learning](#use-notebooks-with-azure-machine-learning)
- [Azure Lab Services](#use-azure-lab-services)
- [GitHub](#use-github)

## <a name="create-an-environment-for-notebooks"></a>Creación de un entorno para Notebooks

Si desea crear un entorno que coincida con el de la versión preliminar de Azure Notebooks retirada, puede usar el archivo de script que se proporciona en GitHub.

1. Vaya al [repositorio de GitHub](https://github.com/microsoft/AzureNotebooks) de Azure Notebooks o [acceda directamente a la carpeta del entorno](https://aka.ms/aznbrequirementstxt).
1. En un símbolo del sistema, vaya al directorio que desea usar para los proyectos.
1. Descargue el contenido de la carpeta del entorno y siga las instrucciones del archivo Léame para instalar las dependencias del paquete de Azure Notebooks.


## <a name="use-notebooks-in-visual-studio-code"></a>Uso de Notebooks en Visual Studio Code

[VS Code](https://code.visualstudio.com/) es un editor de código gratuito que se puede usar tanto localmente como conectado a un proceso remoto. Si se combina con la extensión de Python, ofrece un entorno completo para el desarrollo de Python, lo que incluye una experiencia nativa enriquecida para trabajar con cuadernos de Jupyter Notebook. 

![Compatibilidad de VS Code y Jupyter Notebook](media/vs-code-jupyter-notebook.png)

Si tiene archivos de proyecto existentes o desea crear un nuevo cuaderno, puede usar VS Code. Para obtener instrucciones sobre cómo usar VS Code con cuadernos de Jupyter Notebook, consulte los tutoriales sobre cómo [trabajar con cuadernos de Jupyter Notebook en Visual Studio Code](https://code.visualstudio.com/docs/python/jupyter-support) y sobre la [ciencia de datos en Visual Studio Code](https://code.visualstudio.com/docs/python/data-science-tutorial).

También puede usar el [script de entorno de Azure Notebooks](#create-an-environment-for-notebooks) con Visual Studio Code para crear un entorno que coincida con la versión preliminar de Azure Notebooks.

## <a name="use-notebooks-in-github-codespaces"></a>Uso de cuadernos en GitHub Codespaces

GitHub Codespaces proporciona entornos hospedados en la nube en los que puede editar los cuadernos mediante Visual Studio Code o un explorador web. Ofrece la misma experiencia de Jupyter que VS Code, pero sin necesidad de instalar nada en el dispositivo. Si no desea configurar un entorno local y prefiere una solución con el respaldo de la nube, una solución excelente es crear un espacio de código. Primeros pasos:
1. (Opcional) Recopile los archivos de proyecto que desea usar con GitHub Codespaces.
1. [Cree un repositorio de GitHub](https://help.github.com/github/getting-started-with-github/create-a-repo) para almacenar los cuadernos.   
1. [Agregue los archivos](https://help.github.com/github/managing-files-in-a-repository/adding-a-file-to-a-repository) al repositorio.
1. [Solicitud de acceso a la versión preliminar de GitHub Codespaces](https://github.com/features/codespaces)

## <a name="use-notebooks-with-azure-machine-learning"></a>Uso de Notebooks con Azure Machine Learning

Azure Machine Learning proporciona una plataforma de aprendizaje automático de un extremo a otro que permite a los usuarios crear e implementar modelos más rápidamente en Azure. Azure Machine Learning permite ejecutar cuadernos de Jupyter Notebook en una máquina virtual o en un entorno de computación de clúster compartido. Si necesita una solución en la nube para su carga de trabajo de Machine Learning con seguimiento de experimentos, administración de conjuntos de datos, etc., es aconsejable que use Azure Machine Learning. Para empezar a utilizar Azure Machine Learning:

1. (Opcional) Recopile los archivos de proyecto que desee usar con Azure ML.
1. [Cree un área de trabajo](../machine-learning/how-to-manage-workspace.md) en Azure Portal.

   ![Crear un área de trabajo](../machine-learning/media/how-to-manage-workspace/create-workspace.gif)
 
1. Abra [Azure Studio (versión preliminar)](https://ml.azure.com/).
1. Use la barra de navegación izquierda para seleccionar **Notebooks**.
1. Haga clic en el botón **Cargar archivos** y cargue los archivos del proyecto.

Para más información sobre Azure Machine Learning y la ejecución de Jupyter Notebooks, puede consultar la [documentación](../machine-learning/how-to-run-jupyter-notebooks.md) o probar el módulo [Introducción a Azure Machine Learning](/learn/modules/intro-to-azure-machine-learning-service/) de Microsoft Learn.


## <a name="use-azure-lab-services"></a>Uso de Azure Lab Services

[Azure Lab Services](https://azure.microsoft.com/services/lab-services/) permite a los formadores configurar y proporcionar fácilmente a toda una clase acceso a petición a las máquinas virtuales preconfiguradas. Si busca una manera de trabajar con Jupyter Notebooks y con procesos en la nube en un entorno docente adaptado, Lab Services es una excelente opción.

![imagen](../lab-services/media/tutorial-setup-classroom-lab/new-lab-button.png)

Si tiene archivos de proyecto existentes o desea crear un nuevo cuaderno, puede usar Azure Lab Services. Si desea una guía para configurar un laboratorio, consulte [Configuración de un laboratorio para enseñar ciencia de datos con Python y Jupyter Notebooks](../lab-services/class-type-jupyter-notebook.md)

## <a name="use-github"></a>Uso de GitHub

GitHub ofrece una forma gratuita y con el respaldo del control del origen de almacenar cuadernos (y otros archivos), compartir cuadernos con otros usuarios y trabajar de forma colaborativa. Si busca una forma de compartir proyectos y colaborar con otros usuarios, GitHub es una opción magnífica que, además, se puede combinar con [GitHub Codespaces](#use-notebooks-in-github-codespaces) para que la experiencia de desarrollo sea excelente. Para empezar a usar GitHub

1. (Opcional) Recopile los archivos de proyecto que desee usar con GitHub.
1. [Cree un repositorio de GitHub](https://help.github.com/github/getting-started-with-github/create-a-repo) para almacenar los cuadernos. 
1. [Agregue los archivos](https://help.github.com/github/managing-files-in-a-repository/adding-a-file-to-a-repository) al repositorio.

## <a name="next-steps"></a>Pasos siguientes

- [Información acerca de Python en Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial)
- [Información acerca de Azure Machine Learning y cuadernos de Jupyter Notebook](../machine-learning/how-to-run-jupyter-notebooks.md)
- [Más información sobre GitHub Codespaces](https://github.com/features/codespaces)
- [Información sobre Azure Lab Services](https://azure.microsoft.com/services/lab-services/)
- [Información acerca de GitHub](https://help.github.com/github/getting-started-with-github/)
