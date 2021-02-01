---
title: La máquina virtual no responde mientras espera al servicio del administrador de sesión local
description: En este artículo se proporcionan los pasos para resolver problemas en los que el sistema operativo invitado está detenido a la espera que el administrador de sesión local termine el procesamiento mientras se inicia una máquina virtual de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/22/2020
ms.author: v-mibufo
ms.openlocfilehash: fc3bd5d2590e969db07e9dffa61b4902ea4604c3
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632553"
---
# <a name="vm-is-unresponsive-while-waiting-for-the-local-session-manager-service"></a>La máquina virtual no responde mientras espera al servicio del administrador de sesión local

En este artículo se proporcionan los pasos para resolver problemas en los que el sistema operativo invitado (SO invitado) está detenido a la espera que el administrador de sesión local termine el procesamiento mientras se inicia una máquina virtual de Azure.

## <a name="symptoms"></a>Síntomas

Cuando use los [diagnósticos de arranque](./boot-diagnostics.md) para ver una captura de pantalla de la salida de la máquina virtual, verá que esta captura de pantalla muestra el mensaje "Please wait for the Local Session Manager" (Espere al administrador de sesión local).

![Captura de pantalla en la que se muestra el sistema operativo invitado detenido en Windows Server 2012 R2, con un mensaje "Please wait for the Local Session Manager" (Espere al administrador de sesión local).](media/vm-unresponsive-wait-local-session-manager/vm-unresponsive-wait-local-session-manager-1.png)

## <a name="cause"></a>Causa

Podría haber varios motivos por los que una máquina virtual está detenida a la espera del administrador de sesión local. Si el problema persiste, debe recopilar un volcado de memoria para su análisis.

## <a name="solution"></a>Solución

> [!TIP]
> Si tiene una copia de seguridad reciente de la máquina virtual, puede intentar [restaurarla desde ella](../../backup/backup-azure-arm-restore-vms.md) para corregir el problema de arranque.

En algunos casos, simplemente esperar a que finalice el proceso resolverá el problema. Si la máquina virtual no responde y se bloquea en la pantalla de espera durante más de una hora, debe recopilar un volcado de memoria y, después, ponerse en contacto con el soporte técnico de Microsoft.

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Conexión del disco del sistema operativo a una nueva VM de reparación

1. Para preparar una máquina virtual de reparación, siga los pasos 1 a 3 de los [comandos de reparación de máquinas virtuales](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md).
1. Conéctese a la máquina virtual de reparación mediante la conexión a Escritorio remoto.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Busque el archivo de volcado de memoria y envíe una incidencia de soporte técnico

1. En la máquina virtual de reparación, vaya a la carpeta Windows en el disco del sistema operativo conectado. Por ejemplo, si la letra de unidad asignada al disco del sistema operativo conectado tiene la etiqueta *F*, vaya a `F:\Windows`.
1. Busque el archivo *memory.dmp* y, luego, [envíe una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) y adjunte el archivo de volcado de memoria.
1. Si tiene problemas para encontrar el archivo *memory.dmp*, siga la guía para [generar un archivo de volcado de memoria con llamadas de interrupción no enmascarable (NMI)](/windows/client-management/generate-kernel-or-complete-crash-dump).

Para más información sobre las llamadas a NMI, consulte la guía del usuario de las [llamadas NMI en la consola serie de Azure](./serial-console-windows.md#use-the-serial-console-for-nmi-calls).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Solución de problemas de arranque en máquinas virtuales de Azure](boot-error-troubleshoot.md)