---
title: Configuración de una función de Azure para procesar datos
titleSuffix: Azure Digital Twins
description: Consulte cómo crear una función de Azure a la que se pueda acceder y que se desencadene mediante gemelos digitales.
author: baanders
ms.author: baanders
ms.date: 8/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 6c4f23406c97d647002fbb3ab4a3544866303cf4
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98051350"
---
# <a name="connect-function-apps-in-azure-for-processing-data"></a>Configuración de aplicaciones de funciones de Azure para procesar datos

La actualización de gemelos digitales basados en los datos se controla mediante [**rutas de evento**](concepts-route-events.md) con recursos de proceso como, por ejemplo, una función creada mediante [Azure Functions](../azure-functions/functions-overview.md). Las funciones se pueden utilizar para actualizar un gemelo digital en respuesta a:
* Datos de telemetría de dispositivo procedentes de IoT Hub
* Cambio de propiedad u otros datos procedentes de otro gemelo digital dentro del grafo gemelo

Este artículo le guiará por la creación de una función de Azure para su utilización con Azure Digital Twins. 

Se trata de una introducción a los pasos que contiene:

1. Creación de un proyecto de Azure Functions en Visual Studio
2. Escritura de una función con un desencadenador de [Event Grid](../event-grid/overview.md)
3. Incorporación de código de autenticación a la función (para poder acceder a Azure Digital Twins)
4. Publicación de la aplicación de funciones en Azure
5. Configuración del acceso de [seguridad](concepts-security.md) para la aplicación de funciones

## <a name="prerequisite-set-up-azure-digital-twins-instance"></a>Requisito previo: Configuración de la instancia de Azure Digital Twins

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

## <a name="create-a-function-app-in-visual-studio"></a>Creación de una aplicación de funciones en Visual Studio

En Visual Studio 2019, seleccione _Archivo > Nuevo > Proyecto_, busque la plantilla _Azure Functions_ y seleccione _Siguiente_.

:::image type="content" source="media/how-to-create-azure-function/create-azure-function-project.png" alt-text="Visual Studio: cuadro de diálogo Nuevo proyecto":::

Especifique un nombre para la aplicación de funciones y seleccione _Crear_.

:::image type="content" source="media/how-to-create-azure-function/configure-new-project.png" alt-text="Visual Studio: configuración de un proyecto nuevo":::

Seleccione el tipo de *desencadenador de Event Grid* de la aplicación de funciones y seleccione _Crear_.

:::image type="content" source="media/how-to-create-azure-function/eventgridtrigger-function.png" alt-text="Visual Studio: cuadro de diálogo del desencadenador del proyecto de Azure Functions":::

Una vez creada la aplicación de funciones, Visual Studio tendrá un ejemplo de código rellenado automáticamente en **function.cs** en la carpeta del proyecto. Esta breve función se usa para registrar eventos.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-sample-code.png" alt-text="Visual Studio: Ventana de proyecto con código de ejemplo":::

## <a name="write-a-function-with-an-event-grid-trigger"></a>Escritura de una función con un desencadenador de Event Grid

Para escribir una función, agregue el SDK a la aplicación de funciones. La aplicación de funciones interactúa con Azure Digital Twins mediante el [SDK de Azure Digital Twins para .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true). 

Para poder usar el SDK, debe incluir los siguientes paquetes en el proyecto. Puede instalar los paquetes mediante el administrador de paquetes NuGet de Visual Studio o agregar los paquetes mediante la herramienta de línea de comandos `dotnet`. Elija cualquiera de estos métodos: 

**Opción 1. Agregue paquetes con el administrador de paquetes de Visual Studio:**
    
Para ello, haga clic con el botón derecho en el proyecto y seleccione _Administrar paquetes NuGet_ en la lista. A continuación, en la ventana que se abre, seleccione la pestaña _Examinar_ y busque los siguientes paquetes. Seleccione _Instalar_ y _acepte_ el contrato de licencia para instalar los paquetes.

* `Azure.DigitalTwins.Core`
* `Azure.Identity` 

Para que la configuración de la canalización de Azure SDK se configure correctamente para Azure Functions, también necesitará los paquetes siguientes. Repita el mismo proceso que antes para instalar todos los paquetes.

* `System.Net.Http`
* `Azure.Core.Pipeline`

**Opción 2. Agregar paquetes mediante la herramienta de línea de comandos `dotnet`:**

Como alternativa, puede usar los comandos `dotnet add` siguientes en una herramienta de línea de comandos:
```cmd/sh
dotnet add package System.Net.Http
dotnet add package Azure.Core.Pipeline
```

