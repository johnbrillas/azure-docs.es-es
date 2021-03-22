---
title: Adición de una capa de extrusión de polígono a un mapa de Android | Microsoft Azure Maps
description: Cómo agregar una capa de extrusión de polígonos a Android SDK de Microsoft Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 02/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: ccabffc0e5a65e41f31c3c80703e67f78e8b5f9e
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102101008"
---
# <a name="add-a-polygon-extrusion-layer-to-the-map-android-sdk"></a>Adición de una capa de extrusión de polígono al mapa (Android SDK)

En este artículo se muestra cómo usar la capa de extrusión de polígono para representar áreas de las geometrías de características `Polygon` y `MultiPolygon` como formas extruidas.

## <a name="use-a-polygon-extrusion-layer"></a>Uso de una capa de extrusión de polígono

Conecte la capa de extrusión de polígono a un origen de datos. Luego, cárguelo en el mapa. La capa de extrusión de polígono representa las áreas de las características `Polygon` y `MultiPolygon` como formas extruidas. Las propiedades `height` y `base` de la capa de extrusión de polígono definen la distancia base desde el suelo y la altura de la forma extruida en **metros**. En el código siguiente se muestra cómo crear un polígono, agregarlo a un origen de datos y representarlo con la clase de la capa de extrusión de polígono.

> [!Note]
> El valor `base` definido en la capa de extrusión de polígono debe ser menor o igual al de `height`.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.958383, 40.800279),
            Point.fromLngLat(-73.981547, 40.768459),
            Point.fromLngLat(-73.981246, 40.767761),
            Point.fromLngLat(-73.973618, 40.764616),
            Point.fromLngLat(-73.973060, 40.765128),
            Point.fromLngLat(-73.972599, 40.764908),
            Point.fromLngLat(-73.949446, 40.796584),
            Point.fromLngLat(-73.949661, 40.797088),
            Point.fromLngLat(-73.957815, 40.800523),
            Point.fromLngLat(-73.958383, 40.800279)
        )
    )
));

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
PolygonExtrusionLayer layer = new PolygonExtrusionLayer(source,
    fillColor("#fc0303"),
    fillOpacity(0.7f),
    height(500f)
);

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
map.layers.add(layer, "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create a polygon.
source.add(
    Polygon.fromLngLats(
        Arrays.asList(
            Arrays.asList(
                Point.fromLngLat(-73.958383, 40.800279),
                Point.fromLngLat(-73.981547, 40.768459),
                Point.fromLngLat(-73.981246, 40.767761),
                Point.fromLngLat(-73.973618, 40.764616),
                Point.fromLngLat(-73.973060, 40.765128),
                Point.fromLngLat(-73.972599, 40.764908),
                Point.fromLngLat(-73.949446, 40.796584),
                Point.fromLngLat(-73.949661, 40.797088),
                Point.fromLngLat(-73.957815, 40.800523),
                Point.fromLngLat(-73.958383, 40.800279)
            )
        )
    )
)

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
val layer = PolygonExtrusionLayer(
    source,
    fillColor("#fc0303"),
    fillOpacity(0.7f),
    height(500f)
)

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
map.layers.add(layer, "labels")
```

::: zone-end

En la captura de pantalla siguiente se muestra cómo el código anterior representa un polígono ajustado mediante una capa de extrusión de polígono.

![Mapa con polígono ajustado verticalmente mediante una capa de extrusión de polígono](media/map-extruded-polygon-android/polygon-extrusion-layer.jpg)

## <a name="add-data-driven-polygons"></a>Incorporación de polígonos controlados por datos

Un mapa de coropletas se puede representar mediante la capa de extrusión de polígono. Establezca las propiedades `height` y `fillColor` de la capa de extrusión en la medida de la variable estadística de las geometrías de las características `Polygon` y `MultiPolygon`. En el ejemplo de código siguiente se muestra un mapa de coropletas extruido de Estados Unidos basado en la medida de la densidad de población por estado.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Import the geojson data and add it to the data source.
Utils.importData("https://azuremapscodesamples.azurewebsites.net/Common/data/geojson/US_States_Population_Density.json", this,  (String result) -> {
    //Parse the data as a GeoJSON Feature Collection.
    FeatureCollection fc = FeatureCollection.fromJson(result);

    //Add the feature collection to the data source.
    source.add(fc);
});

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
PolygonExtrusionLayer layer = new PolygonExtrusionLayer(source,
    fillOpacity(0.7f),
    fillColor(
        step(
            get("density"),
            literal("rgba(0, 255, 128, 1)"),
            stop(10, "rgba(9, 224, 118, 1)"),
            stop(20, "rgba(11, 191, 103, 1)"),
            stop(50, "rgba(247, 227, 5, 1)"),
            stop(100, "rgba(247, 199, 7, 1)"),
            stop(200, "rgba(247, 130, 5, 1)"),
            stop(500, "rgba(247, 94, 5, 1)"),
            stop(1000, "rgba(247, 37, 5, 1)")
        )
    ),
    height(
        interpolate(
            linear(),
            get("density"),
            stop(0, 100),
            stop(1200, 960000)
        )
    )
);

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
map.layers.add(layer, "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Import the geojson data and add it to the data source.
Utils.importData("https://azuremapscodesamples.azurewebsites.net/Common/data/geojson/US_States_Population_Density.json",
    this
) { result: String? ->
    //Parse the data as a GeoJSON Feature Collection.
    val fc = FeatureCollection.fromJson(result!!)

    //Add the feature collection to the data source.
    source.add(fc)
}

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
val layer = PolygonExtrusionLayer(
    source,
    fillOpacity(0.7f),
    fillColor(
        step(
            get("density"),
            literal("rgba(0, 255, 128, 1)"),
            stop(10, "rgba(9, 224, 118, 1)"),
            stop(20, "rgba(11, 191, 103, 1)"),
            stop(50, "rgba(247, 227, 5, 1)"),
            stop(100, "rgba(247, 199, 7, 1)"),
            stop(200, "rgba(247, 130, 5, 1)"),
            stop(500, "rgba(247, 94, 5, 1)"),
            stop(1000, "rgba(247, 37, 5, 1)")
        )
    ),
    height(
        interpolate(
            linear(),
            get("density"),
            stop(0, 100),
            stop(1200, 960000)
        )
    )
)

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
map.layers.add(layer, "labels")
```

::: zone-end

En la captura de pantalla siguiente se muestra un mapa coroplético de Estados Unidos y se ajusta verticalmente como polígonos extruidos basados en la densidad de población.

![Mapa coroplético de los estados de Estados Unidos coloreados y ajustados verticalmente como polígonos extruidos basados en la densidad de población](media/map-extruded-polygon-android/android-extruded-choropleth.jpg)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Creación de un origen de datos](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Adición de una capa de polígono](how-to-add-shapes-to-android-map.md)
