---
title: 'Creación de una función de C# con Visual Studio Code: Azure Functions'
description: Obtenga información sobre cómo crear una función de C# y, a continuación, publicar el proyecto local en el hospedaje sin servidor de Azure Functions con la extensión de Azure Functions en Visual Studio Code.
ms.topic: quickstart
ms.date: 11/03/2020
ms.custom: devx-track-csharp
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 2848ce6214d59ba2732dcfc148ccaf9936497f17
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102121038"
---
# <a name="quickstart-create-a-c-function-in-azure-using-visual-studio-code"></a>Inicio rápido: Creación de una función de C# en Azure mediante Visual Studio Code

> [!div class="op_single_selector" title1="Seleccione el lenguaje de la función: "]
> - [C#](create-first-function-vs-code-csharp.md)
> - [Java](create-first-function-vs-code-java.md)
> - [JavaScript](create-first-function-vs-code-node.md)
> - [PowerShell](create-first-function-vs-code-powershell.md)
> - [Python](create-first-function-vs-code-python.md)
> - [TypeScript](create-first-function-vs-code-typescript.md)
> - [Otro (Go/Rust)](create-first-function-vs-code-other.md)

En este artículo se usa Visual Studio Code para crear una función basada en biblioteca de clases de C# que responda a solicitudes HTTP. Después de probar el código localmente, lo implementará en el entorno <abbr title="Un entorno informático en tiempo de ejecución en el que todos los detalles del servidor son transparentes para los desarrolladores de aplicaciones. De este modo, se simplifica el proceso de implementación y administración de código.">sin servidor</abbr> de <abbr title="Un servicio de Azure que proporciona un entorno informático sin servidor y de bajo costo dirigido a las aplicaciones.">Azure Functions</abbr>.

Este inicio rápido supone un pequeño costo en su cuenta de Azure.

También hay una [versión basada en la CLI](create-first-function-cli-csharp.md) de este artículo.
    
## <a name="1-configure-your-environment"></a>1. Configurar su entorno

Antes de empezar, asegúrese de que cumple los siguientes requisitos:

+ Una cuenta de Azure <abbr title="El perfil que mantiene la información de facturación en relación con el uso de Azure.">account</abbr> con una suscripción activa. <abbr title="La estructura organizativa básica en la que administra los recursos de Azure, que normalmente se asocia a un individuo o departamento dentro de una organización.">subscription</abbr>. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools), versión 3.x.

