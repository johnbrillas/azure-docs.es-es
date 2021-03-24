---
title: Acceso a recursos con Herramientas de Data Lake
description: Aprenda a usar Herramientas de Azure Data Lake para acceder a recursos de Azure Data Lake Analytics.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 02/09/2018
ms.openlocfilehash: d04f108b45070b27c4ff9ed833e8fb77b74cd597
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96754549"
---
# <a name="accessing-resources-with-azure-data-lake-tools"></a>Acceso a recursos con Herramientas de Azure Data Lake

Puede acceder a recursos de Azure Data Lake Analytics con comandos o acciones de Herramientas de Azure Data en VS Code fácilmente.

## <a name="integrate-with-azure-data-lake-analytics-through-a-command"></a>Integración con Azure Data Lake Analytics mediante un comando

Puede acceder a los recursos de Azure Data Lake Analytics para enumerar cuentas, acceder a los metadatos y ver los trabajos de análisis.

### <a name="to-list-the-azure-data-lake-analytics-accounts-under-your-azure-subscription"></a>Para enumerar las cuentas de Azure Data Lake Analytics en su suscripción a Azure

1. Seleccione Ctrl+Mayús+P para abrir la paleta de comandos.
2. Escriba **ADL: List Accounts**. Las cuentas aparecen en el panel **Salida**.

### <a name="to-access-azure-data-lake-analytics-metadata"></a>Para acceder a los metadatos de Azure Data Lake Analytics

1. Seleccione Ctrl+Mayús+P y, luego, escriba **ADL: List Tables**.
2. Seleccione una de las cuentas de Data Lake Analytics.
3. Seleccione una de las bases de datos de Data Lake Analytics.
4. Seleccione uno de los esquemas. Puede ver la lista de tablas.

### <a name="to-view-azure-data-lake-analytics-jobs"></a>Para ver los trabajos de Azure Data Lake Analytics

