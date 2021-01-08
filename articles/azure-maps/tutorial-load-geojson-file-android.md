---
title: 'Tutorial: Carga de datos GeoJSON en Android SDK de Azure Maps | Microsoft Azure Maps'
description: Tutorial sobre la carga de archivos de datos GeoJSON en Android SDK de Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 12/10/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: b527cd7b3f841b6cb3dcf2dce6930f3bd9bcc184
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681258"
---
# <a name="tutorial-load-geojson-data-into-azure-maps-android-sdk"></a>Tutorial: Carga de datos GeoJSON en Android SDK de Azure Maps

Este tutorial le guiará por el proceso de importación de un archivo GeoJSON de datos de ubicación en Android SDK de Azure Maps. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Incorporar Azure Maps a una aplicación para Android.
> * Crear un origen de datos y cargarlo en un archivo GeoJSON desde un archivo local o desde la web.
> * Mostrar los datos en el mapa.

## <a name="prerequisites"></a>Prerrequisitos

1. Complete la guía [Inicio rápido: Creación de una aplicación para Android](quick-android-map.md). En este tutorial se ampliará el código de este inicio rápido.
2. Descargue los [puntos de interés de ejemplo](https://raw.githubusercontent.com/Azure-Samples/AzureMapsCodeSamples/master/AzureMapsCodeSamples/Common/data/geojson/SamplePoiDataSet.json) del archivo GeoJSON.

### <a name="import-geojson-data-from-web-or-assets-folder"></a>Importación de los datos GeoJSON de la web o de la carpeta de recursos

La mayoría de los archivos GeoJSON encapsulan todos los datos en `FeatureCollection`. Teniendo esto en cuenta, si los archivos GeoJSON se cargan en la aplicación como cadena, se pueden pasar al método estático `fromJson` de la colección de características, que deserializará la cadena en un objeto `FeatureCollection` de GeoJSON que se puede agregar al mapa.

En los pasos siguientes se muestra cómo importar un archivo GeoJSON en la aplicación y deserializarlo como objeto `FeatureCollection` de GeoJSON.

1. Complete la guía [Inicio rápido: Creación de una aplicación para Android](quick-android-map.md), ya que los pasos siguientes se basan en el uso de esta aplicación.
2. En el panel del proyecto de Android Studio, haga clic con el botón derecho en la carpeta **app** y vaya a `New > Folder > Assets Folder`.
3. Arrastre y coloque los [puntos de interés de ejemplo](https://raw.githubusercontent.com/Azure-Samples/AzureMapsCodeSamples/master/AzureMapsCodeSamples/Common/data/geojson/SamplePoiDataSet.json) del archivo GeoJSON en la carpeta de recursos.
4. Cree un archivo llamado **Utils.java** y agregue el código siguiente a este archivo. Este código proporciona un método estático denominado `importData` que importa de forma asincrónica un archivo de la carpeta `assets` de la aplicación o de la web mediante una dirección URL como cadena y lo devuelve al subproceso de la interfaz de usuario mediante un método de devolución de llamada simple.

    ```java
    //Modify the package name as needed to align with your application.
    package com.example.myapplication;
    
    import android.content.Context;
    import android.os.Handler;
    import android.os.Looper;
    import android.webkit.URLUtil;
    
    import java.io.BufferedReader;
    import java.io.IOException;
    import java.io.InputStream;
    import java.io.InputStreamReader;
    import java.net.URL;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    
    import javax.net.ssl.HttpsURLConnection;
    
    public class Utils {
    
        interface SimpleCallback {
            void notify(String result);
        }
    
        /**
         * Imports data from a web url or asset file name and returns it to a callback.
         * @param urlOrFileName A web url or asset file name that points to data to load.
         * @param context The context of the app.
         * @param callback The callback function to return the data to.
         */
        public static void importData(String urlOrFileName, Context context, SimpleCallback callback){
            importData(urlOrFileName, context, callback, null);
        }
        
        /**
         * Imports data from a web url or asset file name and returns it to a callback.
         * @param urlOrFileName A web url or asset file name that points to data to load.
         * @param context The context of the app.
         * @param callback The callback function to return the data to.
         * @param error A callback function to return errors to.
         */
        public static void importData(String urlOrFileName, Context context, SimpleCallback callback, SimpleCallback error){
            if(urlOrFileName != null && callback != null) {
                ExecutorService executor = Executors.newSingleThreadExecutor();
                Handler handler = new Handler(Looper.getMainLooper());
    
                executor.execute(() -> {
                    String data = null;
    
                    try {
    
                        if(URLUtil.isNetworkUrl(urlOrFileName)){
                            data = importFromWeb(urlOrFileName);
                        } else {
                            //Assume file is in assets folder.
                            data = importFromAssets(context, urlOrFileName);
                        }
    
                        final String result = data;
    
                        handler.post(() -> {
                            //Ensure the resulting data string is not null or empty.
                            if (result != null && !result.isEmpty()) {
                                callback.notify(result);
                            } else {
                                error.notify("No data imported.");
                            }
                        });
                    } catch(Exception e) {
                        if(error != null){
                            error.notify(e.getMessage());
                        }
                    }
                });
            }
        }
    
        /**
         * Imports data from an assets file as a string.
         * @param context The context of the app.
         * @param fileName The asset file name.
         * @return
         * @throws IOException
         */
        private static String importFromAssets(Context context, String fileName) throws IOException {
            InputStream stream = null;
    
            try {
                stream = context.getAssets().open(fileName);
    
                if(stream != null) {
                    return readStreamAsString(stream);
                }
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                // Close Stream and disconnect HTTPS connection.
                if (stream != null) {
                    stream.close();
                }
            }
    
            return null;
        }
    
        /**
         * Imports data from the web as a string.
         * @param url URL to the data.
         * @return
         * @throws IOException
         */
        private static String importFromWeb(String url) throws IOException {
            InputStream stream = null;
            HttpsURLConnection connection = null;
            String result = null;
    
            try {
                connection = (HttpsURLConnection) new URL(url).openConnection();
    
                //For this use case, set HTTP method to GET.
                connection.setRequestMethod("GET");
    
                //Open communications link (network traffic occurs here).
                connection.connect();
    
                int responseCode = connection.getResponseCode();
                if (responseCode != HttpsURLConnection.HTTP_OK) {
                    throw new IOException("HTTP error code: " + responseCode);
                }
    
                //Retrieve the response body as an InputStream.
                stream = connection.getInputStream();
    
                if (stream != null) {
                    return readStreamAsString(stream);
                }
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                // Close Stream and disconnect HTTPS connection.
                if (stream != null) {
                    stream.close();
                }
                if (connection != null) {
                    connection.disconnect();
                }
            }
    
            return result;
        }
    
        /**
         * Reads an input stream as a string.
         * @param stream Stream to convert.
         * @return
         * @throws IOException
         */
        private static String readStreamAsString(InputStream stream) throws IOException {
            //Convert the contents of an InputStream to a String.
            BufferedReader in = new BufferedReader(new InputStreamReader(stream, "UTF-8"));
    
            String inputLine;
            StringBuffer response = new StringBuffer();
    
            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
    
            in.close();
    
            return response.toString();
        }
    }
    ```

5. Vaya al archivo **MainActivity.java** y agregue el código siguiente a la devolución de llamada para el evento `mapControl.onReady`, que se encuentra en el método `onCreate`. Este código usa la utilidad de importación para leer el archivo **SamplePoiDataSet.json** como cadena y lo deserializa como colección de características mediante el método `fromJson` estático de la clase `FeatureCollection`. Este código también calcula el área del rectángulo de selección para todos los datos de la colección de características y la usa para hacer que la cámara del mapa se centre en los datos.

    ```java
    //Create a data source and add it to the map.
    DataSource source = new DataSource();
    map.sources.add(source);
    
    //Import the GeoJSON data and add it to the data source.
    Utils.importData("SamplePoiDataSet.json",
        this,
        (String result) -> {
            //Parse the data as a GeoJSON Feature Collection.
            FeatureCollection fc = FeatureCollection.fromJson(result);
    
            //Add the feature collection to the data source.
            source.add(fc);
    
            //Optionally, update the maps camera to focus in on the data.
    
            //Calculate the bounding box of all the data in the Feature Collection.
            BoundingBox bbox = MapMath.fromData(fc);
    
            //Update the maps camera so it is focused on the data.
            map.setCamera(
                bounds(bbox),

                //Padding added to account for pixel size of rendered points.
                padding(20)
            );
        });
    ```

6. Ahora que hay código para cargar los datos GeoJSON en el mapa con un origen de datos, debemos especificar cómo deben mostrarse los datos en el mapa. Hay varias capas de representación diferentes para los datos de punto; la [capa de burbujas](map-add-bubble-layer-android.md), la [capa de símbolos](how-to-add-symbol-to-android-map.md) y la [capa de mapas térmicos](map-add-heat-map-layer-android.md) son los niveles más frecuentes. Agregue el código siguiente para representar los datos de una capa de burbujas en la devolución de llamada para el evento `mapControl.onReady` después del código para importar los datos.

    ```java
    //Create a layer and add it to the map.
    BubbleLayer layer = new BubbleLayer(source);
    map.layers.add(layer);
    ```

7. Ejecute la aplicación. Se mostrará un mapa centrado en EE UU., con círculos superpuestos para cada ubicación en el archivo GeoJSON.

    ![Mapa de EE. UU. con datos de un archivo GeoJSON mostrados.](media/tutorial-load-geojson-file-android/android-import-geojson.png)


## <a name="clean-up-resources"></a>Limpieza de recursos

Siga estos pasos para limpiar los recursos de este tutorial:

1. Cierre Android Studio y elimine la aplicación que ha creado.
2. Si ha probado la aplicación en un dispositivo externo, desinstálela en ese dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Para ver más ejemplos de código y una experiencia interactiva de codificación:

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Presentación de la información de características](display-feature-information-android.md)

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Adición de una capa de línea](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de polígono](how-to-add-shapes-to-android-map.md)
