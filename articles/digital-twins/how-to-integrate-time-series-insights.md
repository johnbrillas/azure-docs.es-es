---
title: Integración con Azure Time Series Insights
titleSuffix: Azure Digital Twins
description: Vea cómo configurar rutas de eventos de Azure Digital Twins a Azure Time Series Insights.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 1/19/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 6aeb7489b455840eeca0a8e1967c7e6e2ed50b7a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102199907"
---
# <a name="integrate-azure-digital-twins-with-azure-time-series-insights"></a>Integración de Azure Digital Twins con Azure Time Series Insights

En este artículo, aprenderá a integrar Azure Digital Twins con [Azure Time Series Insights (TSI)](../time-series-insights/overview-what-is-tsi.md).

La solución que se describe en este artículo le permitirá recopilar y analizar datos históricos sobre la solución de IoT. Azure Digital Twins es una buena opción para alimentar de datos a Time Series Insights, ya que permite poner en correlación varios flujos de datos y estandarizar la información antes de enviarla a Time Series Insights. 

## <a name="prerequisites"></a>Requisitos previos

Antes de poder configurar una relación con Time Series Insights, debe tener una **instancia de Azure Digital Twins**. Esta instancia debe estar configurada con la posibilidad de actualizar la información del gemelo digital en función de los datos, ya que tendrá que actualizar la información del gemelo digital varias veces para ver que se ha hecho el seguimiento de los datos en Time Series Insights. 

Si todavía no tiene esta configuración, puede crearla si sigue las instrucciones sobre Azure Digital Twins que aparecen en [*Tutorial: Conexión de una solución de un extremo a otro*](./tutorial-end-to-end.md). El tutorial lo guiará a través de la configuración de una instancia de Azure Digital Twins que funciona con un dispositivo IoT virtual para desencadenar las actualizaciones de gemelos digitales.

## <a name="solution-architecture"></a>Arquitectura de la solución

Va a adjuntar Time Series Insights a Azure Digital Twins a través de la ruta de acceso siguiente.

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-integrate-time-series-insights/diagram-simple.png" alt-text="Una vista de los servicios de Azure en un escenario integral, con Time Series Insights resaltado" lightbox="media/how-to-integrate-time-series-insights/diagram-simple.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

## <a name="create-a-route-and-filter-to-twin-update-notifications"></a>Creación de una ruta y un filtro para las notificaciones de actualización de gemelos

Las instancias de Azure Digital Twins pueden emitir [eventos de actualización de gemelos](how-to-interpret-event-data.md) cada vez que se actualiza el estado de uno de estos elementos. En esta sección, creará una [**ruta de eventos**](concepts-route-events.md) de Azure Digital Twins que dirigirá estos eventos de actualización a Azure [Event Hubs](../event-hubs/event-hubs-about.md) para su posterior procesamiento.

El [*Tutorial: Conexión de una solución de un extremo a otro*](./tutorial-end-to-end.md) de Azure Digital Twins lo guía a través de un escenario en el que se usa un termómetro para actualizar un atributo de temperatura en un gemelo digital que representa una sala. Este patrón se basa en las actualizaciones de gemelos, en lugar de reenviar la telemetría desde un dispositivo IoT, lo que ofrece la flexibilidad necesaria para cambiar el origen de datos subyacente sin necesidad de actualizar la lógica de Time Series Insights.

