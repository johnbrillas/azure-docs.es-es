---
title: 'Inicio rápido: Biblioteca de cliente de Computer Vision para Java'
description: En este inicio rápido, se muestra una introducción a la biblioteca cliente de Computer Vision para Java.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/15/2020
ms.custom: devx-track-java
ms.author: pafarley
ms.openlocfilehash: 58266e1d41344baf042349ed8b0ca9974a966827
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103622277"
---
<a name="HOLTop"></a>

La biblioteca cliente de Computer Vision se usa para:

* Analizar una imagen para ver las etiquetas, la descripción de texto, las caras, el contenido para adultos, etc.
* Lea texto impreso y manuscrito con Read API.

[Documentación de referencia](/java/api/overview/azure/cognitiveservices/client/computervision) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cognitiveservices/ms-azure-cs-computervision) |[Artifact (Maven)](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-computervision) | [Ejemplos](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/).
* La última versión de [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* La [herramienta de compilación de Gradle](https://gradle.org/install/) u otro administrador de dependencias.
* Una vez que tenga la suscripción de Azure, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="Creación de un recurso de Computer Vision"  target="_blank">cree un recurso de Computer Vision </a> en Azure Portal para obtener la clave y el punto de conexión. Una vez que se implemente, haga clic en **Ir al recurso**.
    * Necesitará la clave y el punto de conexión del recurso que cree para conectar la aplicación al servicio Computer Vision. En una sección posterior de este mismo inicio rápido pegará la clave y el punto de conexión en el código siguiente.
    * Puede usar el plan de tarifa gratis (`F0`) para probar el servicio y actualizarlo más adelante a un plan de pago para producción.

## <a name="setting-up"></a>Instalación

### <a name="create-a-new-gradle-project"></a>Creación de un proyecto de Gradle

En una ventana de la consola (como cmd, PowerShell o Bash), cree un directorio para la aplicación y vaya a él. 

```console
mkdir myapp && cd myapp
```

Ejecute el comando `gradle init` desde el directorio de trabajo. Este comando creará archivos de compilación esenciales para Gradle, como *build.gradle.kts*, que se usa en tiempo de ejecución para crear y configurar la aplicación.

```console
gradle init --type basic
```

Cuando se le solicite que elija un **DSL**, seleccione **Kotlin**.

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

En este inicio rápido se usa el administrador de dependencias Gradle. Puede encontrar la biblioteca de cliente y la información de otros administradores de dependencias en el [repositorio central de Maven](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-computervision).

Busque *build.gradle.kts* y ábralo con el IDE o el editor de texto que prefiera. A continuación, cópielo en la siguiente configuración de compilación. Esta configuración define el proyecto como una aplicación Java cuyo punto de entrada es la clase **ComputerVisionQuickstarts**. Importa la biblioteca de Computer Vision.

```kotlin
plugins {
    java
    application
}
application { 
    mainClassName = "ComputerVisionQuickstarts"
}
repositories {
    mavenCentral()
}
dependencies {
    compile(group = "com.microsoft.azure.cognitiveservices", name = "azure-cognitiveservices-computervision", version = "1.0.4-beta")
}
```

### <a name="create-a-java-file"></a>Creación de un archivo Java

En el directorio de trabajo, ejecute el siguiente comando para crear una carpeta de origen del proyecto:

```console
mkdir -p src/main/java
```

> [!TIP]
> ¿Desea ver todo el archivo de código de inicio rápido de una vez? Puede encontrarlo en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java), que contiene los ejemplos de código de este inicio rápido.

Vaya a la nueva carpeta y cree un archivo denominado *ComputerVisionQuickstarts.java*. Ábralo en el editor o IDE preferidos.

### <a name="find-the-subscription-key-and-endpoint"></a>Búsqueda de la clave de suscripción y el punto de conexión

Vaya a Azure Portal. Si el recurso de Computer Vision que ha creado en la sección **Requisitos previos** se ha implementado correctamente, haga clic en el botón **Ir al recurso** en **Pasos siguientes**. Las claves de suscripción y el punto de conexión se encuentran en la página **Clave y punto de conexión** del recurso, en **Administración de recursos**. 

Defina la clase **ComputerVisionQuickstarts**. Cree variables para la clave de suscripción y el punto de conexión de Computer Vision. Pegue la clave de suscripción y el punto de conexión en el código siguiente, donde se indica. El punto de conexión de Computer Vision tiene el formato `https://<your_computer_vision_resource_name>.cognitiveservices.azure.com/`.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_imports_and_vars)]

> [!IMPORTANT]
> Recuerde quitar la clave de suscripción del código cuando haya terminado y nunca la haga pública. En el caso de producción, considere la posibilidad de usar alguna forma segura de almacenar las credenciales, y acceder a ellas. Por ejemplo, [Azure Key Vault](../../../../key-vault/general/overview.md).

