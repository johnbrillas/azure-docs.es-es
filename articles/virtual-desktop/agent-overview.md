---
title: Introducción al agente de Windows Virtual Desktop
description: Información general sobre el agente de Windows Virtual Desktop y los procesos de actualización.
author: Sefriend
ms.topic: conceptual
ms.date: 12/16/2020
ms.author: sefriend
manager: clarkn
ms.openlocfilehash: 325502255e84e38a39ca5b90ee4126354c0d425b
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98601245"
---
# <a name="get-started-with-the-windows-virtual-desktop-agent"></a>Introducción al agente de Windows Virtual Desktop

En el marco de servicio de Windows Virtual Desktop, hay tres componentes principales: el cliente de Escritorio remoto, el servicio y las máquinas virtuales. Estas máquinas virtuales residen en la suscripción del cliente en la que están instalados el agente de Windows Virtual Desktop y el cargador de arranque del agente. El agente actúa como comunicador intermedio entre el servicio y las máquinas virtuales, lo que permite la conectividad. Por lo tanto, si tiene problemas con la instalación, la actualización o la configuración del agente, las máquinas virtuales no podrán conectarse al servicio. El cargador de arranque del agente es el ejecutable que carga al agente. 

En este artículo se ofrece una breve descripción de los procesos de instalación y actualización del agente.

>[!NOTE]
>Esta documentación no está dirigida para el agente de FSLogix ni para el agente de cliente de Escritorio remoto.


## <a name="initial-installation-process"></a>Proceso de instalación inicial

El agente de Windows Virtual Desktop se instala inicialmente de una de dos maneras. Si aprovisiona máquinas virtuales (VM) en Azure Portal y Azure Marketplace, el agente y cargador de arranque del agente se instalarán automáticamente. Si aprovisiona VM mediante PowerShell, debe descargar manualmente los archivos .msi del agente y del cargador de arranque del agente al [crear un grupo de hosts de Windows Virtual Desktop con PowerShell](create-host-pools-powershell.md#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool). Una vez instalado el agente, la pila en paralelo de Windows Virtual Desktop y el agente de Geneva Monitoring también se instalan simultáneamente. El componente de pila en paralelo es necesario para que los usuarios establezcan conexiones inversas seguras del servidor al cliente. El agente de Geneva Monitoring supervisa el estado del agente. Los tres componentes son esenciales para que la conectividad de los usuarios de un extremo a otro funcione correctamente.

>[!IMPORTANT]
>Para instalar correctamente el agente de Windows Virtual Desktop, la pila en paralelo y el agente de Geneva Monitoring, debe desbloquear todas las direcciones URL que se enumeran en la [Lista de direcciones URL requeridas](safe-url-list.md#virtual-machines). El desbloqueo de estas direcciones URL es necesario para usar el servicio Windows Virtual Desktop.

## <a name="agent-update-process"></a>Proceso de actualización del agente

El servicio Windows Virtual Desktop actualiza automáticamente el agente cada vez que hay disponible una actualización. Las actualizaciones del agente pueden incluir nuevas funcionalidades o corregir problemas anteriores. Una vez instalada la versión inicial del agente de Windows Virtual Desktop, el agente consulta periódicamente el servicio Windows Virtual Desktop para establecer si hay disponible una versión más reciente del agente y de sus componentes. Si hay una nueva versión, el cargador de arranque del agente descarga automáticamente la versión más reciente del agente, la pila en paralelo y el agente de Geneva Monitoring.

>[!NOTE]
>- Cuando el agente de Geneva Monitoring se actualiza a la versión más reciente, se busca la antigua tarea GenevaTask y se deshabilita antes de crear una nueva tarea para el nuevo agente de supervisión. La versión anterior del agente de supervisión no se elimina en caso de que la versión más reciente del agente de supervisión tenga un problema que requiera revertir a la versión anterior para corregirlo. Si la versión más reciente tiene un problema, el agente de supervisión anterior se volverá a habilitar para continuar con la entrega de datos de supervisión. Todas las versiones de supervisión anteriores a la última que instaló antes de la actualización se eliminarán de la VM.
>- La VM mantiene tres versiones de la pila en paralelo a la vez. Esto permite una recuperación rápida si algo va mal con la actualización. La versión más antigua de la pila se quita de la VM cada vez que se actualiza la pila.

Esta instalación de una actualización normalmente tarda entre 2 y 3 minutos en una nueva VM, y no debe hacer que la VM pierda la conexión o se apague. Este proceso de actualización se aplica a Windows Virtual Desktop (clásico) y a la versión más reciente de Windows Virtual Desktop con Azure Resource Manager.

## <a name="next-steps"></a>Pasos siguientes

Ahora que comprende mejor el agente de Windows Virtual Desktop, aquí tiene algunos recursos que pueden resultarle útiles:

- Si tiene problemas relacionados con el agente o la conectividad, consulte la [Guía de solución de problemas del agente de Windows Virtual Desktop](troubleshoot-agent.md).