---
title: Ejecución de cuadernos de Jupyter en el área de trabajo
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
ms.openlocfilehash: 06ae46eb96db39f44cd052e6e9b0d1a19f898007
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100091575"
---
# <a name="run-jupyter-notebooks-in-your-workspace"></a>Ejecución de cuadernos de Jupyter Notebook en el área de trabajo

Aprenda a ejecutar sus cuadernos de Jupyter directamente en el área de trabajo en Azure Machine Learning Studio. Aunque puede iniciar [Jupyter](https://jupyter.org/) o [JupyterLab](https://jupyterlab.readthedocs.io), también puede editar y ejecutar los cuadernos sin tener que salir del área de trabajo.

Para obtener información sobre cómo crear y administrar archivos, incluidos los cuadernos, consulte [Creación y administración de archivos en el área de trabajo](how-to-manage-files.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLFree) antes de empezar.
* Un área de trabajo de Machine Learning. Consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

## <a name="edit-a-notebook"></a>Edición de un cuaderno

Para editar un cuaderno, abra cualquiera que esté situado en la sección **Archivos de usuario** de su área de trabajo. Haga clic en la celda que desee editar.  Si no tiene ningún cuaderno en esta sección, consulte [Creación y administración de archivos en el área de trabajo](how-to-manage-files.md).

Puede editar el cuaderno sin necesidad de conectarse a una instancia de proceso.  Cuando desee ejecutar las celdas en el cuaderno, seleccione o cree una instancia de proceso.  Si selecciona una instancia de proceso detenida, se iniciará automáticamente al ejecutar la primera celda.

Cuando se ejecuta una instancia de proceso, también puede usar la finalización de código, con la tecnología de [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), en cualquier cuaderno de Python.

También puede iniciar Jupyter o JupyterLab desde la barra de herramientas del cuaderno.  Azure Machine Learning no proporciona actualizaciones ni corrige errores de Jupyter o JupyterLab, ya que son productos de código abierto fuera de los límites del servicio de soporte técnico de Microsoft.

## <a name="focus-mode"></a>Modo de enfoque

Use el modo de enfoque para expandir la vista actual de forma que pueda centrarse en las pestañas activas. El modo de enfoque oculta el explorador de archivos de Notebooks.

1. En la barra de herramientas de la ventana del terminal, seleccione **Modo de enfoque** para activar el modo de enfoque. En función del ancho de la ventana, es posible que se encuentre bajo el elemento de menú **...** de la barra de herramientas.
1. En el modo de enfoque, seleccione **Vista estándar** para volver a la vista estándar.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/focusmode.gif" alt-text="Cambio entre el modo de enfoque y la vista estándar":::

## <a name="use-intellisense"></a>Usar IntelliSense

[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) es una ayuda de finalización de código que incluye una serie de características: enumerar miembros, información de parámetros, información rápida y completar palabra. Estas características le ayudan a obtener más información sobre el código que está usando, realizar un seguimiento de los parámetros que está escribiendo y agregar llamadas a propiedades y métodos con solo unas cuantas pulsaciones de tecla.  

Al escribir código, use Ctrl + barra espaciadora para desencadenar IntelliSense.

## <a name="clean-your-notebook-preview"></a>Limpieza del cuaderno (versión preliminar)

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

## <a name="save-and-checkpoint-a-notebook"></a>Guardar y revisar un cuaderno

Azure Machine Learning crea un archivo de punto de comprobación cuando se crea un archivo *ipynb*.

En la barra de herramientas del cuaderno, seleccione el menú y luego, **Archivo&gt;Guardar y punto de control** para guardar manualmente el cuaderno y agregará un archivo de punto de control asociado con el cuaderno.

:::image type="content" source="media/how-to-run-jupyter-notebooks/file-save.png" alt-text="Captura de pantalla de la herramienta guardar en la barra de herramientas del cuaderno":::

Cada cuaderno se guarda automáticamente cada 30 segundos. El guardado automático actualiza solo el archivo *ipynb* inicial, no el archivo de punto de control.
 
Seleccione **Puntos de control** en el menú del cuaderno para crear un punto de control con nombre y revertir el cuaderno a un punto de control guardado.

## <a name="export-a-notebook"></a>Exportación de un cuaderno

En la barra de herramientas del cuaderno, seleccione el menú y, a continuación, **Exportar como** para exportar el cuaderno en cualquiera de los tipos admitidos:

* Notebook
* Python
* HTML
* LaTeX

:::image type="content" source="media/how-to-run-jupyter-notebooks/export-notebook.png" alt-text="Exportación de un cuaderno al equipo":::

El archivo exportado se guarda en el equipo.

## <a name="run-a-notebook-or-python-script"></a>Ejecución de un cuaderno o un script de Python

Para ejecutar un cuaderno o un script de Python, primero tiene que conectarse a una [instancia de proceso](concept-compute-instance.md) en ejecución.

* Si no tiene una instancia de proceso, siga estos pasos para crear una:

    1. En la barra de herramientas del cuaderno o script, a la derecha de la lista desplegable Proceso, seleccione **+ Nuevo proceso**. En función del tamaño de la pantalla, es posible que se encuentre en el menú **...**
        :::image type="content" source="media/how-to-run-jupyter-notebooks/new-compute.png" alt-text="Creación de un nuevo proceso":::
    1. Asigne un nombre al proceso y elija el **Tamaño de la máquina virtual**. 
    1. Seleccione **Crear**.
    1. La instancia de proceso se conecta automáticamente al archivo.  Ahora puede ejecutar las celdas del cuaderno o el script de Python mediante la herramienta que se encuentra a la izquierda de la instancia de proceso.

* Si tiene una instancia de proceso detenida, seleccione **Start compute** (iniciar proceso) a la derecha de la lista desplegable Proceso. En función del tamaño de la pantalla, es posible que se encuentre en el menú **...**

    :::image type="content" source="media/how-to-run-jupyter-notebooks/start-compute.png" alt-text="Inicio de la instancia de proceso":::

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

## <a name="add-new-kernels"></a>Incorporación de nuevos kernels

[Use el terminal](how-to-access-terminal.md#add-new-kernels) para crear y agregar nuevos kernels a la instancia de proceso. El cuaderno buscará automáticamente todos los kernels de Jupyter instalados en la instancia de proceso conectada.

Use la lista desplegable Kernel de la derecha para cambiar a cualquiera de los kernels instalados.  


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

## <a name="find-compute-details"></a>Detalles de proceso

Busque los detalles sobre las instancias de proceso en la página **Proceso** de [Studio](https://ml.azure.com).

## <a name="troubleshooting"></a>Solucionar problemas

* Si no puede conectarse a un cuaderno, asegúrese de que la comunicación de socket web **no** está deshabilitada. Para que la funcionalidad de Jupyter de instancia de proceso haga su trabajo, debe habilitarse la comunicación de socket web. Asegúrese de que la red permita las conexiones WebSocket a *.instances.azureml.net e *.instances.azureml.ms. 

* Cuando se implementa la instancia de proceso en un área de trabajo de vínculo privado, solo se puede tener acceso a ella desde dentro de la red virtual. Si usa un archivo de host o DNS personalizado, agregue una entrada para <instance-name>.<region>.instances.azureml.ms con la dirección IP privada del punto de conexión privado del área de trabajo. Para obtener más información, consulte el artículo [DNS personalizado](https://docs.microsoft.com/azure/machine-learning/how-to-custom-dns?tabs=azure-cli).
    
## <a name="next-steps"></a>Pasos siguientes

* [Ejecución de su primer experimento](tutorial-1st-experiment-sdk-train.md)
* [Copia de seguridad del almacenamiento de archivos con instantáneas](../storage/files/storage-snapshots-files.md)
* [Trabajo en entornos seguros](https://docs.microsoft.com/azure/machine-learning/how-to-secure-training-vnet#compute-instance)
