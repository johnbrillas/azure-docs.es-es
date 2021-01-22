---
title: 'Tutorial: Extracción masiva de datos de formularios con Azure Data Factory: Form Recognizer'
titleSuffix: Azure Cognitive Services
description: Configure actividades de Azure Data Factory que desencadenen el entrenamiento y la ejecución de modelos de Form Recognizer para digitalizar una gran cantidad de trabajo pendiente de documentos.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: tutorial
ms.date: 01/04/2021
ms.author: pafarley
ms.openlocfilehash: 1780aebc113fa68a9a89cfce9fd67c9b5911fc58
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98606706"
---
# <a name="tutorial-extract-form-data-in-bulk-using-azure-data-factory"></a>Tutorial: Extracción masiva de datos de formularios mediante Azure Data Factory

En este tutorial se explica cómo usar los servicios de Azure para ingerir un gran lote de formularios en medios digitales. En este tutorial se muestra cómo automatizar la ingesta de datos de un lago de datos de documentos de Azure en una base de datos de Azure SQL. Podrá entrenar rápidamente modelos y procesar nuevos documentos con solo unos pocos clics.

## <a name="business-need"></a>Necesidad empresarial

La mayoría de las organizaciones saben ya lo valioso que son los datos que tienen en diferentes formatos (PDF, imágenes y vídeos). Buscan los procedimientos recomendados y las maneras más rentables de digitalizar esos recursos.

Además, nuestros clientes suelen tener distintos tipos de formularios procedentes a su vez de sus propios clientes. A diferencia de los [inicios rápidos](./quickstarts/client-library.md), en este tutorial se muestra cómo entrenar automáticamente un modelo con tipos nuevos y distintos de formularios mediante un enfoque basado en metadatos. Si no tiene un modelo ya existente para el tipo de formulario dado, el sistema lo creará y le proporcionará el identificador del mismo. 

Mediante la extracción de los datos de los formularios y su combinación con los sistemas operativos y almacenes de datos existentes, las empresas pueden obtener información y aportar valor a sus clientes y usuarios empresariales.

Con Azure Form Recognizer, ayudamos a las organizaciones a aprovechar sus datos, automatizar procesos (pagos de facturas, procesamiento de impuestos, etc.), ahorrar tiempo y dinero, y disfrutar de una mejor precisión de los datos.

En este tutorial aprenderá a:

> [!div class="checklist"]
> * Configurar un lago de datos de Azure para almacenar los formularios
> * Usar una base de datos de Azure para crear una tabla de parametrización
> * Usar Azure Key Vault para almacenar credenciales confidenciales
> * Entrenar el modelo de Form Recognizer en un cuaderno de Databricks
> * Extraer los datos del formulario mediante un cuaderno de Databricks
> * Automatizar el entrenamiento y la extracción de formularios con Azure Data Factory

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/)
* Una vez que tenga la suscripción de Azure, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer"  title="cree un recurso de Form Recognizer"  target="_blank">create a Form Recognizer resource <span class="docon docon-navigate-external x-hidden-focus"></span></a> en Azure Portal para obtener la clave y el punto de conexión. Tras su implementación, seleccione **Ir al recurso**.
    * Necesitará la clave y el punto de conexión del recurso que cree para conectar la aplicación a la API Form Recognizer. En una sección posterior de este mismo inicio rápido pegará la clave y el punto de conexión en el código siguiente.
    * Puede usar el plan de tarifa gratis (`F0`) para probar el servicio y actualizarlo más adelante a un plan de pago para producción.
