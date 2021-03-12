---
title: Restablecimiento y reactivación de un dispositivo Azure Stack Edge Pro | Microsoft Docs
description: Aprenda cómo borrar los datos del dispositivo Azure Stack Edge Pro para luego reactivarlo.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/03/2020
ms.author: alkohli
ms.openlocfilehash: 95fbe5dc4a934fee10c558f640bc24f261203a33
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102443558"
---
# <a name="reset-and-reactivate-your-azure-stack-edge-pro-device"></a>Restablecimiento y reactivación de un dispositivo Azure Stack Edge Pro

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

En este artículo se describe cómo restablecer, volver a configurar y reactivar un dispositivo Azure Stack Edge Pro si tiene problemas con el dispositivo o necesita empezar de nuevo por algún otro motivo.

Después de restablecer el dispositivo para quitar los datos, debe reactivarlo como un nuevo recurso. Al restablecer un dispositivo, se quita la configuración del dispositivo, por lo que tendrá que volver a configurarlo a través de la interfaz de usuario web local.

En este artículo aprenderá a:

> [!div class="checklist"]
>
> * Borrar los datos de los discos de datos del dispositivo
> * Reactivar el dispositivo mediante la creación de un nuevo pedido, la reconfiguración del dispositivo y su activación

## <a name="reset-data-from-the-device"></a>Restablecimiento de los datos del dispositivo

Para borrar los datos de los discos de datos del dispositivo, debe restablecer el dispositivo.

Antes de restablecerlo, cree una copia de los datos locales del dispositivo si es necesario. Puede copiar los datos del dispositivo en un contenedor de Azure Storage.

>[!IMPORTANT]
> Al restablecer el dispositivo, se borrarán todos los datos locales y las cargas de trabajo del dispositivo; esto no se podrá revertir. Restablezca el dispositivo solo si quiere empezar de nuevo con el dispositivo.

Puede restablecer el dispositivo en la interfaz de usuario web local o en PowerShell. Para obtener instrucciones de PowerShell, vea [Restablecimiento del dispositivo](./azure-stack-edge-connect-powershell-interface.md#reset-your-device).

[!INCLUDE [Reset data from the device](../../includes/azure-stack-edge-device-reset.md)]

## <a name="reactivate-device"></a>Reactivación del dispositivo

Después de restablecer el dispositivo, debe reactivarlo como un nuevo recurso. Después de realizar un nuevo pedido, deberá volver a configurar el nuevo recurso y, a continuación, reactivarlo.

Para volver a activar el dispositivo existente, siga estos pasos:

1. Cree un nuevo pedido para el dispositivo existente según los pasos descritos en [Creación de un recurso](azure-stack-edge-gpu-deploy-prep.md?tabs=azure-portal#create-a-new-resource). En la pestaña **Dirección de envío**, seleccione **Ya tengo un dispositivo**.

   ![No especifique ningún dispositivo nuevo en Dirección de envío.](./media/azure-stack-edge-reset-reactivate-device/create-resource-with-no-new-device.png)

1. [Obtenga la clave de activación](azure-stack-edge-gpu-deploy-prep.md?tabs=azure-portal#get-the-activation-key).

1. [Conéctese al dispositivo](azure-stack-edge-gpu-deploy-connect.md).

1. [Configure la red para el dispositivo](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md).

1. [Configure el dispositivo](azure-stack-edge-gpu-deploy-set-up-device-update-time.md).

1. [Configure los certificados](azure-stack-edge-gpu-deploy-configure-certificates.md).

1. [Active el dispositivo](azure-stack-edge-gpu-deploy-activate.md).

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [conectar un dispositivo Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-connect.md).