1. Abra la paleta de comandos (Ctrl+Mayús+P) y seleccione **ADL: Show Jobs**.
2. Seleccione una cuenta de Data Lake Analytics o una cuenta local.
3. Espere a que se muestre la lista de trabajos de la cuenta.
4. Seleccione un trabajo de la lista de trabajos. Herramientas de Data Lake abre la vista del trabajo en el panel derecho y muestra alguna información en la salida de VS Code.

    ![Lista de trabajos](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="integrate-with-azure-data-lake-store-through-a-command"></a>Integración con Azure Data Lake Store mediante un comando

Puede usar comandos relacionados con Azure Data Lake Store para:

- [Examinar los recursos de Azure Data Lake Store](#list-the-storage-path)
- [Obtener una vista previa del archivo de Azure Data Lake Store](#preview-the-storage-file)
- Cargar el archivo directamente en Azure Data Lake Store en VS Code
- Descargar el archivo directamente de Azure Data Lake Store en VS Code

### <a name="list-the-storage-path"></a>Enumeración de la ruta de acceso de almacenamiento

### <a name="to-list-the-storage-path-through-the-command-palette"></a>Para enumerar la ruta de acceso de almacenamiento a través de la paleta de comandos

1. Haga clic con el botón derecho en el editor de scripts y seleccione **ADL: List Path**.
2. Elija la carpeta en la lista o seleccione **Indicar ruta de acceso** o **Examinar desde ruta de acceso raíz**. (Usamos **Indicar ruta de acceso** como un ejemplo).
3. Seleccione la cuenta de Data Lake Analytics.
4. Busque o escriba la ruta de acceso de la carpeta de almacenamiento (por ejemplo, /output/).  

En la paleta de comandos se muestra la información de la ruta de acceso en función de las entradas.

![Resultados de la ruta de acceso de almacenamiento](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Una manera más cómoda de enumerar la ruta de acceso relativa es mediante el menú contextual.

### <a name="to-list-the-storage-path-through-the-shortcut-menu"></a>Para enumerar la ruta de acceso de almacenamiento mediante el menú contextual

Haga clic con el botón derecho en la cadena de la ruta de acceso y seleccione **List Path** (Enumerar ruta de acceso).

!["List Path" en el menú contextual](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)

### <a name="preview-the-storage-file"></a>Vista previa del archivo de almacenamiento

1. Haga clic con el botón derecho en el editor de scripts y seleccione **ADL: Preview File**.
2. Seleccione la cuenta de Data Lake Analytics.
3. Escriba una ruta de acceso del archivo de Azure Storage (por ejemplo, /output/SearchLog.txt).

El archivo se abre en VS Code.

![Pasos y resultado para obtener una vista previa del archivo de almacenamiento](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Otra manera de obtener una vista previa del archivo es con el menú contextual en la ruta de acceso completa o relativa del archivo en el editor de scripts.

### <a name="upload-a-file-or-folder"></a>Carga de un archivo o una carpeta

1. Haga clic con el botón derecho en el editor de scripts y seleccione **Cargar archivo** o **Cargar carpeta**.
2. Elija uno o varios archivos si ha seleccionado **Cargar archivo** o elija toda la carpeta si ha seleccionado **Cargar carpeta**. A continuación, seleccione **Cargar**.
3. Elija la carpeta de almacenamiento en la lista o seleccione **Indicar ruta de acceso** o **Examinar desde ruta de acceso raíz**. (Usamos **Indicar ruta de acceso** como un ejemplo).
4. Seleccione la cuenta de Data Lake Analytics.
5. Busque o escriba la ruta de acceso de la carpeta de almacenamiento (por ejemplo, /output/).
6. Seleccione **Elegir la carpeta actual** para especificar el destino de la carga.

![Pasos y resultado para cargar un archivo o una carpeta](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)

Otra manera de cargar archivos en el almacenamiento es con el menú contextual en la ruta de acceso completa o relativa del archivo en el editor de scripts.

También puede [supervisar el estado de la carga](#check-storage-tasks-status).

### <a name="download-a-file"></a>Descarga de un archivo

Puede descargar un archivo mediante el comando **ADL: Download File** o **ADL: Download File (Advanced)** .

### <a name="to-download-a-file-through-the-adl-download-file-advanced-command"></a>Para descargar un archivo mediante el comando ADL: Download File (Advanced)

1. Haga clic con el botón derecho en el editor de scripts y después seleccione **Download File (Advanced)** (Descargar archivo [Avanzado]).
2. VS Code muestra un archivo JSON. Puede especificar las rutas de acceso de archivo y descargar varios archivos al mismo tiempo. Las instrucciones se muestran en la ventana **Salida**. Para continuar con la descarga del archivo o los archivos, guarde (Ctrl+S) el archivo JSON.

    ![Archivo JSON con rutas de acceso para la descarga de archivos](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

La ventana **Salida** muestra el estado de la descarga del archivo.

![Ventana de salida con el estado de la descarga](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)

También puede [supervisar el estado de la descarga](#check-storage-tasks-status).

### <a name="to-download-a-file-through-the-adl-download-file-command"></a>Para descargar un archivo mediante el comando ADL: Download File

1. Haga clic con el botón derecho en el editor de scripts, seleccione **Descargar archivo** y, luego, elija la carpeta de destino en el cuadro de diálogo **Seleccionar carpeta**.

1. Elija la carpeta en la lista o seleccione **Indicar ruta de acceso** o **Examinar desde ruta de acceso raíz**. (Usamos **Indicar ruta de acceso** como un ejemplo).

1. Seleccione la cuenta de Data Lake Analytics.

1. Busque o escriba la ruta de acceso de la carpeta de almacenamiento (por ejemplo: /output/) y, a continuación, elija el archivo que desea descargar.

![Pasos y resultado para descargar un archivo](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png)

Otra manera de descargar archivos del almacenamiento es con el menú contextual en la ruta de acceso completa o relativa del archivo en el editor de scripts.

También puede [supervisar el estado de la descarga](#check-storage-tasks-status).

### <a name="check-storage-tasks-status"></a>Comprobación del estado de las tareas de almacenamiento

El estado de la carga y la descarga aparece en la barra de estado. Seleccione la barra de estado y, a continuación, el estado aparece en el pestaña **Salida**.

![Barra de estado y detalles de la salida](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)

## <a name="integrate-with-azure-data-lake-analytics-from-the-explorer"></a>Integración con Azure Data Lake Analytics desde el explorador

Después de iniciar sesión, todas las suscripciones de la cuenta de Azure aparecen en el panel izquierdo de **Azure Data Lake**.

![Explorador de Data Lake](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

### <a name="data-lake-analytics-metadata-navigation"></a>Exploración de los metadatos de Data Lake Analytics

Expanda la suscripción de Azure. En el nodo **Bases de datos U-SQL**, puede desplazarse por la base de datos U-SQL y ver las carpetas, como **Esquemas**, **Credenciales**, **Ensamblados**, **Tablas** e **Índice**.

### <a name="data-lake-analytics-metadata-entity-management"></a>Administración de entidades de metadatos de Data Lake Analytics

Expanda **Bases de datos U-SQL**. Puede crear una base de datos, un esquema, una tabla, un tipo de tabla, un índice o una estadística haciendo clic con el botón derecho en el nodo correspondiente y, a continuación, seleccionando **Script para crear** en el menú contextual. En la página del script que se abre, edite el script según sus necesidades. A continuación, envíe el trabajo haciendo clic con el botón derecho en él y seleccionando **ADL: Submit Job**.

Cuando termine de crear el elemento, haga clic con el botón derecho en el nodo y, a continuación, seleccione **Actualizar** para mostrar el elemento. También puede eliminar el elemento haciendo clic con el botón derecho en el elemento y seleccionando **Eliminar**.

![Comando "Script para crear" en el menú contextual del Explorador de Data Lake](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create.png)

![Página del script para el nuevo elemento](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create-snippet.png)

### <a name="data-lake-analytics-assembly-registration"></a>Registro de ensamblados de Data Lake Analytics

Puede registrar un ensamblado en la base de datos correspondiente haciendo clic con el botón derecho en el nodo **Ensamblados** y, a continuación, seleccionando **Registrar ensamblado**.

![Comando "Registrar ensamblado" en el menú contextual del nodo Ensamblados](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

## <a name="integrate-with-azure-data-lake-store-from-the-explorer"></a>Integración con Azure Data Lake Store desde el explorador

Vaya a **Data Lake Store**:

- Puede hacer clic con el botón derecho en el nodo de la carpeta y, a continuación, utilizar los comandos **Actualizar**, **Eliminar**, **Cargar**, **Cargar carpeta**, **Copiar ruta de acceso relativa** y **Copiar ruta de acceso completa** del menú contextual.

   ![Comandos del menú contextual para un nodo de carpeta en el Explorador de Data Lake](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-folder-menu.png)

- Puede hacer clic con el botón derecho en el nodo del archivo y, a continuación, usar los comandos **Vista previa**, **Descargar**, **Eliminar**, **Crear script de extracción** (solo disponible para archivos CSV, TSV y TXT files), **Copiar ruta de acceso relativa** y **Copiar ruta de acceso completa** del menú contextual.

   ![Comandos del menú contextual para un nodo de archivo en el Explorador de Data Lake](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-extract.png)

## <a name="integrate-with-azure-blob-storage-from-the-explorer"></a>Integración con Azure Blob Storage desde el explorador

Vaya a Blob Storage:

- Puede hacer clic con el botón derecho en el nodo del contenedor de blobs y, a continuación, utilizar los comandos **Actualizar**, **Eliminar contenedor de blobs** y **Cargar blob** del menú contextual.

   ![Comandos del menú contextual para un nodo de contenedor de blobs en Blob Storage](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-blob-container-node.png)

- Puede hacer clic con el botón derecho en el nodo de la carpeta y, a continuación, usar los comandos **Actualizar** y **Cargar blob** del menú contextual.

   ![Comandos del menú contextual para un nodo de carpeta en Blob Storage](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-folder-node.png)

- Puede hacer clic con el botón derecho en el nodo del archivo y, a continuación, usar los comandos **Vista previa/Editar**, **Descargar**, **Eliminar**, **Crear script de extracción** (solo disponible para archivos CSV, TSV y TXT files), **Copiar ruta de acceso relativa** y **Copiar ruta de acceso completa** del menú contextual.

    ![Comandos del menú contextual para un nodo de archivo en Blob Storage](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu-2.png)

## <a name="open-the-data-lake-explorer-in-the-portal"></a>Apertura del Explorador de Data Lake en el portal

1. Seleccione Ctrl+Mayús+P para abrir la paleta de comandos.
2. Escriba **Abrir el explorador web de Azure Storage** o haga clic con el botón derecho en una ruta de acceso relativa o completa en el editor de scripts y, luego, seleccione **Abrir el explorador web de Azure Storage**.
3. Seleccione una cuenta de Data Lake Analytics.

Herramientas de Data Lake abre la ruta de acceso de Azure Storage en Azure Portal. Puede encontrar la ruta de acceso y obtener la vista previa del archivo desde la Web.

## <a name="additional-features"></a>Características adicionales

Herramientas de Data Lake para VS Code es compatible con las características siguientes:

- **Autocompletar de IntelliSense**: aparecen sugerencias en ventanas emergentes en torno a elementos como palabras clave, métodos y variables. Los distintos iconos representan diferentes tipos de objetos:

  - Tipo de datos de escala
  - Tipo de datos complejo
  - UDT integrada
  - Clases y colección .NET
  - Expresiones de C#
  - UDF, UDO y UDAAG integrados de C#
  - Funciones de U-SQL
  - Funciones de ventana de U-SQL

    ![Tipos de objeto de IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)

- **Autocompletar de IntelliSense en los metadatos de Data Lake Analytics**: Data Lake Tools descarga la información de metadatos de Data Lake Analytics localmente. La característica IntelliSense rellena automáticamente los objetos a partir de los metadatos de Data Lake Analytics. Estos objetos incluyen base de datos, esquema, tabla, vista, función con valores de tabla, procedimientos y ensamblados de C#.

  ![Metadatos de IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

- **Marcador de error de IntelliSense**: Data Lake Tools subraya los errores de edición para U-SQL U y C#.
- **Resaltado de la sintaxis**: Herramientas de Data Lake usa colores distintos para diferenciar elementos como variables, palabras clave, tipo de datos y funciones.

    ![Sintaxis con distintos colores](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

> [!NOTE]
> Se recomienda que actualice a Herramientas de Azure Data Lake para Visual Studio versión 2.3.3000.4 o posterior. Las versiones anteriores no están disponibles para su descarga y han quedado en desuso.  

## <a name="next-steps"></a>Pasos siguientes

- [Desarrollo de trabajos U-SQL con Python, R y CSharp para Azure Data Lake Analytics en VS Code](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [Ejecución y depuración locales de U-SQL con Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Tutorial: Introducción a Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md)
- [Tutorial: Desarrollo de scripts U-SQL mediante Data Lake Tools para Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)