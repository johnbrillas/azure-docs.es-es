---
title: Ejecución de los cuadernos de Jupyter en el área de trabajo
titleSuffix: Azure Machine Learning
description: Aprenda a ejecutar un cuaderno de Jupyter sin dejar el área de trabajo en Azure Machine Learning Studio.
services: machine-learning
author: abeomor
ms.author: osomorog
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.date: 01/19/2021
ms.openlocfilehash: 7bb1ce8141f609feb4f354aa85f202915e197f37
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98599245"
---
# <a name="how-to-run-jupyter-notebooks-in-your-workspace"></a>Ejecución de los cuadernos de Jupyter Notebook en el área de trabajo

Aprenda a ejecutar sus cuadernos de Jupyter directamente en el área de trabajo en Azure Machine Learning Studio. Aunque puede iniciar [Jupyter](https://jupyter.org/) o [JupyterLab](https://jupyterlab.readthedocs.io), también puede editar y ejecutar los cuadernos sin tener que salir del área de trabajo.

## <a name="prerequisites"></a>Requisitos previos

* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLFree) antes de empezar.
* Un área de trabajo de Machine Learning. Consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

## <a name="create-notebooks"></a><a name="create"></a> Creación de cuadernos

En el área de trabajo de Azure Machine Learning, cree un nuevo cuaderno de Jupyter Notebook y empiece a trabajar. El cuaderno recién creado se almacena en el almacenamiento predeterminado del área de trabajo. Este cuaderno se puede compartir con cualquier persona que tenga acceso al área de trabajo. 

Para crear un nuevo cuaderno: 

