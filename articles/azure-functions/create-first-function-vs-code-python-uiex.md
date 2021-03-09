---
title: 'Creación de una función de Python con Visual Studio Code: Azure Functions'
description: Obtenga información sobre cómo crear una función de Python y, a continuación, publicar el proyecto local en el hospedaje sin servidor de Azure Functions con la extensión de Azure Functions en Visual Studio Code.
ms.topic: quickstart
ms.date: 11/04/2020
ms.custom: devx-track-python
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 42f07b76cefed38aad53caba9ba35c74238540fe
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102031742"
---
# <a name="quickstart-create-a-function-in-azure-with-python-using-visual-studio-code"></a>Inicio rápido: Creación de una función en Azure con Python mediante Visual Studio Code

> [!div class="op_single_selector" title1="Seleccione el lenguaje de la función: "]
> - [Python](create-first-function-vs-code-python.md)
> - [C#](create-first-function-vs-code-csharp.md)
> - [Java](create-first-function-vs-code-java.md)
> - [JavaScript](create-first-function-vs-code-node.md)
> - [PowerShell](create-first-function-vs-code-powershell.md)
> - [TypeScript](create-first-function-vs-code-typescript.md)
> - [Otro (Go/Rust)](create-first-function-vs-code-other.md)

En este artículo se usa Visual Studio Code para crear una función de Python que responda a solicitudes HTTP. Después de probar el código localmente, lo implementará en el entorno <abbr title="Un entorno informático en tiempo de ejecución en el que todos los detalles del servidor son transparentes para los desarrolladores de aplicaciones, lo que simplifica el proceso de implementación y administración de código.">sin servidor</abbr> de <abbr title="Un servicio de Azure que proporciona un entorno informático sin servidor de bajo costo dirigido a las aplicaciones.">Azure Functions</abbr>.

Este inicio rápido supone un pequeño costo en su cuenta de Azure.

También hay una [versión basada en la CLI](create-first-function-cli-python.md) de este artículo.

## <a name="1-prepare-your-environment"></a>1. Preparación del entorno

Antes de empezar, asegúrese de que cumple los siguientes requisitos:

+ Una cuenta de Azure <abbr title="El perfil que mantiene la información de facturación en relación con el uso de Azure.">account</abbr> con una suscripción activa. <abbr title="La estructura organizativa básica en la que administra los recursos de Azure, que normalmente se asocia a un individuo o departamento dentro de una organización.">subscription</abbr>. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools), versión 3.x.

