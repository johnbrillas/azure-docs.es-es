---
title: Solución de problemas de Azure Load Balancer
description: Aprenda a solucionar problemas conocidos de Azure Load Balancer.
services: load-balancer
documentationcenter: na
author: asudbring
manager: dcscontentpm
ms.custom: seodoc18
ms.service: load-balancer
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/28/2020
ms.author: allensu
ms.openlocfilehash: bfe2b21a86f2ce4b4630ba69cde87796fd367f4b
ms.sourcegitcommit: e46f9981626751f129926a2dae327a729228216e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98029149"
---
# <a name="troubleshoot-azure-load-balancer-backend-traffic-responses"></a>Solución de problemas de respuestas de tráfico de back-end de Azure Load Balancer

En esta página se proporciona información de solución de problemas para preguntas de Azure Load Balancer.

## <a name="vms-behind-load-balancer-are-not-responding-to-traffic-on-the-configured-data-port"></a>Las VM detrás del equilibrador de carga no responden al tráfico del puerto de datos configurado

Si una máquina virtual del grupo de back-end aparece como correcta y responde a los sondeos de estado, pero aun así no participa en el equilibrio de carga o no responde al tráfico de datos, puede deberse a alguno de los motivos siguientes:
* Que una máquina virtual del grupo de back-end del equilibrador de carga no escuche en el puerto de datos
* Que un grupo de seguridad de red esté bloqueando el puerto de la máquina virtual del grupo de back-end del equilibrador de carga  
* Que se acceda al equilibrador de carga desde la misma máquina virtual y NIC
* Que se acceda al front-end del equilibrador de carga en Internet desde la máquina virtual del grupo de back-end del equilibrador de carga participante

## <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-the-data-port"></a>Causa 1: Que una VM del grupo de back-end del equilibrador de carga no escuche en el puerto de datos

Si una máquina virtual no responde al tráfico de datos, puede deberse a que el puerto de destino no esté abierto en la máquina virtual participante, o a que la máquina virtual no escuche en ese puerto. 

**Validación y resolución**

1. Inicie sesión en la máquina virtual de back-end. 
2. Abra un símbolo del sistema y ejecute el siguiente comando para validar que existe una aplicación de escucha en el puerto de datos:  
            netstat -an 
3. Si el puerto no aparece con el estado "LISTENING" (ESCUCHANDO), configure el puerto de escucha 
4. Si el puerto está marcado como Listening (Escuchando), compruebe si hay problemas en la aplicación de destino de ese puerto.

## <a name="cause-2-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vm"></a>Causa 2: Que un grupo de seguridad de red esté bloqueando el puerto de la máquina virtual del grupo de back-end del equilibrador de carga  

Si uno o varios grupos de seguridad de red configurados en la subred o en la máquina virtual bloquean la dirección IP de origen o el puerto, la máquina virtual no puede responder.

En el caso del equilibrador de carga público, la dirección IP de los clientes de Internet se usará para la comunicación entre los clientes y las VM de back-end del equilibrador de carga. Asegúrese de que la dirección IP de los clientes esté permitida en el grupo de seguridad de red de la VM de back-end.

1. Enumere los grupos de seguridad de red configurados en la máquina virtual de back-end. Para más información, consulte [Administración de los grupos de seguridad de red](../virtual-network/manage-network-security-group.md).
1. En la lista de grupos de seguridad de red, compruebe si:
    - el tráfico entrante o saliente del puerto de datos tiene interferencias. 
    - hay una regla de grupos de seguridad de red **Deny All** en la NIC de la máquina virtual o la subred que tenga una prioridad mayor que la regla predeterminada que permite los sondeos del equilibrador de carga y de tráfico (los grupos de seguridad de red deben permitir la dirección IP del equilibrador de carga 168.63.129.16, que es el puerto de sondeo)
1. Si cualquiera de estas reglas bloquea el tráfico, elimine y vuelva a configurar las reglas para permitir el tráfico de datos.  
1. Pruebe si la máquina virtual ahora responde a los sondeos de estado.

