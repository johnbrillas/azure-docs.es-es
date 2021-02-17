---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 02/03/2021
ms.author: fauhse
ms.custom: include file
ms.openlocfilehash: a086aae35c9a800c6a4cfc3e872a34438bc84095
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99569480"
---
De forma predeterminada, los recursos compartidos de archivos estándar solo pueden abarcar hasta 5 TiB, pero puede aumentar el límite de recursos compartidos a 100 TiB. Para aumentar el límite de recursos compartidos, habilite **Large file share** (Recursos compartidos de archivos grandes) en la cuenta de almacenamiento. Las cuentas de almacenamiento prémium (cuentas de almacenamiento *FileStorage*) no tienen la marca de característica de recursos compartidos de archivos grandes, ya que todos los recursos compartidos de archivos prémium ya están habilitados para aprovisionar hasta la capacidad completa de 100 TiB.

Solo puede habilitar recursos compartidos de archivos grandes en cuentas de almacenamiento estándar con redundancia local o con redundancia de zona. Una vez habilitada la marca de la característica de recursos compartidos de archivos grandes, no puede cambiar el nivel de redundancia a almacenamiento con redundancia geográfica o con redundancia de zona geográfica.

Para permitir recursos compartidos de archivos grandes en una cuenta de almacenamiento existente, vaya a **Recursos compartidos de archivos** en la tabla de contenido de la cuenta de almacenamiento.
En esta hoja, seleccione **Capacidad del recurso compartido**, cambie la capacidad del recurso compartido a **100 TiB** y seleccione **Guardar**.

:::image type="content" source="media/storage-files-tiers-enable-large-shares/enable-lfs.png" alt-text="Captura de pantalla de la habilitación de la opción de inclusión de recursos compartidos de archivos de gran tamaño en Azure Portal." lightbox="media/storage-files-tiers-enable-large-shares/increase-share-capacity.png":::

También puede habilitar recursos compartidos de archivos de 100 TiB mediante el cmdlet [`Set-AzStorageAccount`](/powershell/module/az.storage/set-azstorageaccount) de PowerShell y el comando [`az storage account update`](/cli/azure/storage/account#az-storage-account-update) de la CLI de Azure. Para obtener instrucciones detalladas sobre cómo habilitar los recursos compartidos de archivos de gran tamaño, consulte [Habilitar y crear recursos compartidos de archivos de gran tamaño](../articles/storage/files/storage-files-how-to-create-large-file-share.md).

Para más información sobre cómo crear recursos compartidos de archivos en nuevas cuentas de almacenamiento, consulte [Creación de un recurso compartido de archivos de Azure](../articles/storage/files/storage-how-to-create-file-share.md).