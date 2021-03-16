---
title: Configuración de una función de Azure para procesar datos
titleSuffix: Azure Digital Twins
description: Consulte cómo crear una función de Azure a la que se pueda acceder y que se desencadene mediante gemelos digitales.
author: baanders
ms.author: baanders
ms.date: 8/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 4889744347b72603a0f6318f981bc2db4906b835
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102433546"
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

En Visual Studio 2019, seleccione _Archivo > Nuevo > Proyecto_, busque la plantilla _Azure Functions_. Seleccione _Next_ (Siguiente).

:::image type="content" source="media/how-to-create-azure-function/create-azure-function-project.png" alt-text="Captura de pantalla de Visual Studio que muestra el cuadro de diálogo Nuevo proyecto. La plantilla de proyecto de Azure Functions está resaltada.":::

Especifique un nombre para la aplicación de funciones y seleccione _Crear_.

:::image type="content" source="media/how-to-create-azure-function/configure-new-project.png" alt-text="Captura de pantalla de Visual Studio que muestra el cuadro de diálogo para configurar un nuevo proyecto, incluido el nombre del proyecto, la ubicación de almacenamiento, la opción para crear una nueva solución y el nombre de la solución.":::

Seleccione el tipo de aplicación de funciones de *Desencadenador de Event Grid* y seleccione _Crear_.

:::image type="content" source="media/how-to-create-azure-function/event-grid-trigger-function.png" alt-text="Captura de pantalla de Visual Studio que muestra el cuadro de diálogo para crear una nueva aplicación de Azure Functions. La opción Desencadenador de Event Grid está resaltada.":::

Una vez creada la aplicación de funciones, Visual Studio generará un código de ejemplo en un archivo **Function1.cs** en la carpeta del proyecto. Esta breve función se usa para registrar eventos.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-sample-code.png" alt-text="Captura de pantalla de Visual Studio en la ventana de proyecto del nuevo proyecto que se ha creado. Hay código para una función de ejemplo denominada Function1." lightbox="media/how-to-create-azure-function/visual-studio-sample-code.png":::

## <a name="write-a-function-with-an-event-grid-trigger"></a>Escritura de una función con un desencadenador de Event Grid

Para escribir una función, agregue el SDK a la aplicación de funciones. La aplicación de funciones interactúa con Azure Digital Twins mediante el [SDK de Azure Digital Twins para .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client). 

Para poder usar el SDK, debe incluir los siguientes paquetes en el proyecto. Puede instalar los paquetes mediante el administrador de paquetes NuGet de Visual Studio o agregar los paquetes mediante `dotnet` en una herramienta de línea de comandos.

