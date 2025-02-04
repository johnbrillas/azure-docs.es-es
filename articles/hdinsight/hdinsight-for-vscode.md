---
title: Azure HDInsight para Visual Studio Code
description: Obtenga información sobre cómo usar las herramientas Spark y Hive (Azure HDInsight) para Visual Studio Code. Use las herramientas para crear y enviar consultas y scripts.
ms.service: hdinsight
ms.topic: how-to
ms.date: 10/20/2020
ms.custom: devx-track-python
ms.openlocfilehash: d098af394906dc120a252bdcda65fb3af31e28c8
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104865798"
---
# <a name="use-spark--hive-tools-for-visual-studio-code"></a>Usar las herramientas Spark y Hive para Visual Studio Code

Obtenga información sobre cómo usar las herramientas Apache Spark y Apache Hive para Visual Studio Code. Use las herramientas para crear y enviar trabajos por lotes de Apache Hive, consultas de Hive interactivas y scripts de PySpark para Apache Spark. Primero, describiremos cómo instalar las herramientas Spark y Hive en Visual Studio Code. A continuación, veremos cómo enviar trabajos a las herramientas Spark y Hive.  

Las herramientas Spark y Hive se pueden instalar en todas las plataformas compatibles con Visual Studio Code. Observe los siguientes requisitos previos para las distintas plataformas.

## <a name="prerequisites"></a>Requisitos previos

Los elementos siguientes son necesarios para completar los pasos indicados en este artículo:

- Un clúster de HDInsight de Azure. Para crear un clúster, vea la [introducción a HDInsight](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md). O bien, use un clúster de Hive y Spark que admita un punto de conexión de Apache Livy.
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono solo es obligatorio para Linux y macOS.
- [Un entorno interactivo de PySpark para Visual Studio Code](set-up-pyspark-interactive-environment.md).
- Un directorio local. En este artículo, se usa **C:\HD\HDexample**.

## <a name="install-spark--hive-tools"></a>Instalar Spark & Hive Tools

Después de completar los requisitos previos, puede instalar Spark & Hive Tools para Visual Studio Code siguiendo estos pasos:

1. Abra Visual Studio Code.

2. En la barra de menús, vaya a **Ver** > **Extensiones**.

3. En el cuadro de búsqueda, escriba **Spark & Hive**.

4. Seleccione **Spark & Hive Tools** en los resultados de búsqueda y seleccione **Instalar**:

   :::image type="content" source="./media/hdinsight-for-vscode/install-hdInsight-plugin.png" alt-text="Instalación de Spark & Hive Tools para Visual Studio Code Python":::

5. Seleccione **Recargar** cuando sea necesario.

## <a name="open-a-work-folder"></a>Apertura de una carpeta de trabajo

Para abrir una carpeta de trabajo y crear un archivo en Visual Studio Code, siga estos pasos:

1. En la barra de menús, vaya a **Archivo** > **Abrir carpeta...**  > **C:\HD\HDexample** y, luego, seleccione el botón **Seleccionar carpeta**. La carpeta se mostrará en la vista **Explorador** de la parte izquierda.

2. En la vista **Explorador**, seleccione la carpeta, **HDexample** y, a continuación, el icono **Nuevo archivo** situado junto a la carpeta de trabajo:

   :::image type="content" source="./media/hdinsight-for-vscode/visual-studio-code-new-file.png" alt-text="Icono de archivo nuevo de Visual Studio Code":::

3. Asigne al nuevo archivo la extensión de archivo `.hql` (consultas de Hive) o `.py` (script de Spark). En este ejemplo se utiliza **HelloWorld.hql**.

## <a name="set-the-azure-environment"></a>Establecimiento del entorno de Azure

Para usuarios de la nube nacional, siga los pasos para configurar el entorno de Azure en primer lugar y, a continuación, use el comando **Azure: Sign In** para iniciar sesión en Azure:

