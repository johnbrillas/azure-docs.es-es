---
title: SDK de Java para Azure Data Lake Storage Gen2 para archivos y ACL
description: Use bibliotecas de Azure Storage para Java con el fin de administrar directorios y listas de control de acceso (ACL) de archivos y directorios en cuentas de almacenamiento que tengan habilitado el espacio de nombres jerárquico (HNS).
author: normesta
ms.service: storage
ms.date: 01/11/2021
ms.custom: devx-track-java
ms.author: normesta
ms.topic: how-to
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: 1cc6954569c509c977634a8e1cdd52c5c55b2100
ms.sourcegitcommit: 48e5379c373f8bd98bc6de439482248cd07ae883
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98108134"
---
# <a name="use-java-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Uso de Java para administrar directorios, archivos y ACL en Azure Data Lake Storage Gen2

En este artículo se muestra cómo usar Java para crear y administrar directorios, archivos y permisos en cuentas de almacenamiento que tengan habilitado el espacio de nombres jerárquico (HNS). 

[Paquete (Maven)](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake) | [Muestras](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake) | [Referencia de API](/java/api/overview/azure/storage-file-datalake-readme) | [Asignación de Gen1 a Gen2](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md) | [Envíenos sus comentarios](https://github.com/Azure/azure-sdk-for-java/issues)

## <a name="prerequisites"></a>Requisitos previos

> [!div class="checklist"]
> * Suscripción a Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
> * Una cuenta de almacenamiento que tenga habilitado el espacio de nombres jerárquico (HNS). Siga [estas](../common/storage-account-create.md) instrucciones para crear uno.

## <a name="set-up-your-project"></a>Configurar su proyecto

Para empezar, abra [esta página](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake) y busque la versión más reciente de la biblioteca de Java. Después, abra el archivo *pom.xml* en el editor de texto. Agregue un elemento de dependencia que haga referencia a esa versión.

Si planea autenticar la aplicación cliente mediante Azure Active Directory (AD), agregue una dependencia a la biblioteca cliente de secreto de Azure. Consulte [Adición del paquete de la biblioteca cliente de secreto al proyecto](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity#adding-the-package-to-your-project).

A continuación, agregue estas instrucciones Imports al archivo de código.

```java
import com.azure.storage.common.StorageSharedKeyCredential;
import com.azure.storage.file.datalake.DataLakeDirectoryClient;
import com.azure.storage.file.datalake.DataLakeFileClient;
import com.azure.storage.file.datalake.DataLakeFileSystemClient;
import com.azure.storage.file.datalake.DataLakeServiceClient;
import com.azure.storage.file.datalake.DataLakeServiceClientBuilder;
import com.azure.storage.file.datalake.models.ListPathsOptions;
import com.azure.storage.file.datalake.models.PathItem;
import com.azure.storage.file.datalake.models.AccessControlChangeCounters;
import com.azure.storage.file.datalake.models.AccessControlChangeResult;
import com.azure.storage.file.datalake.models.AccessControlType;
import com.azure.storage.file.datalake.models.PathAccessControl;
import com.azure.storage.file.datalake.models.PathAccessControlEntry;
import com.azure.storage.file.datalake.models.PathPermissions;
import com.azure.storage.file.datalake.models.PathRemoveAccessControlEntry;
import com.azure.storage.file.datalake.models.RolePermissions;
import com.azure.storage.file.datalake.options.PathSetAccessControlRecursiveOptions;
```

## <a name="connect-to-the-account"></a>Conexión con la cuenta 

Para usar los fragmentos de código de este artículo, tiene que crear una instancia de **DataLakeServiceClient** que represente la cuenta de almacenamiento. 

### <a name="connect-by-using-an-account-key"></a>Conexión con una clave de cuenta

Es la manera más sencilla de conectarse a una cuenta. 

En este ejemplo se crea una instancia de **DataLakeServiceClient** con una clave de cuenta.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/Authorize_DataLake.java" id="Snippet_AuthorizeWithKey":::

### <a name="connect-by-using-azure-active-directory-azure-ad"></a>Conexión con Azure Active Directory (Azure AD)

Puede usar la [biblioteca cliente de identidad de Azure para Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) para autenticar la aplicación con Azure AD.

En este ejemplo se crea una instancia de **DataLakeServiceClient** con un identificador de cliente, un secreto de cliente y un identificador de inquilino.  Para obtener estos valores, consulte [Adquisición de un token de Azure AD para la autorización de solicitudes desde una aplicación cliente](../common/storage-auth-aad-app.md).

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/Authorize_DataLake.java" id="Snippet_AuthorizeWithAzureAD":::

> [!NOTE]
> Para ver más ejemplos, consulte la documentación [Biblioteca cliente de identidad de Azure para Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity).


## <a name="create-a-container"></a>Crear un contenedor

Un contenedor actúa como sistema de archivos para sus archivos. Puede crear uno llamando al método **DataLakeServiceClient.createFileSystem**.

En este ejemplo se crea un contenedor denominado `my-file-system`. 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_CreateFileSystem":::

## <a name="create-a-directory"></a>Creación de un directorio

Cree una referencia de directorio llamando al método **DataLakeFileSystemClient.createDirectory**.

En este ejemplo se agrega un directorio denominado `my-directory` a un contenedor y luego se agrega un subdirectorio denominado `my-subdirectory`. 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_CreateDirectory":::

## <a name="rename-or-move-a-directory"></a>Cambio de nombre o traslado de un directorio

Cambie el nombre de un directorio o muévalo llamando al método **DataLakeDirectoryClient.rename**. Pase la ruta de acceso del directorio que busca a un parámetro. 

En este ejemplo se cambia el nombre de un subdirectorio a `my-subdirectory-renamed`.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_RenameDirectory":::

En este ejemplo se mueve un directorio denominado `my-subdirectory-renamed` a un subdirectorio de un directorio denominado `my-directory-2`. 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_MoveDirectory":::

## <a name="delete-a-directory"></a>Eliminación de un directorio

Elimine un directorio llamando al método **DataLakeDirectoryClient.deleteWithResponse**.

En este ejemplo se elimina un directorio denominado `my-directory`.   

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_DeleteDirectory":::

## <a name="upload-a-file-to-a-directory"></a>Carga de un archivo en un directorio

En primer lugar, cree una referencia de archivo en el directorio de destino creando una instancia de la clase **DataLakeFileClient**. Cargue un archivo llamando al método **DataLakeFileClient.append**. Asegúrese de completar la carga llamando al método **DataLakeFileClient.FlushAsync**.

En este ejemplo se carga un archivo de texto en un directorio denominado `my-directory`.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_UploadFile":::

> [!TIP]
> Si el tamaño del archivo es grande, el código tendrá que realizar varias llamadas al método **DataLakeFileClient.append**. Considere la posibilidad de usar en su lugar el método **DataLakeFileClient.uploadFromFile**. De este modo, puede cargar todo el archivo en una sola llamada. 
>
> Vea la sección siguiente para obtener un ejemplo.

## <a name="upload-a-large-file-to-a-directory"></a>Carga de un archivo de grand tamaño en un directorio

Use el método **DataLakeFileClient.uploadFromFile** para cargar archivos grandes sin tener que realizar varias llamadas al método **DataLakeFileClient.append**.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_UploadFileBulk":::

## <a name="download-from-a-directory"></a>Descarga de un directorio

En primer lugar, cree una instancia de **DataLakeFileClient** que represente al archivo que quiere descargar. Use el método **DataLakeFileClient.Read** para leer el archivo. Use cualquier archivo .NET que procese API para guardar bytes de la transmisión en un archivo. 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_DownloadFile":::

## <a name="list-directory-contents"></a>Lista del contenido del directorio

En este ejemplo, se imprimen los nombres de cada uno de los archivos que se ubican en un directorio denominado `my-directory`.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_ListFilesInDirectory":::

## <a name="manage-access-control-lists-acls"></a>Administración de listas de control de acceso

Puede obtener, establecer y actualizar los permisos de acceso de los directorios y archivos.

> [!NOTE]
> Si usa Azure Active Directory (Azure AD) para autorizar el acceso, asegúrese de que la entidad de seguridad tenga asignado el [rol Propietario de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner). Para obtener más información acerca de cómo se aplican los permisos de ACL y las consecuencias de cambiarlos, consulte [Control de acceso en Azure Data Lake Storage Gen2](./data-lake-storage-access-control.md).

### <a name="manage-a-directory-acl"></a>Administración de una lista de control de acceso de directorio

En este ejemplo se obtiene y después se establece la ACL de un directorio denominado `my-directory`. En este ejemplo se concede al usuario propietario permisos de lectura, escritura y ejecución, permisos de solo lectura y ejecución al grupo propietario, y permisos de lectura y escritura al resto.

> [!NOTE]
> Si la aplicación autoriza el acceso mediante Azure Active Directory (Azure AD), asegúrese de que la entidad de seguridad que la aplicación usa para autorizar el acceso tiene asignado el [rol de propietario de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner). Para más información sobre cómo se aplican los permisos de ACL y las consecuencias de cambiarlos, vea [Control de acceso en Azure Data Lake Storage Gen2](./data-lake-storage-access-control.md).

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/ACL_DataLake.java" id="Snippet_ManageDirectoryACLs":::

También puede obtener y establecer la ACL del directorio raíz de un contenedor. Para obtener el directorio raíz, pase una cadena vacía (`""`) en el método **DataLakeFileSystemClient.getDirectoryClient**.

### <a name="manage-a-file-acl"></a>Administración de una lista de control de acceso de archivo

En este ejemplo se obtiene y después se establece la ACL de un archivo denominado `upload-file.txt`. En este ejemplo se concede al usuario propietario permisos de lectura, escritura y ejecución, permisos de solo lectura y ejecución al grupo propietario, y permisos de lectura y escritura al resto.

> [!NOTE]
> Si la aplicación autoriza el acceso mediante Azure Active Directory (Azure AD), asegúrese de que la entidad de seguridad que la aplicación usa para autorizar el acceso tiene asignado el [rol de propietario de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner). Para obtener más información acerca de cómo se aplican los permisos de ACL y las consecuencias de cambiarlos, consulte [Control de acceso en Azure Data Lake Storage Gen2](./data-lake-storage-access-control.md).

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/ACL_DataLake.java" id="Snippet_ManageFileACLs":::

### <a name="set-an-acl-recursively"></a>Establecimiento de una ACL de forma recursiva

Puede agregar, actualizar y quitar las ACL de forma recursiva para los elementos secundarios existentes de un directorio primario sin tener que realizar estos cambios individualmente para cada elemento secundario. Para obtener más información, consulte [Establecimiento de listas de control de acceso (ACL) de forma recursiva para Azure Data Lake Storage Gen2](recursive-access-control-lists.md).

## <a name="see-also"></a>Consulte también

* [Documentación de referencia de API](/java/api/overview/azure/storage-file-datalake-readme)
* [Paquete (Maven)](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake)
* [Muestras](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake)
* [Asignación de Gen1 a Gen2](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)
* [Problemas conocidos](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Envíenos sus comentarios](https://github.com/Azure/azure-sdk-for-java/issues)