---
title: Uso de Azure Service Bus Explorer para realizar operaciones de datos en Service Bus (versión preliminar)
description: En este artículo se proporciona información sobre cómo usar la instancia de Azure Service Bus Explorer basada en el portal para acceder a los datos de Azure Service Bus.
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: 06bc2ffffc6b467aa730e16599099bc95117dac9
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99092224"
---
# <a name="use-service-bus-explorer-to-perform-data-operations-on-service-bus-preview"></a>Uso de Azure Service Bus Explorer para realizar operaciones de datos en Service Bus (versión preliminar)

## <a name="overview"></a>Información general

Azure Service Bus permite que las aplicaciones cliente del remitente y del receptor desacoplen su lógica de negocios con el uso de la conocida semántica de punto a punto (cola) y publicación-suscripción (tema-suscripción).

Las operaciones realizadas en un espacio de nombres de Azure Service Bus son de dos tipos: 
   * Operaciones de administración: creación, actualización, eliminación de espacio de nombres de Service Bus, colas, temas y suscripciones.
   * Operaciones de datos: envío y recepción de mensajes de colas, temas y suscripciones.

Azure Service Bus Explorer amplía la funcionalidad del portal más allá de las operaciones de administración para admitir las operaciones de datos (envío, recepción, inspección) en las colas, temas y suscripciones (y sus subentidades de mensajes fallidos), directamente desde el propio Azure Portal.

> [!NOTE]
> En este artículo se resalta la funcionalidad de Azure Service Bus Explorer que reside en Azure Portal.
>
> La herramienta Azure Service Bus Explorer **no** es la herramienta de OSS propiedad de la comunidad [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).
>

## <a name="prerequisites"></a>Prerrequisitos

Para usar la herramienta Service Bus Explorer, tiene que realizar las siguientes tareas: 

- Aprovisionar un espacio de nombres de Azure Service Bus.
- Crear una cola para enviar y recibir mensajes o un tema con una suscripción para probar la funcionalidad. Para obtener información sobre cómo crear colas, temas y suscripciones, vea los siguientes artículos: 
    - [Inicio rápido: Creación de colas](service-bus-quickstart-portal.md)
    - [Inicio rápido: Creación de temas](service-bus-quickstart-topics-subscriptions-portal.md)