A continuación, agregue dos dependencias más al proyecto ya que las necesitará para trabajar con Azure Digital Twins. Puede usar los vínculos siguientes para desplazarse a los paquetes NuGet, en donde encontrará los comandos de la consola (incluida la CLI de .NET) para agregar la versión más reciente de cada paquete al proyecto.
 * [**Azure.DigitalTwins.Core**](https://www.nuget.org/packages/Azure.DigitalTwins.Core). Este es el paquete del [SDK de Azure Digital Twins para .NET](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true).
 * [**Azure.Identity**](https://www.nuget.org/packages/Azure.Identity). Esta biblioteca proporciona herramientas para facilitar la autenticación en Azure.

Después, en el Explorador de soluciones de Visual Studio, abra el archivo _function.cs_ en el que tiene el código de ejemplo y agregue las siguientes instrucciones _using_ a la función. 

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="Function_dependencies":::

## <a name="add-authentication-code-to-the-function"></a>Incorporación de código de autenticación a la función

Ahora declarará variables de nivel de clase y agregará un código de autenticación que permitirá a la función acceder a Azure Digital Twins. Agregará lo siguiente a la función en el archivo {nombre de función}.cs.

* Lea la dirección URL del servicio ADT como variable de entorno. Se recomienda leer la dirección URL del servicio en una variable de entorno, en lugar de codificarla de forma rígida en la función.

    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="ADT_service_URL":::

* Una variable estática para contener una instancia de HttpClient. HttpClient es relativamente costoso de crear y queremos evitar tener que hacerlo para cada invocación de función.

    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="HTTP_client":::

* Puede usar las credenciales de identidad administrada en Azure Functions.
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="ManagedIdentityCredential":::

* Agregue una variable local _DigitalTwinsClient_ dentro de la función para que contenga la instancia del cliente de Azure Digital Twins en el proyecto de función. *No* haga que esta variable sea estática dentro de la clase.
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="DigitalTwinsClient":::

* Agregue una comprobación nula para _adtInstanceUrl_ y ajuste la lógica de la función en un bloque try catch para detectar cualquier excepción.

Después de estos cambios, el código de la función será similar al siguiente:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs":::

## <a name="publish-the-function-app-to-azure"></a>Publicación de la aplicación de funciones en Azure

Para publicar el proyecto en una aplicación de funciones en Azure, haga clic con el botón derecho en el proyecto de función (no en la solución) en el Explorador de soluciones y elija **Publicar**.

> [!IMPORTANT] 
> La publicación de una aplicación de funciones en Azure incurrirá en cargos adicionales por su suscripción, independientemente de Azure Digital Twins.

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function.png" alt-text="Visual Studio: publicación de una función en Azure":::

Seleccione **Azure** como el destino de publicación y, a continuación, elija **Siguiente**.

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-1.png" alt-text="Visual Studio: cuadro de diálogo de publicación de funciones de Azure; seleccione Azure ":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-2.png" alt-text="Visual Studio: cuadro de diálogo de publicación de función de Azure; seleccione Aplicación de funciones de Azure (Windows) o (Linux) en función de su máquina":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-3.png" alt-text="Visual Studio: cuadro de diálogo de publicación de función; Creación de una nueva función de Azure":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-4.png" alt-text="Visual Studio: cuadro de diálogo de publicación de función; rellene los campos y seleccione Crear":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-5.png" alt-text="Visual Studio: cuadro de diálogo de publicación de función; seleccione la aplicación de funciones en la lista y, a continuación, Finalizar":::

En la página siguiente, escriba el nombre que desee para la nueva aplicación de funciones, un grupo de recursos y otros detalles.
Para que la aplicación de funciones pueda acceder a Azure Digital Twins, debe tener una identidad administrada por el sistema y disponer de permisos para acceder a la instancia de Azure Digital Twins.

A continuación, puede configurar el acceso de seguridad para la función mediante la CLI o Azure Portal. Elija cualquiera de estos métodos:

## <a name="set-up-security-access-for-the-function-app"></a>Configuración del acceso de seguridad para la aplicación de funciones
Puede configurar el acceso de seguridad para la aplicación de funciones mediante una de estas opciones:

### <a name="option-1-set-up-security-access-for-the-function-app-using-cli"></a>Opción 1: Configuración del acceso de seguridad para la aplicación de funciones mediante la CLI

El esqueleto de la función de los ejemplos anteriores requiere que se le pase un token de portador para poder autenticarse con Azure Digital Twins. Para asegurarse de que se pasa este token de portador, debe configurar [Managed Service Identity (MSI)](../active-directory/managed-identities-azure-resources/overview.md) para la aplicación de funciones. Esto solo debe realizarse una vez para cada aplicación de funciones.

Puede crear una identidad administrada por el sistema y asignar la identidad de la aplicación de funciones al rol _**Propietario de datos de Azure Digital Twins**_ para la instancia de Azure Digital Twins. Esto proporcionará el permiso de la aplicación de funciones en la instancia para realizar actividades del plano de datos. A continuación, haga que la dirección URL de la instancia de Azure Digital Twins sea accesible para la función mediante la definición de una variable de entorno.

[!INCLUDE [digital-twins-role-rename-note.md](../../includes/digital-twins-role-rename-note.md)]

Use [Azure Cloud Shell](https://shell.azure.com) para ejecutar los comandos.

Use el siguiente comando para crear la identidad administrada por el sistema. Anote el valor del campo _principalId_ de la salida.

```azurecli-interactive 
az functionapp identity assign -g <your-resource-group> -n <your-App-Service-(function-app)-name>   
```
Use el valor de _principalId_ en el siguiente comando para asignar la identidad de la aplicación de funciones al rol _Propietario de datos de Azure Digital Twins_ en la instancia de Azure Digital Twins.

```azurecli-interactive 
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Data Owner"
```
Por último, puede hacer que la dirección URL de la instancia de Azure Digital Twins sea accesible para la función si establece una variable de entorno. Para obtener más información sobre la configuración de variables de entorno, consulte [*Variables de entorno*](/sandbox/functions-recipes/environment-variables). 

> [!TIP]
> Para crear la dirección URL de la instancia de Azure Digital Twins, agregue *https://* al principio del *nombre de host* de la instancia de Azure Digital Twins. Para ver el nombre de host, junto con todas las propiedades de la instancia, puede ejecutar `az dt show --dt-name <your-Azure-Digital-Twins-instance>`.

```azurecli-interactive 
az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-hostname>"
```
### <a name="option-2-set-up-security-access-for-the-function-app-using-azure-portal"></a>Opción 2: Configuración del acceso de seguridad para la aplicación de funciones mediante Azure Portal

Una identidad administrada asignada por el sistema permite que los recursos de Azure se autentiquen en servicios en la nube (por ejemplo, Azure Key Vault) sin almacenar las credenciales en el código. Una vez habilitada, todos los permisos necesarios se pueden conceder mediante el control de acceso basado en roles de Azure. El ciclo de vida de este tipo de identidad administrada está ligado al ciclo de vida de este recurso. Además, cada recurso (por ejemplo, máquina virtual) solo puede tener una identidad administrada asignada por el sistema.

En [Azure Portal](https://portal.azure.com/), busque _aplicación de funciones_ en la barra de búsqueda con el nombre de la aplicación de funciones que creó anteriormente. Seleccione la *Aplicación de funciones* en la lista. 

:::image type="content" source="media/how-to-create-azure-function/portal-search-for-functionapp.png" alt-text="Azure Portal: buscar aplicaciones de funciones":::

En la ventana de la aplicación de funciones, seleccione _Identidad_ en la barra de navegación de la izquierda para habilitar la identidad administrada.
En la pestaña _Asignada por el sistema_, cambie el _Estado_ a Activado y _Guarde_. Verá un menú emergente para _Habilitar identidad administrada asignada por el sistema_.
Seleccione el botón _Sí_. 

:::image type="content" source="media/how-to-create-azure-function/enable-system-managed-identity.png" alt-text="Azure Portal: habilitar la identidad administrada por el sistema":::

Puede comprobar en las notificaciones que la función se ha registrado correctamente con Azure Active Directory.

:::image type="content" source="media/how-to-create-azure-function/notifications-enable-managed-identity.png" alt-text="Azure Portal: notificaciones":::

Anote también el **id. de objeto** que se muestra en la página _Identidad_ , ya que se usará en la sección siguiente.

:::image type="content" source="media/how-to-create-azure-function/object-id.png" alt-text="Copiar el id. de objeto para usarlo en el futuro":::

### <a name="assign-access-roles-using-azure-portal"></a>Asignación de roles de acceso mediante Azure Portal

Seleccione el botón _Asignaciones de roles de Azure_ para abrir la página *Asignaciones de roles de Azure*. Luego, seleccione _+ Agregar asignación de roles (versión preliminar)_ .

:::image type="content" source="media/how-to-create-azure-function/add-role-assignments.png" alt-text="Azure Portal: agregar asignación de roles":::

En la ventana _Agregar asignación de roles (versión preliminar)_ que se abre, seleccione:

* _Ámbito_: grupo de recursos
* _Suscripción_: seleccione su suscripción de Azure.
* _Grupo de recursos_: seleccione el grupo de recursos en la lista desplegable.
* _Rol_: seleccione _Propietario de datos de Azure Digital Twins_ en el menú desplegable.

Luego, presione el botón _Guardar_ para guardar los detalles.

:::image type="content" source="media/how-to-create-azure-function/add-role-assignment.png" alt-text="Azure Portal: agregar asignación de roles (versión preliminar) ":::

### <a name="configure-application-settings-using-azure-portal"></a>Configuración de las opciones de la aplicación mediante Azure Portal

Puede hacer que la dirección URL de la instancia de Azure Digital Twins sea accesible para la función si establece una variable de entorno. Para más información, consulte [*Variables de entorno*](/sandbox/functions-recipes/environment-variables). La configuración de la aplicación se expone como variables de entorno para tener acceso a la instancia de Digital Twins. 

Necesitará ADT_INSTANCE_URL para crear una configuración de aplicación.

Para obtener ADT_INSTANCE_URL, anexe **_https://_** al nombre de host de la instancia. En Azure Portal, para encontrar el nombre del host de la instancia de Digital Twins, busque su instancia en la barra de búsqueda. A continuación, seleccione _Información general_ en la barra de navegación izquierda para ver el _Nombre de host_. Para crear una opción de aplicación, copie este valor.

:::image type="content" source="media/how-to-create-azure-function/adt-hostname.png" alt-text="Azure Portal: Información general -> Copie el nombre de host para usarlo en el campo _Value_.":::

Ahora puede seguir estos pasos para crear una configuración de la aplicación:

* Busque la aplicación mediante el nombre de la aplicación de funciones en la barra de búsqueda y selecciónela en la lista.
* Seleccione _Configuración_ en la barra de navegación de la izquierda para crear una nueva configuración de la aplicación.
* En la pestaña _Configuración de la aplicación_, seleccione _+ Nueva configuración de la aplicación_

:::image type="content" source="media/how-to-create-azure-function/search-for-azure-function.png" alt-text="Azure Portal: Búsqueda de una aplicación de funciones ya existente":::

:::image type="content" source="media/how-to-create-azure-function/application-setting.png" alt-text="Azure Portal: Configuración de la aplicación":::

En la ventana que se abre, use el valor copiado anteriormente para crear una configuración de la aplicación. \
_Nombre_: URL_SERVICIO_ADT \
_Valor_: https://{nombre-de-host-de-azure-digital-twins}

Seleccione _Aceptar_ para crear una configuración de la aplicación.

:::image type="content" source="media/how-to-create-azure-function/add-application-setting.png" alt-text="Azure Portal: agregar la configuración de la aplicación.":::

Puede ver la configuración de la aplicación con el nombre de la aplicación bajo el campo _Nombre_ . A continuación, para guardar la configuración de la aplicación, seleccione el botón _Guardar_.

:::image type="content" source="media/how-to-create-azure-function/application-setting-save-details.png" alt-text="Azure Portal: visualizar la aplicación creada y reiniciarla":::

Cualquier cambio en la configuración de la aplicación requiere que se reinicie la aplicación. Seleccione _Continuar_ para reiniciar la aplicación.

:::image type="content" source="media/how-to-create-azure-function/save-application-setting.png" alt-text="Azure Portal: guardar la configuración de la aplicación":::

Para ver que la configuración de la aplicación se actualiza, seleccione el icono _Notificaciones_. Si no se crea la configuración de la aplicación, puede volver a intentar agregar una configuración de aplicación siguiendo el proceso anterior.

:::image type="content" source="media/how-to-create-azure-function/notifications-update-web-app-settings.png" alt-text="Azure Portal: notificaciones para actualizar la configuración de la aplicación":::

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha seguido los pasos para configurar una aplicación de funciones de Azure para usarla con Azure Digital Twins. A continuación, puede suscribir la función a Event Grid para escuchar en un punto de conexión. Este punto de conexión podría ser:
* Un punto de conexión de Event Grid asociado a Azure Digital Twins para procesar los mensajes procedentes del propio Azure Digital Twins (como los mensajes de cambio de propiedad, los mensajes de telemetría que genera [Digital Twins](concepts-twins-graph.md) en el grafo de gemelos o los mensajes del ciclo de vida).
* Los temas del sistema de IoT que IoT Hub usa para enviar telemetría y otros eventos de dispositivo
* Un punto de conexión de Event Grid que reciba los mensajes de otros servicios

A continuación, consulte cómo crear una función básica para ingerir datos de IoT Hub en Azure Digital Twins:
* [*Procedimiento: Ingesta de telemetría de IoT Hub*](how-to-ingest-iot-hub-data.md)
