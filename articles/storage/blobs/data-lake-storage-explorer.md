---
title: Uso del Explorador de Azure Storage con Azure Data Lake Storage Gen2
description: Use el Explorador de Azure Storage para administrar directorios y listas de control de acceso (ACL) de archivos y directorios en cuentas de almacenamiento que tengan habilitado el espacio de nombres jerárquico (HNS).
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 02/05/2021
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 5ccef241a37e63467b681d5fd12c65072cb92e58
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2021
ms.locfileid: "99626491"
---
# <a name="use-azure-storage-explorer-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Uso del Explorador de Azure Storage para administrar directorios, archivos y ACL en Azure Data Lake Storage Gen2

En este artículo se explica cómo usar el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) para crear y administrar directorios, archivos y listas de control de acceso (ACL) en cuentas de almacenamiento que tengan habilitado un espacio de nombres jerárquico (HNS).

## <a name="prerequisites"></a>Prerrequisitos

- Suscripción a Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).

- Una cuenta de almacenamiento que tenga habilitado el espacio de nombres jerárquico (HNS). Siga [estas](../common/storage-account-create.md) instrucciones para crear uno.

- Explorador de Azure Storage instalado en el equipo local. Para instalar el Explorador de Azure Storage para Windows, Macintosh o Linux, consulte [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

## <a name="sign-in-to-storage-explorer"></a>Inicio de sesión en el Explorador de Storage

Cuando se inicia por primera vez el Explorador de Storage, aparece la ventana **Explorador de Microsoft Azure Storage - Conectar**. Si bien el Explorador de Storage proporciona varias formas de conectarse a las cuentas de almacenamiento, actualmente solo se admite una para administrar las ACL.

|Tarea|Propósito|
|---|---|
|Agregar una cuenta de Azure | Le redirige a la página de inicio de sesión de su organización para autenticarlo en Azure. Actualmente, es el único método de autenticación compatible si quiere administrar y establecer las ACL.|
|Usar una cadena de conexión o un identificador URI de firma de acceso compartido | Puede utilizarse para tener acceso directamente a un contenedor o a la cuenta de almacenamiento con un token de SAS o una cadena de conexión compartida. |
|Usar un nombre y clave de la cuenta de almacenamiento| Use el nombre y la clave de la cuenta de almacenamiento para conectarse a Azure Storage.|

Seleccione **Add an Azure Account** (Agregar una cuenta de Azure) y haga clic en **Iniciar sesión**. Siga las indicaciones de la pantalla para registrarse en su cuenta de Azure.

![Captura de pantalla que muestra el Explorador de Microsoft Azure Storage y resalta la opción Add an Azure Account (Agregar una cuenta de Azure) y el botón Sign-in (Iniciar sesión).](media/storage-quickstart-blobs-storage-explorer/connect.png)

Cuando se completa la conexión, el Explorador de Microsoft Azure Storage se carga y se muestra la pestaña **Explorador**. Esta vista proporciona una visión general de todas las cuentas de Azure Storage, así como del almacenamiento local que se configuró mediante el [emulador de almacenamiento Azurite](../common/storage-use-azurite.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), las cuentas de [Cosmos DB](../../cosmos-db/storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) o los entornos de [Azure Stack](/azure-stack/user/azure-stack-storage-connect-se?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

![Explorador de Microsoft Azure Storage: ventana Conectar](media/storage-quickstart-blobs-storage-explorer/mainpage.png)

## <a name="create-a-container"></a>Crear un contenedor

Un contenedor contiene directorios y archivos. Para crear uno, expanda la cuenta de almacenamiento que creó en el paso anterior. Seleccione **Contenedores de blob**, haga clic con el botón derecho y seleccione **Crear contenedor de blobs**. Escriba el nombre del contenedor. Para ver una lista de reglas y restricciones en la nomenclatura de contenedores, consulte la sección [Crear un contenedor](storage-quickstart-blobs-dotnet.md#create-a-container). Cuando haya finalizado, presione **Entrar** para crear el contenedor. Una vez que el contenedor se ha creado correctamente, se muestra en la carpeta **Contenedores de blobs** de la cuenta de almacenamiento seleccionada.

![Explorador de Microsoft Azure Storage: creación de un contenedor](media/data-lake-storage-explorer/creating-a-filesystem.png)

## <a name="create-a-directory"></a>Creación de un directorio

Para crear un directorio, seleccione el contenedor que creó en el paso anterior. En la cinta de opciones del contenedor, elija el botón **Nueva carpeta**. Escriba el nombre del directorio. Cuando haya finalizado, presione **Entrar** para crear el directorio. Cuando el directorio se ha creado correctamente, aparece en la ventana del editor.

![Explorador de Microsoft Azure Storage: creación de un directorio](media/data-lake-storage-explorer/creating-a-directory.png)

## <a name="upload-blobs-to-the-directory"></a>Carga de blobs en el directorio

En la cinta de opciones del directorio, elija el botón **Cargar**. Esta operación da la opción de cargar un archivo o una carpeta.

Elija los archivos o carpetas para cargar.

![Explorador de Microsoft Azure Storage: Carga de un blob](media/data-lake-storage-explorer/upload-file.png)

Cuando se selecciona **Aceptar**, los archivos seleccionados se ponen en cola para cargar y se cargan de uno en uno. Una vez finalizada la carga, los resultados se muestran en la ventana **Actividades**.

## <a name="view-blobs-in-a-directory"></a>Visualización de blobs en un directorio

En la aplicación **Explorador de Azure Storage**, seleccione un directorio en una cuenta de almacenamiento. El panel principal muestra una lista de los blobs en el directorio seleccionado.

![Explorador de Microsoft Azure Storage: enumeración de los blobs de un directorio](media/data-lake-storage-explorer/list-files.png)

## <a name="download-blobs"></a>Descargar blobs

Para descargar los archivos mediante el **Explorador de Azure Storage**, con un archivo seleccionado, seleccione **Descargar** desde la cinta de opciones. Se abre un cuadro de diálogo de archivo que permite escribir un nombre de archivo. Seleccione **Guardar** para iniciar la descarga de un archivo en la ubicación local.

<a id="managing-access"></a>

## <a name="manage-acls"></a>Administración de ACL

Haga clic con el botón derecho en el contenedor, en un directorio o en un archivo y, a continuación, haga clic en **Administrar listas de control de acceso**.  En la captura de pantalla siguiente se muestra el menú que aparece al hacer clic con el botón derecho en un directorio.

> [!div class="mx-imgBorder"]
> ![Hacer clic con el botón derecho en un directorio en el Explorador de Azure Storage](./media/data-lake-storage-explorer/manage-access-control-list-option.png)

El cuadro de diálogo **Administrar acceso** permite administrar los permisos de propietario y del grupo de propietarios. También permite agregar usuarios y grupos nuevos a la lista de control de acceso para la que quiere administrar los permisos.

> [!div class="mx-imgBorder"]
> ![Cuadro de diálogo Administrar acceso](./media/data-lake-storage-explorer/manage-access-dialog-box.png)

Seleccione el botón **Agregar** para agregar un usuario o grupo nuevo a la lista de control de acceso. A continuación, escriba la entrada de Azure Active Directory (AAD) correspondiente que quiere agregar a la lista y, luego, seleccione **Agregar**.  El usuario o grupo ahora aparecerá en el campo **Usuarios y grupos:** , lo que le permitirá empezar a administrar sus permisos.

> [!NOTE]
> Se trata de un procedimiento recomendado para crear un grupo de seguridad en AAD y mantener los permisos en el grupo en lugar de usuarios individuales. Para obtener más detalles sobre esta recomendación, así como otros procedimientos recomendados, consulte los [Modelo de control de acceso de Azure Data Lake Storage Gen2](data-lake-storage-access-control-model.md).

Use los controles de casilla para establecer el acceso y las ACL predeterminadas. Para obtener más información sobre la diferencia entre estos tipos de ACL, consulte [Tipos de ACL](data-lake-storage-access-control.md#types-of-acls).

<a id="apply-acls-recursively"></a>

## <a name="apply-acls-recursively"></a>Aplicación de ACL de forma recursiva

Puede aplicar entradas de ACL de forma recursiva en los elementos secundarios existentes de un directorio primario sin tener que realizar estos cambios individualmente para cada elemento secundario.

Para aplicar las entradas de ACL de forma recursiva, haga clic con el botón derecho en el contenedor o en un directorio y, a continuación, haga clic en **Propagar las listas de control de acceso**.  En la captura de pantalla siguiente se muestra el menú que aparece al hacer clic con el botón derecho en un directorio.

> [!div class="mx-imgBorder"]
> ![Hacer clic con el botón derecho en un directorio y elegir la opción Propagar control de acceso](./media/data-lake-storage-explorer/propagate-access-control-list-option.png)

## <a name="next-steps"></a>Pasos siguientes

Conozca las listas de control de acceso de Data Lake Storage Gen2.

> [!div class="nextstepaction"]
> [Control de acceso en Azure Data Lake Storage Gen2](./data-lake-storage-access-control.md)
