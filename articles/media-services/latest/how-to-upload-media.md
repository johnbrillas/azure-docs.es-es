---
título: Carga de soportes físicos: Descripción de Azure Media Services: Obtenga información sobre cómo cargar elementos multimedia para la transmisión por secuencias o la codificación.
services: media-services documentationcenter: '' author: IngridAtMicrosoft manager: femila editor: ''

ms.service: media-services ms.workload: ms.topic: how-to ms.date: 08/31/2020 ms.author: inhenkel
---

# <a name="upload-media-for-streaming-or-encoding"></a>Carga de elementos multimedia para la transmisión por secuencias o la codificación

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

En Media Services, los archivos digitales (elementos multimedia) se cargan en un contenedor de blobs asociado con un recurso. La entidad [Recurso](/rest/api/media/operations/asset) puede contener archivos de vídeo, audio, imágenes, colecciones de miniaturas, pistas de texto y subtítulos (y los metadatos sobre estos archivos). Una vez cargados los archivos en el contenedor del recurso, el contenido se almacena de forma segura en la nube para un posterior procesamiento y streaming.

Antes de empezar, deberá recopilar o pensar algunos valores.

1. La ruta de acceso del archivo local que desea cargar
1. El identificador de recurso del recurso (contenedor)
1. El nombre que desea usar para el archivo cargado, incluida la extensión
1. El nombre de la cuenta de almacenamiento que utiliza
1. La clave de acceso de la cuenta de almacenamiento

## <a name="portal"></a>[Portal](#tab/portal/)

[!INCLUDE [Upload files with the portal](./includes/task-upload-file-to-asset-portal.md)]

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [Upload files with the CLI](./includes/task-upload-file-to-asset-cli.md)]

## <a name="python"></a>[Python](#tab/python)

Suponiendo que el código ya ha establecido la autenticación y que ya ha creado un recurso de entrada, use el siguiente fragmento de código para cargar los archivos locales en ese recurso (in_container).

```python
#The storage objects
from azure.storage.blob import BlobServiceClient, BlobClient

#Establish storage variables
storage_account_name = '<your storage account name'
storage_account_key = '<your storage account key'
storage_blob_url = 'https://<your storage account name>.blob.core.windows.net/'

in_container = 'asset-' + inputAsset.asset_id

#The file path of local file you want to upload
source_file = "ignite.mp4"

# Use the Storage SDK to upload the video
blob_service_client = BlobServiceClient(account_url=storage_blob_url, credential=storage_account_key)
blob_client = blob_service_client.get_blob_client(in_container,source_file)

# Upload the video to storage as a block blob
with open(source_file, "rb") as data:
    blob_client.upload_blob(data, blob_type="BlockBlob")
```

---
<!-- add these to the tabs when available -->
Para otros métodos, consulte la [documentación de Azure Storage](../../storage/blobs/index.yml) para trabajar con blobs en [.NET](../../storage/blobs/storage-quickstart-blobs-dotnet.md), [Java](../../storage/blobs/storage-quickstart-blobs-java.md), [Python](../../storage/blobs/storage-quickstart-blobs-python.md) y [JavaScript (Node.js)](../../storage/blobs/storage-quickstart-blobs-nodejs.md).

## <a name="next-steps"></a>Pasos siguientes

> [Introducción a Media Services](media-services-overview.md)
