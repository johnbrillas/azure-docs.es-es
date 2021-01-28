---
author: baanders
description: Archivo de inclusión del proceso de publicación de una función de Azure desde Visual Studio
ms.service: digital-twins
ms.topic: include
ms.date: 1/21/2021
ms.author: baanders
ms.openlocfilehash: 63b393f519ad29baa05fef046ee1e8ba9e5330d8
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98701173"
---
Para publicar el proyecto en una aplicación de funciones en Azure, haga clic con el botón derecho en el proyecto en el *Explorador de soluciones* y elija **Publicar**.

> [!IMPORTANT] 
> La publicación de una aplicación de funciones en Azure incurrirá en cargos adicionales por su suscripción, independientemente de Azure Digital Twins.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-1.png" alt-text="Captura de pantalla de Visual Studio que muestra el menú de la solución de la selección con el botón derecho. La publicación se resalta en el menú.":::

En la página *Publicar* que aparece a continuación, deje el destino predeterminado **Azure** y presione *Siguiente*. 

Para seleccionar un destino concreto, elija **Azure Function App (Windows)** y presione *Siguiente*.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-2.png" alt-text="Captura de pantalla de Visual Studio: cuadro de diálogo Publicar una función de Azure. Azure Function App (Windows) está seleccionado en la página Destino específico.":::

En la página *Functions instance* (Instancia de Functions), elija su suscripción. Se rellenará un cuadro con los *grupos de recursos* de la suscripción.

Seleccione el grupo de recursos de la instancia y haga clic en *+* para crear una nueva función de Azure.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-3.png" alt-text="Captura de pantalla de Visual Studio: cuadro de diálogo Publicar una función de Azure. El botón + para crear una nueva función aparece resaltado en la página Functions instance (Instancia de Functions).":::

En la ventana *Function App (Windows) - Create new* [Aplicación de funciones (Windows): Crear nueva], rellene los siguientes campos:
* **Name** (Nombre) es el nombre del plan de consumo que Azure va a usar para hospedar la aplicación de Azure Functions. También será el nombre de la aplicación de funciones que contiene la función real. Puede elegir su propio valor o dejar el que se sugiere, que es el predeterminado.
* Asegúrese de que el valor del campo **Subscription** (Suscripción) coincida con la suscripción que desea usar. 
* Asegúrese de que el valor del campo **Resource group** (Grupos de recursos) coincida con el grupo de recursos que desea utilizar.
* En **Plan type** (Tipo de plan), deje *Consumption* (Consumo).
* En el campo **Location** (Ubicación), elija la ubicación del grupo de recursos.
* Cree un recurso de **Azure Storage** mediante el vínculo *New...* (Nuevo...). Establezca la ubicación que coincida con la del grupo de recursos, use los restantes valores predeterminados y pulse "Ok" (Aceptar).

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-4.png" alt-text="Captura de pantalla de Visual Studio: cuadro de diálogo Publicar una función de Azure. Se están rellenando los detalles de una nueva aplicación de funciones, incluidos el nombre, suscripción, grupo de recursos, tipo de plan, ubicación e instancia de Azure Storage.":::

Seleccione **Crear**.

Debería volver a la página *Functions instance* (Instancia de Functions), donde la nueva aplicación de funciones ya se ve debajo del grupo de recursos. Pulse *Finish* (Finalizar).

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-5.png" alt-text="Publicar una función de Azure en Visual Studio: instancia de Functions (después de la aplicación de funciones)":::

En el panel *Publish* (Publicar) que se abre en la ventana principal de Visual Studio, compruebe que toda la información parece correcta y seleccione **Publish** (Publicar).

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-6.png" alt-text="Captura de pantalla de Visual Studio: cuadro de diálogo Publicar una función de Azure. La nueva aplicación de funciones aparece en la lista de aplicaciones de funciones y hay un botón Finalizar.":::

> [!NOTE]
> Si ve un elemento emergente similar al siguiente: :::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-7.png" alt-text="Captura de pantalla de la ventana emergente de Visual Studio denominada Credenciales de publicación. Contiene campos para un nombre de usuario y una contraseña, y un botón para intentar recuperar las credenciales de Azure." border="false":::
> Seleccione **Attempt to retrieve credentials from Azure** (Intentar recuperar credenciales de Azure) y **Save** (Guardar).
>
> Si ve una advertencia en la que se indica que *actualice la versión de Functions en Azure* o que *la versión del entorno de ejecución de Functions no coincide con la versión que se ejecuta en Azure*:
>
> siga las indicaciones para actualizar a la versión más reciente del entorno de ejecución de Azure Functions. Este problema puede producirse si utiliza una versión anterior de Visual Studio.

Para que la aplicación de funciones pueda acceder a Azure Digital Twins, debe tener una identidad administrada por el sistema y disponer de permisos para acceder a la instancia de Azure Digital Twins. Va a realizar esa configuración a continuación.
