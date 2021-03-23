---
title: Uso de puntos de conexión privados para integrar Azure Functions con una red virtual
description: Tutorial paso a paso donde se explica cómo conectar una función a una red virtual de Azure y cómo bloquearla con puntos de conexión privados
ms.topic: article
ms.date: 2/22/2021
ms.openlocfilehash: a7bad58167009b4089724165813eb061996f1e6b
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102200213"
---
# <a name="tutorial-integrate-azure-functions-with-an-azure-virtual-network-using-private-endpoints"></a>Tutorial: Integración de Azure Functions con una red virtual de Azure mediante puntos de conexión privados

En este tutorial se explica cómo se utiliza Azure Functions para conectarse a recursos de una red virtual de Azure mediante puntos de conexión privados. Creará una función con una cuenta de almacenamiento bloqueada tras una red virtual que usa un desencadenador de cola de bus de servicio.

> [!div class="checklist"]
> * Creación de una aplicación de funciones en el plan Premium
> * Creación de recursos de Azure (bus de servicio, cuenta de almacenamiento, red virtual)
> * Bloqueo de la cuenta de almacenamiento tras un punto de conexión privado
> * Bloqueo del bus de servicio tras un punto de conexión privado
> * Implementación de una aplicación de funciones con desencadenadores de bus de servicio y HTTP
> * Bloqueo de la aplicación de funciones tras un punto de conexión privado
> * Prueba para comprobar si la aplicación de funciones está protegida tras la red virtual
> * Limpieza de recursos

## <a name="create-a-function-app-in-a-premium-plan"></a>Creación de una aplicación de funciones en un plan Premium

En primer lugar, creará una aplicación de funciones para .NET en el [plan Premium], ya que este tutorial usará C#. En Windows también se admiten otros lenguajes. Este plan ofrece escala sin servidor, al mismo tiempo que admite la integración de red virtual.

1. En el menú de Azure Portal o en la página **Principal**, seleccione **Crear un recurso**.

1. En la página **Nuevo**, seleccione **Compute** > **Function App**.