* [Azure.DigitalTwins.Core](https://www.nuget.org/packages/Azure.DigitalTwins.Core/)
* [Azure.Identity](https://www.nuget.org/packages/Azure.Identity/)
* [System.Net.Http](https://www.nuget.org/packages/System.Net.Http/)
* [Azure.Core](https://www.nuget.org/packages/Azure.Core/)

Después, en el Explorador de soluciones de Visual Studio, abra el archivo _Function1.cs_ en el que tiene el código de ejemplo y agregue las siguientes instrucciones `using` para estos paquetes a la función. 

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="Function_dependencies":::

## <a name="add-authentication-code-to-the-function"></a>Incorporación de código de autenticación a la función

Ahora declarará variables de nivel de clase y agregará un código de autenticación que permitirá a la función acceder a Azure Digital Twins. Agregará lo siguiente a la función en el archivo _Function1.cs_.

* Código para leer la dirección URL del servicio Azure Digital Twins como una **variable de entorno**. Se recomienda leer la dirección URL del servicio en una variable de entorno, en lugar de codificarla de forma rígida en la función. Establecerá el valor de esta variable de entorno [más adelante en este artículo](#set-up-security-access-for-the-function-app). Para más información sobre las variables de entorno, consulte [*Administración de la aplicación de funciones*](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal).

    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="ADT_service_URL":::

* Una variable estática para contener una instancia de HttpClient. HttpClient es relativamente costoso de crear y queremos evitar tener que hacerlo para cada invocación de función.

    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="HTTP_client":::

* Puede usar las credenciales de identidad administrada en Azure Functions.
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="ManagedIdentityCredential":::

* Agregue una variable local _DigitalTwinsClient_ dentro de la función para que contenga la instancia del cliente de Azure Digital Twins. *No* haga que esta variable sea estática dentro de la clase.
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="DigitalTwinsClient":::

* Agregue una comprobación nula para _adtInstanceUrl_ y ajuste la lógica de la función en un bloque try/catch para detectar cualquier excepción.

Después de estos cambios, el código de la función será similar al siguiente:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs":::

Ahora que la aplicación está escrita, puede publicarla en Azure siguiendo los pasos descritos en la sección a continuación.

## <a name="publish-the-function-app-to-azure"></a>Publicación de la aplicación de funciones en Azure

[!INCLUDE [digital-twins-publish-azure-function.md](../../includes/digital-twins-publish-azure-function.md)]

## <a name="set-up-security-access-for-the-function-app"></a>Configuración del acceso de seguridad para la aplicación de funciones

Puede configurar el acceso de seguridad para la aplicación de funciones mediante la CLI de Azure o Azure Portal. Siga los pasos para su opción preferida a continuación.

# <a name="cli"></a>[CLI](#tab/cli)

Puede ejecutar estos comandos en [Azure Cloud Shell](https://shell.azure.com) o en una [instalación local de la CLI de Azure](/cli/azure/install-azure-cli).

### <a name="assign-access-role"></a>Asignación de roles de acceso

El esqueleto de la función de los ejemplos anteriores requiere que se le pase un token de portador para poder autenticarse con Azure Digital Twins. Para asegurarse de que se pasa este token de portador, debe configurar permisos a la [identidad de servicio administrada (MSI)](../active-directory/managed-identities-azure-resources/overview.md) para que la aplicación de funciones pueda acceder a Azure Digital Twins. Esto solo debe realizarse una vez para cada aplicación de funciones.

Puede usar la identidad administrada por el sistema de la aplicación de funciones para asignarle el rol _**Propietario de datos de Azure Digital Twins**_ para la instancia de Azure Digital Twins. Esto proporcionará el permiso de la aplicación de funciones en la instancia para realizar actividades del plano de datos. A continuación, haga que la dirección URL de la instancia de Azure Digital Twins sea accesible para la función mediante la definición de una variable de entorno.

1. Use el siguiente comando para ver los detalles de la identidad administrada por el sistema de la función. Anote el valor del campo _principalId_ de la salida.

    ```azurecli-interactive 
    az functionapp identity show -g <your-resource-group> -n <your-App-Service-(function-app)-name> 
    ```

    >[!NOTE]
    > Si el resultado está vacío en lugar de mostrar los detalles de una identidad, cree una nueva identidad administrada por el sistema para la función mediante este comando:
    > 
    >```azurecli-interactive    
    >az functionapp identity assign -g <your-resource-group> -n <your-App-Service-(function-app)-name>  
    >```
    >
    > La salida mostrará los detalles de la identidad, incluido el valor de _principalId_ requerido para el siguiente paso. 

1. Use el valor de _principalId_ en el siguiente comando para asignar la identidad de la aplicación de funciones al rol _Propietario de datos de Azure Digital Twins_ en la instancia de Azure Digital Twins.

    ```azurecli-interactive 
    az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Data Owner"
    ```

### <a name="configure-application-settings"></a>Configuración de la aplicación

Por último, haga que la dirección URL de la instancia de Azure Digital Twins sea accesible para la función mediante el establecimiento de una **variable de entorno** para ella. Para más información sobre las variables de entorno, consulte [*Administración de la aplicación de funciones*](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal). 

> [!TIP]
> Para crear la dirección URL de la instancia de Azure Digital Twins, agregue *https://* al principio del *nombre de host* de la instancia de Azure Digital Twins. Para ver el nombre de host, junto con todas las propiedades de la instancia, puede ejecutar `az dt show --dt-name <your-Azure-Digital-Twins-instance>`.

```azurecli-interactive 
az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-host-name>"
```

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Realice los pasos siguientes en [Azure Portal](https://portal.azure.com/).

### <a name="assign-access-role"></a>Asignación de roles de acceso

Una identidad administrada asignada por el sistema permite que los recursos de Azure se autentiquen en servicios en la nube (por ejemplo, Azure Key Vault) sin almacenar las credenciales en el código. Una vez habilitada, todos los permisos necesarios se pueden conceder mediante el control de acceso basado en roles de Azure. El ciclo de vida de este tipo de identidad administrada está ligado al ciclo de vida de este recurso. Además, cada recurso solo puede tener una identidad administrada asignada por el sistema.

1. Busque la aplicación de funciones en [Azure Portal](https://portal.azure.com/); para ello, escriba su nombre en la barra de búsqueda. Seleccione la aplicación en los resultados. 

    :::image type="content" source="media/how-to-create-azure-function/portal-search-for-function-app.png" alt-text="Captura de pantalla de Azure Portal: Se está buscando el nombre de la aplicación de funciones en la barra de búsqueda del portal y el resultado de la búsqueda está resaltado.":::

1. En la página de la aplicación de funciones, seleccione _Identidad_ en la barra de navegación de la izquierda para trabajar con una identidad administrada para la función. En la página _Asignado por el sistema_, compruebe que el _Estado_ está establecido en **Activado** (si no es así, establézcalo ahora y *guarde* el cambio).

    :::image type="content" source="media/how-to-create-azure-function/verify-system-managed-identity.png" alt-text="Captura de pantalla de Azure Portal: en la página Identidad de la aplicación de funciones, la opción Estado está establecida en Activado." lightbox="media/how-to-create-azure-function/verify-system-managed-identity.png":::

1. Seleccione el botón _Asignaciones de roles de Azure_ para abrir la página *Asignaciones de roles de Azure*.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-1.png" alt-text="Captura de pantalla de Azure Portal: Un resaltado alrededor del botón Asignaciones de roles de Azure en Permisos en la página Identidad de Azure Functions." lightbox="media/how-to-create-azure-function/add-role-assignment-1.png":::

    Seleccione _+ Agregar asignación de roles (versión preliminar)_ .

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-2.png" alt-text="Captura de pantalla de Azure Portal con un resaltado alrededor de + Agregar asignación de roles (versión preliminar) en la página Asignaciones de roles de Azure." lightbox="media/how-to-create-azure-function/add-role-assignment-2.png":::

1. En la página _Agregar asignación de roles (versión preliminar)_ que se abre, seleccione los siguientes valores:

    * **Ámbito**: grupo de recursos
    * **Suscripción**: seleccione la suscripción de Azure.
    * **Grupo de recursos**: seleccione el grupo de recursos en la lista desplegable.
    * **Rol**: seleccione _Propietario de datos de Azure Digital Twins_ en la lista desplegable.

    Luego, presione el botón _Guardar_ para guardar los detalles.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-3.png" alt-text="Captura de pantalla de Azure Portal: Cuadro de diálogo para agregar una nueva asignación de roles (versión preliminar). Hay campos para Ámbito, Suscripción, Grupo de recursos y Rol.":::

### <a name="configure-application-settings"></a>Configuración de la aplicación

Para hacer que la dirección URL de la instancia de Azure Digital Twins sea accesible para la función, establezca una **variable de entorno** para ella. Para más información sobre las variables de entorno, consulte [*Administración de la aplicación de funciones*](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal). La configuración de la aplicación se expone como variables de entorno para acceder a la instancia de Digital Twins. 

Para establecer una variable de entorno con la dirección URL de la instancia, obtenga primero la dirección URL buscando el nombre de host de la instancia de Azure Digital Twins. Busque la instancia en la barra de búsqueda de [Azure Portal](https://portal.azure.com). A continuación, seleccione _Información general_ en la barra de navegación izquierda para ver el _Nombre de host_. Copie este valor.

:::image type="content" source="media/how-to-create-azure-function/instance-host-name.png" alt-text="Captura de pantalla de Azure Portal: En la página Información general de la instancia de Azure Digital Twins, el valor Nombre de host está resaltado.":::

Ahora puede seguir estos pasos para crear una configuración de la aplicación:

1. Busque la aplicación de funciones en la barra de búsqueda del portal y selecciónela en los resultados.

    :::image type="content" source="media/how-to-create-azure-function/portal-search-for-function-app.png" alt-text="Captura de pantalla de Azure Portal: Se está buscando el nombre de la aplicación de funciones en la barra de búsqueda del portal y el resultado de la búsqueda está resaltado.":::

1. Seleccione _Configuración_ en la barra de navegación de la izquierda. En la pestaña _Configuración de la aplicación_, seleccione _+ Nueva configuración de la aplicación_.

    :::image type="content" source="media/how-to-create-azure-function/application-setting.png" alt-text="Captura de pantalla de Azure Portal: En la página Configuración de la aplicación de funciones, el botón para crear una nueva configuración de aplicación está resaltado.":::

1. En la ventana que se abre, use el valor de nombre de host copiado anteriormente para crear una configuración de la aplicación.
    * **Nombre**: ADT_SERVICE_URL
    * **Valor**: https://{nombre-de-host-de-azure-digital-twins}
    
    Seleccione _Aceptar_ para crear una configuración de la aplicación.
    
    :::image type="content" source="media/how-to-create-azure-function/add-application-setting.png" alt-text="Captura de pantalla de Azure Portal: El botón Aceptar se resalta después de rellenar los campos Nombre y Valor en la página Agregar o editar la configuración de la aplicación.":::

1. Después de crear la configuración, debe aparecer en la pestaña _Configuración de la aplicación_. Compruebe que *ADT_SERVICE_URL* aparece en la lista y, a continuación, guarde la nueva configuración de la aplicación; para ello, seleccione _Guardar_.

    :::image type="content" source="media/how-to-create-azure-function/application-setting-save-details.png" alt-text="Captura de pantalla de Azure Portal: La página Configuración de la aplicación, con la nueva opción ADT_SERVICE_URL resaltada. El botón Guardar también está resaltado.":::

1. Cualquier cambio en la configuración de la aplicación requiere un reinicio de la aplicación para que surta efecto; por lo tanto, seleccione _Continuar_ para reiniciar la aplicación cuando se le solicite.

    :::image type="content" source="media/how-to-create-azure-function/save-application-setting.png" alt-text="Captura de pantalla de Azure Portal: Hay un aviso de que los cambios realizados en la configuración de la aplicación reiniciarán la aplicación. El botón Continuar está resaltado.":::

---

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha seguido los pasos para configurar una aplicación de funciones de Azure para usarla con Azure Digital Twins.

A continuación, consulte cómo crear una función básica para ingerir datos de IoT Hub en Azure Digital Twins:
* [*Procedimiento: Ingesta de telemetría de IoT Hub*](how-to-ingest-iot-hub-data.md)
