---
title: Adición de una capa de polígono a mapas de Android | Microsoft Azure Maps
description: Obtenga información sobre cómo agregar polígonos o círculos a los mapas. Vea cómo usar Android SDK de Azure Maps para personalizar las formas geométricas y facilitar su actualización y mantenimiento.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 1712cedab9cef23108fcc48b8e09bdc3e33065c4
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97679477"
---
# <a name="add-a-polygon-layer-to-the-map-android-sdk"></a>Adición de una capa de polígono al mapa (Android SDK)

En este artículo se muestra cómo representar las áreas de las geometrías de las características `Polygon` y `MultiPolygon` en el mapa con una capa de polígono.

## <a name="prerequisites"></a>Prerrequisitos

Asegúrese de completar los pasos descritos en el documento [Inicio rápido: Creación de una aplicación de Android](quick-android-map.md). Los bloques de código de este artículo se pueden insertar en el controlador de eventos `onReady` de mapas.

## <a name="use-a-polygon-layer"></a>Uso de una capa de polígono

Cuando una capa de polígono se conecta a un origen de datos y se carga en el mapa, representa el área con las características `Polygon` y `MultiPolygon`. Para crear un polígono, agréguelo a un origen de datos y represéntelo con una capa de polígono mediante la clase `PolygonLayer`.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a rectangular polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.79680),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source, 
    fillColor("red"),
    fillOpacity(0.7f)
), "labels");
```

En la captura de pantalla siguiente se muestra cómo el código anterior representa el área de un polígono mediante una capa de polígono.

![Polígono con su área de relleno representada](media/how-to-add-shapes-to-android-map/android-polygon-layer.png)

## <a name="use-a-polygon-and-line-layer-together"></a>Uso de un polígono y una capa de línea conjuntamente

Una capa de línea se usa para representar el contorno de los polígonos. En el ejemplo de código siguiente se representa un polígono como en el ejemplo anterior, pero ahora se agrega una capa de línea. Esta capa de línea es una segunda capa conectada al origen de datos.  

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a rectangular polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.79680),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source,
    fillColor("rgba(0, 200, 200, 0.5)")
), "labels");

//Create and add a line layer to render the outline of the polygon.
map.layers.add(new LineLayer(source,
    strokeColor("red"),
    strokeWidth(2f)
));
```

En la siguiente captura de pantalla se muestra cómo el código anterior representa un polígono con su contorno representado mediante una capa de línea.

![Polígono con el área de relleno y el contorno representados](media/how-to-add-shapes-to-android-map/android-polygon-and-line-layer.png)

> [!TIP]
> Cuando cree el contorno de un polígono con una capa de línea, asegúrese de cerrar todos los anillos de los polígonos, de modo que cada matriz de puntos tenga el mismo punto inicial y final. Si no lo hace así, es posible que la capa de línea no conecte el último punto del polígono al primer punto.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Creación de un origen de datos](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Adición de una capa de línea](android-map-add-line-layer.md)