1. En la página **Básico**, utilice la configuración de la aplicación de funciones como se especifica en la tabla siguiente:

    | Configuración      | Valor sugerido  | Descripción |
    | ------------ | ---------------- | ----------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se creará esta nueva aplicación de función. |
    | **[Grupo de recursos](../azure-resource-manager/management/overview.md)** |  *myResourceGroup* | Nombre para el nuevo grupo de recursos en el que se va a crear la Function App. |
    | **Nombre de la aplicación de función** | Nombre único globalmente | Nombre que identifica la nueva Function App. Los caracteres válidos son `a-z` (no distingue mayúsculas de minúsculas), `0-9` y `-`.  |
    |**Publicar**| Código | Opción para publicar archivos de código o un contenedor de Docker. |
    | **Pila en tiempo de ejecución** | .NET | En este tutorial se usa .NET. |
    |**Región**| Región preferida | Elija una [región](https://azure.microsoft.com/regions/) cerca de usted o cerca de otros servicios a los que tendrán acceso las funciones. |

1. Seleccione **Siguiente: Hospedaje**. En la página **Hospedaje**, escriba la siguiente configuración:

    | Configuración      | Valor sugerido  | Descripción |
    | ------------ | ---------------- | ----------- |
    | **[Cuenta de almacenamiento](../storage/common/storage-account-create.md)** |  Nombre único globalmente |  Cree una cuenta de almacenamiento que use la aplicación de función. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente que debe cumplir los [requisitos de la cuenta de almacenamiento](./storage-considerations.md#storage-account-requirements). |
    |**Sistema operativo**| Windows | En este tutorial se usa Windows. |
    | **[Plan](./functions-scale.md)** | Premium | Plan de hospedaje que define cómo se asignan los recursos a la Function App. Seleccione **Premium**. De forma predeterminada, se crea un plan de App Service. El valor predeterminado de **SKU y tamaño** es **EP1**, donde EP son las siglas de _Elástico Premium_. Para más información, consulte la lista de [SKU Premium](./functions-premium-plan.md#available-instance-skus).<br/>Al ejecutar las funciones de JavaScript en un plan Prémium, debe elegir una instancia que tenga menos vCPU. Para más información, consulte el apartado sobre la [elección de planes Premium de un solo núcleo](./functions-reference-node.md#considerations-for-javascript-functions).  |

1. Seleccione **Siguiente: Supervisión**. En la página **Supervisión**, escriba la siguiente configuración:

    | Configuración      | Valor sugerido  | Descripción |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](./functions-monitoring.md)** | Valor predeterminado | Crea un recurso de Application Insights con el mismo *nombre de aplicación* en la región más cercana que lo admita. Si expande esta configuración, puede cambiar el valor de **Nuevo nombre de recurso**  o elegir otro valor en **Ubicación** en la [ubicación geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) para almacenar los datos. |

1. Seleccione **Revisar y crear** para revisar las selecciones de configuración de la aplicación.

1. En la página **Revisar y crear**, revise la configuración y, a continuación, seleccione **Crear** para aprovisionar e implementar la aplicación de función.

1. Seleccione el icono **Notificaciones** de la esquina superior derecha del portal y observe el mensaje **Implementación correcta**.

1. Seleccione **Ir al recurso** para ver la nueva aplicación de función. También puede seleccionar **Anclar al panel**. Dicho anclaje facilita la vuelta a este recurso de aplicación de función desde el panel.

1. ¡Enhorabuena! Ha creado correctamente la aplicación de funciones prémium.

## <a name="create-azure-resources"></a>Creación de recursos de Azure

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Para las redes virtuales se necesita una cuenta de almacenamiento distinta de la creada para la aplicación de funciones.

1. En el menú de Azure Portal o en la página **Principal**, seleccione **Crear un recurso**.

1. En la página Nuevo, busque **Cuenta de almacenamiento** y seleccione **Crear**.

1. En la pestaña **Aspectos básicos**, establezca la configuración que se especifica en la tabla siguiente. Las demás opciones se pueden dejar con los valores predeterminados:

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se han creado los recursos. | 
    | **[Grupo de recursos](../azure-resource-manager/management/overview.md)**  | myResourceGroup | Elija el grupo de recursos que ha creado con la aplicación de funciones. |
    | **Nombre** | mysecurestorage| El nombre de la cuenta de almacenamiento a la que se aplicará el punto de conexión privado. |
    | **[Región](https://azure.microsoft.com/regions/)** | myFunctionRegion | Elija la región en la que ha creado la aplicación de funciones. |

1. Seleccione **Revisar + crear**. Una vez completada la validación, seleccione **Crear**.

### <a name="create-a-service-bus"></a>Creación de un bus de servicio

1. En el menú de Azure Portal o en la página **Principal**, seleccione **Crear un recurso**.

1. En la página Nuevo, busque **Bus de servicio** y seleccione **Crear**.

1. En la pestaña **Aspectos básicos**, establezca la configuración que se especifica en la tabla siguiente. Las demás opciones se pueden dejar con los valores predeterminados:

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se han creado los recursos. |
    | **[Grupo de recursos](../azure-resource-manager/management/overview.md)**  | myResourceGroup | Elija el grupo de recursos que ha creado con la aplicación de funciones. |
    | **Nombre** | myServiceBus| El nombre del bus de servicio al que se aplicará el punto de conexión privado. |
    | **[Región](https://azure.microsoft.com/regions/)** | myFunctionRegion | Elija la región en la que ha creado la aplicación de funciones. |
    | **Plan de tarifa** | Premium | Elija este nivel para usar puntos de conexión privados con el bus de servicio. |

1. Seleccione **Revisar + crear**. Una vez completada la validación, seleccione **Crear**.

### <a name="create-a-virtual-network"></a>Creación de una red virtual

Los recursos de Azure de este tutorial se integran o se incluyen en una red virtual. Usará puntos de conexión privados para mantener el tráfico de red contenido en la red virtual.

En el tutorial se crean dos subredes:
- **default**: subred para los puntos de conexión privados. Desde esta subred se proporcionan direcciones IP privadas.
- **functions**: subred para la integración de la red virtual de Azure Functions. Esta subred se delega a la aplicación de funciones.

Ahora, cree la red virtual con la que se integra la aplicación de funciones.

1. En el menú de Azure Portal o en la página Principal, seleccione **Crear un recurso**.

1. En la página Nuevo, busque **Red virtual** y seleccione **Crear**.

1. En la pestaña **Aspectos básicos**, use la configuración de red virtual que se especifica a continuación:

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se han creado los recursos. | 
    | **[Grupo de recursos](../azure-resource-manager/management/overview.md)**  | myResourceGroup | Elija el grupo de recursos que ha creado con la aplicación de funciones. |
    | **Nombre** | myVirtualNet| El nombre de la red virtual a la que se conectará la aplicación de funciones. |
    | **[Región](https://azure.microsoft.com/regions/)** | myFunctionRegion | Elija la región en la que ha creado la aplicación de funciones. |

1. En la pestaña **Direcciones IP**, seleccione **Agregar subred**. Al agregar una subred, use la configuración que se especifica a continuación:

    :::image type="content" source="./media/functions-create-vnet/1-create-vnet-ip-address.png" alt-text="Captura de pantalla de la vista de configuración de la creación de una red virtual.":::

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Nombre de subred** | functions | El nombre de la subred a la que se conectará la aplicación de funciones. | 
    | **Intervalo de direcciones de subred** | 10.0.1.0/24 | Observe que el espacio de direcciones IPv4 de la imagen anterior es 10.0.0.0/16. Si fuese 10.1.0.0/16, el *intervalo de direcciones de subred* recomendado sería 10.1.1.0/24. |

1. Seleccione **Revisar + crear**. Una vez completada la validación, seleccione **Crear**.

## <a name="lock-down-your-storage-account-with-private-endpoints"></a>Bloqueo de la cuenta de almacenamiento con puntos de conexión privados

Los puntos de conexión privados de Azure se usan para conectarse a recursos específicos de Azure mediante una dirección IP privada. Esta conexión garantiza que el tráfico de red permanece dentro de la red virtual elegida y que el acceso solo está disponible para recursos específicos. Ahora, cree los puntos de conexión privados para Azure File Storage y Azure Blob Storage con la cuenta de almacenamiento.

1. En la nueva cuenta de almacenamiento, seleccione **Redes** en el menú de la izquierda.

1. Seleccione la pestaña **Conexiones de puntos de conexión privados** y seleccione **Punto de conexión privado**.

    :::image type="content" source="./media/functions-create-vnet/2-navigate-private-endpoint-store.png" alt-text="Captura de pantalla de la navegación para crear puntos de conexión privados para la cuenta de almacenamiento.":::

1. En la pestaña **Aspectos básicos**, use la configuración de punto de conexión privado que se especifica a continuación:

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se han creado los recursos. | 
    | **[Grupo de recursos](../azure-resource-manager/management/overview.md)**  | myResourceGroup | Elija el grupo de recursos que ha creado con la aplicación de funciones. | |
    | **Nombre** | file-endpoint | El nombre del punto de conexión privado para los archivos de la cuenta de almacenamiento. |
    | **[Región](https://azure.microsoft.com/regions/)** | myFunctionRegion | Elija la región en la que ha creado la cuenta de almacenamiento. |

1. En la pestaña **Recurso**, use la configuración de punto de conexión privado que se especifica a continuación:

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se han creado los recursos. | 
    | **Tipo de recurso**  | Microsoft.Storage/storageAccounts | Este es el tipo de recurso de las cuentas de almacenamiento. |
    | **Recurso** | mysecurestorage | La cuenta de almacenamiento que acaba de crear. |
    | **Recurso secundario de destino** | archivo | Este punto de conexión privado se usará para los archivos de la cuenta de almacenamiento. |

1. En la pestaña **Configuración**, elija **default** para la configuración de subred.

1. Seleccione **Revisar + crear**. Una vez completada la validación, seleccione **Crear**. Ahora los recursos de la red virtual pueden comunicarse con los archivos de almacenamiento.

1. Cree otro punto de conexión privado para los blobs. En la pestaña **Recursos**, use la siguiente configuración. En el resto de opciones, use la misma configuración que aplicó en los pasos de creación del punto de conexión privado para archivos que acaba de seguir.

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se han creado los recursos. | 
    | **Tipo de recurso**  | Microsoft.Storage/storageAccounts | Este es el tipo de recurso de las cuentas de almacenamiento. |
    | **Recurso** | mysecurestorage | La cuenta de almacenamiento que acaba de crear. |
    | **Recurso secundario de destino** | blob | Este punto de conexión privado se usará para los blobs de la cuenta de almacenamiento. |

## <a name="lock-down-your-service-bus-with-a-private-endpoint"></a>Bloqueo del bus de servicio con un punto de conexión privado

Ahora, cree el punto de conexión privado para la instancia de Azure Service Bus.

1. En el nuevo bus de servicio, seleccione **Redes** en el menú de la izquierda.

1. Seleccione la pestaña **Conexiones de puntos de conexión privados** y seleccione **Punto de conexión privado**.

    :::image type="content" source="./media/functions-create-vnet/3-navigate-private-endpoint-service-bus.png" alt-text="Captura de pantalla de la navegación a los puntos de conexión privados para el bus de servicio.":::

1. En la pestaña **Aspectos básicos**, use la configuración de punto de conexión privado que se especifica a continuación:

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se han creado los recursos. | 
    | **[Grupo de recursos](../azure-resource-manager/management/overview.md)**  | myResourceGroup | Elija el grupo de recursos que ha creado con la aplicación de funciones. |
    | **Nombre** | sb-endpoint | El nombre del punto de conexión privado para los archivos de la cuenta de almacenamiento. |
    | **[Región](https://azure.microsoft.com/regions/)** | myFunctionRegion | Elija la región en la que ha creado la cuenta de almacenamiento. |

1. En la pestaña **Recurso**, use la configuración de punto de conexión privado que se especifica a continuación:

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se han creado los recursos. | 
    | **Tipo de recurso**  | Microsoft.ServiceBus/namespaces | Este es el tipo de recurso del bus de servicio. |
    | **Recurso** | myServiceBus | El bus de servicio que ha creado anteriormente en el tutorial. |
    | **Subrecurso de destino** | namespace | Este punto de conexión privado se usará para el espacio de nombres del bus de servicio. |

1. En la pestaña **Configuración**, elija **default** para la configuración de subred.

1. Seleccione **Revisar + crear**. Una vez completada la validación, seleccione **Crear**. Ahora los recursos de la red virtual pueden comunicarse con el bus de servicio.

## <a name="create-a-file-share"></a>Creación de un recurso compartido de archivos

1. En la cuenta de almacenamiento que ha creado, seleccione **Recursos compartidos de archivos** en el menú de la izquierda.

1. Seleccione **+ Recursos compartidos de archivos**. Para este tutorial, indique **files** como nombre del recurso compartido de archivos.

    :::image type="content" source="./media/functions-create-vnet/4-create-file-share.png" alt-text="Captura de pantalla sobre cómo crear un recurso compartido de archivos en la cuenta de almacenamiento.":::

## <a name="get-storage-account-connection-string"></a>Cadena de conexión de la cuenta de almacenamiento

1. En la cuenta de almacenamiento que ha creado, seleccione **Claves de acceso** en el menú de la izquierda.

1. Seleccione **Mostrar claves**. Copie la cadena de conexión de key1 y guárdela. Necesitaremos esta cadena de conexión más adelante, al configurar las opciones de la aplicación.

    :::image type="content" source="./media/functions-create-vnet/5-get-store-connection-string.png" alt-text="Captura de pantalla sobre cómo obtener la cadena de conexión de una cuenta de almacenamiento.":::

## <a name="create-a-queue"></a>Creación de una cola

Esta será la cola de la que el desencadenador del bus de servicio de Azure Functions obtendrá los eventos.

1. En el bus de servicio, seleccione **Colas** en el menú de la izquierda.

1. Seleccione **Directivas de acceso compartido**. Para este tutorial, indique **queue** como nombre de la cola.

    :::image type="content" source="./media/functions-create-vnet/6-create-queue.png" alt-text="Captura de pantalla sobre cómo crear una cola de bus de servicio.":::

## <a name="get-service-bus-connection-string"></a>Obtención de la cadena de conexión del bus de servicio

1. En el bus de servicio, seleccione **Directivas de acceso compartido** en el menú de la izquierda.

1. Seleccione **RootManageSharedAccessKey**. Copie la **Cadena de conexión principal** y guárdela. Necesitaremos esta cadena de conexión más adelante, al configurar las opciones de la aplicación.

    :::image type="content" source="./media/functions-create-vnet/7-get-service-bus-connection-string.png" alt-text="Captura de pantalla sobre cómo obtener la cadena de conexión de un bus de servicio.":::

## <a name="integrate-function-app-with-your-virtual-network"></a>Integración de la aplicación de funciones con la red virtual

Para usar la aplicación de funciones con redes virtuales, deberá unirla a una subred. Usaremos una subred específica para la integración con la red virtual de Azure Functions y la subred predeterminada para todos los demás puntos de conexión privados creados en este tutorial.

1. En la aplicación de funciones, seleccione **Redes** en el menú de la izquierda.

1. En Integración con red virtual, seleccione **Haga clic aquí para configurar**.

    :::image type="content" source="./media/functions-create-vnet/8-connect-app-vnet.png" alt-text="Captura de pantalla sobre cómo navegar a la integración con redes virtuales.":::

1. Seleccione **Agregar VNET**.

1. En la hoja que se abre en **Red virtual**, seleccione la red virtual que creó anteriormente.

1. Seleccione la **Subred** que creó anteriormente llamada **functions**. Ahora la aplicación de funciones está integrada con la red virtual.

    :::image type="content" source="./media/functions-create-vnet/9-connect-app-subnet.png" alt-text="Captura de pantalla sobre cómo conectar una aplicación de funciones a una subred.":::

## <a name="configure-your-function-app-settings-for-private-endpoints"></a>Configuración de las opciones de la aplicación de funciones para puntos de conexión privados

1. En la aplicación de funciones, seleccione **Configuración** en el menú de la izquierda.

1. Para usar la aplicación de funciones con redes virtuales, debe actualizar los siguientes valores de la aplicación. Seleccione **+ Nueva configuración de aplicación** o el lápiz junto a **Editar** en la columna situada más a la derecha de la tabla de configuración de la aplicación, según corresponda. Cuando haya terminado, seleccione **Guardar**.

    :::image type="content" source="./media/functions-create-vnet/10-configure-app-settings.png" alt-text="Captura de pantalla sobre cómo configurar las opciones de la aplicación de funciones para puntos de conexión privados.":::

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **AzureWebJobsStorage** | mysecurestorageConnectionString | La cadena de conexión de la cuenta de almacenamiento que ha creado. Esta es la cadena de conexión de almacenamiento de [Cadena de conexión de la cuenta de almacenamiento](#get-storage-account-connection-string). Al cambiar esta configuración, la aplicación de funciones usará la cuenta de almacenamiento segura para las operaciones normales en tiempo de ejecución. | 
    | **WEBSITE_CONTENTAZUREFILECONNECTIONSTRING**  | mysecurestorageConnectionString | La cadena de conexión de la cuenta de almacenamiento que ha creado. Al cambiar esta configuración, la aplicación de funciones usará la cuenta de almacenamiento segura en Azure Files, que se utiliza al implementar. |
    | **WEBSITE_CONTENTSHARE** | archivos | El nombre del recurso compartido de archivos creado en la cuenta de almacenamiento. Esta configuración de la aplicación se utiliza en combinación con WEBSITE_CONTENTAZUREFILECONNECTIONSTRING. |
    | **SERVICEBUS_CONNECTION** | myServiceBusConnectionString | Cree una configuración de aplicación para la cadena de conexión del bus de servicio. Esta es la cadena de conexión de almacenamiento de [Obtención de la cadena de conexión del bus de servicio](#get-service-bus-connection-string).|
    | **WEBSITE_CONTENTOVERVNET** | 1 | Cree esta configuración de aplicación. Un valor de 1 permite escalar la aplicación de funciones cuando tiene una cuenta de almacenamiento restringida a una red virtual. Debe habilitar esta opción al restringir la cuenta de almacenamiento a una red virtual. |
    | **WEBSITE_DNS_SERVER** | 168.63.129.16 | Cree esta configuración de aplicación. Una vez que la aplicación se integra con una red virtual, usará el mismo servidor DNS que esta. Es una de las dos opciones necesarias para poder utilizar la aplicación de funciones con las zonas privadas de Azure DNS y también cuando se usan puntos de conexión privados. Estos valores enviarán todas las llamadas salientes de la aplicación a la red virtual. |
    | **WEBSITE_VNET_ROUTE_ALL** | 1 | Cree esta configuración de aplicación. Una vez que la aplicación se integra con una red virtual, usará el mismo servidor DNS que esta. Es una de las dos opciones necesarias para poder utilizar la aplicación de funciones con las zonas privadas de Azure DNS y también cuando se usan puntos de conexión privados. Estos valores enviarán todas las llamadas salientes de la aplicación a la red virtual. |

1. En la vista **Configuración**, seleccione la pestaña **Configuración del tiempo de ejecución de la función**.

1. Establezca **Supervisión de la escala en tiempo de ejecución** en **Activado** y seleccione **Guardar**. El escalado controlado por el entorno de ejecución permite conectar funciones de desencadenador no HTTP a los servicios que se ejecutan dentro de la red virtual.

    :::image type="content" source="./media/functions-create-vnet/11-enable-runtime-scaling.png" alt-text="Captura de pantalla sobre cómo habilitar el escalado controlado por el entorno de ejecución para Azure Functions.":::

## <a name="deploy-a-service-bus-trigger-and-http-trigger-to-your-function-app"></a>Implementación de un desencadenador de bus de servicio y un desencadenador HTTP en la aplicación de funciones

1. En GitHub, busque el siguiente repositorio de ejemplo, que contiene una aplicación de funciones con dos funciones, un desencadenador HTTP y un desencadenador de cola de bus de servicio.

    <https://github.com/Azure-Samples/functions-vnet-tutorial>

1. En la parte superior de la página, seleccione el botón **Bifurcar** para crear una bifurcación de este repositorio en su propia organización o cuenta de GitHub.

1. En la aplicación de funciones, seleccione **Centro de implementación** en el menú de la izquierda. A continuación, seleccione **Configuración**.

1. En la pestaña **Configuración**, use la configuración de implementación que se especifica a continuación:

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Origen** | GitHub | Debe haber creado un repositorio de GitHub con el código de ejemplo del paso 2. | 
    | **Organización**  | myOrganization | Se trata de la organización en la que se inserta el repositorio, normalmente la cuenta. |
    | **Repositorio** | myRepo | El repositorio que ha creado con el código de ejemplo. |
    | **Rama** | main (principal) | Este es el repositorio que acaba de crear, por lo que usa la rama principal. |
    | **Pila en tiempo de ejecución** | .NET | El código de ejemplo está en C#. |

1. Seleccione **Guardar**. 

    :::image type="content" source="./media/functions-create-vnet/12-deploy-portal.png" alt-text="Captura de pantalla sobre cómo implementar código de Azure Functions mediante el portal.":::

1. La implementación inicial puede tardar unos minutos. Verá un mensaje de estado **correcto (activo)** en la pestaña **Registros** cuando la aplicación se haya implementado correctamente. Si es necesario, actualice la página. 

1. ¡Enhorabuena! Ha implementado correctamente la aplicación de funciones de ejemplo.

## <a name="lock-down-your-function-app-with-a-private-endpoint"></a>Bloqueo de la aplicación de funciones con un punto de conexión privado

Ahora, cree el punto de conexión privado para la aplicación de funciones. Este punto de conexión privado conectará la aplicación de funciones de forma privada y segura a la red virtual mediante una dirección IP privada. Para obtener más información sobre los puntos de conexión privados, consulte la [documentación sobre los puntos de conexión privados](https://docs.microsoft.com/azure/private-link/private-endpoint-overview).

1. En la aplicación de funciones, seleccione **Redes** en el menú de la izquierda.

1. Seleccione **Haga clic aquí para configurar** en Conexiones de punto de conexión privado.

    :::image type="content" source="./media/functions-create-vnet/14-navigate-app-private-endpoint.png" alt-text="Captura de pantalla sobre cómo navegar a un punto de conexión privado de la aplicación de funciones.":::

1. Seleccione **Agregar**.

1. En el menú que se abre, use la configuración de punto de conexión privado tal como se especifica a continuación:

    :::image type="content" source="./media/functions-create-vnet/15-create-app-private-endpoint.png" alt-text="Captura de pantalla sobre cómo crear un punto de conexión privado de la aplicación de funciones.":::

1. Seleccione **Aceptar** para agregar el punto de conexión privado. ¡Enhorabuena! Ha protegido correctamente la aplicación de funciones, el bus de servicio y la cuenta de almacenamiento con puntos de conexión privados.

### <a name="test-your-locked-down-function-app"></a>Prueba de la aplicación de funciones bloqueada

1. En la aplicación de funciones, seleccione **Funciones** en el menú de la izquierda.

1. Seleccione **ServiceBusQueueTrigger**.

1. En el menú de la izquierda, seleccione **Supervisar**. Verá que no puede supervisar la aplicación. Esto se debe a que el explorador no tiene acceso a la red virtual, por lo que no puede acceder directamente a los recursos de la red virtual. Ahora vamos a mostrar otro método con el que puede seguir supervisando la función, Application Insights.

1. En la aplicación de funciones, seleccione **Application Insights** en el menú de la izquierda y seleccione **Ver datos de Application Insights**.

    :::image type="content" source="./media/functions-create-vnet/16-app-insights.png" alt-text="Captura de pantalla sobre cómo ver información sobre una aplicación de funciones.":::

1. Seleccione **Métricas en directo** en el menú de la izquierda.

1. Abra una nueva pestaña. En el bus de servicio, seleccione **Colas** en el menú de la izquierda.

1. Seleccione la cola.

1. Seleccione **Service Bus Explorer** en el menú de la izquierda. En **Enviar**, elija **Texto/sin formato** como **Tipo de contenido** y escriba un mensaje. 

1. Seleccione **Enviar** para enviar el mensaje.

    :::image type="content" source="./media/functions-create-vnet/17-send-service-bus-message.png" alt-text="Captura de pantalla sobre cómo enviar mensajes de Service Bus mediante el portal.":::

1. En la pestaña con **Métricas en directo** abiertas, debería ver que se ha desencadenado el desencadenador de cola del bus de servicio. Si no es así, vuelva a enviar el mensaje desde **Service Bus Explorer**.

    :::image type="content" source="./media/functions-create-vnet/18-hello-world.png" alt-text="Captura de pantalla sobre cómo ver mensajes mediante métricas en directo de las aplicaciones de funciones.":::

1. ¡Enhorabuena! Ha probado correctamente la configuración de la aplicación de funciones con puntos de conexión privados.

### <a name="private-dns-zones"></a>Zonas DNS privadas
El uso de un punto de conexión privado para conectarse a los recursos de Azure implica conectarse a una dirección IP privada en lugar de hacerlo a un punto de conexión público. Los servicios de Azure existentes están configurados para usar DNS existente para conectarse al punto de conexión público. Por tanto, deberá invalidarse la configuración de DNS para conectar al punto de conexión privado.

Se crea una zona DNS privada para cada recurso de Azure configurado con un punto de conexión privado. También se crea un registro D de DNS para cada dirección IP privada asociada con el punto de conexión privado.

En este tutorial se han creado las siguientes zonas DNS:

- privatelink.file.core.windows.net
- privatelink.blob.core.windows.net
- privatelink.servicebus.windows.net
- privatelink.azurewebsites.net

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado una aplicación de funciones prémium, una cuenta de almacenamiento y un bus de servicio, y ha protegido todos estos elementos tras los puntos de conexión privados. Obtenga más información sobre las distintas características de red disponibles a continuación:

> [!div class="nextstepaction"]
> [Más información sobre las opciones de redes en Functions](./functions-networking-options.md)

[Plan Premium]: functions-premium-plan.md
