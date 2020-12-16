---
title: Problemas con un disco de sistema operativo completo en una máquina virtual Linux
description: Resolución de problemas con un disco de sistema operativo completo en una máquina virtual Linux
author: v-miegge
ms.author: tibasham
manager: dcscontentpm
ms.service: virtual-machines
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: troubleshooting
ms.date: 11/20/2020
ms.openlocfilehash: 2e350fe297b2aaf89ac302baaefb29092cfe2532
ms.sourcegitcommit: 84e3db454ad2bccf529dabba518558bd28e2a4e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96523336"
---
# <a name="issues-with-a-full-os-disk-on-a-linux-virtual-machine"></a>Problemas con un disco de sistema operativo completo en una máquina virtual Linux

Cuando se llena el disco del sistema operativo de una máquina virtual Linux (VM), la máquina virtual puede dejar de funcionar correctamente.

## <a name="symptom"></a>Síntoma

Al intentar crear un archivo, aparece este mensaje:

```
username@AZUbuntu1404:~$ touch new 
touch: cannot touch “new”: No space left on device 
username@AZUbuntu1404:~$
```

A continuación, varios demonios indican que no pueden crear archivos temporales durante la sesión de arranque.

```
ERROR:IOError: [Errno 28] No space left on device: '/var/lib/waagent/events/1474306860983232.tmp' 
    
OSError: [Errno 28] No space left on device: '/var/lib/cloud/data/tmpDZCq0g'
```
    
## <a name="cause"></a>Causa

Hay varias razones por las que puede aparecer este mensaje:

1. Puede que el disco esté lleno.
1. Es posible que el sistema de archivos se haya quedado sin inodes.
1. Es posible que un disco de datos se haya montado en un directorio existente, lo que hace que los archivos estén ocultos.
1. Archivos que abre un proceso y que, después, elimina.

## <a name="solution"></a>Solución

### <a name="process-overview"></a>Información general del proceso

1. Cree una VM de reparación y acceda a esta.
1. Libere espacio en el disco.
1. Vuelva a compilar la VM.

> [!NOTE]
> Este error indica que el sistema operativo invitado no está operativo. Resuelva este problema en el modo sin conexión.

### <a name="create-and-access-a-repair-vm"></a>Creación de una máquina virtual de reparación y acceso a ella

1. Siga los pasos 1 a 3 de los [comandos de reparación de VM](./repair-linux-vm-using-azure-virtual-machine-repair-commands.md) para preparar una VM de reparación.
1. Utilice SSH para conectarse a dicha máquina virtual.

### <a name="free-up-space-on-the-disk"></a>Liberación de espacio en el disco

Para resolver el problema:

- Cambie el tamaño del disco al máximo de 1 TB si no tiene ya este tamaño.
- Libere espacio en disco.

1. Compruebe si el disco está lleno. Si el tamaño del disco no llega a 1 TB, expándalo hasta un máximo de 1 TB [mediante la CLI de Azure](../linux/expand-disks.md).
1. Si el disco ya tiene 1 TB, tendrá que liberar espacio en disco.
1. Una vez que haya terminado de cambiar el tamaño y de limpiar el disco, vuelva a crear la máquina virtual.

### <a name="rebuild-the-vm"></a>Recompilación de la máquina virtual

Realice el [paso 5 de los comandos de reparación de VM](./repair-linux-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) para volver a crear la VM.