* Un conjunto de al menos cinco formularios del mismo tipo. Idealmente, este flujo de trabajo está pensado para admitir grandes conjuntos de documentos. Consulte [Creación de un conjunto de datos de aprendizaje](./build-training-data-set.md) para ver sugerencias y opciones para reunir el conjunto de datos de aprendizaje. En este tutorial puede usar los archivos de la carpeta **Entrenar** del [conjunto de datos de ejemplo](https://go.microsoft.com/fwlink/?linkid=2128080).

## <a name="project-architecture"></a>Arquitectura de proyecto 

Este proyecto representa un conjunto de canalizaciones de Azure Data Factory que desencadenan cuadernos de Python que entrenan, analizan y extraen datos de los documentos de una cuenta de Azure Data Lake Storage.

La API REST Form Recognizer requiere algunos parámetros como entrada. Por motivos de seguridad, algunos de estos parámetros se almacenarán en una instancia de Azure Key Vault, mientras que otros parámetros menos confidenciales, como el nombre de la carpeta de blobs de almacenamiento, se almacenarán en una tabla de parametrización de una base de datos de Azure SQL.

Para el tipo de formulario que se va a analizar, los ingenieros de datos o los científicos de datos rellenarán una fila de la tabla de parámetros. Después, usarán Azure Data Factory para iterar por la lista de tipos de formulario detectados y pasar los parámetros pertinentes a un cuaderno de Databricks para entrenar o volver a entrenar los modelos de Form Recognizer. También se puede usar una función de Azure aquí.

A continuación, el cuaderno de Azure Databricks usa los modelos entrenados para extraer los datos del formulario y exportarlos a una base de datos de Azure SQL.

:::image type="content" source="./media/tutorial-bulk-processing/architecture.png" alt-text="arquitectura de proyecto":::


## <a name="set-up-azure-data-lake"></a>Configuración de Azure Data Lake Storage

El trabajo pendiente de formularios puede estar en el entorno local o en un servidor FTP. En este tutorial se usan los formularios de una cuenta de Azure Data Lake Storage Gen 2. Puede transferir los archivos allí mediante Azure Data Factory, Explorador de Azure Storage o AzCopy. Los conjuntos de datos de entrenamiento y puntuación pueden estar en diferentes contenedores, pero los conjuntos de datos de entrenamiento de todos los tipos de formularios deben estar en el mismo contenedor (aunque pueden estar en carpetas diferentes).

Para crear un lago de datos, siga las instrucciones de [Creación de una cuenta de almacenamiento para su uso con Azure Data Lake Storage Gen2 habilitado](https://docs.microsoft.com/azure/storage/blobs/create-data-lake-storage-account).

## <a name="create-a-parameterization-table"></a>Creación de una tabla de parametrización

A continuación, vamos a crear una tabla de metadatos en una base de datos de Azure SQL Esta tabla contendrá los datos no confidenciales que requiere la API REST Form Recognizer. Siempre que haya un nuevo tipo de formulario en nuestro conjunto de datos, insertaremos un nuevo registro en esta tabla y desencadenaremos la canalización de entrenamiento y puntuación (que se implementará más adelante).

Se usarán los siguientes campos en la tabla:

* **form_description**: este campo no es necesario como parte del entrenamiento. Proporciona una descripción del tipo de formulario para el que se va a entrenar el modelo (por ejemplo, los "formularios del cliente A" o los "formularios del Hotel B").
* **training_container_name**: este campo es el nombre del contenedor de la cuenta de almacenamiento donde se ha almacenado el conjunto de datos de entrenamiento. Puede ser el mismo contenedor que el de **scoring_container_name**.
* **training_blob_root_folder**: la carpeta de la cuenta de almacenamiento donde se almacenarán los archivos para el entrenamiento del modelo.
* **scoring_container_name**: este campo es el nombre del contenedor de la cuenta de almacenamiento donde se han almacenado los archivos de los que se desea extraer los pares clave-valor. Puede ser el mismo contenedor que el de **training_container_name**.
* **scoring_input_blob_folder**: la carpeta de la cuenta de almacenamiento donde se almacenarán los archivos de los que se van a extraer los datos.
* **model_id**: el identificador del modelo que se desea volver a entrenar. En la primera ejecución, el valor debe establecerse en -1, lo que hará que el cuaderno de entrenamiento cree un nuevo modelo personalizado para entrenar. El cuaderno de entrenamiento devolverá el identificador del modelo recién creado a la instancia de Azure Data Factory y, mediante una actividad de procedimiento almacenado, se actualizará este valor en la base de datos de Azure SQL.

  Siempre que desee ingerir un nuevo tipo de formulario, deberá restablecer manualmente el identificador del modelo en -1 antes de entrenar el modelo.

* **file_type**: Los tipos de formulario admitidos son `application/pdf`, `image/jpeg`, `image/png` y `image/tif`.

  Si tiene formularios con tipos de archivo diferentes, deberá cambiar este valor y el de **model_id** al entrenar un nuevo tipo de formulario.
* **form_batch_group_id**: Con el tiempo, es posible que tenga varios tipos de formulario para entrenar en el mismo modelo. El identificador **form_batch_group_id** le permitirá especificar todos los tipos de formulario que se han entrenado con un modelo específico.

### <a name="create-the-table"></a>Creación de la tabla

[Cree una instancia de Azure SQL Database](https://ms.portal.azure.com/#create/Microsoft.SQLDatabase) y, a continuación, ejecute el siguiente script de SQL en el [editor de consultas](https://docs.microsoft.com/azure/azure-sql/database/connect-query-portal) para crear la tabla necesaria.

```sql
CREATE TABLE dbo.ParamFormRecogniser(
    form_description varchar(50) NULL,
  training_container_name varchar(50) NOT NULL,
    training_blob_root_folder varchar(50) NULL,
    scoring_container_name varchar(50) NOT NULL,
    scoring_input_blob_folder varchar(50) NOT NULL,
    scoring_output_blob_folder varchar(50) NOT NULL,
    model_id varchar(50) NULL,
    file_type varchar(50) NULL
) ON PRIMARY
GO
```

Ejecute el siguiente script para crear el procedimiento para actualizar automáticamente **model_id** una vez entrenado.

```SQL
CREATE PROCEDURE [dbo].[update_model_id] ( @form_batch_group_id  varchar(50),@model_id varchar(50))
AS
BEGIN 
    UPDATE [dbo].[ParamFormRecogniser]   
        SET [model_id] = @model_id  
    WHERE form_batch_group_id =@form_batch_group_id
END
```

## <a name="use-azure-key-vault-to-store-sensitive-credentials"></a>Usar Azure Key Vault para almacenar credenciales confidenciales

Por motivos de seguridad, no queremos almacenar cierta información confidencial en la tabla de parametrización de la base de datos de Azure SQL. Almacenaremos los parámetros confidenciales como secretos de Azure Key Vault.

### <a name="create-an-azure-key-vault"></a>Crear una instancia de Azure Key Vault

[Creación de un recurso de Key Vault](https://ms.portal.azure.com/#create/Microsoft.KeyVault). A continuación, vaya al recurso de Key Vault una vez creado y, en la sección **Configuración**, seleccione **Secretos** para agregar los parámetros.

Aparecerá una nueva ventana, seleccione **Generar o importar**. Escriba el nombre del parámetro y su valor, y haga clic en Crear. Hágalo con los siguientes parámetros:

* **CognitiveServiceEndpoint**: la dirección URL del punto de conexión de la API Form Recognizer.
* **CognitiveServiceSubscriptionKey**: la clave de acceso para el servicio Form Recognizer. 
* **StorageAccountName**: la cuenta de almacenamiento donde se almacenan el conjunto de datos de entrenamiento y los formularios de los que queremos extraer los pares clave-valor. Si están en cuentas diferentes, especifique cada uno de los nombres de cuenta como secretos independientes. Recuerde que los conjuntos de datos de entrenamiento deben estar en el mismo contenedor para todos los tipos de formulario, pero pueden estar en carpetas diferentes.
* **StorageAccountSasKey**: la firma de acceso compartido (SAS) de la cuenta de almacenamiento. Para recuperar la dirección URL de SAS, vaya al recurso de almacenamiento y seleccione la pestaña **Explorador de Storage**. Vaya al contenedor, haga clic con el botón derecho y seleccione **Obtener firma de acceso compartido**. Es importante obtener la firma de acceso compartido para el contenedor, no para la propia cuenta de almacenamiento. Asegúrese de que los permisos de **lectura** y **enumeración** están marcados y haga clic en **Create** (Crear). A continuación, copie el valor en la sección **URL**. Debe tener el formato `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

## <a name="train-your-form-recognizer-model-in-a-databricks-notebook"></a>Entrenar el modelo de Form Recognizer en un cuaderno de Databricks

Usará Azure Databricks para almacenar y ejecutar el código de Python que interactúa con el servicio Form Recognizer.

### <a name="create-a-notebook-in-databricks"></a>Creación de un cuaderno en Databricks

[Cree un recurso de Azure Databricks](https://ms.portal.azure.com/#create/Microsoft.Databricks) en Azure Portal. Vaya hasta el recurso una vez que se haya creado e inicie el área de trabajo.

### <a name="create-a-secret-scope-backed-by-azure-key-vault"></a>Creación de un ámbito de secreto respaldado por Azure Key Vault

Para hacer referencia a los secretos de la instancia de Azure Key Vault que hemos creado anteriormente, debe crear un ámbito secreto en Databricks. Siga los pasos que se indican en [Creación de un ámbito de secreto compatible con Azure Key Vault](https://docs.microsoft.com/azure/databricks/security/secrets/secret-scopes#--create-an-azure-key-vault-backed-secret-scope).

### <a name="create-a-databricks-cluster"></a>Creación de un clúster de Databricks

Un clúster es una colección de recursos de cálculo de Databricks. Para crear un clúster:

1. Haga clic en el botón **Clústeres** en la barra lateral.
1. En la página **Clústeres**, haga clic en **Crear clúster**.
1. En la página **Crear clúster**, especifique un nombre de clúster y seleccione **7.2 (Scala 2.12, Spark 3.0.0)** en el menú desplegable de versiones de Databricks Runtime.
1. Haga clic en **Create Cluster** (Crear clúster).

### <a name="write-a-settings-notebook"></a>Escritura de un cuaderno de configuración

Ahora está listo para agregar cuadernos de Python. En primer lugar, cree un cuaderno llamado **Settings**. Este cuaderno asignará los valores de la tabla de parametrización a las variables del script. Azure Data Factory pasará posteriormente los valores como parámetros. También asignaremos valores de los secretos de Key Vault a las variables. 

1. Para crear el cuaderno **Settings**, haga clic en el botón **Área de trabajo** y, en la nueva pestaña, haga clic en la lista desplegable y seleccione **Crear** y **Cuaderno**.
1. En la ventana emergente, escriba el nombre que desea dar al cuaderno y seleccione **Python** como lenguaje predeterminado. Seleccione el clúster de Databricks y seleccione **Crear**.
1. En la primera celda del cuaderno, se recuperan los parámetros que pasa Azure Data Factory.

    ```python 
    dbutils.widgets.text("form_batch_group_id", "","")
    dbutils.widgets.get("form_batch_group_id")
    form_batch_group_id = getArgument("form_batch_group_id")
    
    dbutils.widgets.text("model_id", "","")
    dbutils.widgets.get("model_id")
    model_id = getArgument("model_id")
    
    dbutils.widgets.text("training_container_name", "","")
    dbutils.widgets.get("training_container_name")
    training_container_name = getArgument("training_container_name")
    
    dbutils.widgets.text("training_blob_root_folder", "","")
    dbutils.widgets.get("training_blob_root_folder")
    training_blob_root_folder=  getArgument("training_blob_root_folder")
    
    dbutils.widgets.text("scoring_container_name", "","")
    dbutils.widgets.get("scoring_container_name")
    scoring_container_name=  getArgument("scoring_container_name")
    
    dbutils.widgets.text("scoring_input_blob_folder", "","")
    dbutils.widgets.get("scoring_input_blob_folder")
    scoring_input_blob_folder=  getArgument("scoring_input_blob_folder")
    
    
    dbutils.widgets.text("file_type", "","")
    dbutils.widgets.get("file_type")
    file_type = getArgument("file_type")
    
    dbutils.widgets.text("file_to_score_name", "","")
    dbutils.widgets.get("file_to_score_name")
    file_to_score_name=  getArgument("file_to_score_name")
    ```

1. En la segunda celda, se recuperan los secretos de Key Vault y se asignan a las variables.

    ```python 
    cognitive_service_subscription_key = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveserviceSubscriptionKey")
    cognitive_service_endpoint = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveServiceEndpoint")
    
    training_storage_account_name = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountName")
    storage_account_sas_key= dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountSasKey")  
    
    ScoredFile = file_to_score_name+ "_output.json"
    training_storage_account_url="https://"+training_storage_account_name+".blob.core.windows.net/"+training_container_name+storage_account_sas_key 
    ```

### <a name="write-a-training-notebook"></a>Escritura de un cuaderno de entrenamiento

Ahora que hemos completado la creación del cuaderno **Settings**, se puede crear un cuaderno para entrenar el modelo. Como se mencionó anteriormente, usaremos los archivos almacenados en una carpeta en una cuenta de almacenamiento de Azure Data Lake Gen 2 (**training_blob_root_folder**). El nombre de la carpeta se ha pasado como una variable. Cada conjunto de tipos de formulario estará en la misma carpeta y, a medida que se recorra la tabla de parámetros, el modelo se entrenará con todos los tipos de formulario. 

1. Cree un nuevo cuaderno llamado **TrainFormRecognizer**. 
1. En la primera celda, ejecute el cuaderno Settings:

    ```python
    %run "./Settings"
    ```

1. En la celda siguiente, asigne las variables del archivo **Settings** y entrene dinámicamente el modelo para cada tipo de formulario, aplicando el código del [inicio rápido de REST](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-train-extract.md#get-training-results%20).

    ```python
    import json
    import time
    from requests import get, post
    
    post_url = cognitive_service_endpoint + r"/formrecognizer/v2.0/custom/models"
    source = training_storage_account_url
    prefix= training_blob_root_folder
    
    includeSubFolders=True
    useLabelFile=False
    headers = {
        # Request headers
        'Content-Type': file_type,
        'Ocp-Apim-Subscription-Key': cognitive_service_subscription_key,
    }
    body =     {
        "source": source
        ,"sourceFilter": {
            "prefix": prefix,
            "includeSubFolders": includeSubFolders
       },
    }
    if model_id=="-1": # if you don't already have a model you want to retrain. In this case, we create a model and use it to extract the key-value pairs
      try:
          resp = post(url = post_url, json = body, headers = headers)
          if resp.status_code != 201:
              print("POST model failed (%s):\n%s" % (resp.status_code, json.dumps(resp.json())))
              quit()
          print("POST model succeeded:\n%s" % resp.headers)
          get_url = resp.headers["location"]
          model_id=get_url[get_url.index('models/')+len('models/'):]     
          
      except Exception as e:
          print("POST model failed:\n%s" % str(e))
          quit()
    else :# if you already have a model you want to retrain, we reuse it and (re)train with the new form types.  
      try:
        get_url =post_url+r"/"+model_id
          
      except Exception as e:
          print("POST model failed:\n%s" % str(e))
          quit()
    ```

1. El último paso del proceso de entrenamiento es obtener el resultado del entrenamiento en formato JSON.

    ```python
    n_tries = 10
    n_try = 0
    wait_sec = 5
    max_wait_sec = 5
    while n_try < n_tries:
        try:
            resp = get(url = get_url, headers = headers)
            resp_json = resp.json()
            print (resp.status_code)
            if resp.status_code != 200:
                print("GET model failed (%s):\n%s" % (resp.status_code, json.dumps(resp_json)))
                n_try += 1
                quit()
            model_status = resp_json["modelInfo"]["status"]
            print (model_status)
            if model_status == "ready":
                print("Training succeeded:\n%s" % json.dumps(resp_json))
                n_try += 1
                quit()
            if model_status == "invalid":
                print("Training failed. Model is invalid:\n%s" % json.dumps(resp_json))
                n_try += 1
                quit()
            # Training still running. Wait and retry.
            time.sleep(wait_sec)
            n_try += 1
            wait_sec = min(2*wait_sec, max_wait_sec)     
            print (n_try)
        except Exception as e:
            msg = "GET model failed:\n%s" % str(e)
            print(msg)
            quit()
    print("Train operation did not complete within the allocated time.")
    ```

## <a name="extract-form-data-using-a-notebook"></a>Extracción de datos del formulario mediante un cuaderno

### <a name="mount-the-azure-data-lake-storage"></a>Montaje de Azure Data Lake Storage

El siguiente paso es puntuar los diferentes formularios que hemos utilizado mediante el modelo entrenado. Se montará la cuenta de Azure Data Lake Storage en Databricks y se hará referencia al montaje durante el proceso de ingesta.

Al igual que en la fase de entrenamiento, se usa Azure Data Factory para invocar la extracción de los pares clave-valor de los formularios. Se recorrerán los formularios de las carpetas especificadas en la tabla de parámetros.

1. El cuaderno se crea para montar la cuenta de almacenamiento en Databricks. Lo llamaremos **MountDataLake**. 
1. Debe llamar primero al cuaderno **Settings**:

    ```python
    %run "./Settings"
    ```

1. En la segunda celda, se definirán variables para los parámetros confidenciales, los cuales se recuperarán de nuestros secretos de Key Vault.

    ```python
    cognitive_service_subscription_key = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveserviceSubscriptionKey")
    cognitive_service_endpoint = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveServiceEndpoint")
    
    scoring_storage_account_name = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountName")
    scoring_storage_account_sas_key= dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountSasKey")  
    
    scoring_mount_point = "/mnt/"+scoring_container_name 
    scoring_source_str = "wasbs://{container}@{storage_acct}.blob.core.windows.net/".format(container=scoring_container_name, storage_acct=scoring_storage_account_name) 
    scoring_conf_key = "fs.azure.sas.{container}.{storage_acct}.blob.core.windows.net".format(container=scoring_container_name, storage_acct=scoring_storage_account_name)
    
    ```

1. A continuación, se intentará desmontar la cuenta de almacenamiento en caso de que se hubiera montado previamente.

    ```python
    try:
      dbutils.fs.unmount(scoring_mount_point) # Use this to unmount as needed
    except:
      print("{} already unmounted".format(scoring_mount_point))
    
    ```

1. Por último, se montará la cuenta de almacenamiento.


    ```python
    try: 
      dbutils.fs.mount( 
        source = scoring_source_str, 
        mount_point = scoring_mount_point, 
        extra_configs = {scoring_conf_key: scoring_storage_account_sas_key} 
      ) 
    except Exception as e: 
      print("ERROR: {} already mounted. Run previous cells to unmount first".format(scoring_mount_point))
    
    ```

    > [!NOTE]
    > En este caso, solo se ha montado la cuenta de almacenamiento de entrenamiento; los archivos de entrenamiento y los archivos de los que se quieren extraer pares clave-valor se encuentran en la misma cuenta de almacenamiento. Si las cuentas de almacenamiento de puntuación y entrenamiento son diferentes, tendrá que montar ambas cuentas de almacenamiento aquí. 

### <a name="write-the-scoring-notebook"></a>Escritura del cuaderno de puntuación

Ahora se puede crear un cuaderno de puntuación. Al igual que con el cuaderno de entrenamiento, se usan los archivos almacenados en las carpetas de la cuenta de Azure Data Lake Storage que se acaba de montar. El nombre de la carpeta se pasa como una variable. Se recorren todos los formularios de la carpeta especificada y se extraen los pares clave-valor de ellos. 

1. Cree un nuevo cuaderno y llámelo **ScoreFormRecognizer**. 
1. Ejecute los cuadernos **Settings** y **MountDataLake**.

    ```python
    %run "./Settings"
    %run "./MountDataLake"
    ```

1. A continuación, agregue el código siguiente, que llama a la API [Analyze](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm).

    ```python
    ########### Python Form Recognizer Async Analyze #############
    import json
    import time
    from requests import get, post 
    
    
    #prefix= TrainingBlobFolder
    post_url = cognitive_service_endpoint + "/formrecognizer/v2.0/custom/models/%s/analyze" % model_id
    source = r"/dbfs/mnt/"+scoring_container_name+"/"+scoring_input_blob_folder+"/"+file_to_score_name
    output = r"/dbfs/mnt/"+scoring_container_name+"/scoringforms/ExtractionResult/"+os.path.splitext(os.path.basename(source))[0]+"_output.json"
    
    params = {
        "includeTextDetails": True
    }
    
    headers = {
        # Request headers
        'Content-Type': file_type,
        'Ocp-Apim-Subscription-Key': cognitive_service_subscription_key,
    }
    
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers, params = params)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % json.dumps(resp.json()))
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit() 
    ```

1. En la celda siguiente, se obtendrán los resultados de la extracción de los pares clave-valor. Esta celda generará el resultado. Dado que se desea el resultado en formato JSON para un mayor procesamiento en Azure SQL Database o Cosmos DB, el resultado se escribirá en un archivo .json. El nombre del archivo de salida será el nombre del archivo puntuado, concatenado con "_output.json". El archivo se almacenará en la misma carpeta que el archivo de origen.

    ```python
    n_tries = 10
    n_try = 0
    wait_sec = 5
    max_wait_sec = 5
    while n_try < n_tries:
       try:
           resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": cognitive_service_subscription_key})
           resp_json = resp.json()
           if resp.status_code != 200:
               print("GET analyze results failed:\n%s" % json.dumps(resp_json))
               n_try += 1
               quit()
           status = resp_json["status"]
           if status == "succeeded":
               print("Analysis succeeded:\n%s" % json.dumps(resp_json))
               n_try += 1
               quit()
           if status == "failed":
               print("Analysis failed:\n%s" % json.dumps(resp_json))
               n_try += 1
               quit()
           # Analysis still running. Wait and retry.
           time.sleep(wait_sec)
           n_try += 1
           wait_sec = min(2*wait_sec, max_wait_sec)     
       except Exception as e:
           msg = "GET analyze results failed:\n%s" % str(e)
           print(msg)
           n_try += 1
           print("Analyze operation did not complete within the allocated time.")
           quit()
    
    ```
1. Realice el procedimiento de escritura de archivo en una nueva celda:

    ```python
    import requests
    file = open(output, "w")
    file.write(str(resp_json))
    file.close()
    ```

## <a name="automate-training-and-scoring-with-azure-data-factory"></a>Automatización del entrenamiento y la puntuación con Azure Data Factory

El único paso que queda es configurar el servicio Azure Data Factory (ADF) para automatizar los procesos de entrenamiento y puntuación. En primer lugar, siga los pasos que se indican en [Crear una factoría de datos](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory). Después de crear el recurso de Azure Data Factory, tiene que crear tres canalizaciones: una para el entrenamiento y dos para la puntuación (se explica a continuación).

### <a name="training-pipeline"></a>Canalización de entrenamiento

La primera actividad de la canalización de entrenamiento es una búsqueda para leer y devolver los valores de la tabla de parametrización en la base de datos de Azure SQL. Dado que todos los conjuntos de datos de entrenamiento estarán en la misma cuenta de almacenamiento y contenedor (aunque posiblemente en carpetas diferentes), conservaremos el valor predeterminado del atributo **First row only** (Solo primera fila) en la configuración de la actividad de búsqueda. Para cada tipo de formulario en el que entrenar el modelo, se entrenará el modelo con todos los archivos de **training_blob_root_folder**.

:::image type="content" source="./media/tutorial-bulk-processing/training-pipeline.png" alt-text="Canalización de entrenamiento en Data Factory":::

El procedimiento almacenado toma dos parámetros: **model_id** y **form_batch_group_id**. El código para devolver el identificador del modelo del cuaderno de Databricks es `dbutils.notebook.exit(model_id)` y el código para leer el código de la actividad de procedimiento almacenado en Data Factory es `@activity('GetParametersFromMetadaTable').output.firstRow.form_batch_group_id`.

### <a name="scoring-pipelines"></a>Canalizaciones de puntuación

Para extraer los pares clave-valor, se analizarán todas las carpetas de la tabla de parametrización y, para cada carpeta, se extraerán los pares clave-valor de todos los archivos que contenga. A partir de hoy, ADF no admite bucles ForEach anidados. En su lugar, se crearán dos canalizaciones. La primera canalización realizará la búsqueda en la tabla de parametrización y pasará la lista de carpetas como un parámetro a la segunda canalización.

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-1a.png" alt-text="Primera canalización de entrenamiento en Data Factory":::

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-1b.png" alt-text="Primera canalización de entrenamiento en Data Factory, detalles":::

La segunda canalización usará una actividad GetMeta para obtener la lista de los archivos de la carpeta y pasarla como un parámetro al cuaderno de puntuación de Databricks.

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-2a.png" alt-text="Segunda canalización de entrenamiento en Data Factory":::

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-2b.png" alt-text="Segunda canalización de entrenamiento en Data Factory, detalles":::

### <a name="specify-a-degree-of-parallelism"></a>Especificación de un grado de paralelismo

En las canalizaciones de entrenamiento y puntuación, puede especificar el grado de paralelismo para procesar varios formularios simultáneamente.

Para establecer el grado de paralelismo en la canalización de Azure Data Factory:

* Seleccione la actividad ForEach.
* Desactive la casilla **Secuencial**.
* Establezca el grado de paralelismo en el cuadro de texto **Batch count** (Número de lotes). Se recomienda un número máximo de lotes de 15 para la puntuación.

:::image type="content" source="./media/tutorial-bulk-processing/parallelism.png" alt-text="Configuración de paralelismo para la actividad de puntuación en Azure Data Factory":::

## <a name="how-to-use"></a>Cómo se usa

Ahora tiene una canalización automatizada para digitalizar el trabajo pendiente de formularios y ejecutar algunos análisis. Al agregar nuevos formatos de tipo conocido a una carpeta de almacenamiento existente, simplemente vuelva a ejecutar las canalizaciones de puntuación y estas actualizarán todos los archivos de salida, incluidos los archivos de salida de los nuevos formularios. 

Si agrega formularios de un nuevo tipo, también tendrá que cargar un conjunto de datos de entrenamiento en el contenedor adecuado. A continuación, agregue una nueva fila a la tabla de parametrización, especificando las ubicaciones de los nuevos documentos y su conjunto de datos de entrenamiento. Escriba un valor de -1 para **model_ID** para indicar que se debe entrenar un nuevo modelo para estos formularios. A continuación, ejecute la canalización de entrenamiento en Azure Data Factory. Este leerá de la tabla, entrenará un modelo y sobrescribirá el identificador del modelo en la tabla. Después, puede llamar a las canalizaciones de puntuación para empezar a escribir los archivos de salida.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha configurado actividades de Azure Data Factory que desencadenan el entrenamiento y la ejecución de modelos de Form Recognizer para digitalizar una gran cantidad de trabajo pendiente de archivos. A continuación, explore la API Form Recognizer para ver qué más puede hacer.

* [API REST Form Recognizer](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/AnalyzeBusinessCardAsync)
