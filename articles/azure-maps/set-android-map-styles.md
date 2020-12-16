---
title: Establecimiento de un estilo de mapa mediante Android SDK de Azure Maps
description: Obtenga información sobre dos maneras de establecer el estilo de un mapa. Vea cómo se utiliza Android SDK de Microsoft Azure Maps en el archivo de diseño o en la clase de actividad para ajustar el estilo.
author: anastasia-ms
ms.author: v-stharr
ms.date: 11/18/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 8c7689fb87575ac6e150f793b43f35e8bf6adc83
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96532490"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Establecimiento del estilo de mapa mediante Android SDK de Azure Maps

En este artículo se muestra cómo establecer estilos de mapa mediante Android SDK de Azure Maps. Azure Maps tiene cuatro estilos de mapas entre los que puede elegir. Para obtener más información sobre los estilos de mapa compatibles, consulte [Estilos de mapa admitidos en Azure Maps](./supported-map-styles.md).

## <a name="prerequisites"></a>Prerrequisitos

1. [Cree una cuenta de Azure Maps](quick-demo-map-app.md#create-an-azure-maps-account).
2. [Obtenga una clave de suscripción principal](quick-demo-map-app.md#get-the-primary-key-for-your-account), también conocida como clave principal o clave de suscripción.
3. Descargue e instale [Android SDK de Azure Maps](./how-to-use-android-map-control-library.md).


## <a name="set-map-style-in-the-layout"></a>Establecimiento del estilo del mapa en el diseño

Puede establecer un estilo de mapa en el archivo de diseño de su clase de actividad. Edite `res > layout > activity_main.xml`, para que su aspecto sea similar al siguiente:

```XML
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/mapcontrol"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
        app:mapcontrol_style="grayscale_dark"
        />

</FrameLayout>
```

El atributo `mapcontrol_style` anterior establece el estilo de mapa en **grayscale_dark**.

:::image type="content" source="./media/set-android-map-styles/grayscale-dark.png" border="true" alt-text="Azure Maps, imagen de mapa que muestra el estilo como grayscale_dark":::

## <a name="set-map-style-in-the-mainactivity-class"></a>Establecimiento del estilo del mapa en la clase MainActivity

El estilo del mapa también se puede establecer en la clase MainActivity. Abra el archivo `java > com.example.myapplication > MainActivity.java` y copie el siguiente fragmento de código en el método **onCreate()** . Este código establecerá el estilo del mapa en **satellite_road_labels**.

>[!WARNING]
>Es posible que Android Studio no haya importado las clases necesarias.  En consecuencia, el código tendrá algunas referencias que no se pueden resolver. Para importar las clases necesarias, simplemente mantenga el mouse sobre cada referencia no resuelta y presione `Alt + Enter` (Opción + Retorno en un equipo Mac).

```Java
mapControl.onReady(map -> {

    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle((style(SATELLITE_ROAD_LABELS)));
       
});
```

:::image type="content" source="./media/set-android-map-styles/satellite-road-labels.png" border="true" alt-text="Azure Maps, imagen de mapa que muestra el estilo como satellite_road_labels":::