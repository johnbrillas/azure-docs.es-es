---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/02/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e207866b61d21334bc9923d0d784b900906b0045
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2021
ms.locfileid: "98539717"
---
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y navegue hasta el disco del que desea crear una instantánea.
1. En el disco, seleccione **Crear una instantánea**.

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-snapshot-button-incremental.png" alt-text="Captura de pantalla. Hoja del disco con la opción **+ Crear instantánea** resaltada, que es la que debe seleccionar.":::

1. Seleccione el grupo de recursos que desea usar y escriba un nombre.
1. Seleccione **Incremental** y elija **Revisar y crear**.

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/incremental-snapshot-create-snapshot-blade.png" alt-text="Captura de pantalla. Cree una hoja de instantánea, rellene el nombre y seleccione Incremental. A continuación, cree la instantánea.":::

1. Seleccione **Crear**

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-incremental-snapshot-validation.png" alt-text="Captura de pantalla. Página de validación de la instantánea. Confirme sus selecciones y, a continuación, cree la instantánea.":::