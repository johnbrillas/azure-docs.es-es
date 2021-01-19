---
title: 'Tutorial: Entrenamiento e implementación de un modelo: Machine Learning en Azure IoT Edge'
description: En este tutorial aprenderá a entrenar un modelo de Machine Learning mediante Azure Machine Learning y, a continuación, empaquetar el modelo como una imagen de contenedor que puede implementarse como un módulo de Azure IoT Edge.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 3/24/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 2cc96db88d9a2aec02de5e2fc4ed18b445972e7b
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98121159"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Tutorial: Entrenamiento e implementación de un modelo de Azure Machine Learning

En este artículo, realizará las tareas siguientes:

* Uso de Azure Machine Learning Studio para entrenar un modelo de Machine Learning.
* Empaquetará el modelo entrenado como una imagen de contenedor.
* Implementará la imagen de contenedor como un módulo de Azure IoT Edge.

Azure Machine Learning Studio es un bloque fundamental para experimentar, entrenar e implementar modelos de Machine Learning.

Normalmente, los científicos de datos se encargan de los pasos descritos en este artículo.

En esta sección del tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear cuadernos de Jupyter Notebook en el área de trabajo de Azure Machine Learning para entrenar un modelo de aprendizaje automático.
> * Contenedorizar el modelo de aprendizaje automático entrenado.
> * Crear un módulo de Azure IoT Edge a partir del modelo de aprendizaje automático contenedorizado.

## <a name="prerequisites"></a>Requisitos previos

Este artículo forma parte de un tutorial sobre el uso de Azure Machine Learning en IoT Edge. Cada artículo de la serie se basa en el trabajo del artículo anterior. Si ha llegado a este artículo directamente, visite el [primer artículo](tutorial-machine-learning-edge-01-intro.md) de la serie.

## <a name="set-up-azure-machine-learning"></a>Configuración de Azure Machine Learning 

Usamos Azure Machine Learning Studio para hospedar los dos cuadernos de Jupyter Notebook y los archivos auxiliares. A continuación, creamos y configuramos un proyecto de Azure Machine Learning. Si no ha usado Jupyter o Azure Machine Learning Studio, a continuación encontrará un par de documentos de introducción:

* **Cuadernos de Jupyter Notebook:** [Trabajo con cuadernos de Jupyter Notebook en Visual Studio Code.](https://code.visualstudio.com/docs/python/jupyter-support)
* **Azure Machine Learning:** [Introducción a Azure Machine Learning en cuadernos de Jupyter Notebook](../machine-learning/tutorial-1st-experiment-sdk-setup.md).


> [!NOTE]
> Una vez configurado, puede acceder al servicio Azure Machine Learning desde cualquier máquina. Durante el proceso de configuración, debe usar la máquina virtual de desarrollo que tiene todos los archivos que necesitará.

### <a name="install-azure-machine-learning-visual-studio-code-extension"></a>Instalación de la extensión Azure Machine Learning para Visual Studio Code
En la máquina virtual de desarrollo VS Code debe tener instalada esta extensión. Si la ejecución se realiza en otra instancia, vuelva a instalar la extensión tal como se describe [aquí](../machine-learning/tutorial-setup-vscode-extension.md).

### <a name="create-an-azure-machine-learning-account"></a>Creación de una cuenta de Azure Machine Learning  
Para aprovisionar recursos y ejecutar cargas de trabajo en Azure, tiene que iniciar sesión con las credenciales de su cuenta de Azure.

1. En Visual Studio Code, abra la paleta de comandos; para ello, seleccione **View** > **Command Palette** (Ver > Paleta de comandos) en la barra de menús. 

1. Escriba el comando `Azure: Sign In` en la paleta de comandos para empezar el proceso de inicio de sesión. Siga las instrucciones para completar el inicio de sesión. 

1. Cree una instancia de Proceso de Azure Machine Learning para ejecutar la carga de trabajo. Con la paleta de comandos, escriba el comando `Azure ML: Create Compute`. 
1. Selección de una suscripción a Azure
1. Seleccione **Create a new Azure ML workspace** (Crear un área de trabajo de Azure Machine Learning) y escriba el nombre `turbofandemo`.
1. Seleccione el grupo de recursos que ha usado para esta demostración.
1. Debería poder ver el progreso de del proceso de creación del área de trabajo en la esquina inferior derecha de la ventana de VS Code: **Creating Workspace: turobofandemo** (Creando área de trabajo: turbofandemo) (puede tardar un par de minutos). 
1. Espere a que el área de trabajo se haya creado correctamente. Debe decir **Azure ML workspace turbofandemo created** (Área de trabajo turbofandemo de Azure Machine Learning creada).


### <a name="upload-jupyter-notebook-files"></a>Carga de archivos de Jupyter Notebook

Cargaremos archivos de cuaderno de ejemplo en un área de trabajo de Azure Machine Learning nueva.

1. Vaya a ml.azure.com e inicie sesión.
1. Seleccione el directorio de Microsoft, la suscripción de Azure y el área de trabajo de Azure Machine Learning recién creada.

    :::image type="content" source="media/tutorial-machine-learning-edge-04-train-model/select-studio-workspace.png" alt-text="Seleccione el área de trabajo de Azure Machine Learning." :::

1. Una vez iniciada su sesión en el área de trabajo de Azure Machine Learning, vaya a la sección **Notebooks** mediante el menú del lado izquierdo.
1. Seleccione la pestaña **My files** (Mis archivos).

1. Seleccione **Upload** (Cargar) (el icono de flecha hacia arriba). 


1. Vaya a **C:\source\IoTEdgeAndMlSample\AzureNotebooks**. Seleccione todos los archivos de la lista y haga clic en **Open** (Abrir).

1. Active la casilla **I trust the content of these files** (Confío en el contenido de estos archivos).

1. Seleccione **Cargar** para empezar a cargarlos y, a continuación, seleccione **Listo** una vez completado el proceso.

### <a name="jupyter-notebook-files"></a>Archivos de Jupyter Notebook

Vamos a revisar los archivos cargados en el área de trabajo de Azure Machine Learning. Las actividades de esta parte del tutorial abarcan dos archivos de cuaderno, que usan algunos archivos auxiliares.

* **01-turbofan\_regression.ipynb:** Este cuaderno usa el área de trabajo de Machine Learning Service para crear y ejecutar un experimento de aprendizaje automático. En líneas generales, el cuaderno realiza los siguientes pasos:

  1. Descarga datos de la cuenta de Azure Storage generada por la herramienta de ejecución de dispositivos.
  1. Explora y prepara los datos, y usa los datos para entrenar el modelo clasificador.
  1. Evalúe el modelo desde el experimento mediante un conjunto de datos de prueba (Test\_FD003.txt).
  1. Publica el mejor modelo de clasificación en el área de trabajo de Machine Learning Service.

* **02-turbofan\_deploy\_model.ipynb:** este cuaderno toma el modelo creado en el cuaderno anterior y lo usa para crear una imagen de contenedor lista para implementarse en un dispositivo de Azure IoT Edge. El cuaderno realiza los siguientes pasos:

  1. Crea un script de puntuación para el modelo.
  1. Genera una imagen de contenedor mediante el modelo de clasificación que se guardó en el área de trabajo de Machine Learning Service.
  1. Implementa la imagen como un servicio web en Azure Container Instances.
  1. Usa el servicio web para validar el modelo y el trabajo de la imagen según lo previsto. La imagen validada se implementará en el dispositivo IoT Edge en la sección [Creación e implementación de módulos personalizados de IoT Edge](tutorial-machine-learning-edge-06-custom-modules.md) de este tutorial.

* **Test\_FD003.txt:** este archivo contiene los datos que usaremos como conjunto de prueba al validar el clasificador entrenado. Decidimos usar los datos de prueba tal, y como se proporcionan para el concurso original, como nuestro conjunto de pruebas por su sencillez.

* **RUL\_FD003.txt:** este archivo contiene la vida útil restante (RUL) del último ciclo de todos los dispositivos en el archivo Test\_FD003.txt. Consulte los archivos readme.txt y Damage Propagation Modeling.pdf (modelado de propagación de daños) en C:\\source\\IoTEdgeAndMlSample\\data\\Turbofan para obtener una explicación detallada de los datos.

* **Utils.py:** contiene un conjunto de funciones de utilidad de Python para trabajar con los datos. El primer cuaderno contiene una explicación detallada de las funciones.

* **README.md:** archivo Léame que describe el uso de los cuadernos.  

## <a name="run-jupyter-notebooks"></a>Ejecución de cuadernos de Jupyter Notebook

Ahora que se ha creado el área de trabajo, puede ejecutar los cuadernos. 

1. En la página **My files** (Mis archivos), seleccione **01-turbofan\_regression.ipynb**.

    :::image type="content" source="media/tutorial-machine-learning-edge-04-train-model/select-turbofan-notebook.png" alt-text="Seleccione el primer cuaderno que se ejecutará.":::

1. Si el cuaderno aparece como **Not Trusted** (No de confianza), haga clic en el widget **No de confianza** en la parte superior derecha del cuaderno. Cuando se muestre el cuadro de diálogo, seleccione **Trust** (Confiar).

1. Para obtener unos mejores óptimos, lea la documentación de cada celda y ejecútela de forma individual. Seleccione **Ejecutar** en la barra de herramientas. Más adelante, le resultará conveniente ejecutar varias celdas. Puede obviar las advertencias de actualización y desuso.

    Cuando se ejecuta una celda, muestra un asterisco entre corchetes ([\*]). Cuando se completa la operación de la celda, el asterisco se reemplaza por un número y es posible que se muestre el resultado pertinente. Las celdas de un cuaderno se compilan secuencialmente y no puede haber más de una en ejecución a la vez.

    También puede usar opciones de ejecución del menú **Cell** (Celda), `Ctrl` + `Enter` para ejecutar una celda y `Shift` + `Enter` para ejecutar una celda y pasar a la celda siguiente.

    > [!TIP]
    > Para las operaciones de celda coherentes, evite ejecutar el mismo cuaderno desde varias pestañas del explorador.

1. En la celda que sigue a las instrucciones de **Set global properties** (Establecer propiedades globales), escriba los valores de la suscripción, la configuración y los recursos de Azure. Luego, ejecute la celda.

    ![Establecimiento de las propiedades globales en el cuaderno](media/tutorial-machine-learning-edge-04-train-model/set-global-properties.png)

1. En la celda anterior a **Detalles del área de trabajo**, después de que se haya ejecutado, busque el vínculo en que se le solicita que inicie sesión para autenticarse:

    ![Solicitud de inicio de sesión para la autenticación de dispositivos](media/tutorial-machine-learning-edge-04-train-model/sign-in-prompt.png)

    Abra el vínculo y escriba el código especificado. Este procedimiento de inicio de sesión autentica el cuaderno de Jupyter Notebook para acceder a los recursos de Azure mediante la interfaz de la línea de comandos multiplataforma de Azure.  

    ![Aplicación de autenticación en la confirmación del dispositivo](media/tutorial-machine-learning-edge-04-train-model/cross-platform-cli.png)

1. En la celda que precede a **Explorar los resultados**, copie el valor del identificador de ejecución y péguelo en la celda posterior a **Reconstituir una ejecución**.

   ![Copiar el identificador de ejecución entre celdas](media/tutorial-machine-learning-edge-04-train-model/automl-id.png)

1. Ejecute las restantes celdas en el cuaderno.

1. Guarde el cuaderno y vuelva a la página del proyecto.

1. Abra el archivo **02-turbofan\_deploy\_model.ipynb** y ejecute todas las celdas. Tendrá que iniciar sesión para autenticarse en la celda posterior a **Configurar área de trabajo**.

1. Guarde el cuaderno y vuelva a la página del proyecto.

### <a name="verify-success"></a>Comprobación de que la operación se ha completado correctamente

Para confirmar que los cuadernos se han completado correctamente, compruebe si se han creado algunos elementos.

1. En la pestaña **My files** (Mis archivos) de los cuadernos de Azure Machine Learning, seleccione **Refresh** (Actualizar).

1. Compruebe que se hayan creado los archivos siguientes:

    | Archivo | Descripción |
    | --- | --- |
    | ./aml_config/.azureml/config.json | Archivo de configuración usado para crear el área de trabajo de Azure Machine Learning. |
    | ./aml_config/model_config.json | Archivo de configuración que se necesitará para implementar el modelo en el área de trabajo de Machine Learning **turbofanDemo** en Azure. |
    | myenv.yml| Proporciona información sobre las dependencias para el modelo de Machine Learning implementado.|

1. Compruebe que se han creado los siguientes recursos de Azure. Algunos nombres de recursos se anexan con caracteres aleatorios.

    | Recurso de Azure | Nombre |
    | --- | --- |
    | Área de trabajo de Machine Learning | turborfanDemo |
    | Container Registry | turbofandemoxxxxxxxx |
    | Applications Insights | turbofaninsightxxxxxxxx |
    | Key Vault | turbofankeyvaultbxxxxxxxx |
    | Storage | turbofanstoragexxxxxxxxx |

### <a name="debugging"></a>Depuración

Puede insertar instrucciones de Python en el cuaderno para su depuración, como por ejemplo, el comando `print()` para mostrar valores. Si ve variables u objetos que no están definidos, ejecute las celdas en las que se declaren o se hayan creado instancias de ellos primero.

Si necesita poner al día los cuadernos, es posible que tenga que eliminar tanto los archivos como los recursos de Azure.

## <a name="clean-up-resources"></a>Limpieza de recursos

Este tutorial forma parte de una serie en la que cada artículo complementa el trabajo realizado en los anteriores. No borre ningún recurso hasta que complete el tutorial final.

## <a name="next-steps"></a>Pasos siguientes

En este artículo usamos dos cuadernos de Jupyter Notebook que se ejecutan en Azure Machine Learning Studio para usar los datos de los dispositivos turbofan para el entrenamiento de un clasificador de vida útil restante, guardar el clasificador como modelo, crear una imagen de contenedor, y para implementar y probar la imagen como servicio web.

Consulte el artículo siguiente para crear un dispositivo IoT Edge.

> [!div class="nextstepaction"]
> [Configure un dispositivo IoT Edge](tutorial-machine-learning-edge-05-configure-edge-device.md)