## <a name="cause-3-accessing-the-load-balancer-from-the-same-vm-and-network-interface"></a>Causa 3: Acceso al equilibrador de carga desde la misma VM e interfaz de red 

Que la aplicación hospedada en la máquina virtual de back-end del equilibrador de carga intente acceder a otra aplicación que se hospeda en la misma máquina virtual de back-end a través de la misma interfaz de red es un escenario incompatible y producirá un error. 

**Resolución**: Este problema se puede resolver por una de las siguientes vías:
* Configure las máquinas virtuales del grupo de back-end por separado para cada aplicación. 
* Configure la aplicación en máquinas virtuales de NIC doble para que cada aplicación use su propia interfaz de red y dirección IP. 

## <a name="cause-4-accessing-the-internal-load-balancer-frontend-from-the-participating-load-balancer-backend-pool-vm"></a>Causa 4: Acceso al front-end del equilibrador de carga interno desde la VM del grupo de back-end del equilibrador de carga participante

Si se configura un equilibrador de carga interno dentro de una red virtual y una de las máquinas virtuales de back-end participantes intenta acceder al front-end de este, pueden producirse errores al asignar el flujo a la máquina virtual original. No se admite este escenario.

**Solución**: hay varias maneras para desbloquear este escenario, incluido el uso de un proxy. Evalúe Application Gateway u otros proxys de terceros (por ejemplo, nginx o haproxy). Para más información acerca de Application Gateway, consulte [Introducción a Application Gateway](../application-gateway/overview.md)

**Detalles**: los equilibradores de carga internos no traducen las conexiones de salida que se originan en el front-end de un equilibrador de carga interno, porque ambos están en un espacio de direcciones IP privadas. Los equilibradores de carga públicos proporcionan [conexiones salientes](load-balancer-outbound-connections.md) desde direcciones IP privadas dentro de la red virtual a direcciones IP públicas. En el caso de los equilibradores de carga internos, este enfoque evita el agotamiento del puerto SNAT potencial en un espacio de direcciones IP interno único, donde no se requiere traducción.

Un efecto secundario es que si un flujo de salida de una máquina virtual del grupo de servidores back-end intenta un flujo hacia el front-end del Load Balancer interno en su grupo _y_ se asigna a sí mismo, las dos piernas del flujo no coinciden. Dado que no coinciden, se produce un error en el flujo. El flujo se realiza correctamente si el flujo no se ha asignado a la misma máquina virtual del grupo de servidores back-end que creó el flujo al front-end.

Cuando el flujo se vuelve a asignar a sí mismo, el flujo de salida parece originarse desde la máquina virtual hasta el front-end y el flujo de entrada correspondiente parece provenir de la máquina virtual. Desde el punto de vista del sistema operativo invitado, las partes entrantes y salientes del mismo flujo no coinciden dentro de la máquina virtual. La pila TCP no reconocerá estas mitades del mismo flujo como parte del mismo flujo. El origen y el destino no coinciden. Cuando el flujo se asigna a cualquier otra máquina virtual del grupo de back-end, las mitades del flujo coinciden y la máquina virtual puede responder al flujo.

El síntoma de este escenario es el tiempo de espera de conexión intermitente cuando el flujo vuelve al mismo back-end que originó el flujo. Las soluciones alternativas comunes incluyen la inserción de una capa de proxy detrás del Load Balancer interno y el uso de reglas de estilo de Direct Server Return (DSR). Para más información, consulte [Varios front-ends para Azure Load Balancer](load-balancer-multivip-overview.md).

Puede combinar una Load Balancer interna con cualquier proxy de terceros o usar el [Application Gateway](../application-gateway/overview.md) interno para escenarios de proxy con HTTP/HTTPS. Aunque podría usar un Load Balancer público para mitigar este problema, el escenario resultante es propenso a [agotamiento de SNAT](load-balancer-outbound-connections.md). Evite este segundo enfoque a menos que se administre con cuidado.

## <a name="next-steps"></a>Pasos siguientes

Si los pasos anteriores no resuelven el problema, abra una [incidencia de soporte técnico](https://azure.microsoft.com/support/options/).