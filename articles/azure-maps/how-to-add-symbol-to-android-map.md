---
title: Adición de una capa de símbolo a un mapa mediante Android SDK de Azure Maps
description: Obtenga información sobre cómo agregar un marcador a un mapa. Vea un ejemplo en el que se utiliza Android SDK de Microsoft Azure Maps para agregar una capa de símbolos que contiene datos basados en puntos de un origen de datos.
author: anastasia-ms
ms.author: v-stharr
ms.date: 11/24/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 300a7968b2072459d6d7709e4d89388e1bcf59f3
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96531214"
---
# <a name="add-a-symbol-layer-to-a-map-using-azure-maps-android-sdk"></a>Adición de una capa de símbolo a un mapa mediante Android SDK de Azure Maps

En este artículo se explica cómo representar datos de punto de un origen de datos como una capa de símbolos en un mapa mediante el Android SDK de Azure Maps.

## <a name="prerequisites"></a>Requisitos previos

1. [Cree una cuenta de Azure Maps](quick-demo-map-app.md#create-an-azure-maps-account).
2. [Obtenga una clave de suscripción principal](quick-demo-map-app.md#get-the-primary-key-for-your-account), también conocida como clave principal o clave de suscripción.
3. Descargue e instale [Android SDK de Azure Maps](./how-to-use-android-map-control-library.md).

## <a name="add-a-symbol-layer"></a>Adición de una capa de símbolo

Para agregar un marcador en un mapa mediante la capa de símbolos, siga estos pasos:

1. Edite **res** > **layout** > **activity_main.xml** para que su aspecto sea similar al del siguiente XML:
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
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
            app:mapcontrol_centerLat="47.64"
            app:mapcontrol_centerLng="-122.33"
            app:mapcontrol_zoom="12"
            />

    </FrameLayout>
    ```

2. Copie el siguiente fragmento de código en el método **onCreate()** de la clase `MainActivity.java`.

    ```Java
    mapControl.onReady(map -> {
    
        //Create a data source and add it to the map.
        DataSource dataSource = new DataSource();
        map.sources.add(dataSource);
    
        //Create a point feature and add it to the data source.
        dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
    
        //Add a red custom image icon to the map resources.
        map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
    
        //Create a symbol layer and add it to the map.
        map.layers.add(new SymbolLayer(dataSource,
            iconImage("my-icon")));
        });
    
    ```
    
    Después de agregar el fragmento de código anterior, su `MainActivity.java` debería tener un aspecto similar al siguiente:
    
    ```Java
    package com.example.myapplication;
    
    import android.app.Activity;
    import android.os.Bundle;
    import com.mapbox.geojson.Feature;
    import com.mapbox.geojson.Point;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;
    import static com.microsoft.azure.maps.mapcontrol.options.SymbolLayerOptions.iconImage;
    public class MainActivity extends AppCompatActivity {
        
        static{
                AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
            }
    
        MapControl mapControl;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapControl = findViewById(R.id.mapcontrol);
    
            mapControl.onCreate(savedInstanceState);
    
            mapControl.onReady(map -> {
    
                //Create a data source and add it to the map.
                DataSource dataSource = new DataSource();
                map.sources.add(dataSource);
            
                //Create a point feature and add it to the data source.
                dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
            
                //Add a custom image icon to the map resources.
                map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            
                //Create a symbol layer and add it to the map.
                map.layers.add(new SymbolLayer(dataSource,
                    iconImage("my-icon")));
            });
        }
    
        @Override
        public void onStart() {
            super.onStart();
            mapControl.onStart();
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }
    
        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }
    
        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }
    
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }
    }
    ```

Al ejecutar la aplicación, debería ver un marcador en el mapa tal como se muestra a continuación:

![Marcador en el mapa Android](./media/how-to-add-symbol-to-android-map/android-map-pin.png)

> [!TIP]
> De forma predeterminada, las capas de símbolos optimizan la representación de los símbolos, ya que ocultan los símbolos que se superponen. A medida que se acerca el zoom, los símbolos ocultos se hacen visibles. Para deshabilitar esta característica y representar todos los símbolos en todo momento, establezca la opción `iconAllowOverlap` en `true`.

## <a name="next-steps"></a>Pasos siguientes

Para agregar más datos al mapa, consulte:

> [!div class="nextstepaction"]
> [Adición de formas a un mapa Android](./how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [Presentación de la información de características](display-feature-information-android.md)