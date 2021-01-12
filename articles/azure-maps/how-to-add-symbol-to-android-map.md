---
title: Adición de una capa de símbolo a mapas de Android | Microsoft Azure Maps
description: Obtenga información sobre cómo agregar un marcador a un mapa. Vea un ejemplo en el que se usa el SDK para Android de Azure Maps para agregar una capa de símbolos que contiene datos basados en puntos de un origen de datos.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 040fcde35707074ffaf102ed6c224b2f47a084bb
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97679345"
---
# <a name="add-a-symbol-layer-android-sdk"></a>Adición de una capa de símbolo (Android SDK)

En este artículo se explica cómo representar datos de punto de un origen de datos como una capa de símbolos en un mapa mediante el Android SDK de Azure Maps. Las capas de símbolo representan puntos como una imagen y texto en el mapa.

> [!TIP]
> De forma predeterminada, las capas de símbolo representarán las coordenadas de todos los objetos geométricos en un origen de datos. Para limitar la capa de forma que solo represente las características geométricas del punto, establezca la opción `filter` de la capa en `eq(geometryType(), "Point")`. Si quiere también incluir características MultiPoint, establezca la opción `filter` de la capa en `any(eq(geometryType(), "Point"), eq(geometryType(), "MultiPoint"))`.

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de completar los pasos descritos en el documento [Inicio rápido: Creación de una aplicación de Android](quick-android-map.md). Los bloques de código de este artículo se pueden insertar en el controlador de eventos `onReady` del mapa.

## <a name="add-a-symbol-layer"></a>Adición de una capa de símbolo

Para poder agregar una capa de símbolos al mapa, debe realizar primero algunas operaciones. En primer lugar, cree un origen de datos y agréguelo al mapa. Cree una capa de símbolos. Pase el origen de datos a la capa de símbolos para recuperar los datos de este origen. Por último, agregue datos al origen de datos para que haya algo que representar.

En el código siguiente se muestra lo que debe agregarse al mapa una vez que se haya cargado. En este ejemplo se representa un único punto en el mapa mediante una capa de símbolos.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point and add it to the data source.
source.add(Point.fromLngLat(0, 0));

//Create a symbol layer to render icons and/or text at points on the map.
SymbolLayer layer = new SymbolLayer(source);

//Add the layer to the map.
map.layers.add(layer);
```

Hay tres tipos diferentes de datos de puntos que se pueden agregar al mapa:

- Geometría Point de GeoJSON: Este objeto solo contiene una coordenada de un punto y nada más. El método estático `Point.fromLngLat` se puede usar para crear fácilmente estos objetos.
- Geometría MultiPoint de GeoJSON: este objeto solamente contiene las coordenadas de varios puntos; no contiene nada más. Pase una matriz de puntos a la clase `MultiPoint` para crear estos objetos.
- GeoJSON Feature: Este objeto se compone de cualquier geometría GeoJSON y un conjunto de propiedades que contienen metadatos asociados a la geometría.

Para obtener más información, vea el documento [Creación de un origen de datos](create-data-source-android-sdk.md) sobre cómo crear y agregar datos al mapa.

En el ejemplo de código siguiente se crea una geometría Point de GeoJSON, se pasa a la característica de GeoJSON y se le agrega un valor `title` a sus propiedades. La propiedad `title` se muestra como texto encima del icono de símbolo en el mapa.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(0, 0));

//Add a property to the feature.
feature.addStringProperty("title", "Hello World!");

//Add the feature to the data source.
source.add(feature);

//Create a symbol layer to render icons and/or text at points on the map.
SymbolLayer layer = new SymbolLayer(source, 
    //Get the title property of the feature and display it on the map.
    textField(get("title"))
);

//Add the layer to the map.
map.layers.add(layer);
```

En la captura de pantalla siguiente se muestra el código anterior que representa una característica de punto mediante un icono y una etiqueta de texto con una capa de símbolo.

![Mapa con punto representado mediante una capa de símbolo que muestra un icono y una etiqueta de texto para una característica de punto](media/how-to-add-symbol-to-android-map/android-map-pin.png)

> [!TIP]
> De forma predeterminada, las capas de símbolos optimizan la representación de los símbolos, ya que ocultan los símbolos que se superponen. A medida que se acerca el zoom, los símbolos ocultos se hacen visibles. Para deshabilitar esta característica y representar todos los símbolos en todo momento, establezca las opciones `iconAllowOverlap` y `textAllowOverlap` en `true`.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Adición de un icono personalizado a una capa de símbolo

Las capas de símbolo se representan mediante WebGL. Por tanto, todos los recursos, como las imágenes de icono, se deben cargar en el contexto de WebGL. En este ejemplo, se muestra cómo se agrega un icono personalizado a los recursos del mapa. Este icono se usa después para representar los datos de punto en el mapa con un símbolo personalizado. La propiedad `textField` de la capa de símbolo requiere que se especifique una expresión. En este caso, queremos representar la propiedad "temperature". Como la temperatura es un número, debe convertirse en una cadena. Además, queremos anexarle "°F". Para ello, podemos utilizar la expresión `concat(Expression.toString(get("temperature")), literal("°F"))`.

```java
//Load a custom icon image into the image sprite of the map.
map.images.add("my-custom-icon", R.drawable.showers);

//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(-73.985708, 40.75773));

//Add a property to the feature.
feature.addNumberProperty("temperature", 64);

//Add the feature to the data source.
source.add(feature);

//Create a symbol layer to render icons and/or text at points on the map.
SymbolLayer layer = new SymbolLayer(source,
    iconImage("my-custom-icon"),
    iconSize(0.5f),

    //Get the title property of the feature and display it on the map.
    textField(concat(Expression.toString(get("temperature")), literal("°F"))),
    textOffset(new Float[]{0f, -1.5f})
);
```

En este ejemplo, la imagen siguiente se ha cargado en la carpeta Drawable de la aplicación.

| ![Imagen del icono meteorológico de tormentas](media/how-to-add-symbol-to-android-map/showers.png)|
|:-----------------------------------------------------------------------:|
| showers.png                                                  |

En la captura de pantalla siguiente se muestra el código anterior que representa una característica de punto mediante un icono personalizado y una etiqueta de texto con formato con una capa de símbolo.

![Mapa con punto representado mediante una capa de símbolo que muestra un icono personalizado y una etiqueta de texto con formato para una característica de punto](media/how-to-add-symbol-to-android-map/android-custom-symbol-layer.png)

> [!TIP]
> Si solamente desea representar texto con una capa de símbolos, puede ocultar el icono estableciendo la propiedad `iconImage` de las opciones del icono en `"none"`.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Creación de un origen de datos](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Adición de una capa de burbuja](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Presentación de la información de características](display-feature-information-android.md)