1. En [Azure Machine Learning Studio](https://ml.azure.com), abra el área de trabajo.
1. En el lado izquierdo, seleccione **Notebooks**. 
1. Seleccione el icono **Crear archivo** situado encima de la lista **Archivos de usuario** en la sección **Mis archivos**.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/create-new-file.png" alt-text="Crear un nuevo archivo":::

1. Dele un nombre al archivo. 
1. En el caso de los archivos del cuaderno de Jupyter, seleccione **Cuaderno** como tipo de archivo.
1. Seleccione un directorio de archivos.
1. Seleccione **Crear**.

También puede crear archivos de texto.  Seleccione **Texto** como el tipo de archivo y agregue la extensión al nombre (por ejemplo, miarchivo.py o miarchivo.txt).  

También puede cargar carpetas y archivos, incluidos cuadernos, con las herramientas que se encuentra en la parte superior de la página Notebooks.  Los cuadernos y la mayoría de los tipos de archivo de texto se muestran en la sección de vista previa.  Para la mayoría de los demás tipos de archivo la vista previa no está disponible.

> [!IMPORTANT]
> El contenido de los cuadernos y scripts puede leer los datos de las sesiones y acceder a los datos sin su organización en Azure.  Cargar solo archivos de fuentes de confianza. Para más información, vea [Prácticas recomendadas de código seguro](concept-secure-code-best-practice.md#azure-ml-studio-notebooks).

### <a name="clone-samples"></a>Clonación de ejemplos

El área de trabajo contiene una carpeta de **ejemplos** con cuadernos diseñados para ayudarle a explorar el SDK y servir como ejemplo para sus propios proyectos de aprendizaje automático.  Puede clonar estos cuadernos en su propia carpeta en el contenedor de almacenamiento del área de trabajo.  

Para obtener un ejemplo, vea [Tutorial: Creación del primer experimento de Machine Learning](tutorial-1st-experiment-sdk-setup.md#azure).

### <a name="use-files-from-git-and-version-my-files"></a><a name="terminal"></a> Uso de los archivos de Git y versión Mis archivos

Puede acceder a todas las operaciones de Git mediante una ventana de terminal. Todos los archivos y carpetas de Git se almacenarán en el sistema de archivos del área de trabajo.

> [!NOTE]
> Agregue los archivos y las carpetas en cualquier parte dentro de la carpeta **~/cloudfiles/code/Users** para que estén visibles en todos los entornos de Jupyter.

Para tener acceso al terminal:

1. En [Azure Machine Learning Studio](https://ml.azure.com), abra el área de trabajo.
1. En el lado izquierdo, seleccione **Notebooks**.
1. Seleccione cualquier cuaderno situado en la sección **Archivos de usuario** en el lado izquierdo.  Si allí no tiene ningún cuaderno, primero [cree un cuaderno](#create).
1. Seleccione un destino de **Proceso** o cree uno nuevo y espere hasta que se esté ejecutando.
1. Seleccione el icono de **Abrir terminal**.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/open-terminal.png" alt-text="Abra el terminal":::.

1. Si no ve el icono, seleccione los puntos **...** que se encuentran a la derecha del destino de proceso y, a continuación, seleccione **Abrir terminal**.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/alt-open-terminal.png" alt-text="Abrir el terminal desde los puntos ...":::.


Obtenga más información sobre [la clonación de repositorios de GIT de en el sistema de archivos del área de trabajo](concept-train-model-git-integration.md#clone-git-repositories-into-your-workspace-file-system).

### <a name="copy-and-paste-in-terminal"></a>Copiar y pegar en el terminal

> * Windows: `Ctrl-Insert` para copiar y `Ctrl-Shift-v` o `Shift-Insert` para pegar.
> * Mac OS: `Cmd-c` para copiar y `Cmd-v` para pegar.
> * Es posible que Firefox o IE no admitan los permisos del Portapapeles correctamente.

### <a name="share-notebooks-and-other-files"></a>Compartición de cuadernos y otros archivos

Copie y pegue la dirección URL para compartir un bloc de notas o un archivo.  Solo otros usuarios del área de trabajo pueden acceder a esta dirección URL.  Obtenga más información sobre la [concesión de acceso a su área de trabajo](how-to-assign-roles.md).

## <a name="edit-a-notebook"></a>Edición de un cuaderno

Para editar un cuaderno, abra cualquiera que esté situado en la sección **Archivos de usuario** de su área de trabajo. Haga clic en la celda que desee editar. 

Puede editar el cuaderno sin necesidad de conectarse a una instancia de proceso.  Cuando desee ejecutar las celdas en el cuaderno, seleccione o cree una instancia de proceso.  Si selecciona una instancia de proceso detenida, se iniciará automáticamente al ejecutar la primera celda.

Cuando se ejecuta una instancia de proceso, también puede usar la finalización de código, con la tecnología de [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), en cualquier cuaderno de Python.

También puede iniciar Jupyter o JupyterLab desde la barra de herramientas del cuaderno.  Azure Machine Learning no proporciona actualizaciones ni corrige errores de Jupyter o JupyterLab, ya que son productos de código abierto fuera de los límites del servicio de soporte técnico de Microsoft.

### <a name="focus-mode"></a>Modo de enfoque

Use el modo de enfoque para expandir la vista actual de forma que pueda centrarse en las pestañas activas. El modo de enfoque oculta el explorador de archivos de Notebooks.

1. En la barra de herramientas de la ventana del terminal, seleccione **Modo de enfoque** para activar el modo de enfoque. En función del ancho de la ventana, es posible que se encuentre bajo el elemento de menú **...** de la barra de herramientas.
1. En el modo de enfoque, seleccione **Vista estándar** para volver a la vista estándar.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/focusmode.gif" alt-text="Cambio entre el modo de enfoque y la vista estándar":::


### <a name="use-intellisense"></a>Usar IntelliSense

[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) es una ayuda de finalización de código que incluye una serie de características: enumerar miembros, información de parámetros, información rápida y completar palabra. Estas características le ayudan a obtener más información sobre el código que está usando, realizar un seguimiento de los parámetros que está escribiendo y agregar llamadas a propiedades y métodos con solo unas cuantas pulsaciones de tecla.  

Al escribir código, use Ctrl + barra espaciadora para desencadenar IntelliSense.

### <a name="clean-your-notebook-preview"></a>Limpieza del cuaderno (versión preliminar)

> [!IMPORTANT]
> La característica de recopilación actualmente está en versión preliminar pública.
> Se ofrece la versión preliminar sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Durante la creación de un cuaderno, normalmente acaba con las celdas usadas para la exploración o la depuración de datos. La característica de *recopilación* le ayudará a crear un cuaderno limpio sin estas celdas extrañas.

1. Ejecute todas las celdas del cuaderno.
1. Seleccione la celda que contiene el código que desea que ejecute el nuevo cuaderno. Por ejemplo, el código que envía un experimento o quizás el código que registra un modelo.
1. Seleccione el icono de **recopilación** que aparece en la barra de herramientas de celda.
    :::image type="content" source="media/how-to-run-jupyter-notebooks/gather.png" alt-text="Captura de pantalla: seleccione el icono de recopilación":::
1. Escriba el nombre del nuevo cuaderno "recopilado".  

El nuevo cuaderno solo contiene celdas de código, con todas las celdas necesarias para generar los mismos resultados que la celda seleccionada para la recopilación.

### <a name="save-and-checkpoint-a-notebook"></a>Guardar y revisar un cuaderno

Azure Machine Learning crea un archivo de punto de comprobación cuando se crea un archivo *ipynb*.

En la barra de herramientas del cuaderno, seleccione el menú y luego, **Archivo&gt;Guardar y punto de control** para guardar manualmente el cuaderno y agregará un archivo de punto de control asociado con el cuaderno.

:::image type="content" source="media/how-to-run-jupyter-notebooks/file-save.png" alt-text="Captura de pantalla de la herramienta guardar en la barra de herramientas del cuaderno":::

Cada cuaderno se guarda automáticamente cada 30 segundos. El guardado automático actualiza solo el archivo *ipynb* inicial, no el archivo de punto de control.
 
Seleccione **Puntos de control** en el menú del cuaderno para crear un punto de control con nombre y revertir el cuaderno a un punto de control guardado.

## <a name="delete-a-notebook"></a>Eliminación de un cuaderno

Los cuadernos de **ejemplo** *no* se pueden eliminar.  Estos cuadernos forman parte de Studio y se actualizan cada vez que se publica un nuevo SDK.  

Los **archivos de usuario** y los cuadernos *se pueden* eliminar de cualquiera de estas formas:

* En Studio, seleccione los puntos **...** al final de una carpeta o un archivo.  Asegúrese de usar un explorador compatible (Microsoft Edge, Chrome o Firefox).
* En la barra de herramientas de cualquier cuaderno, seleccione [**Abrir terminal**](#terminal) para acceder a la ventana de terminal de la instancia de proceso.
* En Jupyter o JupyterLab con sus herramientas.

## <a name="run-a-notebook-or-python-script"></a>Ejecución de un cuaderno o un script de Python

Para ejecutar un cuaderno o un script de Python, primero tiene que conectarse a una [instancia de proceso](concept-compute-instance.md) en ejecución. Si no tiene una instancia de proceso, siga estos pasos para crear una: 

1. Seleccione **+** en el cuaderno o la barra de herramientas del script. 
2. Asigne un nombre al proceso y elija el **Tamaño de la máquina virtual**. 
3. Seleccione **Crear**.
4. La instancia de proceso se conecta automáticamente al archivo.  Ahora puede ejecutar las celdas del cuaderno o el script de Python mediante la herramienta que se encuentra a la izquierda de la instancia de proceso.

Solo puede ver y usar las instancias de proceso que usted cree.  Los **Archivos de usuario** se almacenan de forma independiente de la máquina virtual y se comparten entre todas las instancias de proceso en el área de trabajo.

### <a name="view-logs-and-output"></a>Ver registros y resultados

Use los [widgets del cuaderno](/python/api/azureml-widgets/azureml.widgets?preserve-view=true&view=azure-ml-py) para ver el progreso de la ejecución y los registros. Un widget es asincrónico y proporciona actualizaciones hasta que finaliza el entrenamiento. Los widgets de Azure Machine Learning también se admiten en Jupyter y JupterLab.

:::image type="content" source="media/how-to-run-jupyter-notebooks/jupyter-widget.png" alt-text="Captura de pantalla: Widget "::: del cuaderno de Jupyter

## <a name="explore-variables-in-the-notebook"></a>Exploración de las variables del cuaderno

En la barra de herramientas del cuaderno, use la herramienta **Explorador de variables** para mostrar el nombre, el tipo, la longitud y los valores de ejemplo de todas las variables que se han creado en el cuaderno.

:::image type="content" source="media/how-to-run-jupyter-notebooks/variable-explorer.png" alt-text="Captura de pantalla: herramienta Explorador de variables":::

Seleccione la herramienta para mostrar la ventana del explorador de variables.

:::image type="content" source="media/how-to-run-jupyter-notebooks/variable-explorer-window.png" alt-text="Captura de pantalla: ventana del explorador de variables":::

## <a name="navigate-with-a-toc"></a>Navegación con una tabla de contenido

En la barra de herramientas del cuaderno, use la herramienta **Tabla de contenido** para mostrar u ocultar la misma.  Inicie una celda de Markdown con un encabezado para agregarla a la tabla de contenido. Haga clic en una entrada de la tabla para desplazarse a esa celda en el cuaderno.  

:::image type="content" source="media/how-to-run-jupyter-notebooks/table-of-contents.png" alt-text="Captura de pantalla: tabla de contenido del cuaderno":::

## <a name="change-the-notebook-environment"></a>Cambio del entorno del cuaderno.

La barra de herramientas del cuaderno le permite cambiar el entorno en el que se ejecuta el mismo.  

Estas acciones no cambiarán el estado del cuaderno o los valores de las variables del mismo:

|Acción  |Resultado  |
|---------|---------| --------|
|Detención del kernel     |  Detiene cualquier celda en ejecución. Al ejecutar una celda, se reiniciará automáticamente el kernel. |
|Desplazamiento a otra sección del área de trabajo     |     Se detienen las celdas en ejecución. |

Estas acciones restablecerán el estado del cuaderno y todas las variables del mismo.

|Acción  |Resultado  |
|---------|---------| --------|
| Cambio del kernel | Notebook usa un nuevo kernel |
| Cambio de proceso    |     Notebook utiliza automáticamente el nuevo proceso. |
| Restablecimiento del proceso | Se inicia de nuevo cuando se intenta ejecutar una celda. |
| Detención del proceso     |    No se ejecutarán las celdas.  |
| Apertura del cuaderno en Jupyter o JupyterLab     |    Notebook se abre en una nueva pestaña.  |

### <a name="add-new-kernels"></a>Incorporación de nuevos kernels

El cuaderno buscará automáticamente todos los kernels de Jupyter instalados en la instancia de proceso conectada.  Para agregar un kernel a la instancia de proceso:

1. Seleccione [**Abrir terminal**](#terminal) en la barra de herramientas del cuaderno.
1. Use la ventana de terminal para crear un nuevo entorno.  Por ejemplo, el código siguiente crea `newenv`:
    ```shell
    conda create -y --name newenv
    ```
1. Active el entorno.  Por ejemplo, después de crear `newenv`:

    ```shell
    conda activate newenv
    ```
1. Instalación del paquete PIP e ipykernel en el entorno nuevo y creación de un kernel para ese entorno conda

    ```shell
    conda install -y pip
    conda install -y ipykernel
    python -m ipykernel install --user --name newenv --display-name "Python (newenv)"
    ```

> [!NOTE]
> Para la administración de paquetes en un cuaderno, use las funciones magic **%pip** o **%conda** para instalar paquetes automáticamente en el **kernel que actualmente está en ejecución**, en lugar de **!pip** o **!conda** que hacen referencia a todos los paquetes (incluidos los paquetes que se encuentran fuera del kernel que actualmente está en ejecución)

Se puede instalar cualquiera de los [kernels de Jupyter](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) disponibles.

### <a name="status-indicators"></a>Indicadores de estado

Un indicador situado junto al elemento de la lista desplegable **Proceso** muestra su estado.  El estado también se muestra en el mismo elemento de la lista desplegable.  

|Color |Estado de proceso |
|---------|---------| 
| Verde | Proceso en ejecución |
| Rojo |Error de proceso | 
| Negro | Proceso detenido |
|  Azul claro |Proceso creando, iniciando, reiniciando, configurando |
|  Gris |Proceso eliminando, deteniendo |

Un indicador situado junto al elemento de lista desplegable **Kernel** muestra su estado.

|Color |Estado del kernel |
|---------|---------|
|  Verde |Kernel conectado, inactivo, ocupado|
|  Gris |Kernel no conectado |

## <a name="shortcut-keys"></a>Teclas de método abreviado
De forma similar a los cuadernos de Jupyter Notebook, los cuadernos de Azure Machine Learning Studio tienen una interfaz de usuario modal. El teclado realiza diferentes acciones en función del modo en que se encuentre la celda del cuaderno. Los cuadernos de Azure Machine Learning Studio admiten los siguientes dos modos para una celda de código determinada: el modo de comando y el de edición.

### <a name="command-mode-shortcuts"></a>Métodos abreviados del modo de comando

Una celda se encuentra en modo de comando cuando no hay ningún cursor de texto que le pida que escriba. Cuando una celda está en modo de comando, puede editar el cuaderno en su conjunto, pero no escribir en celdas individuales. Para ingresar al modo de comando, presione `ESC` o use el mouse para seleccionar fuera del área del editor de una celda.  El borde izquierdo de la celda activa es azul y sólido, y el botón **Ejecutar** es azul.

   :::image type="content" source="media/how-to-run-jupyter-notebooks/command-mode.png" alt-text="Celda de cuaderno en el modo de comando ":::

| Acceso directo                      | Descripción                          |
| ----------------------------- | ------------------------------------|
| Entrar                         | Acceso al modo de edición             |        
| Mayús + Entrar                 | Ejecución de la celda, selección del siguiente contenido         |     
| Control/Comando + Entrar       | Ejecución de una celda                            |
| Alt + Entrar                   | Ejecución de una celda, inserción de la celda de código siguiente    |
| Control/Comando + Alt + Entrar | Ejecución de una celda, inserción de la celda de Markdown siguiente|
| Alt + R                       | Ejecutar todo      |                       
| Y                             | Convertir la celda en código    |                         
| M                             | Convertir la celda en Markdown  |                       
| Arriba/K                          | Seleccionar la celda anterior    |               
| Abajo/J                        | Seleccionar la celda siguiente    |               
| Un                             | Insertar la celda de código más arriba  |            
| B                             | Insertar la celda de código abajo   |           
| Control/Comando + Mayús + A   | Insertar la celda de Markdown arriba    |      
| Control/Comando + Mayús + B   | Insertar la celda de Markdown abajo   |       
| X                             | Cortar la celda seleccionada    |               
| C                             | Copiar la celda seleccionada   |               
| Mayús + V                     | Pegar la celda seleccionada arriba           |
| V                             | Pegar la celda seleccionada abajo    |       
| D D                           | Eliminar la celda seleccionada|                
| O                             | Toggle Output         |              
| Mayús + O                     | Alternar el desplazamiento de salida   |          
| I I                           | Interrumpir kernel |                   
| 0 0                           | Reiniciar kernel |                     
| Mayús + barra espaciadora                 | Desplazarse hacia arriba  |                         
| Space                         | Desplazarse hacia abajo|
| Pestaña                           | Cambiar el foco al siguiente elemento enfocable (cuando se deshabilita la captura de pestañas)|
| Control/comando + S           | Guardar cuaderno |                      
| 1                             | Cambiar a H1|                       
| 2                             | Cambiar a H2|                        
| 3                             | Cambiar a H3|                        
| 4                             | Cambiar a H4 |                       
| 5                             | Cambiar a H5 |                       
| 6                             | Cambiar a H6 |                       

### <a name="edit-mode-shortcuts"></a>Métodos abreviados del modo de edición

El modo de edición se indica mediante un cursor de texto que le pide que escriba en el área del editor. Cuando una celda se encuentra en modo de edición, puede escribir en la celda. Para ingresar al modo de edición, presione `Enter` o use el mouse para seleccionar en el área del editor de una celda. El borde izquierdo de la celda activa es verde y está sombreado, y el botón **Ejecutar** es verde. En el modo de edición también verá la solicitud del cursor en la celda.

   :::image type="content" source="media/how-to-run-jupyter-notebooks/edit-mode.png" alt-text="Celda del cuaderno en el modo de edición":::

Con los siguientes métodos abreviados de teclado, puede navegar y ejecutar código más fácilmente en cuadernos de Azure Machine Learning en el modo de edición.

| Acceso directo                      | Descripción|                                     
| ----------------------------- | ----------------------------------------------- |
| Escape                        | Especificar el modo de comandos|  
| Control/Comando + Espacio       | Activar IntelliSense |
| Mayús + Entrar                 | Ejecución de la celda, selección del siguiente contenido |                         
| Control/Comando + Entrar       | Ejecución de una celda  |                                      
| Alt + Entrar                   | Ejecución de una celda, inserción de la celda de código siguiente  |              
| Control/Comando + Alt + Entrar | Ejecución de una celda, inserción de la celda de Markdown siguiente  |          
| Alt + R                       | Ejecución de todas las celdas     |                              
| Subir                            | Subir el cursor o la celda anterior    |             
| Bajar                          | Bajar el cursor o la celda siguiente |                  
| Control/comando + S           | Guardar cuaderno   |                                
| Control/Comando + arriba          | Ir al inicio de la celda   |                             
| Control/Comando + abajo        | Ir al final de la celda |                                 
| Pestaña                           | Finalizar el código o aplicarle sangría (si se habilita la captura de pestañas) |
| Control/Comando + M           | Habilitar o deshabilitar la captura de pestañas  |                       
| Control/Cmando + ]           | Aplicar sangría |                                         
| Control/Comando + [           | Desaplicar sangría  |                                        
| Control/Comando + A           | Seleccionar todo|                                      
| Control/Comando + Z           | Deshacer |                                           
| Control/Comando + Mayús + Z   | Rehacer |                                           
| Control/Comando + Y           | Rehacer |                                           
| Control/Comando + Inicio        | Ir al inicio de la celda|                                
| Control/Comando + Fin         | Ir al final de la celda   |                               
| Control/Comando + Izquierda        | Ir una palabra a la izquierda |                               
| Control/Comando + Derecha       | Ir una palabra a la derecha |                              
| Control/Comando + Retroceso   | Eliminar palabra anterior |                             
| Control/Comando + Eliminar      | Eliminar palabra posterior |                              
| Control/Comando + /           | Alternar comentario en cu

## <a name="find-compute-details"></a>Detalles de proceso

Busque los detalles sobre las instancias de proceso en la página **Proceso** de [Studio](https://ml.azure.com).

## <a name="next-steps"></a>Pasos siguientes

* [Ejecución de su primer experimento](tutorial-1st-experiment-sdk-train.md)
* [Copia de seguridad del almacenamiento de archivos con instantáneas](../storage/files/storage-snapshots-files.md)