+ Azure Functions (x64) admite [Python 3.8](https://www.python.org/downloads/release/python-381/), [Python 3.7](https://www.python.org/downloads/release/python-375/) y [Python 3.6](https://www.python.org/downloads/release/python-368/).

+ [Visual Studio Code](https://code.visualstudio.com/) en una de las [plataformas admitidas](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

+ La [extensión de Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) para Visual Studio Code  

+ La [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para Visual Studio Code.

<hr/>
<br/>

## <a name="2-create-your-local-project"></a>2. <a name="create-an-azure-functions-project"></a>Creación del proyecto local

1. Seleccione el icono de Azure en la <abbr title="El grupo vertical de iconos ubicados en el lado izquierdo de la ventana de Visual Studio Code.">barra de actividades</abbr>y, a continuación, en el área **Azure: Functions**, seleccione el icono **Crear proyecto...**

    ![Elija Crear un proyecto.](./media/functions-create-first-function-vs-code/create-new-project.png)

1. Elija una ubicación de directorio para el área de trabajo del proyecto y elija **Seleccionar**.

    > [!NOTE]
    > Estos pasos se han diseñado para completarse fuera de un área de trabajo. Por tanto, no seleccione una carpeta de proyecto que forme parte de un área de trabajo.

1. Escriba la siguiente información cuando se le indique:

    + **Seleccione un lenguaje para el proyecto de funciones**: Elija `Python`.

    + **Seleccione un alias de Python para crear un entorno virtual**: Elija la ubicación del intérprete de Python. Si no se muestra la ubicación, escriba la ruta de acceso completa al archivo binario de Python.  

    + **Seleccione una plantilla para la primera función del proyecto**: Elija `HTTP trigger`.

    + **Especifique un nombre de función**: Escriba `HttpExample`.

    + **Nivel de autorización**: Elija `Anonymous`, que permite que cualquier llame al punto de conexión de la función. Para obtener información sobre los niveles de autorización, consulte [Claves de autorización](functions-bindings-http-webhook-trigger.md#authorization-keys).

    + **Seleccione cómo desea que se abra el proyecto**: Elija `Add to workspace`.

<br/>
<details>
<summary><strong>¿No puede crear un proyecto de aplicación de funciones?</strong></summary>

Entre los problemas más comunes que se pueden resolver al crear un proyecto local de aplicación de funciones, se incluyen los siguientes:
* No tener instalada la extensión de Azure Functions. 
</details>

<hr/>
<br/>

## <a name="run-the-function-locally"></a>Ejecución local de la función

1. Presione <kbd>F5</kbd> para iniciar el proyecto de aplicación de funciones.

1. En el panel **Terminal**, observe el punto de conexión de la URL de la función que se ejecuta localmente.

    ![Salida de la función local de VS Code](../../includes/media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)


1. Mientras se ejecuta Core Tools, vaya al área de **Azure: Área Functions**. En **Functions**, expanda **Proyecto local** > **Functions**. Haga clic con el botón derecho (Windows) o <kbd>Ctrl</kbd> + clic en (macOS) en la función `HttpExample` y elija **Execute Function Now...** (Ejecutar la función ahora...).

    :::image type="content" source="../../includes/media/functions-run-function-test-local-vs-code/execute-function-now.png" alt-text="Ejecutar la función ahora desde Visual Studio Code":::

1. En **Enter request body** (Especificar el cuerpo de la solicitud) verá el valor del cuerpo del mensaje de solicitud de `{ "name": "Azure" }`. Presione Entrar para enviar este mensaje de solicitud a la función.  

1. Cuando la función se ejecuta localmente y devuelve una respuesta, se genera una notificación en Visual Studio Code. La información sobre la ejecución de la función se muestra en el panel **Terminal**.

1. Presione <kbd>Ctrl + C</kbd> para detener Core Tools y desconectar el depurador.

<br/>
<details>
<summary><strong>¿No puede ejecutar la función localmente?</strong></summary>

Entre los problemas más comunes que se pueden resolver al ejecutar un proyecto local de Functions, se incluyen los siguientes:
* No tener instalado el conjunto de herramientas Core Tools. 
*  Si tiene problemas de ejecución en Windows, asegúrese de que el shell de terminal predeterminado para Visual Studio Code no se haya establecido en **WSL Bash**. 
</details>

<hr/>
<br/>

## <a name="4-sign-in-to-azure"></a>4. Inicio de sesión en Azure

Para publicar la aplicación, inicie sesión en Azure. Si ya ha iniciado sesión, vaya a la sección siguiente.

1. Seleccione el icono de Azure en la barra de actividades y, a continuación, en el área **Azure: Functions**, seleccione **Iniciar sesión en Azure...** .

    ![Inicio de sesión en Azure en VS Code](../../includes/media/functions-sign-in-vs-code/functions-sign-into-azure.png)

1. Cuando se le solicite en el explorador, **elija su cuenta de Azure** e **inicie sesión** con sus credenciales.

1. Cuando haya iniciado sesión correctamente, cierre la nueva ventana del explorador y vuelva a Visual Studio Code. 

<hr/>
<br/>

## <a name="5-publish-the-project-to-azure"></a>5. Publicar el proyecto en Azure

La primera implementación de código incluye crear un recurso de función en la suscripción de Azure.

1. Seleccione el icono de Azure en la barra de actividades y después en el área **Azure: Functions**, seleccione el botón de **implementación en la aplicación de funciones**.

    ![Publicación del proyecto en Azure](../../includes/media/functions-publish-project-vscode/function-app-publish-project.png)

1. Escriba la siguiente información cuando se le indique:

    + **Select folder** (Seleccionar carpeta): Elija la carpeta que contiene la aplicación de funciones.

    + **Seleccione la suscripción**: elija la suscripción que desee usar. No se mostrará esta opción si solo tiene una suscripción.

    + **Seleccione la aplicación de funciones en Azure**: Elija `+ Create new Function App`.

    + **Escriba un nombre único global para la aplicación de funciones**: Escriba un nombre que sea válido en una ruta de acceso de la dirección URL. El nombre que escriba se valida para garantizar que sea <abbr title="El nombre debe ser único para todos los clientes de Azure de forma global. Por ejemplo, se puede usar una combinación del nombre personal o de la empresa, el nombre de la aplicación y un identificador numérico, como en contoso-bizapp-func-20.">único en Azure</abbr>. 

    + **Seleccione un entorno de ejecución**: Elija la versión de Python en la que se ha estado ejecutando localmente. Ejecute el comando `python --version` para comprobar la versión.

    + **Select a location for new resources** (Seleccionar la ubicación de los nuevos recursos): para mejorar el rendimiento, elija una [región](https://azure.microsoft.com/regions/) que se encuentre cerca de usted.

    La extensión muestra el estado de los recursos individuales a medida que se crean en Azure en el área de notificación.

    :::image type="content" source="../../includes/media/functions-publish-project-vscode/resource-notification.png" alt-text="Notificación de creación de recursos de Azure":::

1. Una vez que se haya creado la aplicación de función se mostrará una notificación y se aplicará el paquete de implementación. Seleccione **View Output** (Ver salida) para ver los resultados de creación e implementación. 

    ![Creación de la notificación completa](./media/functions-create-first-function-vs-code/function-create-notifications.png)

<br/>
<details>
<summary><strong>¿No puede publicar la función?</strong></summary>

En esta sección se han creado los recursos de Azure y se ha implementado el código local en la aplicación de funciones. Si las acciones no se realizaron correctamente, haga lo siguiente:

* Revise la salida para obtener información sobre el error. El icono de campana que aparece en la esquina inferior derecha también permite ver la salida. 
* ¿Ha realizado la publicación en una aplicación de funciones existente? Esa acción sobrescribe el contenido de la aplicación en Azure.
</details>


<br/>
<details>
<summary><strong>¿Qué recursos se crearon?</strong></summary>

Cuando se complete, se crearán los siguientes recursos de Azure en la suscripción con nombres que se basan en el nombre de la aplicación de funciones: 
* **Grupo de recursos**: un grupo de recursos es un contenedor lógico para recursos relacionados de la misma región.
* **Cuenta de Azure Storage**: un recurso de almacenamiento mantiene información de estado y de otro tipo sobre su proyecto.
* **Plan de consumo**: Un plan de consumo define el host subyacente para la aplicación de funciones sin servidor.
* **Aplicación de funciones**: una aplicación de funciones proporciona el entorno para ejecutar el código de la función y las funciones de grupo como una unidad lógica.
* **Application Insights**: Application Insights hace un seguimiento del uso de la función sin servidor.

</details>

<hr/>
<br/>

## <a name="6-run-the-function-in-azure"></a>6. Ejecución de la función en Azure

1. En la barra lateral de **Azure: Functions**, expanda la nueva aplicación de funciones.
1. Expanda **Functions**, haga clic con el botón derecho (Windows) o <kbd>Ctrl</kbd> + clic (macOS) en la función `HttpExample` y elija **Execute Function Now...** (Ejecutar la función ahora...).

    :::image type="content" source="../../includes/media/functions-vs-code-run-remote/execute-function-now.png" alt-text="Ejecutar la función ahora en Azure desde Visual Studio Code":::

1. En **Enter request body** (Especificar el cuerpo de la solicitud) verá el valor del cuerpo del mensaje de solicitud de `{ "name": "Azure" }`.

    Presione Entrar para enviar este mensaje de solicitud a la función.  

1. Cuando la función se ejecuta en Azure y devuelve una respuesta, se genera una notificación en Visual Studio Code.

## <a name="7-clean-up-resources"></a>7. Limpieza de recursos

Cuando vaya al [paso siguiente](#next-steps) y agregue un <abbr title="Una forma de asociar una función a una cola de almacenamiento, de tal manera que pueda crear mensajes en la cola."> Enlace de salida de la cola de Azure Storage</abbr> a la función, mantenga todos los recursos en su lugar, ya que se basará en lo que ha hecho hasta ahora.

De lo contrario, puede usar los pasos siguientes para eliminar la aplicación de funciones y sus recursos relacionados para evitar incurrir en costos adicionales.

[!INCLUDE [functions-cleanup-resources-vs-code-inner.md](../../includes/functions-cleanup-resources-vs-code-inner.md)]

Para más información sobre los costos de Functions, consulte [Estimación de los costos según el plan de consumo](functions-consumption-costs.md).

## <a name="next-steps"></a>Pasos siguientes

Expansión de la función mediante la incorporación de un enlace de <abbr title="Una conexión declarativa entre una función y otros recursos. Un enlace de entrada proporciona datos a la función, mientras que un enlace de salida proporciona datos de la función a otros recursos.">salida</abbr>. Este enlace escribe la cadena de la solicitud HTTP en un mensaje en una cola de Azure Queue Storage. 

> [!div class="nextstepaction"]
> [Conexión a una cola de Azure Storage](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-python)

[¿Tiene problemas? Háganoslo saber.](https://aka.ms/python-functions-qs-survey)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
