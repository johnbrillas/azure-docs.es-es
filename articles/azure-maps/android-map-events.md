---
title: Control de eventos de mapa en los mapas de Android | Microsoft Azure Maps
description: Obtenga información sobre los eventos que se desencadenan cuando los usuarios interactúan con mapas. Vea una lista de todos los eventos de mapa admitidos. Vea cómo usar Android SDK de Azure Maps para controlar eventos.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 818d33947fa079a130c3009a34dcb9b72ad52f91
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681328"
---
# <a name="interact-with-the-map-android-sdk"></a>Interactuación con el mapa (Android SDK)

En este artículo se muestra cómo usar el administrador de eventos de mapa.

## <a name="interact-with-the-map"></a>Interacción con el mapa

El mapa administra todos los eventos por medio de su propiedad `events`. En la siguiente tabla se muestran todos los eventos de mapa admitidos.

| Evento                  | Formato del controlador de eventos | Descripción |
|------------------------|----------------------|-------------|
| `OnCameraIdle`         | `()`                 | <p>Se genera después del último fotograma representado antes de que el mapa entre en un estado "inactivo":<ul><li>No hay transiciones de cámara en curso.</li><li>Todos los elementos solicitados actualmente se han cargado.</li><li>Todas las animaciones de fundido y transición se han completado.</li></ul></p> |
| `OnCameraMove`         | `()`                 | Se genera repetidamente durante una transición animada desde una vista a otra, como resultado de la interacción del usuario o los métodos. |
| `OnCameraMoveCanceled` | `()`                 | Se desencadena cuando se cancela una solicitud de movimiento a la cámara. |
| `OnCameraMoveStarted`  | `(int reason)`       | Se genera justo antes de que el mapa inicie una transición desde una vista a otra, como resultado de la interacción del usuario o los métodos. El argumento `reason` del cliente de escucha de eventos devuelve un valor entero que proporciona detalles sobre cómo se inició el movimiento de la cámara. A continuación se muestra la lista de posibles motivos:<ul><li>1: Gesto</li><li>2: Animación de desarrollador</li><li>3: Animación de API</li></ul>   |
| `OnClick`              | `(double lat, double lon)` | Se genera cuando el mapa se presiona y se libera en el mismo punto del mapa. |
| `OnFeatureClick`       | `(List<Feature>)`    | Se genera cuando el mapa se presiona y se libera en el mismo punto de una característica.  |
| `OnLongClick`          | `(double lat, double lon)` | Se genera cuando el mapa se presiona, se mantiene durante un momento y, a continuación, se libera en el mismo punto del mapa. |
| `OnLongFeatureClick `  | `(List<Feature>)`    | Se genera cuando el mapa se presiona, se mantiene durante un momento y, a continuación, se libera en el mismo punto de una característica. |
| `OnReady`              | `(AzureMap map)`     | Se desencadena cuando el mapa se carga inicialmente o cuando la orientación de la aplicación cambia, los recursos de mapa mínimos necesarios se cargan y el mapa está listo para que se interactúe con él mediante programación. |

En el siguiente código se muestra cómo agregar los eventos `OnClick`, `OnFeatureClick` y `OnCameraMove` al mapa.

```java
map.events.add((OnClick) (lat, lon) -> {
    //Map clicked.
});

map.events.add((OnFeatureClick) (features) -> {
    //Feature clicked.
});

map.events.add((OnCameraMove) () -> {
    //Map camera moved.
});
```

Para obtener más información, consulte en [Navegación por el mapa](how-to-use-android-map-control-library.md#navigating-the-map) cómo interactuar con el mapa y desencadenar eventos.

## <a name="scope-feature-events-to-layer"></a>Delimitación de los eventos de características a una capa

Al agregar los eventos `OnFeatureClick` o `OnLongFeatureClick` al mapa, se puede pasar un identificador de capa como segundo parámetro. Cuando se pasa un identificador de capa, el evento solo se activa si el evento se produce en esa capa. Los eventos que se limitan a determinadas capas se admiten en las capas de símbolos, burbujas, líneas y polígonos.

```java
//Create a data source.
DataSource source = new DataSource();
map.sources.add(source);

//Add data to the data source.
source.add(Point.fromLngLat(0, 0));

//Create a layer and add it to the map.
BubbleLayer layer = new BubbleLayer(source);
map.layers.add(layer);

//Add a feature click event to the map and pass the layer ID to limit the event to the specified layer.
map.events.add((OnFeatureClick) (features) -> {
    //One or more features clicked.
}, layer.getId());

//Add a long feature click event to the map and pass the layer ID to limit the event to the specified layer.
map.events.add((OnLongFeatureClick) (features) -> {
    //One or more features long clicked.
}, layer.getId());
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener ejemplos de código completo:

> [!div class="nextstepaction"]
> [Navegación por el mapa](how-to-use-android-map-control-library.md#navigating-the-map)

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Adición de una capa de burbuja](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Adición de una capa de línea](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de polígono](how-to-add-shapes-to-android-map.md)
