---
title: Solución de problemas del agente de Linux de Azure
description: Resuelva problemas con el agente de Linux de Azure.
services: virtual-machines-linux
ms.service: virtual-machines-linux
author: axelg
manager: dcscontentpm
editor: ''
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2020
ms.author: axelg
ms.openlocfilehash: 62b462d8e75fc291ac599ac99dbe4fb3a74fde2b
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98878704"
---
# <a name="troubleshoot-the-azure-linux-agent"></a>Solución de problemas del agente de Linux de Azure

El [agente de Linux de Azure](../extensions/agent-linux.md) permite que la máquina virtual se comunique con el controlador de tejido (el servidor físico subyacente en el que se hospeda la máquina virtual) en la dirección IP 168.63.129.16.

>[!NOTE]
>Esta dirección IP es una dirección IP pública virtual que facilita la comunicación y no debe estar bloqueada. Para obtener más información, vea [¿Qué es la dirección IP 168.63.129.16?](../../virtual-network/what-is-ip-address-168-63-129-16.md).

## <a name="before-you-begin"></a>Antes de empezar

Compruebe el estado y la versión del agente para asegurarse de que sigue siendo compatible. Consulte [Versión mínima admitida para los agentes de la máquina virtual en Azure](/troubleshoot/azure/virtual-machines/support-extensions-agent-version) para comprobar la compatibilidad de versiones, o consulte las [preguntas más frecuentes de WALinuxAgent](https://github.com/Azure/WALinuxAgent/wiki/FAQ#what-does-goal-state-agent-mean-in-waagent---version-output) para conocer los pasos para encontrar el estado y la versión.

## <a name="troubleshoot-a-not-ready-status"></a>Solución de problemas con el estado No está listo

1. Compruebe el estado del servicio del agente de Linux de Azure para asegurarse de que se está ejecutando. El nombre del servicio podría ser **walinuxagent** o **waagent**.

   ```
   root@nam-u18:/home/nam# service walinuxagent status
   ● walinuxagent.service - Azure Linux Agent
      Loaded: loaded (/lib/systemd/system/walinuxagent.service; enabled; vendor preset: enabled)
      Active: active (running) since Thu 2020-10-08 17:10:29 UTC; 3min 9s ago
    Main PID: 1036 (python3)
       Tasks: 4 (limit: 4915)
      CGroup: /system.slice/walinuxagent.service
              ├─1036 /usr/bin/python3 -u /usr/sbin/waagent -daemon
              └─1156 python3 -u bin/WALinuxAgent-2.2.51-py2.7.egg -run-exthandlers
   Oct 08 17:10:33 nam-u18 python3[1036]: 2020-10-08T17:10:33.129375Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.OSTCExtensions.VMAccessForLinux-1.5.10, path: /sys/fs/cgroup/memory/sys
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.189020Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Target handler state: enabled [incarnation 2]
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.197932Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] [Enable] current handler state is: enabled
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.212316Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Update settings file: 0.settings
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.224062Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Enable extension [bin/run-command-shim enable]
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.236993Z INFO ExtHandler ExtHandler Started extension in unit 'Microsoft.CPlat.Core.RunCommandLinux_1.0.1_db014406-294a-49ed-b112-c7912a86ae9e
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.263572Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.CPlat.Core.RunCommandLinux-1.0.1, path: /sys/fs/cgroup/cpu,cpuacct/syst
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.280691Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.CPlat.Core.RunCommandLinux-1.0.1, path: /sys/fs/cgroup/memory/system.sl
   Oct 08 17:10:37 nam-u18 python3[1036]: 2020-10-08T17:10:37.349090Z INFO ExtHandler ExtHandler ProcessGoalState completed [incarnation 2; 4496 ms]
   Oct 08 17:10:37 nam-u18 python3[1036]: 2020-10-08T17:10:37.365590Z INFO ExtHandler ExtHandler [HEARTBEAT] Agent WALinuxAgent-2.2.51 is running as the goal state agent [DEBUG HeartbeatCounter: 1;Heartb
   root@nam-u18:/home/nam#
   ```

   Si el servicio se está ejecutando, reinícielo para resolver el problema. Si el servicio está detenido, inícielo, espere unos minutos y vuelva a comprobar el estado.

1. Compruebe que la actualización automática está habilitada. Compruebe la configuración de actualización automática en **/etc/waagent.conf**.

   ```
   AutoUpdate.Enabled=y
   ```

   Para obtener más información sobre cómo actualizar el agente de Linux de Azure, consulte [Actualización del agente Linux de Azure en una máquina virtual](../extensions/update-linux-agent.md).

1. Asegúrese de que la máquina virtual se puede conectar al controlador de tejido. Use una herramienta como cURL para probar si la máquina virtual puede conectarse a 168.63.129.16 en los puertos 80, 443 y 32526. Si la máquina virtual no se conecta según lo previsto, compruebe si la comunicación saliente a través de los puertos 80, 443 y 32526 está abierta en el firewall local de la máquina virtual. Si esta dirección IP está bloqueada, el agente de máquina virtual podría mostrar un comportamiento inesperado.

## <a name="advanced-troubleshooting"></a>Pasos detallados para solucionar problemas de conexión a Escritorio remoto a máquinas virtuales Windows en Azure

Los eventos para solucionar problemas del agente de Linux de Azure se registran en el archivo **/var/log/waagent.log**.

### <a name="unable-to-connect-to-wireserver-ip-host-ip"></a>No se puede conectar con la dirección IP de WireServer (IP de host)

El siguiente error aparece en el archivo **/var/log/waagent.logg** cuando la máquina virtual no puede tener acceso a la dirección IP de WireServer en el servidor host.

```
2020-10-02T18:11:13.148998Z WARNING ExtHandler ExtHandler An error occurred while retrieving the goal state:
```

Para solucionar este problema:

* Conéctese a la máquina virtual mediante SSH y, a continuación, intente acceder a la siguiente dirección URL desde rizo: http://168.63.129.16/?comp=versions.
* Compruebe si hay algún problema provocado por un firewall, un proxy u otro origen que pueda estar bloqueando el acceso a la dirección IP 168.63.129.16.
* Compruebe si Linux IPTables o un firewall de terceros está bloqueando el acceso a los puertos 80, 443 y 32526.

## <a name="next-steps"></a>Pasos siguientes

Para solucionar más problemas del agente de Linux de Azure, [póngase en contacto con el servicio de soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).