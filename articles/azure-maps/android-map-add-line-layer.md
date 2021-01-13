---
title: Adición de una capa de línea a mapas de Android | Microsoft Azure Maps
description: Aprenda a agregar líneas a mapas. Consulte los ejemplos que usan Android SDK de Azure Maps para agregar capas de línea a los mapas y para personalizar las líneas con símbolos y degradados de color.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 3e68be79a4405af103512a9009187857a0d9af39
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681356"
---
# <a name="add-a-line-layer-to-the-map-android-sdk"></a>Adición de una capa de línea al mapa (Android SDK)

Una capa de línea se puede usar para representar las características `LineString` y `MultiLineString` como rutas de acceso o rutas en el mapa. También se puede usar una capa de línea para representar el contorno de las características `Polygon` y `MultiPolygon`. Se puede conectar un origen de datos a la capa de línea para que proporcione los datos que se van a representar.

> [!TIP]
> Las capas de línea de forma predeterminada representarán las coordenadas de los polígonos y las líneas en un origen de datos. Para limitar la capa de forma que solo represente las características geométricas LineString, establezca la opción `filter` de la capa en `eq(geometryType(), "LineString")`. Si quiere incluir también características MultiLineString, establezca la opción `filter` de la capa en `any(eq(geometryType(), "LineString"), eq(geometryType(), "MultiLineString"))`.

## <a name="prerequisites"></a>Prerrequisitos

Asegúrese de completar los pasos descritos en el documento [Inicio rápido: Creación de una aplicación de Android](quick-android-map.md). Los bloques de código de este artículo se pueden insertar en el controlador de eventos `onReady` de mapas.

## <a name="add-a-line-layer"></a>Adición de una capa de línea

En el código siguiente, se muestra cómo se crea una línea. Agregue la línea a un origen de datos y represéntela con una capa de líneas utilizando la clase `LineLayer`.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a list of points.
List<Point> points = Arrays.asList(
    Point.fromLngLat(-73.972340, 40.743270),
    Point.fromLngLat(-74.004420, 40.756800));

//Create a LineString geometry and add it to the data source.
source.add(LineString.fromLngLats(points));

//Create a line layer and add it to the map.
LineLayer layer = new LineLayer(source,
    strokeColor("blue"),
    strokeWidth(5f)
);

map.layers.add(layer);
```

En la siguiente captura de pantalla se muestra cómo el código anterior representa una línea en una capa de línea.

![Mapa con una línea representada mediante una capa de línea](media/android-map-add-line-layer/android-line-layer.png)

## <a name="data-drive-line-style"></a>Estilo de línea controlado por datos

El siguiente código crea dos características de línea y agrega un valor de límite de velocidad como propiedad a cada línea. Una capa de línea usa una expresión de estilo controlada por datos para asignar color a las líneas en función del valor de límite de velocidad. Dado que los datos de línea se superponen a lo largo de las carreteras, el siguiente código agrega la capa de línea debajo de la capa de etiqueta para que se puedan leer claramente las etiquetas de la carretera.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a line feature.
Feature feature = Feature.fromGeometry(
    LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.131821, 47.704033),
        Point.fromLngLat(-122.099919, 47.703678))));

//Add a property to the feature.
feature.addNumberProperty("speedLimitMph", 35);

//Add the feature to the data source.
source.add(feature);

//Create a second line feature.
Feature feature2 = Feature.fromGeometry(
    LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.126662, 47.708265),
        Point.fromLngLat(-122.126877, 47.703980))));

//Add a property to the second feature.
feature2.addNumberProperty("speedLimitMph", 15);

//Add the second feature to the data source.
source.add(feature2);

//Create a line layer and add it to the map.
LineLayer layer = new LineLayer(source,
    strokeColor(
        interpolate(
            linear(),
            get("speedLimitMph"),
            stop(0, color(Color.GREEN)),
            stop(30, color(Color.YELLOW)),
            stop(60, color(Color.RED))
        )
    ),
    strokeWidth(5f)
);

map.layers.add(layer, "labels");
```

En la siguiente captura de pantalla se muestra cómo el código anterior representa dos líneas en una capa de línea con el color que se recupera de una expresión de estilo controlada por datos en función de una propiedad de las características de línea.

![Mapa con líneas que tienen el estilo controlado por datos representadas en una capa de línea](media/android-map-add-line-layer/android-line-layer-data-drive-style.png)

## <a name="add-symbols-along-a-line"></a>Adición de símbolos a lo largo de una línea

En este ejemplo se muestra cómo agregar iconos de flecha situados a lo largo de una línea en el mapa. Si usa una capa de símbolos, establezca la opción `symbolPlacement` en `SymbolPlacement.LINE`. Esta opción representará los símbolos a lo largo de la línea y girará los iconos (0 grados = derecha).

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Load a image of an arrow into the map image sprite and call it "arrow-icon".
map.images.add("arrow-icon", R.drawable.purple-arrow-right);

//Create and add a line to the data source.
source.add(LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.18822, 47.63208),
        Point.fromLngLat(-122.18204, 47.63196),
        Point.fromLngLat(-122.17243, 47.62976),
        Point.fromLngLat(-122.16419, 47.63023),
        Point.fromLngLat(-122.15852, 47.62942),
        Point.fromLngLat(-122.15183, 47.62988),
        Point.fromLngLat(-122.14256, 47.63451),
        Point.fromLngLat(-122.13483, 47.64041),
        Point.fromLngLat(-122.13466, 47.64422),
        Point.fromLngLat(-122.13844, 47.65440),
        Point.fromLngLat(-122.13277, 47.66515),
        Point.fromLngLat(-122.12779, 47.66712),
        Point.fromLngLat(-122.11595, 47.66712),
        Point.fromLngLat(-122.11063, 47.66735),
        Point.fromLngLat(-122.10668, 47.67035),
        Point.fromLngLat(-122.10565, 47.67498))));

//Create a line layer and add it to the map.
map.layers.add(new LineLayer(source,
    strokeColor("DarkOrchid"),
    strokeWidth(5f)
));

//Create a symbol layer and add it to the map.
map.layers.add(new SymbolLayer(source,
    //Space symbols out along line.
    symbolPlacement(SymbolPlacement.LINE),

    //Spread the symbols out 100 pixels apart.
    symbolSpacing(100f),
    
    //Use the arrow icon as the symbol.
    iconImage("arrow-icon"),

    //Allow icons to overlap so that they aren't hidden if they collide with other map elements.
    iconAllowOverlap(true),

    //Center the symbol icon.
    iconAnchor(AnchorType.CENTER),

    //Scale the icon size.
    iconSize(0.8f)
));
```

En este ejemplo, la imagen siguiente se ha cargado en la carpeta Drawable de la aplicación.

| ![Imagen de icono de flecha púrpura](media/android-map-add-line-layer/purple-arrow-right.png)|
|:-----------------------------------------------------------------------:|
|                                                  |

En la siguiente captura de pantalla se muestra cómo el código anterior representa una línea que tiene iconos de flecha a lo largo.

![Mapa con líneas que tienen el estilo controlado por datos e incluyen flechas, representadas en una capa de línea](media/android-map-add-line-layer/android-symbols-along-line-path.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Creación de un origen de datos](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Adición de una capa de polígono](how-to-add-shapes-to-android-map.md)
