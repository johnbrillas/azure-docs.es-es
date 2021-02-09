---
title: Blobs cambiados y eliminados
titleSuffix: Azure Cognitive Search
description: Después de la creación inicial del índice de búsqueda que importa desde Azure Blob Storage, la posterior indexación puede elegir los blobs que se cambian o eliminan. En este artículo se explican los detalles.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/29/2021
ms.openlocfilehash: b23dabb4388331de9e37ee9db1d4b9d727ccde68
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99430567"
---
# <a name="how-to-set-up-change-and-deletion-detection-for-blobs-in-azure-cognitive-search-indexing"></a>Configuración de la detección de cambios y eliminaciones para blobs en la indexación de Azure Cognitive Search

Después de crear un índice de búsqueda inicial, es posible que desee que los trabajos posteriores del indexador solo recojan documentos nuevos y modificados. En el caso del contenido de búsqueda cuyo origen sea Azure Blob Storage, la detección de cambios se produce automáticamente cuando se usa una programación para desencadenar la indexación. De forma predeterminada, el servicio reindexa solo los blobs modificados, tal como lo determina la marca de tiempo `LastModified` del blob. A diferencia de otros orígenes de datos compatibles con indexadores de búsqueda, los blobs siempre tienen una marca de tiempo, lo que elimina la necesidad de configurar manualmente una directiva de detección de cambios.

Aunque la detección de cambios es una obviedad, la detección de eliminaciones no lo es. Si desea detectar documentos eliminados, asegúrese de usar un enfoque de "eliminación temporal". Si elimina los blobs directamente, los documentos correspondientes no se quitarán del índice de búsqueda.

Hay dos maneras de implementar el enfoque de eliminación temporal:

+ Eliminación temporal de blobs nativos (versión preliminar), que se describe a continuación.
+ [Eliminación temporal con metadatos personalizados](#soft-delete-using-custom-metadata)

## <a name="native-blob-soft-delete-preview"></a>Eliminación temporal de blobs nativos (versión preliminar)

Para este enfoque de detección de eliminación, Cognitive Search depende de la característica de [eliminación temporal de blobs nativos](../storage/blobs/soft-delete-blob-overview.md) de Azure Blob Storage para determinar si los blobs han pasado a un estado de eliminación temporal. Cuando se detectan blobs en este estado, un indexador de búsqueda usa esta información para quitar el documento correspondiente del índice.

> [!IMPORTANT]
> La compatibilidad con la eliminación temporal de blobs nativos está en versión preliminar. La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). En la [API de REST, versión 2020-06-30-Preview](./search-api-preview.md) se proporciona esta característica. Actualmente no hay compatibilidad con el portal ni con el SDK de .NET.

### <a name="prerequisites"></a>Requisitos previos

+ [Habilitación de la eliminación temporal para blobs](../storage/blobs/soft-delete-blob-enable.md).
+ Los blobs deben estar en un contenedor de Azure Blob Storage. No se admite la directiva de eliminación temporal de blobs nativos de Cognitive Search para los blobs de Azure Data Lake Storage Gen2.
+ Las claves de documento de los documentos del índice se deben asignar a una propiedad de blob o a metadatos de blob.
+ Debe usar la API REST en versión preliminar (`api-version=2020-06-30-Preview`) para configurar la compatibilidad con la eliminación temporal.

### <a name="how-to-configure-deletion-detection-using-native-soft-delete"></a>Configuración de la detección de eliminación mediante la eliminación temporal nativa

1. En Blob Storage, al habilitar la eliminación temporal, establezca la directiva de retención en un valor mucho mayor que la programación del intervalo del indexador. De este modo, si hay un problema al ejecutar el indexador o si tiene un gran número de documentos para indexar, el indexador tendrá mucho tiempo para procesar los blobs eliminados temporalmente. Los indexadores de Azure Cognitive Search solo eliminarán un documento del índice si procesa el blob mientras se encuentra en un estado de eliminación temporal.

1. En Cognitive Search, establezca una directiva de detección de eliminación temporal de blobs nativos en el origen de datos. A continuación se muestra un ejemplo. Dado que esta característica se encuentra en versión preliminar, debe usar la versión preliminar de la API REST.

    ```http
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2020-06-30-Preview
    Content-Type: application/json
    api-key: [admin key]
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.NativeBlobSoftDeleteDeletionDetectionPolicy"
        }
    }
    ```

1. [Ejecute el indexador](/rest/api/searchservice/run-indexer) o establézcalo para que se ejecute [según una programación](search-howto-schedule-indexers.md). Cuando el indexador ejecuta y procesa un blob que tiene un estado de eliminación temporal, el documento de búsqueda correspondiente se quita del índice.

### <a name="reindexing-undeleted-blobs-using-native-soft-delete-policies"></a>Reindexación de blobs recuperados (mediante directivas de eliminación temporal nativas)

Si restaura un blob eliminado temporalmente en Blob Storage, el indexador no siempre lo volverá a indexar. Esto se debe a que el indexador utiliza la marca de tiempo `LastModified` del blob para determinar si es necesaria la indexación. Cuando se recupera un blob de eliminación temporal, su marca de tiempo `LastModified` no se actualiza, por lo que si el indexador ya ha procesado blobs con marcas de tiempo `LastModified` más recientes, no volverá a indexar el blob recuperado. 

Para garantizar que se vuelva a indexar un blob recuperado, debe actualizar la marca de tiempo `LastModified` del blob. Una forma de hacerlo consiste en volver a guardar los metadatos de ese blob. No es necesario cambiar los metadatos, pero si vuelve a guardar estos, se actualizará la marca de tiempo `LastModified` del blob para que el indexador sepa que debe reindexarlos de nuevo.

## <a name="soft-delete-using-custom-metadata"></a>Eliminación temporal con metadatos personalizados

Este método usa los metadatos de un blob para determinar si un documento de búsqueda se debe eliminar del índice. Este método requiere dos acciones independientes, la eliminación del documento de búsqueda del índice, seguida de la eliminación de blobs en Azure Storage.

Hay pasos que se deben seguir tanto en Blob Storage como en Cognitive Search, pero no hay otras dependencias de características. Esta funcionalidad se admite en las API con disponibilidad general.

1. Agregar un par clave-valor de metadatos personalizado al blob para indicar a Azure Cognitive Search que se ha eliminado de forma lógica.

1. Configurar una directiva de detección de columnas de eliminación temporal en el origen de datos. Por ejemplo, la siguiente directiva considera que un blob se va a eliminar si tiene una propiedad de metadatos `IsDeleted` con el valor `true`:

    ```http
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }
    ```

1. Una vez que el indexador ha procesado el blob y eliminado el documento del índice, puede eliminar el blob de Azure Blob Storage.

### <a name="reindexing-undeleted-blobs-using-custom-metadata"></a>Reindexación de blobs recuperados (mediante metadatos personalizados)

Después de que un indexador procese un blob eliminado y quite el documento de búsqueda correspondiente del índice, no volverá a visitar ese blob si lo restaura más tarde si la marca de tiempo del `LastModified` del blob es anterior a la última ejecución del indexador.

Si desea volver a indexar ese documento, cambie el valor de `"softDeleteMarkerValue" : "false"` del blob y vuelva a ejecutar el indexador.

## <a name="next-steps"></a>Pasos siguientes

+ [Indexadores de Azure Cognitive Search](search-indexer-overview.md)
+ [Configuración de un indexador de blobs](search-howto-indexing-azure-blob-storage.md)
+ [Introducción a la indexación de blobs](search-blob-storage-integration.md)