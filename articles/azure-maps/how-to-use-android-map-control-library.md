---
title: Introducción a Android SDK para Azure Maps
description: Familiarícese con Android SDK de Microsoft Azure Maps. Vea cómo crear un proyecto en Android Studio, instalar el SDK y crear un mapa interactivo.
author: anastasia-ms
ms.author: v-stharr
ms.date: 11/18/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 1da003bb8d285dbedde87cbc6cd4708fda2dc38b
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96531268"
---
# <a name="getting-started-with-azure-maps-android-sdk"></a>Introducción a Android SDK para Azure Maps

Android SDK para Azure Maps es una biblioteca de mapas vectoriales para Android. En este artículo se ofrece orientación sobre los procesos para instalar Android SDK para Azure Maps y cargar un mapa.

## <a name="prerequisites"></a>Prerrequisitos

### <a name="create-an-azure-maps-account"></a>Crear una cuenta de Azure Maps

1. [Cree una cuenta de Azure Maps](quick-demo-map-app.md#create-an-azure-maps-account).
2. [Obtenga una clave de suscripción principal](quick-demo-map-app.md#get-the-primary-key-for-your-account), también conocida como clave principal o clave de suscripción.
Para más información sobre la autenticación en Azure Maps, consulte [Administración de la autenticación en Azure Maps](./how-to-manage-authentication.md).
3. [Descarga e instalación de Android Studio de Google.](https://developer.android.com/studio/)

## <a name="create-a-project-in-android-studio"></a>Creación de un proyecto en Android Studio

Siga estos pasos para crear un proyecto de Android Studio:

1. Inicie Android Studio.
2. Haga clic en **+ Create New Project** (+ Crear nuevo proyecto).
3. En la pestaña **Phone and Tablet** (Teléfono y tableta), haga clic en **Empty Activity** (Actividad vacía). Haga clic en **Siguiente**.
4. En **Configure your project** (Configurar proyecto), seleccione `API 21: Android 5.0.0 (Lollipop)` como el SDK mínimo.
5. Seleccione `Java` como idioma.
6. Acepte la opción `Name` predeterminada para el proyecto. Haga clic en **Finalizar**

Consulte la [documentación de Android Studio](https://developer.android.com/studio/intro/) a fin de obtener más ayuda para la instalación de Android Studio y la creación de un proyecto.

![Creación de un proyecto en Android Studio ](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-device"></a>Configurar un dispositivo

Para probar la aplicación durante el desarrollo, puede usar un teléfono Android o un emulador de Android.

Para más información sobre cómo configurar un AVD (dispositivo virtual Android), consulte la [documentación de Android Studio](https://developer.android.com/studio/run/managing-avds).

## <a name="install-the-azure-maps-android-sdk"></a>Instalación de Android SDK para Azure Maps

El siguiente paso en la creación de la aplicación es instalar el Android SDK para Azure Maps.

Para instalar el SDK, siga estos pasos:

1. En la pestaña Project (Proyecto), expanda **Gradle Scripts** (Scripts de Gradle). Abra **build.gradle (Project: My_Application)** [build.gradle (Proyecto: My_Application)] y agregue el código siguiente a la sección **all projects** (todos los proyectos), sección `repositories`:

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Abra **build.gradle (Module: My_Application)** [build.gradle (Módulo: My_Application)].

3. Asegúrese de que el valor de **minSdkVersion** de la sección `defaultConfig` sea API 21 o superior.

4. Agregue el siguiente código a la sección Android:

    ```
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    ```

5. Agregue el siguiente código a la sección `dependencies`:

    ```
    implementation "com.microsoft.azure.maps:mapcontrol:0.6"
    ```

6. Haga clic en **File** (Archivo) en la barra de herramientas principal y seleccione **Sync Project with Gradle Files** (Sincronizar proyecto con archivos de Gradle).

7. Abra `res > layout > activity_main.xml`. Haga clic en la vista `Code` en la esquina superior derecha. Agregue el siguiente XML dentro del elemento `<androidx.constraintlayout.widget.ConstraintLayout>`.
    
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
            />
    </FrameLayout>
    ```

8. En el archivo `java > com.example.myapplication > MainActivity.java`, tendrá que:

    * agregar las importaciones para el SDK de Azure Maps.
    * establecer la información de autenticación de Azure Maps.
    * obtener la instancia del control de mapa en el método **onCreate**.

    Para evitar tener que agregar información de autenticación para cada vista de aplicación, estableceremos la información de autenticación globalmente llamando a `AzureMaps.setSubscriptionKey`. También puede llamar a `AzureMaps.setAadProperties`, si desea autenticarse con Azure Active Directory.

    El control de mapa invalida los siguientes métodos de ciclo de vida de la clase MainActivity. Estos métodos son responsables de administrar el ciclo de vida de OpenGL de Android.

    * onCreate(Bundle)
    * onStart()
    * onResume()
    * onPause()
    * onStop()
    * onDestroy()
    * onSaveInstanceState(Bundle)
    * onLowMemory()

    Edite el `MainActivity.java` como sigue:

    ```java
    package com.example.myapplication;

    //For older versions use: import android.support.v7.app.AppCompatActivity; 
    import androidx.appcompat.app.AppCompatActivity;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {

        static {
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);

            //Wait until the map resources are ready.
            mapControl.onReady(map -> {
                //Add your post map load code here.

            });
        }

        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }

        @Override
        protected void onStart(){
            super.onStart();
            mapControl.onStart();
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

>[!WARNING]
>Es posible que Android Studio no haya importado las clases necesarias.  En consecuencia, el código tendrá algunas referencias que no se pueden resolver. Para importar las clases necesarias, simplemente mantenga el mouse sobre cada referencia no resuelta y presione `Alt + Enter` (Opción + Retorno en un equipo Mac).

Android Studio tardará unos segundos en crear la aplicación. Una vez finalizada la compilación, puede probar la aplicación en el dispositivo Android emulado. Debería ver un mapa como el siguiente:

:::image type="content" source="./media/how-to-use-android-map-control-library/android-map.png" border="true" alt-text="Azure Maps en la aplicación de Android":::

## <a name="localizing-the-map"></a>Localización del mapa

Android SDK de Azure Maps proporciona tres formas diferentes de establecer el idioma y la configuración regional del mapa.

1. Establezca la configuración regional y de idioma llamando a los métodos estáticos en la clase AzureMaps.

    ```Java
    static {
        //Set your Azure Maps Key.
        AzureMaps.setSubscriptionKey("<Your Azure Maps Key>");

        //Set the language to be used by Azure Maps.
        AzureMaps.setLanguage("fr-FR");

        //Set the regional view.
        AzureMaps.setLanguage("Auto");
    
    }
    ```

2. Defina la configuración regional y de idioma en el XML del control de mapa.

    ```XML
    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/myMap"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_language="fr-FR"
        app:mapcontrol_view="Auto"
        />
    ```

3. Establezca la configuración regional y de idioma llamando a los métodos en el control de mapa. Esta opción permite cambiar la configuración durante el tiempo de ejecución.

    ```Java
    mapControl.onReady(map -> {
        map.setStyle(StyleOptions.language("fr-FR"));
        map.setStyle(StyleOptions.view("Auto"));
    
    });
    ```

Este es un ejemplo de Azure Maps con el idioma establecido en `fr-FR`.

:::image type="content" source="./media/how-to-use-android-map-control-library/android-localization.png" border="true" alt-text="Imagen del mapa de Azure Maps que muestra las etiquetas en francés":::

[Aquí](supported-languages.md) encontrará una lista completa de los idiomas y vistas regionales admitidos.

## <a name="navigating-the-map"></a>Navegación por el mapa

Hay varias maneras de ampliar, desplazar lateralmente, girar e inclinar el mapa. A continuación se detallan todas las formas de navegar por el mapa.

**Acercamiento/alejamiento del mapa**

- Toque el mapa con dos dedos y acérquelos para alejarse o aléjelos para acercarse.
- Pulse dos veces el mapa para acercarse un nivel.
- Pulse dos veces con dos dedos para alejar el mapa un nivel.
- Pulse dos veces; en la segunda pulsación, mantenga presionado el dedo en el mapa y arrástrelo hacia arriba o hacia abajo para acercar o alejar la imagen, respectivamente.

**Desplazamiento lateral del mapa**

- Toque el mapa y arrastre en cualquier dirección.

**Giro del mapa**

- Toque el mapa con dos dedos y gírelos.

**Inclinación del mapa**

- Toque el mapa con dos dedos y arrástrelos hacia arriba o hacia abajo.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo agregar datos de superposición en el mapa:

> [!div class="nextstepaction"]
> [Add a symbol layer to an Android map](how-to-add-symbol-to-android-map.md)(Adición de una capa de símbolo a un mapa Android)

> [!div class="nextstepaction"]
> [Adición de formas a un mapa Android](./how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [Cambio de estilos de mapa en mapas de Android](./set-android-map-styles.md)