---
title: Adición de una capa de mosaico a mapas de Android | Microsoft Azure Maps
description: Aprenda a agregar una capa de mosaico a un mapa. Vea un ejemplo en el que se usa Android SDK de Azure Maps para agregar una superposición de radar meteorológico a un mapa.
author: rbrundritt
ms.author: richbrun
ms.date: 2/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: 6a920dc222cae4aedd77b667644de317637bbb69
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102047509"
---
# <a name="add-a-tile-layer-to-a-map-android-sdk"></a>Adición de una capa de mosaico a un mapa (Android SDK)

En este artículo se muestra cómo representar una capa de mosaico en un mapa mediante Android SDK de Azure Maps. Las capas de mosaico permiten superponer imágenes encima de los mosaicos de mapa base de Azure Maps. Puede encontrar más información en el sistema de mosaico de Azure Maps en la documentación [Niveles de zoom y cuadrícula de mosaico](zoom-levels-and-tile-grid.md).

Una capa de mosaico carga los mosaicos desde un servidor. Estas imágenes pueden representarse previamente y almacenarse como cualquier otra imagen en un servidor, mediante una convención de nomenclatura que la capa de mosaico entienda. O bien, estas imágenes se pueden representar con un servicio dinámico que genere las imágenes casi en tiempo real. Hay tres convenciones diferentes de nomenclatura de servicio de mosaico compatibles con la clase TileLayer de Azure Maps:

* X, Y notación de zoom: según el nivel de zoom, x es la columna e y es la posición de fila del mosaico en la cuadrícula de mosaico.
* Notación Quadkey: combinación de la información de x, y y zoom en un solo valor de cadena que es un identificador único de un mosaico.
* Rectángulo delimitador: las coordenadas del rectángulo delimitador se pueden usar para especificar una imagen con el formato `{west},{south},{east},{north}`, que suele utilizarse en [Web Mapping Services (WMS)](https://www.opengeospatial.org/standards/wms).

> [!TIP]
> Un elemento TileLayer es una excelente manera de visualizar grandes conjuntos de datos en el mapa. No solo puede generarse una capa de mosaico a partir de una imagen, sino que también se pueden representar datos de vector como una capa de mosaico. Al representar datos de vectores como una capa de mosaico, el control de mapa solo necesita cargar los mosaicos, que pueden tener un tamaño de archivo bastante más reducido que los datos de vector que representan. Esta técnica la usan muchos usuarios que necesitan representar millones de filas de datos en el mapa.

La dirección URL del mosaico pasada a una capa de mosaico debe ser una dirección URL HTTP/HTTPS que apunte a un recurso TileJSON o a una plantilla de URL de mosaico que usa los siguientes parámetros: 

* `{x}`: posición del mosaico en X. También necesita `{y}` y `{z}`.
* `{y}`: posición del mosaico en Y. También necesita `{x}` y `{z}`.
* `{z}`: nivel de zoom del mosaico. También necesita `{x}` y `{y}`.
* `{quadkey}`: identificador quadkey de mosaico basado en la convención de nomenclatura del sistema de mosaico de Bing Maps.
* `{bbox-epsg-3857}`: una cadena de un cuadro delimitador con el formato `{west},{south},{east},{north}` en el sistema de referencia espacial EPSG 3857.
* `{subdomain}`: Marcador de posición para los valores del subdominio, si se especifica el valor de subdominio.
* `azmapsdomain.invalid`: un marcador de posición para alinear el dominio y la autenticación de las solicitudes de mosaico con los mismos valores utilizados por el mapa. Úselo al llamar a un servicio de mosaico hospedado por Azure Maps.

## <a name="prerequisites"></a>Prerrequisitos

Para completar el proceso en este artículo, debe instalar [Android SDK de Azure Maps](how-to-use-android-map-control-library.md) para cargar un mapa.

## <a name="add-a-tile-layer-to-the-map"></a>Incorporación de una capa de mosaico a un mapa

En este ejemplo se muestra cómo crear una capa de mosaico que señale a un conjunto de mosaicos. En este ejemplo se usa el sistema de mosaicos de "zoom, x e y". El origen de esta capa de mosaico es el [proyecto OpenSeaMap](https://openseamap.org/index.php), que contiene cartas náuticas financiadas por la comunidad. A menudo, al ver las capas del mosaico, es conveniente poder ver claramente las etiquetas de las ciudades en el mapa. Este comportamiento se puede conseguir si se inserta la capa de mosaico debajo de las capas de etiqueta del mapa.

::: zone pivot="programming-language-java-android"

```java
TileLayer layer = new TileLayer(
    tileUrl("https://tiles.openseamap.org/seamark/{z}/{x}/{y}.png"),
    opacity(0.8f),
    tileSize(256),
    minSourceZoom(7),
    maxSourceZoom(17)
);

map.layers.add(layer, "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = TileLayer(
    tileUrl("https://tiles.openseamap.org/seamark/{z}/{x}/{y}.png"),
    opacity(0.8f),
    tileSize(256),
    minSourceZoom(7),
    maxSourceZoom(17)
)

map.layers.add(layer, "labels")
```

::: zone-end

En la captura de pantalla siguiente se muestra el código anterior que muestra una capa de mosaico de información náutica en un mapa con un estilo de escala de grises oscuro.

![Mapa de Android en el que se muestra la capa de mosaico](media/how-to-add-tile-layer-android-map/xyz-tile-layer-android.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte el siguiente artículo para obtener más información sobre las formas de superponer imágenes en un mapa.

> [!div class="nextstepaction"]
> [Capa de imagen](map-add-image-layer-android.md)