1. Vaya a **Archivo** > **Preferencias** > **Configuración**.
2. Busque la siguiente cadena: **Azure: Cloud** (Azure: nube).
3. Seleccione la nube nacional de la lista:

   :::image type="content" source="./media/hdinsight-for-vscode/set-default-login-entry-configuration.png" alt-text="Establecer la configuración de entrada de inicio de sesión predeterminada":::

## <a name="connect-to-an-azure-account"></a>Conexión a la cuenta de Azure

Para poder enviar scripts a los clústeres desde Visual Studio Code, el usuario puede iniciar sesión en la suscripción de Azure o [vincular un clúster de HDInsight](#link-a-cluster). Use el nombre de usuario y la contraseña de Ambari o las credenciales del clúster ESP para conectarse al clúster de HDInsight. Siga estos pasos para conectarse a Azure:

1. En la barra de menús, vaya a **Ver** > **Paleta de comandos...** y escriba **Azure: Sign In**:

   :::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png" alt-text="Spark & Hive Tools para el inicio de sesión de Visual Studio Code":::

2. Siga las instrucciones para iniciar sesión en Azure. Una vez establecida la conexión, el nombre de la cuenta de Azure se muestra en la barra de estado en la parte inferior de la ventana de Visual Studio Code.  

## <a name="link-a-cluster"></a>Vinculación de un clúster

### <a name="link-azure-hdinsight"></a>Vínculo: HDInsight de Azure

Puede vincular un clúster normal mediante un nombre de usuario administrado de [Apache Ambari](https://ambari.apache.org/) o vincular un clúster de Hadoop de seguridad de Enterprise Security Pack mediante un nombre de usuario de dominio (como `user1@contoso.com`).

1. En la barra de menús, vaya a **Ver** > **Paleta de comandos...** y escriba **Spark / Hive: Link a Cluster** (vincular un clúster).

   :::image type="content" source="./media/hdinsight-for-vscode/link-cluster-command.png" alt-text="Paleta de comandos, comando de vinculación de clúster":::

2. Seleccione el tipo de clúster vinculado **Azure HDInsight**.

3. Escriba la dirección URL del clúster de HDInsight.

4. Escriba el nombre de usuario de Ambari; el valor predeterminado es **admin**.

5. Escriba la contraseña de Ambari.

6. Seleccione el tipo de clúster.

7. Establezca el nombre para mostrar del clúster (opcional).

8. Revise la vista **SALIDA**.

   > [!NOTE]  
   > Si el clúster se registró en la suscripción de Azure y se vinculó, se usan el nombre de usuario y la contraseña vinculados.  

### <a name="link-generic-livy-endpoint"></a>Vínculo: punto de conexión de Livy genérico

1. En la barra de menús, vaya a **Ver** > **Paleta de comandos...** y escriba **Spark / Hive: Link a Cluster** (vincular un clúster).

2. Seleccione el tipo de clúster vinculado **Generic Livy Endpoint** (Punto de conexión de Livy genérico).

3. Escriba el punto de conexión de Livy genérico. Por ejemplo, http\://10.172.41.42:18080.

4. Seleccione el tipo de autorización **Básica** o **Ninguna**.  Si selecciona **Básico**:  
   
   1. Escriba el nombre de usuario de Ambari; el valor predeterminado es **admin**.  

   2. Escriba la contraseña de Ambari.

5. Revise la vista **SALIDA**.

## <a name="list-clusters"></a>Lista de clústeres

1. En la barra de menús, vaya a **Ver** > **Paleta de comandos...** y escriba **Spark / Hive: List Cluster** (lista de clústeres).

2. Seleccione la suscripción que desea.

3. Revise la ventana de **SALIDA**. Esta vista muestra el clúster vinculado (o los clústeres vinculados) y todos los clústeres de su suscripción de Azure:

   :::image type="content" source="./media/hdinsight-for-vscode/list-cluster-result1.png" alt-text="Establecer una configuración de clúster predeterminado":::

## <a name="set-the-default-cluster"></a>Establecimiento de clúster predeterminado

1. Vuelva a abrir la carpeta **HDexample** que se trató [anteriormente](#open-a-work-folder), en caso de que esté cerrada.  

2. Seleccione el archivo **HelloWorld.hql** que se creó [anteriormente](#open-a-work-folder). Se abre en el editor de scripts.

3. Haga clic con el botón derecho en el editor de scripts y, después, seleccione **Spark / Hive: Set Default Cluster** (establecer clúster predeterminado).  

4. [Conéctese](#connect-to-an-azure-account) a su cuenta de Azure o vincule un clúster si no lo ha hecho aún.

5. Seleccione un clúster como el clúster predeterminado para el archivo de script actual. Las herramientas actualizan automáticamente el archivo de configuración **.VSCode\settings.json**:

   :::image type="content" source="./media/hdinsight-for-vscode/set-default-cluster-configuration.png" alt-text="Establecer la configuración del clúster predeterminado":::

## <a name="submit-interactive-hive-queries-and-hive-batch-scripts"></a>Envío de consultas de Hive interactivas y scripts por lotes de Hive

Con Spark & Hive Tools para Visual Studio Code, puede enviar consultas de Hive interactivas y scripts por lotes de Hive a sus clústeres.

1. Vuelva a abrir la carpeta **HDexample** que se trató [anteriormente](#open-a-work-folder), en caso de que esté cerrada.  

2. Seleccione el archivo **HelloWorld.hql** que se creó [anteriormente](#open-a-work-folder). Se abre en el editor de scripts.

3. Copie y pegue el código siguiente en el archivo de Hive y guárdelo:

   ```hiveql
   SELECT * FROM hivesampletable;
   ```

4. [Conéctese](#connect-to-an-azure-account) a su cuenta de Azure o vincule un clúster si no lo ha hecho aún.

5. Haga clic con el botón derecho en el editor de scripts y seleccione **Hive: Interactive** (Hive: Interactivo) para enviar la consulta, o bien use el acceso directo Ctrl+Alt +I.  Seleccione **Hive: Batch** (Hive: Lote) para enviar la consulta, o bien use el acceso directo Ctrl+Alt+H.  

6. Seleccione el clúster si no ha especificado un clúster predeterminado. Las herramientas también permiten enviar un bloque de código en lugar del archivo de script completo mediante el menú contextual. Transcurridos unos instantes, los resultados de la consulta aparecen en una pestaña nueva:

   :::image type="content" source="./media/hdinsight-for-vscode/interactive-hive-result.png" alt-text="Resultado de consulta interactiva de Apache Hive":::

   - Panel **RESULTADOS**: Puede guardar todo el resultado en forma de archivo CSV, JSON o de Excel en una ruta de acceso local, o bien seleccionar únicamente varias líneas.

   - Panel **MENSAJES**: Al seleccionar el número de **Línea**, se salta a la primera línea del script en ejecución.

## <a name="submit-interactive-pyspark-queries"></a>Enviar consultas de PySpark interactivas

Los usuarios pueden llevar a cabo PySpark Interactive de las siguientes maneras:

### <a name="using-the-pyspark-interactive-command-in-py-file"></a>Con el comando de PySpark Interactive en el archivo PY
Para usar el comando de PySpark Interactive con el fin de enviar las consultas, siga estos pasos:

1. Vuelva a abrir la carpeta **HDexample** que se trató [anteriormente](#open-a-work-folder), en caso de que esté cerrada.  

2. Cree un nuevo archivo **HelloWorld.py** siguiendo los pasos [anteriores](#open-a-work-folder).

3. Copie y pegue el código siguiente en el archivo de script:

   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```

4. El mensaje para instalar el kernel de PySpark/Synapse Pyspark se muestra en la esquina inferior derecha de la ventana. Puede hacer clic en el botón **Instalar** para continuar con las instalaciones de Pyspark o Synaps PySpark, o bien hacer clic en el botón **Omitir** para omitir este paso.

   :::image type="content" source="./media/hdinsight-for-vscode/install-the-pyspark-kernel.png" alt-text="Captura de pantalla que muestra una opción para omitir la instalación de PySpark.":::

5. Si tiene que instalarlo más adelante, puede ir a **Archivo** > **Preferencia** > **Configuración** y desactivar **HDInsight: Enable Skip Pyspark Installation** (Habilitar Omitir la instalación de Pyspark) en las opciones. 
    
    :::image type="content" source="./media/hdinsight-for-vscode/enable-skip-pyspark-installation.png" alt-text="Captura de pantalla que muestra la opción Enable Skip Pyspark Installation (Habilitar Omitir la instalación de Pyspark).":::

6. Si la instalación se realiza correctamente en el paso 4, el cuadro de mensaje "PySpark installed succesfully" (PySpark se instaló correctamente) se muestra en la esquina inferior derecha de la ventana. Haga clic en botón **Reload** (Recargar) para volver a cargar la ventana.

   :::image type="content" source="./media/hdinsight-for-vscode/pyspark-kernel-installed-successfully.png" alt-text="pyspark se ha instalado correctamente":::


7. En la barra de menús, vaya a **View** (Ver)  > **Command Palette** (Paleta de comandos) o use el método abreviado de teclado **Mayús + Ctrl + P** y escriba **Python: Seleccione Interpreter (Intérprete) para iniciar el servidor de Jupyter**.

   :::image type="content" source="./media/hdinsight-for-vscode/select-interpreter-to-start-jupyter-server.png" alt-text="Seleccionar Interpreter para iniciar el servidor de Jupyter":::

8. Seleccione la opción de Python que aparece a continuación.

   :::image type="content" source="./media/hdinsight-for-vscode/choose-the-below-option.png" alt-text="Elija la opción siguiente":::
    
9. En la barra de menús, vaya a **View**(Ver) > **Command Palette** (Paleta de comandos) o use el método abreviado de teclado **Mayús + Ctrl + P** y escriba **Desarrollador: Recargar ventana**.

    :::image type="content" source="./media/hdinsight-for-vscode/reload-window.png" alt-text="Recargar ventana":::

10. [Conéctese](#connect-to-an-azure-account) a su cuenta de Azure o vincule un clúster si no lo ha hecho aún.

11. Seleccione todo el código, haga clic con el botón derecho en el editor de scripts y seleccione **Spark: PySpark Interactive/Synapse: Pyspark Interactive** para enviar la consulta. 

    :::image type="content" source="./media/hdinsight-for-vscode/pyspark-interactive-right-click.png" alt-text="menú contextual de PySpark Interactive":::

12. Seleccione el clúster si no ha especificado un clúster predeterminado. En un momento, los resultados de **Python Interactive** se mostrarán en una pestaña nueva. Haga clic en PySpark para cambiar el kernel a **PySpark/Synapse Pyspark** y así el código se ejecuta correctamente. Si se quiere cambiar al kernel de Synapse Pyspark, se recomienda deshabilitar la configuración automática en Azure Portal. En caso contrario, es posible que se tarde mucho tiempo en reactivar el clúster y establecer el kernel de Synapse para el primer uso. Si las herramientas también permiten enviar un bloque de código en lugar del archivo de script completo mediante el menú contextual:

    :::image type="content" source="./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png" alt-text="ventana de Python Interactive de PySpark Interactive":::

13. Escriba **%%info** y presione Mayús+Entrar para ver información sobre el trabajo (opcional):

    :::image type="content" source="./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png" alt-text="PySpark interactivo: ver información de trabajo":::

La herramienta también admite la consulta **Spark SQL**:

  :::image type="content" source="./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png" alt-text="visualización de resultado de pyspark interactive":::


### <a name="perform-interactive-query-in-py-file-using-a--comment"></a>Consulta interactiva en el archivo PY con un comentario #%%

1. Agregue **#%%** antes del código de Py para obtener la experiencia del cuaderno.

   :::image type="content" source="./media/hdinsight-for-vscode/run-cell.png" alt-text="agregue #%%":::

2. Haga clic en **Run Cell** (Ejecutar celda). En un momento, los resultados de Python Interactive se mostrarán en una pestaña nueva. Haga clic en PySpark para cambiar el kernel a PySpark/Synapse PySpark y luego vuelva a hacer clic en **Ejecutar celda** para que el código se ejecute correctamente.

   :::image type="content" source="./media/hdinsight-for-vscode/run-cell-get-results.png" alt-text="ejecución de los resultados de celda":::

## <a name="leverage-ipynb-support-from-python-extension"></a>Aprovechar la compatibilidad con IPYNB de la extensión de Python

1. Puede crear un comando de Jupyter Notebook en la paleta de comandos o mediante la creación de un nuevo archivo. ipynb en el área de trabajo. Para más información, consulte [Trabajo con cuadernos de Jupyter Notebook en Visual Studio Code](https://code.visualstudio.com/docs/python/jupyter-support).

2. Haga clic en el botón **Run cell** (Ejecutar celda), siga las indicaciones para **Set the default spark pool** (Establecer el grupo de Spark predeterminado) (se recomienda encarecidamente establecer el grupo o clúster predeterminado cada vez antes de abrir un cuaderno) y, a continuación, **Reload** (Recargar).

   :::image type="content" source="./media/hdinsight-for-vscode/set-the-default-spark-pool-and-reload.png" alt-text="Establecer el grupo de Spark predeterminado y volver a cargarlo":::

3. Haga clic en PySpark para cambiar el kernel a **PySpark/Synapse Pyspark** y luego haga clic en **Ejecutar celda**; al cabo de un momento se muestra el resultado.

   :::image type="content" source="./media/hdinsight-for-vscode/run-ipynb-file-results.png" alt-text="ejecución de los resultados de ipynb":::


> [!NOTE]
>
> Se ha resuelto el problema ["Las versiones de ms-python a partir de 2020.5.78807 no se admiten en esta extensión"](#issues-changed). Actualice ahora **ms-python** a la **versión más reciente**.

## <a name="submit-pyspark-batch-job"></a>Enviar trabajo por lotes de PySpark

1. Vuelva a abrir la carpeta **HDexample** que se trató [anteriormente](#open-a-work-folder), en caso de que esté cerrada.  

2. Cree un nuevo archivo **BatchFile.py** siguiendo los pasos [anteriores](#open-a-work-folder).

3. Copie y pegue el código siguiente en el archivo de script:

   ```python
   from __future__ import print_function
   import sys
   from operator import add
   from pyspark.sql import SparkSession
   if __name__ == "__main__":
       spark = SparkSession\
           .builder\
           .appName("PythonWordCount")\
           .getOrCreate()
   
       lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
       counts = lines.flatMap(lambda x: x.split(' '))\
                  .map(lambda x: (x, 1))\
                   .reduceByKey(add)
       output = counts.collect()
       for (word, count) in output:
           print("%s: %i" % (word, count))
       spark.stop()
   ```

4. [Conéctese](#connect-to-an-azure-account) a su cuenta de Azure o vincule un clúster si no lo ha hecho aún.

5. Haga clic con el botón derecho en el editor de scripts y, después, seleccione **Spark: PySpark Batch** (Spark: lote de PySpark) o **Synapse: PySpark Batch*** (Synapse: lote de PySpark).

6. Seleccione un clúster o grupo de Spark para enviar el trabajo de PySpark a:

   :::image type="content" source="./media/hdinsight-for-vscode/submit-pythonjob-result.png" alt-text="Enviar la salida del resultado del trabajo de Python":::

Después de enviar trabajo de Python, los registros de envío se muestran en la ventana de **SALIDA** en Visual Studio Code. También se muestran la dirección URL de interfaz de usuario de Spark y la dirección URL de interfaz de usuario de Yarn. Si envía el trabajo por lotes a un grupo de Apache Spark, también se muestran la dirección URL de la interfaz de usuario del historial de Spark y la dirección URL de la interfaz de usuario de la aplicación de trabajo de Spark. Para realizar un seguimiento del estado del trabajo, puede abrir la URL en un explorador web.

## <a name="integrate-with-hdinsight-identity-broker-hib"></a>Integración con HDInsight Identity Broker (HIB)

### <a name="connect-to-your-hdinsight-esp-cluster-with-id-broker-hib"></a>Conexión al clúster de HDInsight ESP con el agente de HDInsight Identity Broker (HIB)

Puede seguir los pasos normales para iniciar sesión en la suscripción de Azure para conectarse a su clúster de HDInsight ESP con el agente de HDInsight Identity Broker (HIB). Después de iniciar sesión, verá la lista de clústeres en Azure Explorer. Para conocer más instrucciones, consulte [Conexión al clúster de HDInsight](#connect-to-an-azure-account).

### <a name="run-a-hivepyspark-job-on-an-hdinsight-esp-cluster-with-id-broker-hib"></a>Ejecución de un trabajo de Hive/PySpark en un clúster de HDInsight ESP con el agente de identidad (HIB)

Para ejecutar un trabajo de Hive, puede seguir los pasos normales para enviar el trabajo a un clúster de HDInsight ESP con el agente de identidad (HIB). Consulte [Envío de consultas de Hive interactivas y scripts por lotes de Hive](#submit-interactive-hive-queries-and-hive-batch-scripts) para más instrucciones.

Para ejecutar un trabajo de PySpark interactivo, puede seguir los pasos normales para enviar el trabajo a un clúster de HDInsight ESP con el agente de identidad (HIB). Consulte [Enviar consultas de PySpark interactivas](#submit-interactive-pyspark-queries) para obtener más instrucciones.

Para ejecutar un trabajo por lotes de PySpark, puede seguir los pasos normales para enviar el trabajo a un clúster de HDInsight ESP con el agente de identidad (HIB). Consulte [Enviar trabajo por lotes de PySpark](#submit-pyspark-batch-job) para obtener más instrucciones.

## <a name="apache-livy-configuration"></a>Configuración de Apache Livy

La [configuración de Apache Livy](https://livy.incubator.apache.org/) es compatible. Puede configurarla en el archivo **.VSCode\settings.json** en la carpeta del área de trabajo. En la actualidad, la configuración de Livy solo admite el script de Python. Para más información, vea el [LÉAME de Livy](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Cómo activar la configuración de Livy**

### <a name="method-1"></a>Método 1  

1. En la barra de menús, vaya a **Archivo** > **Preferencias** > **Configuración**.
2. En el cuadro de texto **Configuración de la búsqueda** escriba **HDInsight Job Submission: configuración de Livy**.  
3. Seleccione **Editar en settings.json** para el resultado de la búsqueda relevante.

### <a name="method-2"></a>Método 2

Envíe un archivo y observe que la carpeta `.vscode` se agrega automáticamente a la carpeta de trabajo. Puede encontrar la configuración de Livy si selecciona **.vscode\settings.json**.

- Configuración del proyecto:

  :::image type="content" source="./media/hdinsight-for-vscode/hdi-apache-livy-config.png" alt-text="Configuración de Apache Livy en HDInsight":::

  >[!NOTE]
  >Para la configuración de **driverMemory** y **executorMemory**, establezca el valor y la unidad. Por ejemplo: 1g o 1024m.

- Configuraciones de Livy admitidas:

  **POST /batches**
  
  **Cuerpo de la solicitud**

  | name | description | type |
  | --- | --- | --- |
  | archivo | Archivo que contiene la aplicación que se ejecutará | Ruta (obligatorio) |
  | proxyUser | Usuario que se suplantará al ejecutar el trabajo | String |
  | className | Clase principal de Java/Spark de la aplicación | String |
  | args | Argumentos de la línea de comandos para la aplicación | Lista de cadenas |
  | jars | Archivos JAR que se usarán en esta sesión | Lista de cadenas | 
  | pyFiles | Archivos de Python que se usarán en esta sesión | Lista de cadenas |
  | files | Archivos que se usarán en esta sesión | Lista de cadenas |
  | driverMemory | Cantidad de memoria que se usará para el proceso del controlador | String |
  | driverCores | Número de núcleos que se usarán para el proceso del controlador | Int |
  | executorMemory | Cantidad de memoria que se usará por proceso de ejecutor | String |
  | executorCores | Número de núcleos que se usará para cada ejecutor | Int |
  | numExecutors | Número de ejecutores que se iniciarán para esta sesión | Int |
  | archives | Archivos que se usarán en esta sesión | Lista de cadenas |
  | cola | El nombre de la cola YARN a la que se enviará| String |
  | name | Nombre de esta sesión | String |
  | conf | Propiedades de configuración de Spark | Mapa de clave=valor |

  **Cuerpo de respuesta** El objeto Batch creado.

  | name | description | type |
  | --- | ---| --- |
  | id | Identificador de sesión | Int |
  | appId | Id. de aplicación de esta sesión | String |
  | appInfo | Información detallada de la aplicación | Mapa de clave=valor |
  | log | Líneas de registro | Lista de cadenas |
  | state |Estado del lote | String |

  > [!NOTE]
  > La configuración de Livy asignada se muestra en el panel de salida al enviar el script.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integración con Azure HDInsight desde el explorador

Puede obtener una vista previa de la tabla de Hive directamente en los clústeres a través del explorador de **Azure HDInsight**:

1. [Conéctese](#connect-to-an-azure-account) a su cuenta de Azure si no lo ha hecho aún.

2. Seleccione el icono **Azure** de la columna situada en el extremo izquierdo.

3. En el panel izquierdo, expanda **AZURE: HDINSIGHT**. Aparecen las suscripciones y clústeres disponibles.

4. Expanda el clúster para ver el esquema de base de datos y tablas de metadatos de Hive.

5. Haga clic con el botón derecho en la tabla de Hive. Por ejemplo: **hivesampletable**. Seleccione **Vista previa**.

   :::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png" alt-text="Tabla de Hive con la vista previa de Hive y Spark para Visual Studio Code":::

6. Se abre la ventana **Vista previa de resultados**:

   :::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png" alt-text="Ventana de resultados con la vista previa de Hive y Spark para Visual Studio Code":::

- Panel RESULTADOS

   Puede guardar todo el resultado en forma de archivo CSV, JSON o Excel en una ruta de acceso local, o bien seleccionar únicamente varias líneas.

- Panel MENSAJES

  1. Cuando el número de filas de la tabla es superior a 100, aparece un mensaje que informa de que se muestran las 100 primeras filas de la tabla de Hive.
  2. Cuando el número de filas de la tabla es menor o igual a 100, aparece el siguiente mensaje: se muestran 60 filas de la tabla de Hive.
  3. Cuando no haya contenido en la tabla, verá el siguiente mensaje: "`0 rows are displayed for Hive table.`"

     >[!NOTE]
     >
     >En Linux, instale xclip para permitir la copia de datos de tabla.
     >
     >:::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png" alt-text="Hive y Spark para Visual Studio Code en Linux":::

## <a name="additional-features"></a>Características adicionales

Hive y Spark para Visual Studio Code admite también las siguientes características:

- **Autocompletar de IntelliSense**. Aparecen sugerencias para palabras clave, métodos, variables y otros elementos de programación. Los distintos iconos representan diferentes tipos de objetos:

    :::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png" alt-text="Objetos de IntelliSense de Spark & Hive Tools para Visual Studio Code":::

- **Marcador de error de IntelliSense**. El servicio de lenguaje subraya los errores de edición del script de Hive.     
- **Sintaxis resaltada**. El servicio de lenguaje usa colores diferentes para diferenciar variables, palabras clave, tipos de datos, funciones y otros elementos de programación:

    :::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png" alt-text="Sintaxis resaltada de Spark & Hive Tools para Visual Studio Code":::

## <a name="reader-only-role"></a>Rol de solo lectura

Los usuarios a los que se haya asignado el rol de solo lector para el clúster ya no pueden enviar trabajos al clúster de HDInsight ni pueden ver la base de datos de Hive. Póngase en contacto con el administrador de clústeres para actualizar el rol a [**operador de clústeres de HDInsight**](./hdinsight-migrate-granular-access-cluster-configurations.md#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) en [Azure Portal](https://portal.azure.com/). Si tiene credenciales de Ambari válidas, puede vincular manualmente el clúster siguiendo estas instrucciones.

### <a name="browse-the-hdinsight-cluster"></a>Examen del clúster de HDInsight  

Al seleccionar el explorador de Azure HDInsight para expandir un clúster de HDInsight, se le pedirá que vincule el clúster si tiene el rol de solo lector en el clúster. Use el método siguiente para vincular al clúster con sus credenciales de Ambari.

### <a name="submit-the-job-to-the-hdinsight-cluster"></a>Envío del trabajo al clúster de HDInsight

Al enviar un trabajo al clúster de HDInsight, se le pide que vincule el clúster si tiene el rol de solo lector en el clúster. Siga estos pasos para vincular al clúster con sus credenciales de Ambari.

### <a name="link-to-the-cluster"></a>Vínculo al clúster

1. Escriba un nombre de usuario de Ambari válido.
2. Escriba una contraseña válida.

   :::image type="content" source="./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png" alt-text="Nombre de usuario de Spark & Hive Tools para Visual Studio Code":::

   :::image type="content" source="./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png" alt-text="Contraseña de Spark & Hive Tools para Visual Studio Code":::

   > [!NOTE]
   >
   >Puede usar `Spark / Hive: List Cluster` para comprobar el clúster vinculado:
   >
   >:::image type="content" source="./media/hdinsight-for-vscode/list-cluster-result1.png" alt-text="Lector vinculado de Spark & Hive Tools para Visual Studio Code":::

## <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

### <a name="browse-a-data-lake-storage-gen2-account"></a>Examen de una cuenta de Data Lake Storage Gen2

Seleccione el explorador de Azure HDInsight para expandir una cuenta de Data Lake Storage Gen2. Se le pedirá que especifique la clave de acceso de almacenamiento si su cuenta de Azure no tiene acceso al almacenamiento de Gen2. Una vez validada la clave de acceso, la cuenta de Data Lake Storage Gen2 se expande automáticamente.

### <a name="submit-jobs-to-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Envío de trabajos a un clúster de HDInsight con Data Lake Storage Gen2

Envíe un trabajo a un clúster de HDInsight con Data Lake Storage Gen2. Se le pedirá que especifique la clave de acceso de almacenamiento si su cuenta de Azure no tiene acceso de escritura al almacenamiento de Gen2. El trabajo se enviará sin problemas una vez validada correctamente la clave de acceso.

:::image type="content" source="./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png" alt-text="Clave de acceso de Spark & Hive Tools para Visual Studio Code":::

> [!NOTE]
>
> Puede obtener la clave de acceso de la cuenta de almacenamiento desde Azure Portal. Para más información, consulte [Administración de las claves de acceso de la cuenta de almacenamiento](../storage/common/storage-account-keys-manage.md).

## <a name="unlink-cluster"></a>Desvincular clúster

1. En la barra de menús, vaya a **Vista** > **Paleta de comandos** y escriba **Spark / Hive: desvincular un clúster**.  

2. Seleccione el clúster que desea desvincular.  

3. Revise la vista **SALIDA** para comprobarlo todo.  

## <a name="sign-out"></a>Cerrar sesión  

En la barra de menús, vaya a **Vista** > **Paleta de comandos** y escriba **Azure: Sign Out** (Cerrar sesión).

## <a name="issues-changed"></a>Incidencias modificadas

Se ha resuelto la incidencia "Las versiones de ms-python a partir de 2020.5.78807 no se admiten en esta extensión", actualice ahora **ms-python** a la **versión más reciente**.


## <a name="next-steps"></a>Pasos siguientes

Para ver un vídeo de demostración del uso de Spark y Hive para Visual Studio Code, consulte [Spark y Hive para Visual Studio Code](https://go.microsoft.com/fwlink/?linkid=858706).