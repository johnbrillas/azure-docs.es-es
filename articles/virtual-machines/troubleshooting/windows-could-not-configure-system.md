---
title: 'Solución de problemas: Windows no pudo acabar de configurar el sistema'
titlesuffix: Azure Virtual Machines
description: En este artículo se proporcionan los pasos para resolver problemas en los que el proceso Sysprep impide el arranque de una máquina virtual de Azure.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 5fc57a8f-5a0c-4b5f-beef-75eecb4d310d
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/09/2020
ms.author: v-miegge
ms.openlocfilehash: 6cb3467fec99bd12810ed058a61de1be7b39cdd0
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98629596"
---
# <a name="troubleshoot-windows-could-not-finish-configuring-the-system"></a>Solución de problemas: Windows no pudo acabar de configurar el sistema

En este artículo se proporcionan los pasos para resolver problemas en los que el proceso Sysprep impide el arranque de una máquina virtual (VM) de Azure.

## <a name="symptom"></a>Síntoma

Si usa [Diagnósticos de arranque](./boot-diagnostics.md) para ver la captura de pantalla de la VM, verá que se muestra un error de instalación de Windows mientras la configuración de Windows inicia los servicios. El error mostrará el mensaje:

`Windows could not finish configuring the system. To attempt to resume configuration, restart the computer. Setup is starting services`

  ![En la figura 1 se muestra un error de instalación de Windows con el mensaje: "Windows no pudo finalizar la configuración del sistema. Para intentar reanudar la configuración, reinicie el equipo. El programa de instalación está iniciando los servicios".](./media/windows-could-not-configure-system/1-windows-error-configure.png)

## <a name="cause"></a>Causa

Este error se produce cuando el sistema operativo (SO) no puede completar el [proceso Sysprep](/windows-hardware/manufacture/desktop/sysprep-process-overview). Este error se producirá cuando se intente un arranque inicial de una máquina virtual generalizada. Si se produce este problema, vuelva a crear la imagen generalizada, ya que la imagen se encuentra en un estado no implementable y no se puede recuperar.

## <a name="solution"></a>Solución

> [!TIP]
> Si tiene una copia de seguridad reciente de la máquina virtual, puede intentar [restaurarla](../../backup/backup-azure-arm-restore-vms.md) para corregir el problema de arranque.

Para corregir este problema, siga las [instrucciones de Azure para preparar o capturar una imagen](../windows/upload-generalized-managed.md) y prepare una nueva imagen generalizada.