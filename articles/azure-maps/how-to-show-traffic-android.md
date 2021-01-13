---
title: Visualización de los datos de tráfico en mapas de Android | Microsoft Azure Maps
description: En este artículo aprenderá cómo mostrar los datos de tráfico en un mapa mediante el Android SDK de Microsoft Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 12/04/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 113f39ac2976b870c9e07851cdd0919e2578940f
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97680465"
---
# <a name="show-traffic-data-on-the-map-android-sdk"></a>Visualización de datos de tráfico en el mapa (Android SDK)

Los datos de flujo y los datos de incidentes son los dos tipos de datos de tráfico que se pueden mostrar en el mapa. En esta guía se muestra cómo visualizar ambos tipos de datos de tráfico. Los datos de incidentes se componen de datos basados en puntos y líneas para aspectos como construcciones, cierres de carreteras y accidentes. Los datos de flujo proporcionan métricas sobre el flujo de tráfico en la ruta.

## <a name="prerequisites"></a>Prerrequisitos

Asegúrese de completar los pasos descritos en el documento [Inicio rápido: Creación de una aplicación de Android](quick-android-map.md). Los bloques de código de este artículo se pueden insertar en el controlador de eventos `onReady` de mapas.

## <a name="show-traffic-on-the-map"></a>Visualización del tráfico en el mapa

Hay dos tipos de datos de tráfico disponibles en Azure Maps:

- Datos de incidentes, que se componen de datos basados en puntos y líneas para aspectos como construcción, cierres de carreteras y accidentes.
- Datos de flujo, que proporcionan métricas sobre el flujo de tráfico en las carreteras. A menudo, los datos de flujo de tráfico se utilizan para colorear las carreteras. Los colores se basan en la cantidad de tráfico que ralentiza el flujo en relación con el límite de velocidad u otra métrica. Hay cuatro valores que se pueden pasar a la opción `flow` de tráfico del mapa.

    |Valor de flujo | Descripción|
    | :-- | :-- |
    | TrafficFlow.NONE | No muestra los datos de tráfico en el mapa. |
    | TrafficFlow.RELATIVE | Muestra los datos de tráfico relativos a la velocidad de flujo libre de la carretera. |
    | TrafficFlow.RELATIVE_DELAY | Muestra las áreas que son más lentas que el retraso promedio esperado. |
    | TrafficFlow.ABSOLUTE | Muestra la velocidad absoluta de todos los vehículos en la carretera. |

En el código siguiente se muestra cómo mostrar los datos de tráfico en el mapa.

```java
//Show traffic on the map using the traffic options.
map.setTraffic(
    incidents(true),
    flow(TrafficFlow.RELATIVE)
);
```

En la siguiente captura de pantalla se muestra cómo el código anterior representa información de tráfico en tiempo real en el mapa.

![Mapa que muestra información de tráfico en tiempo real](media/how-to-show-traffic-android/android-show-traffic.png)

## <a name="get-traffic-incident-details"></a>Obtención de detalles sobre incidentes de tráfico

Los detalles sobre un incidente de tráfico están disponibles en las propiedades de la característica que se usa para mostrar el incidente en el mapa. Los incidentes de tráfico se agregan al mapa mediante el servicio de mosaico vectorial de incidentes de tráfico de Azure Maps. El formato de los datos de estos mosaicos vectoriales se [documenta aquí](https://developer.tomtom.com/traffic-api/traffic-api-documentation-traffic-incidents/vector-incident-tiles). El siguiente código agrega un evento Click al mapa, recupera la característica de incidente de tráfico donde se hizo clic y muestra un mensaje de notificación con algunos de los detalles.

```java
//Show traffic information on the map.
map.setTraffic(
    incidents(true),
    flow(TrafficFlow.RELATIVE)
);

//Add a click event to the map.
map.events.add((OnFeatureClick) (features) -> {

    if (features != null && features.size() > 0) {
        Feature incident = features.get(0);

        //Ensure that the clicked feature is an traffic incident feature.
        if (incident.properties() != null && incident.hasProperty("incidentType")) {

            StringBuilder sb = new StringBuilder();
            String incidentType = incident.getStringProperty("incidentType");

            if (incidentType != null) {
                sb.append(incidentType);
            }

            if (sb.length() > 0) {
                sb.append("\n");
            }

            //If the road is closed, find out where it is closed from.
            if ("Road Closed".equals(incidentType)) {
                String from = incident.getStringProperty("from");

                if (from != null) {
                    sb.append(from);
                }
            } else {
                //Get the description of the traffic incident.
                String description = incident.getStringProperty("description");

                if (description != null) {
                    sb.append(description);
                }
            }

            String message = sb.toString();

            if (message.length() > 0) {
                Toast.makeText(this, message, Toast.LENGTH_LONG).show();
            }
        }
    }
});
```

En la siguiente captura de pantalla se muestra cómo el código anterior representa información de tráfico en tiempo real en el mapa con un mensaje de notificación que incluye detalles sobre el incidente.

![Mapa que muestra información de tráfico en tiempo real con un mensaje de notificación que incluye detalles del incidente](media/how-to-show-traffic-android/android-traffic-details.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte las guías siguientes para obtener información sobre cómo agregar más datos al mapa:

> [!div class="nextstepaction"]
> [Adición de una capa de icono](how-to-add-tile-layer-android-map.md)
