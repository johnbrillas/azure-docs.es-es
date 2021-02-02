---
title: Reemplazo de un dispositivo Azure Stack Edge Pro | Microsoft Docs
description: Se describe cómo obtener el reemplazo de un dispositivo Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 01/22/2021
ms.author: alkohli
ms.openlocfilehash: 91cb446da31f353162f90778d855056a9697d455
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98726590"
---
# <a name="replace-your-azure-stack-edge-pro-device"></a>Reemplazo de un dispositivo Azure Stack Edge Pro

En este artículo se describe cómo reemplazar un dispositivo Azure Stack Edge Pro. Se necesita un dispositivo de sustitución cuando el dispositivo existente tiene un error de hardware o necesita una actualización. 


En este artículo aprenderá a:

> [!div class="checklist"]
>
> * Abrir una incidencia de soporte técnico para el problema de hardware.
> * Crear un nuevo pedido para el dispositivo de reemplazo en Azure Portal.
> * Instalar y activar el dispositivo de reemplazo.
> * Devolver el dispositivo original.

## <a name="open-a-support-ticket"></a>Abrir una incidencia de soporte técnico

Si el dispositivo existente tiene un error de hardware, abra una incidencia de soporte técnico con estos pasos:

1. Abra una incidencia de soporte técnico con Microsoft Support para indicar que quiere devolver el dispositivo. Seleccione el tipo de problema **Azure Stack Edge Pro Hardware** (hardware de Azure Stack Pro) y elija el subtipo **Hardware issues** (problemas de hardware).  

    ![Abrir una incidencia de soporte técnico](media/azure-stack-edge-replace-device/open-support-ticket-1.png)  

2. Un ingeniero de Soporte técnico de Microsoft se pondrá en contacto con usted para determinar si una unidad de reemplazo en la ubicación (FRU) puede corregir el problema y está disponible para este caso. Si una FRU no está disponible o el dispositivo necesita una actualización de hardware, el soporte técnico le guiará para realizar un nuevo pedido y devolver el dispositivo antiguo.

## <a name="create-a-new-order"></a>Creación de un pedido

Cree un nuevo recurso para la activación del dispositivo de reemplazo con los pasos descritos en [Creación de un recurso](azure-stack-edge-gpu-deploy-prep.md#create-a-new-resource).

> [!NOTE]
> No se admite la activación de un dispositivo de reemplazo en un recurso existente. El nuevo recurso se considera un nuevo pedido. Comenzará a recibir facturas 14 días después de que se le envíe el dispositivo.

## <a name="install-and-activate-the-replacement-device"></a>Instalación y activación del dispositivo de reemplazo

Siga estos pasos para instalar y activar el dispositivo de reemplazo:

1. [Instale el dispositivo](azure-stack-edge-deploy-install.md).
2. [Active el dispositivo](azure-stack-edge-deploy-connect-setup-activate.md) con el nuevo recurso que creó anteriormente.

## <a name="return-your-existing-device"></a>Devolución del dispositivo existente

Siga todos los pasos para devolver el dispositivo original:

1. [Borre los datos del dispositivo](azure-stack-edge-return-device.md#erase-data-from-the-device).
2. [Iniciar la devolución del dispositivo](azure-stack-edge-return-device.md#initiate-device-return) original.
3. [Programe una recogida](azure-stack-edge-return-device.md#schedule-a-pickup).
4. Una vez que Microsoft recibe el dispositivo, puede [eliminar el recurso](azure-stack-edge-return-device.md#delete-the-resource) asociado al dispositivo devuelto.


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [devolver un dispositivo Azure Stack Edge Pro](azure-stack-edge-return-device.md).