+ [Visual Studio Code](https://code.visualstudio.com/) en una de las [plataformas admitidas](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

+ La [extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) para Visual Studio Code.  

+ La [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para Visual Studio Code.

## <a name="2-create-your-local-project"></a><a name="create-an-azure-functions-project"></a>2. Creación del proyecto local

En esta sección se usa Visual Studio Code para crear un <abbr title="Un contenedor lógico para una o varias funciones individuales que se pueden implementar y administrar juntas.">local de Azure Functions</abbr> en C#. Más adelante en este artículo, publicará el código de función en Azure.

1. Seleccione el icono de Azure en la barra de actividades. <abbr title="El grupo vertical de iconos ubicados en el lado izquierdo de la ventana de Visual Studio Code.">barra de actividades</abbr>y, a continuación, en el área **Azure: Functions**, seleccione el icono **Crear proyecto...**

    ![Elija Crear un proyecto.](./media/functions-create-first-function-vs-code/create-new-project.png)

1. Elija una ubicación de directorio para el área de trabajo del proyecto y elija **Seleccionar**.

    > [!NOTE]
    > Estos pasos se han diseñado para completarse fuera de un área de trabajo. Por tanto, no seleccione una carpeta de proyecto que forme parte de un área de trabajo.

1. Escriba la siguiente información cuando se le indique:

    + **Seleccione un lenguaje para el proyecto de funciones**: Elija `C#`.

    + **Seleccione una plantilla para la primera función del proyecto**: Elija `HTTP trigger`.

    + **Especifique un nombre de función**: Escriba `HttpExample`.

    + **Especifique un espacio de nombres**: Escriba `My.Functions`.

    + **Nivel de autorización**: Elija `Anonymous`, que permite que cualquier llame al punto de conexión de la función. Para obtener información sobre los niveles de autorización, consulte [Claves de autorización](functions-bindings-http-webhook-trigger.md#authorization-keys).

    + **Seleccione cómo desea que se abra el proyecto**: Elija `Add to workspace`.

1. Con ayuda de esta información, Visual Studio Code genera un proyecto de Azure Functions con un desencadenador HTTP. <abbr title="El tipo de evento que invoca el código de la función, como una solicitud HTTP, un mensaje de cola o una hora específica.">desencadenador</abbr>. Los archivos del proyecto locales se pueden ver en Explorer. Para obtener más información sobre los archivos que se crean, consulte [Archivos del proyecto generados](functions-develop-vs-code.md#generated-project-files).

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Después de comprobar que la función se ejecuta correctamente en el equipo local, es el momento de usar Visual Studio Code para publicar el proyecto directamente en Azure.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

## <a name="5-publish-the-project-to-azure"></a>5. Publicar el proyecto en Azure

En esta sección, va a crear una aplicación de funciones y los recursos relacionados en su suscripción de Azure y, después, va a implementar el código. 

> [!IMPORTANT]
> La publicación en una aplicación de función existente sobrescribe el contenido de esa aplicación en Azure. 

1. Seleccione el icono de Azure en la barra de actividades y después en el área **Azure: Functions**, seleccione el botón de **implementación en la aplicación de funciones**.

    

1. Escriba la siguiente información cuando se le indique:

    + **Seleccione la carpeta**: elija una carpeta en el área de trabajo o busque una que contenga la aplicación de funciones. No verá esta opción si ya tiene una aplicación de funciones válida abierta.

    + **Seleccione la suscripción**: elija la suscripción que desee usar. No se mostrará esta opción si solo tiene una suscripción.

    + **Seleccione la aplicación de funciones en Azure**: Elija `+ Create new Function App`. (No elija la opción `Advanced`, que no se trata en este artículo).

    + **Escriba un nombre único global para la aplicación de funciones**: Escriba un nombre que sea válido en una ruta de acceso de la dirección URL. El nombre que escriba se valida para garantizar que sea <abbr title="El nombre debe ser único en todos los proyectos de Functions utilizados por todos los clientes de Azure de forma global. Normalmente, se usa una combinación del nombre personal o de la empresa, el nombre de la aplicación y un identificador numérico, como en contoso-bizapp-func-20.">único en Azure Functions</abbr>. 

    + **Seleccione una ubicación para los nuevos recursos**:  Para mejorar el rendimiento, elija una [región](https://azure.microsoft.com/regions/) cerca de usted.

    La extensión muestra el estado de los recursos individuales a medida que se crean en Azure en el área de notificación.

    :::image type="content" source="../../includes/media/functions-publish-project-vscode/resource-notification.png" alt-text="Notificación de creación de recursos de Azure":::

1. Cuando se complete, se crearán los siguientes recursos de Azure en la suscripción con nombres que se basan en el nombre de la aplicación de funciones:

    + Un **grupo de recursos**, que es un contenedor lógico de recursos relacionados.
    + Una **cuenta de Azure Storage** estándar, que mantiene información de estado y de otro tipo sobre los proyectos.
    + Un **plan de consumo**, que define el host subyacente para su aplicación de funciones sin servidor. 
    + Una **aplicación de funciones**, que proporciona el entorno para ejecutar el código de función. Una aplicación de funciones permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos en el mismo plan de hospedaje.
    + Una **instancia de Application Insights** conectada a la aplicación de funciones, que hace un seguimiento del uso de la función sin servidor.

    Una vez que se haya creado la aplicación de función se mostrará una notificación y se aplicará el paquete de implementación. 

    > [!TIP]
    > De forma predeterminada, los recursos de Azure que requiere la aplicación de funciones se crean según el nombre de la aplicación de funciones que proporcione. De forma predeterminada, también se crean en el mismo grupo de recursos nuevo con la aplicación de funciones. Si quiere personalizar los nombres de estos recursos o reutilizar los recursos existentes, debe [publicar el proyecto con opciones de creación avanzadas](functions-develop-vs-code.md#enable-publishing-with-advanced-create-options).


1. Seleccione **View Output** (Ver salida) en esta notificación para ver la creación y los resultados de la implementación, incluidos los recursos de Azure que ha creado. Si se pierde la notificación, seleccione el icono de campana en la esquina inferior derecha para verlo de nuevo.

    ![Creación de la notificación completa](./media/functions-create-first-function-vs-code/function-create-notifications.png)

## <a name="6-run-the-function-in-azure"></a>6. Ejecución de la función en Azure

1. Vuelva al área **Azure: Functions** en la barra lateral y expanda su suscripción, la nueva aplicación de funciones y **Functions**. Haga clic con el botón derecho (Windows) o <kbd>Ctrl</kbd> + clic en (macOS) en la función `HttpExample` y elija **Execute Function Now...** (Ejecutar la función ahora...).

    :::image type="content" source="../../includes/media/functions-vs-code-run-remote/execute-function-now.png" alt-text="Ejecutar la función ahora en Azure desde Visual Studio Code":::

1. En **Enter request body** (Especificar el cuerpo de la solicitud) verá el valor del cuerpo del mensaje de solicitud de `{ "name": "Azure" }`.

    Presione Entrar para enviar este mensaje de solicitud a la función.  

1. Cuando la función se ejecuta en Azure y devuelve una respuesta, se genera una notificación en Visual Studio Code.

## <a name="5-clean-up-resources"></a>5. Limpieza de recursos

Cuando vaya al [paso siguiente](#next-steps) y agregue un <abbr title="Una forma de asociar una función a una cola de almacenamiento, de tal manera que pueda crear mensajes en la cola."> Enlace de salida de la cola de Azure Storage</abbr> a la función, mantenga todos los recursos en su lugar, ya que se basará en lo que ha hecho hasta ahora.

De lo contrario, puede usar los pasos siguientes para eliminar la aplicación de funciones y sus recursos relacionados para evitar incurrir en costos adicionales.

[!INCLUDE [functions-cleanup-resources-vs-code-inner.md](../../includes/functions-cleanup-resources-vs-code-inner.md)]

Para más información sobre los costos de Functions, consulte [Estimación de los costos según el plan de consumo](functions-consumption-costs.md).

## <a name="next-steps"></a>Pasos siguientes

Ha usado Visual Studio Code para crear una aplicación de función con una función simple desencadenada por HTTP. En el siguiente artículo, esa función se expande al incorporar un enlace de salida. <abbr title="Una conexión declarativa entre una función y otros recursos. Un enlace de entrada proporciona datos a la función, mientras que un enlace de salida proporciona datos de la función a otros recursos.">binding</abbr>. Este enlace escribe la cadena de la solicitud HTTP en un mensaje en una cola de Azure Queue Storage. 

> [!div class="nextstepaction"]
> [Conexión a una cola de Azure Storage](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-csharp)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
