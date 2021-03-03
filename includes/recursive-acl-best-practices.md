---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: normesta
ms.service: storage
ms.topic: include
ms.date: 02/03/2021
ms.author: normesta
ms.custom: include file
ms.openlocfilehash: b0be074c995fcc62f63a4a2ebf4149d3040e7b6b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101749943"
---
## <a name="best-practices"></a>Procedimientos recomendados

En esta sección se proporcionan algunas directrices de procedimientos recomendados para configurar las ACL de forma recursiva. 

#### <a name="handling-runtime-errors"></a>Manejo de errores de tiempo de ejecución

Se puede producir un error de tiempo de ejecución por muchos motivos (por ejemplo: una interrupción o un problema de conectividad de cliente). Si se produce un error de tiempo de ejecución, reinicie el proceso de ACL recursivo. Las ACL se pueden volver a aplicar a los elementos sin provocar ningún efecto negativo. 

#### <a name="handling-permission-errors-403"></a>Manejo de errores de permisos (403)

Si experimenta una excepción de control de acceso al ejecutar un proceso de ACL recursivo, es posible que la [entidad de seguridad](../articles/role-based-access-control/overview.md#security-principal) de AD no tenga permisos suficientes para aplicar una ACL a uno o varios de los elementos secundarios de la jerarquía de directorios. Cuando se produce un error de permiso, el proceso se detiene y se proporciona un token de continuación. Repare el problema de permisos y, después, use el token de continuación para procesar el conjunto de datos restante. Los directorios y archivos que ya se han procesado correctamente no tendrán que procesarse de nuevo. También puede optar por reiniciar el proceso de ACL recursivo. Las ACL se pueden volver a aplicar a los elementos sin provocar ningún efecto negativo. 

#### <a name="credentials"></a>Credenciales 

Se recomienda que aprovisione una entidad de seguridad de Azure AD a la que se haya asignado el rol [Propietario de datos de blobs de almacenamiento](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner) en el ámbito de la cuenta de almacenamiento o el contenedor de destino. 

#### <a name="performance"></a>Rendimiento 

Para reducir la latencia, se recomienda ejecutar el proceso de ACL recursivo en una máquina virtual de Azure que se encuentre en la misma región que la cuenta de almacenamiento. 

#### <a name="acl-limits"></a>Límites de ACL

El número máximo de ACL que puede aplicar a un directorio o archivo es de 32 ACL de acceso y 32 ACL predeterminadas. Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md).