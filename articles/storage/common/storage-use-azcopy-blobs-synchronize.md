---
title: Sincronización con Azure Blob Storage mediante AzCopy v10 | Microsoft Docs
description: Este artículo contiene una colección de comandos de ejemplo de AzCopy que le ayudarán a realizar la sincronización con Azure Blob Storage.
author: normesta
ms.service: storage
ms.topic: how-to
ms.date: 12/08/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: 6d1e9e8eeddaaa2ce8c891888935faad12d40295
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96907414"
---
# <a name="synchronize-with-azure-blob-storage-by-using-azcopy-v10"></a>Sincronización con Azure Blob Storage mediante AzCopy v10

Puede sincronizar el almacenamiento local con Azure Blob Storage mediante la utilidad de línea de comandos AzCopy v10. 

Puede sincronizar el contenido de un sistema de archivos local con un contenedor de blobs. También puede sincronizar los contenedores y directorios virtuales entre sí. La sincronización es unidireccional. En otras palabras, tendrá que elegir cuál de estos dos puntos de conexión es el origen y cuál es el destino. La sincronización también usa las API de servidor a servidor. Los ejemplos que se presentan en esta sección también funcionan con las cuentas que tienen un espacio de nombres jerárquico. 

> [!NOTE]
> La versión actual de AzCopy no sincroniza entre otros orígenes y destinos (por ejemplo: Almacenamiento de archivos o cubos de Amazon Web Services (AWS) S3).

Para ver ejemplos de otros tipos de tareas, como cargar archivos, descargar blobs o copiar blobs entre cuentas, consulte los vínculos presentados en la sección [Pasos siguientes](#next-steps) de este artículo.

## <a name="get-started"></a>Introducción

Vea el artículo [Introducción a AzCopy](storage-use-azcopy-v10.md) para descargar AzCopy y obtener información sobre las formas de proporcionar credenciales de autorización para el servicio de almacenamiento.

> [!NOTE] 
> En los ejemplos de este artículo se da por hecho que ha proporcionado credenciales de autorización mediante Azure Active Directory (Azure AD).
>
> Si prefiere usar un token de SAS para autorizar el acceso a los datos de blob, puede anexar ese token a la dirección URL de recursos en cada comando AzCopy. Por ejemplo, `'https://<storage-account-name>.blob.core.windows.net/<container-name><SAS-token>'`.

## <a name="guidelines"></a>Directrices

- El comando [sync](storage-ref-azcopy-sync.md) compara nombres de archivo y las marcas de tiempo de la última modificación. Establezca la marca opcional `--delete-destination` en un valor de `true` o `prompt` para eliminar archivos en el directorio de destino si esos archivos ya no existen en el directorio de origen.

- Si establece la marca `--delete-destination` en `true`, AzCopy elimina los archivos sin proporcionar un aviso. Si quiere que aparezca un mensaje antes de que AzCopy elimine un archivo, establezca la marca `--delete-destination` en `prompt`.

- Para evitar eliminaciones accidentales, asegúrese de habilitar la característica de [eliminación temporal](../blobs/soft-delete-blob-overview.md) antes de usar la marca `--delete-destination=prompt|true`.

## <a name="update-a-container-with-changes-to-a-local-file-system"></a>Actualización de un contenedor con los cambios realizados en un sistema de archivos local

En este caso, el contenedor es el destino y el sistema de archivos local es el origen. 

> [!TIP]
> En este ejemplo los argumentos de ruta de acceso se encierran entre comillas simples ('). Use comillas simples en todos los shells de comandos excepto en el shell de comandos de Windows (cmd.exe). Si usa un shell de comandos de Windows (cmd.exe), incluya los argumentos de la ruta de acceso entre comillas dobles ("") en lugar de comillas simples ('').

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy sync '<local-directory-path>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Ejemplo** | `azcopy sync 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive` |

## <a name="update-a-local-file-system-with-changes-to-a-container"></a>Actualización de un sistema de archivos local con los cambios realizados en un contenedor

En este caso, el sistema de archivos local es el destino y el contenedor es el origen.

> [!TIP]
> En este ejemplo los argumentos de ruta de acceso se encierran entre comillas simples ('). Use comillas simples en todos los shells de comandos excepto en el shell de comandos de Windows (cmd.exe). Si usa un shell de comandos de Windows (cmd.exe), incluya los argumentos de la ruta de acceso entre comillas dobles ("") en lugar de comillas simples ('').

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy sync 'https://<storage-account-name>.blob.core.windows.net/<container-name>' 'C:\myDirectory' --recursive` |
| **Ejemplo** | `azcopy sync 'https://mystorageaccount.blob.core.windows.net/mycontainer' 'C:\myDirectory' --recursive` |

## <a name="update-a-container-with-changes-in-another-container"></a>Actualización de un contenedor con cambios en otro contenedor

El primer contenedor que aparece en este comando es el contenedor origen. El segundo es el destino.

> [!TIP]
> En este ejemplo los argumentos de ruta de acceso se encierran entre comillas simples ('). Use comillas simples en todos los shells de comandos excepto en el shell de comandos de Windows (cmd.exe). Si usa un shell de comandos de Windows (cmd.exe), incluya los argumentos de la ruta de acceso entre comillas dobles ("") en lugar de comillas simples ('').

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy sync 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Ejemplo** | `azcopy sync 'https://mysourceaccount.blob.core.windows.net/mycontainer' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

## <a name="update-a-directory-with-changes-to-a-directory-in-another-file-share"></a>Actualización de un directorio con cambios en un directorio de otro recurso compartido de archivos

El primer directorio que aparece en este comando es el origen. El segundo es el destino.

> [!TIP]
> En este ejemplo los argumentos de ruta de acceso se encierran entre comillas simples ('). Use comillas simples en todos los shells de comandos excepto en el shell de comandos de Windows (cmd.exe). Si usa un shell de comandos de Windows (cmd.exe), incluya los argumentos de la ruta de acceso entre comillas dobles ("") en lugar de comillas simples ('').

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy sync 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' --recursive` |
| **Ejemplo** | `azcopy sync 'https://mysourceaccount.blob.core.windows.net/<container-name>/myDirectory' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myDirectory' --recursive` |

## <a name="synchronize-with-optional-flags"></a>Sincronización con marcas opcionales

Puede modificar las operaciones de sincronización mediante marcas opcionales. Estos son algunos ejemplos.

|Escenario|Marca|
|---|---|
|Especificar cómo de estrictamente se deben validar los hashes MD5 al descargarse.|**--check-md5**=\[NoCheck\|LogOnly\|FailIfDifferent\|FailIfDifferentOrMissing\]|
|Excluir archivos en función de un patrón.|**--exclude-path**|
|Especifique el grado de detalles que quiere que sean las entradas de registro relacionadas con la sincronización.|**--log-level**=\[WARNING\|ERROR\|INFO\|NONE\]|

Para obtener una lista completa, vea las [opciones](storage-ref-azcopy-sync.md#options).

## <a name="next-steps"></a>Pasos siguientes

Encuentre más ejemplos en estos artículos:

- [Ejemplos: Carga](storage-use-azcopy-blobs-upload.md)
- [Ejemplos: descarga](storage-use-azcopy-blobs-download.md)
- [Ejemplos: Copia entre cuentas](storage-use-azcopy-blobs-copy.md)
- [Ejemplos: Cubos de Amazon S3](storage-use-azcopy-s3.md)
- [Ejemplos: Azure Files](storage-use-azcopy-files.md)
- [Tutorial: Migración de datos locales al almacenamiento en la nube mediante AzCopy](storage-use-azcopy-migrate-on-premises-data.md)
- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)