1. Primero, cree un espacio de nombres del centro de eventos que recibirá eventos de la instancia de Azure Digital Twins. Puede usar las instrucciones de la CLI de Azure que aparecen a continuación o usar Azure Portal: [*Guía de inicio rápido: Creación de un centro de eventos mediante Azure Portal*](../event-hubs/event-hubs-create.md). Para ver qué regiones admiten Event Hubs, visite [*Productos de Azure disponibles por región*](https://azure.microsoft.com/global-infrastructure/services/?products=event-hubs).

    ```azurecli-interactive
    az eventhubs namespace create --name <name for your Event Hubs namespace> --resource-group <resource group name> -l <region>
    ```

2. Cree un centro de eventos en el espacio de nombres para recibir eventos de cambio de gemelo. Especifique un nombre para el centro de eventos.

    ```azurecli-interactive
    az eventhubs eventhub create --name <name for your Twins event hub> --resource-group <resource group name> --namespace-name <Event Hubs namespace from above>
    ```

3. Cree una [regla de autorización](/cli/azure/eventhubs/eventhub/authorization-rule#az-eventhubs-eventhub-authorization-rule-create) con permisos de envío y recepción. Especifique un nombre para la regla.

    ```azurecli-interactive
        az eventhubs eventhub authorization-rule create --rights Listen Send --resource-group <resource group name> --namespace-name <Event Hubs namespace from above> --eventhub-name <Twins event hub name from above> --name <name for your Twins auth rule>
    ```

4. Cree un [punto de conexión](concepts-route-events.md#create-an-endpoint) de Azure Digital Twins que vincule el centro de eventos con la instancia de Azure Digital Twins.

    ```azurecli-interactive
    az dt endpoint create eventhub -n <your Azure Digital Twins instance name> --endpoint-name <name for your Event Hubs endpoint> --eventhub-resource-group <resource group name> --eventhub-namespace <Event Hubs namespace from above> --eventhub <Twins event hub name from above> --eventhub-policy <Twins auth rule from above>
    ```

5. Cree una [ruta](concepts-route-events.md#create-an-event-route) en Azure Digital Twins para enviar eventos de actualización de gemelos al punto de conexión. El filtro de esta ruta permitirá que solo los mensajes de actualización de gemelos pasen al punto de conexión.

    >[!NOTE]
    >Actualmente hay un **problema conocido** en Cloud Shell que afecta a estos grupos de comandos: `az dt route`, `az dt model` y `az dt twin`.
    >
    >Para resolverlo, ejecute `az login` en Cloud Shell antes de ejecutar el comando, o bien use la [CLI local](/cli/azure/install-azure-cli) en lugar de Cloud Shell. Para obtener más información, consulte [*Solución de problemas: Problemas conocidos en Azure Digital Twins*](troubleshoot-known-issues.md#400-client-error-bad-request-in-cloud-shell).

    ```azurecli-interactive
    az dt route create -n <your Azure Digital Twins instance name> --endpoint-name <Event Hub endpoint from above> --route-name <name for your route> --filter "type = 'Microsoft.DigitalTwins.Twin.Update'"
    ```

Antes de seguir, anote el *espacio de nombres de Event Hubs* y el *grupo de recursos*, porque los volverá a usar más adelante en este artículo para crear otro centro de eventos.

## <a name="create-a-function-in-azure"></a>Crear una función en Azure

A continuación, usará Azure Functions para crear una **función desencadenada por Event Hubs** dentro de una aplicación de funciones. Puede usar la aplicación de funciones creada en el tutorial integral ([*Tutorial: Conexión de una solución de un extremo a otro*](./tutorial-end-to-end.md)) o una propia. 

Esta función convertirá esos eventos de actualización de gemelos de su forma original como documentos de revisión JSON a objetos JSON, que contienen solo valores actualizados y agregados de su gemelos.

Para más información sobre cómo usar Event Hubs con Azure Functions, consulte [*Desencadenador de Azure Event Hubs para Azure Functions*](../azure-functions/functions-bindings-event-hubs-trigger.md).

Dentro de la aplicación de funciones publicada, agregue una nueva función denominada **ProcessDTUpdatetoTSI** con el siguiente código.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/updateTSI.cs":::

>[!NOTE]
>Es posible que tenga que agregar los paquetes al proyecto mediante el comando `dotnet add package` o el administrador de paquetes NuGet de Visual Studio.

A continuación, **publique** la nueva función de Azure. Para obtener instrucciones sobre cómo hacerlo, consulte [*Procedimiento: Configuración de una función de Azure para procesar datos*](how-to-create-azure-function.md#publish-the-function-app-to-azure).

En adelante, esta función enviará los objetos JSON que crea a un segundo centro de eventos, el que se conectará a Time Series Insights. Ese centro de eventos se creará en la siguiente sección.

Más adelante, también establecerá algunas variables de entorno que esta función usará para conectarse a sus propios centros de eventos.

## <a name="send-telemetry-to-an-event-hub"></a>Envío de telemetría a un centro de eventos

Ahora creará un segundo centro de eventos y configurará la función para transmitir su salida a ese centro de eventos. Este centro de eventos se conectará a Time Series Insights.

### <a name="create-an-event-hub"></a>Creación de un centro de eventos

Para crear el segundo centro de eventos, puede usar las instrucciones de la CLI de Azure que aparecen a continuación o usar Azure Portal: [*Guía de inicio rápido: Creación de un centro de eventos mediante Azure Portal*](../event-hubs/event-hubs-create.md).

1. Prepare el *espacio de nombres de Event Hubs* y el nombre del *grupo de trabajos* que anotó anteriormente en este artículo.

2. Cree un nuevo centro de eventos. Especifique un nombre para el centro de eventos.

    ```azurecli-interactive
    az eventhubs eventhub create --name <name for your TSI event hub> --resource-group <resource group name from earlier> --namespace-name <Event Hubs namespace from earlier>
    ```
3. Cree una [regla de autorización](/cli/azure/eventhubs/eventhub/authorization-rule#az-eventhubs-eventhub-authorization-rule-create) con permisos de envío y recepción. Especifique un nombre para la regla.

    ```azurecli-interactive
    az eventhubs eventhub authorization-rule create --rights Listen Send --resource-group <resource group name> --namespace-name <Event Hubs namespace from earlier> --eventhub-name <TSI event hub name from above> --name <name for your TSI auth rule>
    ```

## <a name="configure-your-function"></a>Configuración de la función

A continuación, tendrá que establecer variables de entorno en la aplicación de funciones anterior, que contiene las cadenas de conexión para los centros de eventos que ha creado.

### <a name="set-the-twins-event-hub-connection-string"></a>Establecimiento de la cadena de conexión del centro de eventos de Twins

1. Obtenga la [cadena de conexión del centro de eventos](../event-hubs/event-hubs-get-connection-string.md) de gemelos con las reglas de autorización que creó anteriormente para el centro de Twins.

    ```azurecli-interactive
    az eventhubs eventhub authorization-rule keys list --resource-group <resource group name> --namespace-name <Event Hubs namespace> --eventhub-name <Twins event hub name from earlier> --name <Twins auth rule from earlier>
    ```

2. Use el valor de *primaryConnectionString* que obtiene del resultado para crear una configuración de aplicación en la aplicación de funciones que contiene la cadena de conexión:

    ```azurecli-interactive
    az functionapp config appsettings set --settings "EventHubAppSetting-Twins=<Twins event hub connection string>" -g <resource group> -n <your App Service (function app) name>
    ```

### <a name="set-the-time-series-insights-event-hub-connection-string"></a>Establecimiento de la cadena de conexión del centro de eventos de Time Series Insights

1. Obtenga la [cadena de conexión del centro de eventos](../event-hubs/event-hubs-get-connection-string.md) de TSI con las reglas de autorización que creó anteriormente para el centro de Time Series Insights:

    ```azurecli-interactive
    az eventhubs eventhub authorization-rule keys list --resource-group <resource group name> --namespace-name <Event Hubs namespace> --eventhub-name <TSI event hub name> --name <TSI auth rule>
    ```

2. Use el valor de *primaryConnectionString* que obtiene del resultado para crear una configuración de aplicación en la aplicación de funciones que contiene la cadena de conexión:

    ```azurecli-interactive
    az functionapp config appsettings set --settings "EventHubAppSetting-TSI=<TSI event hub connection string>" -g <resource group> -n <your App Service (function app) name>
    ```

## <a name="create-and-connect-a-time-series-insights-instance"></a>Creación y conexión de una instancia de Time Series Insights

A continuación, configurará una instancia de Time Series Insights para recibir los datos del segundo centro de eventos (TSI). Siga los pasos que aparecen a continuación y, para más detalles sobre este proceso, consulte [*Tutorial: Configuración de un entorno de pago por uso de Azure Time Series Insights Gen2*](../time-series-insights/tutorials-set-up-tsi-environment.md).

1. En Azure Portal, empiece a crear un entorno de Time Series Insights. 
    1. Seleccione el plan de tarifa **Gen2(L1)** .
    2. Tenga que elegir un **identificador de serie temporal** para este entorno. El identificador de serie temporal puede tener hasta tres valores que usará para buscar los datos en Time Series Insights. Para este tutorial, puede usar **$dtId**. Obtenga más información sobre cómo seleccionar un valor de id. en [*Procedimientos recomendados al elegir un id. de serie temporal*](../time-series-insights/how-to-select-tsid.md).
    
        :::image type="content" source="media/how-to-integrate-time-series-insights/create-time-series-insights-environment-1.png" alt-text="La experiencia de usuario del portal de creación para un entorno de Time Series Insights. Seleccione la suscripción, el grupo de recursos y la ubicación en las listas desplegables correspondientes y elija un nombre para el entorno." lightbox="media/how-to-integrate-time-series-insights/create-time-series-insights-environment-1.png":::
        
        :::image type="content" source="media/how-to-integrate-time-series-insights/create-time-series-insights-environment-2.png" alt-text="La experiencia de usuario del portal de creación para un entorno de Time Series Insights. Está seleccionado el plan de tarifa Gen2(L1) y el nombre de la propiedad de identificador de serie temporal es $dtId" lightbox="media/how-to-integrate-time-series-insights/create-time-series-insights-environment-2.png":::

2. Seleccione **Siguiente: Origen del evento** y seleccione la información del centro de eventos de TSI que se indicó anteriormente. También necesitará crear un grupo de consumidores de Event Hubs.
    
    :::image type="content" source="media/how-to-integrate-time-series-insights/event-source-twins.png" alt-text="La experiencia de usuario del portal de creación para un origen de eventos del entorno de Time Series Insights. Está creando un origen de eventos con la información del centro de eventos que se indicó anteriormente. También creará un grupo de consumidores." lightbox="media/how-to-integrate-time-series-insights/event-source-twins.png":::

## <a name="begin-sending-iot-data-to-azure-digital-twins"></a>Envío de datos de IoT a Azure Digital Twins

Para empezar a enviar datos a Time Series Insights, deberá iniciar la actualización de las propiedades de gemelos digitales en Azure Digital Twins con valores de datos variables. Use el comando [az dt twin update](/cli/azure/ext/azure-iot/dt/twin#ext-azure-iot-az-dt-twin-update).

Si usa el tutorial integral ([*Tutorial: Conexión de una solución de un extremo a otro*](tutorial-end-to-end.md)) para ayudar a configurar el entorno, puede empezar a enviar datos simulados de IoT mediante la ejecución del proyecto *DeviceSimulator* del ejemplo. Las instrucciones se encuentran en la sección [*Configuración y ejecución de la simulación*](tutorial-end-to-end.md#configure-and-run-the-simulation) del tutorial.

## <a name="visualize-your-data-in-time-series-insights"></a>Visualización de los datos en Time Series Insights

Ahora, los datos deben fluir a la instancia de Time Series Insights, listos para su análisis. Siga los pasos que se indican a continuación para explorar los datos entrantes.

1. Abra el entorno de Time Series Insights en [Azure Portal](https://portal.azure.com) (puede buscar el nombre del entorno en la barra de búsqueda del portal). Vaya a la *dirección URL del Explorador de Time Series Insights* que se muestra en la información general de la instancia.
    
    :::image type="content" source="media/how-to-integrate-time-series-insights/view-environment.png" alt-text="Selección de la dirección URL del Explorador de Time Series Insights en la pestaña de información general del entorno de Time Series Insights":::

2. En el explorador, verá los tres gemelos de Azure Digital Twins que se muestran a la izquierda. Seleccione _**thermostat67**_, **Temperature** (Temperatura) y haga clic en **Add** (Agregar).

    :::image type="content" source="media/how-to-integrate-time-series-insights/add-data.png" alt-text="Seleccione **thermostat67**, **temperature** (Temperatura) y haga clic en **add** (Agregar)":::

3. Ahora debería ver las lecturas de temperatura iniciales del termostato, como se muestra a continuación. La misma lectura de temperatura se actualiza para *room21* y *floor1* y puede visualizar esos flujos de datos de manera simultánea.
    
    :::image type="content" source="media/how-to-integrate-time-series-insights/initial-data.png" alt-text="Los datos de temperatura inicial se representan gráficamente en el explorador de TSI. Es una línea de valores aleatorios entre 68 y 85":::

4. Si permite que la simulación se ejecute durante mucho más tiempo, la visualización tendrá un aspecto similar al siguiente:
    
    :::image type="content" source="media/how-to-integrate-time-series-insights/day-data.png" alt-text="Los datos de temperatura de cada gemelo se representan gráficamente en tres líneas paralelas de colores distintos.":::

## <a name="next-steps"></a>Pasos siguientes

De forma predeterminada, los gemelos digitales se almacenan como una jerarquía plana en Time Series Insights, pero se pueden enriquecer con la información del modelo y una jerarquía de varios niveles para la organización. Para más información sobre este proceso, lea: 

* [*Tutorial: Definir y aplicar un modelo*](../time-series-insights/tutorials-set-up-tsi-environment.md#define-and-apply-a-model) 

Puede escribir la lógica personalizada para proporcionar automáticamente esta información con los datos del modelo y del gráfico ya almacenados en Azure Digital Twins. Para obtener más información sobre cómo administrar, actualizar y recuperar información del grafo de gemelos, vea las referencias siguientes:

* [*Procedimiento: Administración de un gemelo digital*](./how-to-manage-twin.md)
* [*Procedimiento: Consulta del grafo gemelo*](./how-to-query-graph.md)