En el método **main** de la aplicación, agregue llamadas para los métodos que se usan en este inicio rápido. Se definirán más adelante.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_maincalls)]

> [!div class="nextstepaction"]
> [He configurado el cliente](?success=set-up-client#object-model) [He tenido un problema](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=Java&Section=set-up-client)

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales del SDK de Java para Computer Vision.

|Nombre|Descripción|
|---|---|
| [ComputerVisionClient](/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient) | Esta clase es necesaria para todas las funcionalidades de Computer Vision. Cree una instancia de ella con la información de suscripción y úsela para generar instancias de otras clases.|
|[ComputerVision](/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervision)| Esta clase procede del objeto de cliente y controla directamente todas las operaciones de imagen, como el análisis de imágenes, la detección de texto y la generación de miniaturas.|
|[VisualFeatureTypes](/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes)| Esta enumeración define los diferentes tipos de análisis de imágenes que se pueden realizar en una operación de análisis estándar. Debe especificar un conjunto de valores de VisualFeatureTypes en función de sus necesidades. |

## <a name="code-examples"></a>Ejemplos de código

En estos fragmentos de código se muestra cómo realizar las siguientes tareas con la biblioteca de cliente de Computer Vision para Java:

* [Autenticar el cliente](#authenticate-the-client)
* [Analizar una imagen](#analyze-an-image)
* [Leer texto manuscrito e impreso](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>Autenticar el cliente

En un nuevo método, cree una instancia de un objeto [ComputerVisionClient](/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient) con su punto de conexión y clave.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_auth)]

> [!div class="nextstepaction"]
> [He autenticado el cliente](?success=authenticate-client#analyze-an-image) [He tenido un problema](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=Java&Section=authenticate-client)

## <a name="analyze-an-image"></a>Análisis de una imagen

El código siguiente define un método, `AnalyzeLocalImage`, que utiliza el objeto de cliente para analizar una imagen local e imprimir los resultados. El método devuelve una descripción de texto, categorización, lista de etiquetas, caras detectadas, marcas de contenido para adultos, colores principales y tipo de imagen.

> [!TIP]
> También puede analizar una imagen remota mediante su dirección URL. Consulte los métodos [ComputerVision](/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervision), como **AnalyzeImage**. O bien, consulte el código de ejemplo en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java) para escenarios relacionados con imágenes remotas.

### <a name="set-up-test-image"></a>Configuración de una imagen de prueba

En primer lugar, cree una carpeta **recursos/** en la carpeta **src /main/** del proyecto y agregue una imagen que desee analizar. A continuación, agregue la siguiente definición de método a la clase **ComputerVisionQuickstarts**. Cambie el valor de `pathToLocalImage` para que coincida con el archivo de imagen. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_refs)]

### <a name="specify-visual-features"></a>Especificación de características visuales

A continuación, especifique qué características visuales desea extraer en el análisis. Vea la enumeración [VisualFeatureTypes](/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes) para obtener una lista completa.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_features)]

### <a name="analyze"></a>Analizar
Este bloque imprime resultados detallados de cada ámbito del análisis de imágenes en la consola. El método **analyzeImageInStream** devuelve un objeto **ImageAnalysis** que contiene toda la información extraída.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_analyze)]

En las secciones siguientes se muestra cómo analizar esta información en detalle.

### <a name="get-image-description"></a>Obtención de la descripción de la imagen

El código siguiente obtiene la lista de títulos generados para la imagen. Para más información, consulte [Descripción de imágenes](../../concept-describing-images.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_captions)]

### <a name="get-image-category"></a>Obtención de la categoría de imagen

El código siguiente obtiene la categoría detectada de la imagen. Para más información, consulte [Categorización de imágenes](../../concept-categorizing-images.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_category)]

### <a name="get-image-tags"></a>Obtención de etiquetas de imagen

El código siguiente obtiene el conjunto de las etiquetas detectadas en la imagen. Para más información, consulte [Etiquetas de contenido](../../concept-tagging-images.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_tags)]

### <a name="detect-faces"></a>Detección de caras

El código siguiente devuelve las caras detectadas en la imagen con sus coordenadas de rectángulo y selecciona los atributos de cara. Para más información, consulte [Detección de caras](../../concept-detecting-faces.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_faces)]

### <a name="detect-objects"></a>Detección de objetos

El código siguiente devuelve los objetos detectados en la imagen con sus coordenadas. Para más información, consulte [Detección de objetos](../../concept-object-detection.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_objects)]


### <a name="detect-brands"></a>Detección de marcas

El código siguiente devuelve los logotipos de marca detectados en la imagen con sus coordenadas. Para más información, consulte [Detección de marcas](../../concept-brand-detection.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_brands)]



### <a name="detect-adult-racy-or-gory-content"></a>Detección de contenido para adultos, explícito o sangriento

El siguiente código imprime la presencia detectada de contenido para adultos en la imagen. Para más información, consulte [Contenido para adultos, subido de tono y sangriento](../../concept-detecting-adult-content.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_adult)]

### <a name="get-image-color-scheme"></a>Obtención de la combinación de colores de imagen

El código siguiente imprime los atributos de color detectados en la imagen, como los colores dominantes y el color de énfasis. Para más información, consulte [Combinaciones de colores](../../concept-detecting-color-schemes.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_colors)]

### <a name="get-domain-specific-content"></a>Obtención de contenido específico del dominio

Computer Vision puede usar un modelo especializado para realizar análisis adicionales en las imágenes. Para más información, consulte [Contenido específico del dominio](../../concept-detecting-domain-content.md). 

En el código siguiente se analizan los datos sobre las celebridades detectadas en la imagen.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_celebrities)]

En el código siguiente se analizan los datos sobre los paisajes detectados en la imagen.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_landmarks)]

### <a name="get-the-image-type"></a>Obtención del tipo de imagen

El código siguiente imprime información sobre el tipo de imagen (si es una imagen prediseñada o dibujo lineal).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_imagetype)]

> [!div class="nextstepaction"]
> [He analizado una imagen](?success=analyze-image#read-printed-and-handwritten-text) [He tenido un problema](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=Java&Section=analyze-image)

### <a name="close-out-the-method"></a>Cierre del método

Complete el bloque try/catch y cierre el método.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyze_catch)]


## <a name="read-printed-and-handwritten-text"></a>Lectura de texto manuscrito e impreso

Computer Vision puede leer texto visible de una imagen y convertirlo en un flujo de caracteres. En esta sección se define un método, `ReadFromFile`, que toma una ruta de acceso de archivo local e imprime el texto de la imagen en la consola.

> [!TIP]
> También puede leer el texto de una imagen remota referenciada por una dirección URL. Consulte los métodos [ComputerVision](/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervision), como **read**. O bien, consulte el código de ejemplo en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java) para escenarios relacionados con imágenes remotas.

### <a name="set-up-test-image"></a>Configuración de una imagen de prueba

Cree una carpeta **resources/** en la carpeta **src/main/** del proyecto y agregue una imagen de la que quiera leer texto. Puede descargar una [imagen de ejemplo](https://raw.githubusercontent.com/MicrosoftDocs/azure-docs/master/articles/cognitive-services/Computer-vision/Images/readsample.jpg) para usarla.

A continuación, agregue la siguiente definición de método a la clase **ComputerVisionQuickstarts**. Cambie el valor de `localFilePath` para que coincida con el archivo de imagen. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_setup)]

### <a name="call-the-read-api"></a>Llamada a la API Read

A continuación, agregue el siguiente código para llamar al método **readInStreamWithServiceResponseAsync** de la imagen especificada.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_call)]

El siguiente bloque de código extrae el identificador de la operación de la respuesta de la llamada de lectura. Este identificador se usa con un método auxiliar para imprimir los resultados de lectura de texto en la consola. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_response)]

Cierre el bloque try/catch y la definición del método.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_catch)]

### <a name="get-read-results"></a>Obtención de resultados de lectura

A continuación, agregue una definición para el método auxiliar. Este método usa el identificador de la operación del paso anterior para consultar la operación de lectura y obtener los resultados de OCR cuando están disponibles.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_result_helper_call)]

El resto del método analiza los resultados de OCR y los imprime en la consola.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_result_helper_print)]

Por último, agregue el otro método auxiliar usado anteriormente, que extrae el identificador de la operación de la respuesta inicial.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_opid_extract)]

> [!div class="nextstepaction"]
> [He leído un texto](?success=read-printed-handwritten-text#run-the-application) [He tenido un problema](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=Java&Section=read-printed-handwritten-text)

## <a name="run-the-application"></a>Ejecución de la aplicación

Puede compilar la aplicación con:

```console
gradle build
```

Ejecute la aplicación con el comando `gradle run`:

```console
gradle run
```

> [!div class="nextstepaction"]
> [He ejecutado la aplicación](?success=run-the-application#clean-up-resources) [He tenido un problema](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=Java&Section=run-the-application)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

> [!div class="nextstepaction"]
> [He limpiado los recursos](?success=clean-up-resources#next-steps) [He tenido un problema](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=Java&Section=clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a usar la biblioteca de Computer Vision para Java para realizar tareas básicas. A continuación, consulte la documentación de referencia para más información sobre la biblioteca.

> [!div class="nextstepaction"]
>[Referencia de Computer Vision (Java)](/java/api/overview/azure/cognitiveservices/client/computervision)


* [¿Qué es Computer Vision?](../../overview.md)
* El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java).