- Asegúrese de que es miembro de uno de estos roles en el espacio de nombres: 
    - [Propietario de los datos de Service Bus](/azure/role-based-access-control/built-in-roles#azure-service-bus-data-owner) 
    - [Colaborador](/azure/role-based-access-control/built-in-roles#contributor) 
    - [Propietario](/azure/role-based-access-control/built-in-roles#owner)


## <a name="using-the-service-bus-explorer"></a>Uso de Azure Service Bus Explorer

Para usar Azure Service Bus Explorer, debe ir al espacio de nombres de Service Bus en el que desee realizar las operaciones de envío, inspección y recepción.

Si quiere realizar operaciones en una cola, seleccione *Colas* en el menú de navegación. Si desea realizar operaciones en un tema (y en sus suscripciones relacionadas), seleccione **Temas**. 

:::image type="content" source="./media/service-bus-explorer/queue-topics-left-navigation.png"alt-text="Selección de entidad":::

Después de seleccionar **Colas** o **Temas**, seleccione la cola o el tema específico.

Seleccione **Azure Service Bus Explorer (versión preliminar)** en el menú de navegación izquierdo.

:::image type="content" source="./media/service-bus-explorer/left-navigation-menu-selected.png" alt-text="Menú de navegación izquierdo de Azure Service Bus Explorer":::

### <a name="sending-a-message-to-a-queue-or-topic"></a>Envío de un mensaje a una cola o un tema

Para enviar un mensaje a una **Cola** o un **Tema**, haga clic en la pestaña *_Enviar_* de Service Bus Explorer.

Para redactar un mensaje aquí: 

1. Establezca el *Tipo de contenido* en "Text/Plain", "Application/Xml" o "Application/Json".
2. Agregue el **contenido** del mensaje. Asegúrese de que coincida con el **tipo de contenido** establecido anteriormente.
3. Establezca las **propiedades avanzadas** (opcional): entre estas se incluyen el identificador de correlación, el identificador de mensaje, la etiqueta, ReplyTo, el período de vida (TTL) y el tiempo de puesta en cola programado (para los mensajes programados).
4. Establezca las **propiedades personalizadas**: puede ser cualquier propiedad de usuario establecida en una clave de diccionario.

Una vez redactado el mensaje, presione Enviar.

:::image type="content" source="./media/service-bus-explorer/send-experience.png" alt-text="Redacción del mensaje":::

Si la operación de envío se completa correctamente, 

* si se envía a la cola, se incrementará el contador de la métrica **Mensajes activos**.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="QueueAfterSendMetrics":::

* Si envía al tema, se incrementará el contador de la métrica **Mensajes activos** en la suscripción a la que se enrutó el mensaje.

    :::image type="content" source="./media/service-bus-explorer/topic-after-send-metrics.png" alt-text="TopicAfterSendMetrics":::

### <a name="receiving-a-message-from-a-queue"></a>Recepción de un mensaje de una cola

La función de recepción de Service Bus Explorer permite recibir un solo mensaje a la vez. La operación de recepción se realiza con el modo **ReceiveAndDelete**.

> [!IMPORTANT]
> Tenga en cuenta que la operación Recibir realizada por Service Bus Explorer es una *_recepción destructiva_*, es decir, el mensaje se quita de la cola cuando se muestra en la herramienta Service Bus Explorer.
>
> Para examinar los mensajes sin quitarlos de la cola, considere la posibilidad de usar la funcionalidad _*_Ver_*_.
>

Para recibir un mensaje de una cola (o su subcola de mensajes fallidos) 

1. Haga clic en la pestaña _*_Recibir_*_ de Service Bus Explorer.
2. Compruebe en las métricas si hay *Mensajes activos* o **Mensajes fallidos** por recibir.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="QueueAfterSendMetrics":::

3. Elija entre la *_Cola_* o la subcola _*_Mensajes fallidos_*_.

    :::image type="content" source="./media/service-bus-explorer/queue-or-deadletter.png" alt-text="QueueOrDeadletter":::

4. Haga clic en el botón _*_Recibir_*_ y después en _*_Sí_*_ para confirmar la operación "Recibir y eliminar".


Cuando la operación de recepción se realice correctamente, los detalles del mensaje se verán en la cuadrícula como se muestra a continuación. Puede seleccionar el mensaje de la cuadrícula para mostrar sus detalles.

:::image type="content" source="./media/service-bus-explorer/receive-message-from-queue-2.png" alt-text="Captura de pantalla de la ventana Colas en Azure Service Bus Explorer, con los detalles de un mensaje activo seleccionado en la cola.":::


### <a name="peeking-a-message-from-a-queue"></a>Inspección de un mensaje de una cola

Con la funcionalidad de inspección, puede usar Azure Service Bus Explorer para ver los primeros 32 mensajes de una cola o de la cola de mensajes fallidos.

1. Para inspeccionar el mensaje en una cola, haga clic en la pestaña _*_Ver_*_ de Service Bus Explorer.

    :::image type="content" source="./media/service-bus-explorer/peek-tab-selected.png" alt-text="PeekTab":::

2. Compruebe en las métricas si hay *Mensajes activos* o **Mensajes fallidos** por inspeccionar.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="QueueAfterSendMetrics":::

3. A continuación, elija entre la *_Cola_* o la subcola _*_Mensajes fallidos_*_.

    :::image type="content" source="./media/service-bus-explorer/queue-or-deadletter.png" alt-text="QueueOrDeadletter":::

4. Haga clic en el botón _*_Ver_*_. 

Una vez completada la operación de inspección, se mostrarán hasta 32 mensajes en la cuadrícula, como se muestra a continuación. Para ver los detalles de un mensaje determinado, selecciónelo en la cuadrícula. 

:::image type="content" source="./media/service-bus-explorer/peek-message-from-queue-2.png" alt-text="PeekMessageFromQueue":::

> [!NOTE]
>
> Dado que la inspección no es una operación destructiva, el mensaje *no* se quita de la cola.
>

### <a name="receiving-a-message-from-a-subscription"></a>Recepción de un mensaje de una suscripción

Al igual que con una cola, la operación *_Recibir_* se puede realizar en una suscripción (o su entidad de mensajes fallidos). Sin embargo, puesto que una suscripción reside en el contexto del tema, para llevar a cabo la operación de recepción, hay que ir a Azure Service Bus Explorer para un tema determinado.

> [!IMPORTANT]
> Tenga en cuenta que la operación Recibir realizada por Service Bus Explorer es una _*_recepción destructiva_*_, es decir, el mensaje se quita de la cola cuando se muestra en la herramienta Service Bus Explorer.
>
> Para examinar los mensajes sin quitarlos de la cola, considere la posibilidad de usar la funcionalidad _*_Ver_*_.
>

1. Haga clic en la pestaña _*_Recibir_*_ y seleccione la _*_Suscripción_*_ específica en el selector desplegable.

    :::image type="content" source="./media/service-bus-explorer/receive-subscription-tab-selected.png" alt-text="ReceiveTabSelected":::

2. Elija entre la _*_Suscripción_*_ o la subentidad _*_Mensajes fallidos_*_.

    :::image type="content" source="./media/service-bus-explorer/subscription-or-deadletter.png" alt-text="SubscriptionOrDeadletter":::

3. Haga clic en el botón _*_Recibir_*_ y después en _*_Sí_*_ para confirmar la operación "Recibir y eliminar".

Si la operación de recepción se realiza correctamente, el mensaje recibido se mostrará en la cuadrícula como se indica a continuación. Para ver los detalles del mensaje, haga clic en él.

:::image type="content" source="./media/service-bus-explorer/receive-message-from-subscription.png" alt-text="Captura de pantalla de la pestaña Recibir en Azure Service Bus Explorer, que muestra los detalles de un mensaje activo que se ha recibido.":::

### <a name="peeking-a-message-from-a-subscription"></a>Inspección de un mensaje desde una suscripción

Para simplemente examinar los mensajes de una suscripción o de su subentidad de mensajes fallidos, también se puede usar la funcionalidad _*_Ver_*_ en la suscripción.

1. Haga clic en la pestaña _*_Ver_*_ y seleccione la _*_Suscripción_*_ específica en el selector desplegable.

    :::image type="content" source="./media/service-bus-explorer/peek-subscription-tab-selected.png" alt-text="PeekTabSelected":::

2. Elija entre la _*_Suscripción_*_ o la subentidad _*_Mensajes fallidos_*_.

    :::image type="content" source="./media/service-bus-explorer/subscription-or-deadletter.png" alt-text="SubscriptionOrDeadletter":::

3. Haga clic en el botón _*_Ver_*_.

Una vez completada la operación de inspección, se mostrarán hasta 32 mensajes en la cuadrícula, como se muestra a continuación. Para ver los detalles de un mensaje determinado, selecciónelo en la cuadrícula. 

:::image type="content" source="./media/service-bus-explorer/peek-message-from-subscription.png" alt-text="PeekMessageFromSubscription":::

> [!NOTE]
>
> Dado que la inspección no es una operación destructiva, el mensaje *no* se quita de la cola.
>

## <a name="next-steps"></a>Pasos siguientes

   * Más información sobre las [colas](service-bus-queues-topics-subscriptions.md#queues) y los [temas](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) de Service Bus.
   * Más información sobre la creación de [colas de Service Bus mediante Azure Portal](service-bus-quickstart-portal.md)
   * Más información sobre la creación de [temas y suscripciones de Service Bus mediante Azure Portal](service-bus-quickstart-topics-subscriptions-portal.md)
