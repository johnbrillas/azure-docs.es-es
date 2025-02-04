---
title: Solución de problemas de la condición del sondeo de estado de Azure Load Balancer
description: Aprenda a solucionar problemas conocidos sobre la condición del estado de sondeo de estado de Azure Load Balancer.
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
ms.date: 12/02/2020
ms.author: allensu
ms.openlocfilehash: 28823c997cd974d5061829df88680ed52075caa0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98029153"
---
# <a name="troubleshoot-azure-load-balancer-health-probe-status"></a>Solución de problemas de la condición del sondeo de estado de Azure Load Balancer

En esta página se proporciona información de solución de problemas para preguntas frecuentes sobre el sondeo de estado de Azure Load Balancer.

## <a name="symptom-vms-behind-the-load-balancer-are-not-responding-to-health-probes"></a>Síntoma: Las máquinas virtuales detrás del equilibrador de carga no responden a los sondeos de estado
Para que los servidores back-end participen en el conjunto del equilibrador de carga, deben pasar la comprobación del sondeo. Para más información sobre los sondeos de estado, consulte [Descripción de los sondeos del equilibrador de carga](load-balancer-custom-probe-overview.md). 

Es posible que las máquinas virtuales del grupo de back-end del equilibrador de carga no responda a los sondeos por alguno de los motivos siguientes: 
- Que una máquina virtual del grupo de back-end del equilibrador de carga esté dañada 
- Que una máquina virtual del grupo de back-end del equilibrador de carga no escuche en el puerto de sondeo 
- Que el firewall o un grupo de seguridad de red esté bloqueando el puerto de las máquinas virtuales del grupo de back-end del equilibrador de carga 
- Otros errores de configuración del equilibrador de carga

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>Causa 1: Que una máquina virtual del grupo de back-end del equilibrador de carga esté dañada

**Validación y resolución**

Para resolver este problema, inicie sesión en las máquinas virtuales participantes, compruebe si el estado de la máquina virtual es correcto y responde a **PsPing** o **TCPing** de otra máquina virtual del grupo. Si la máquina virtual está dañada o no responde al sondeo, debe corregir el problema y reparar la máquina virtual para que pueda volver a participar en el equilibrio de carga.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-the-probe-port"></a>Causa 2: Que una máquina virtual del grupo de back-end del equilibrador de carga no escuche en el puerto de sondeo
Si la máquina virtual está correcta, pero no responde al sondeo, una razón podría ser que el puerto de sondeo no esté abierto en la máquina virtual participante o que esta no escuche en ese puerto.

**Validación y resolución**

1. Inicie sesión en la máquina virtual de back-end.
2. Abra un símbolo del sistema y ejecute el siguiente comando para validar que existe una aplicación de escucha en el puerto de datos: netstat -an.
3. Si el estado del puerto no aparece como **LISTENING** (ESCUCHANDO), configure el puerto correcto. 
4. Como alternativa, seleccione otro puerto que aparezca como **LISTENING** (ESCUCHANDO) y actualice en consecuencia la configuración del equilibrador de carga.

### <a name="cause-3-firewall-or-a-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vms"></a>Causa 3: Que el firewall o un grupo de seguridad de red esté bloqueando el puerto de las VM del grupo de back-end del equilibrador de carga
Si el firewall de la máquina virtual está bloqueando el puerto de sondeo, o uno o varios grupos de seguridad de red configurados en la subred o en la máquina virtual no permite que el sondeo alcance el puerto, la máquina virtual no puede responder al sondeo de estado.

**Validación y resolución**

1. Si el firewall está habilitado, compruebe si está configurado para permitir el puerto de sondeo. De lo contrario, configure el firewall para permitir el tráfico en el puerto de sondeo y pruebe de nuevo.
2. En la lista de grupos de seguridad de red, compruebe si el tráfico entrante o saliente del puerto de sondeo tiene interferencias.
3. Además, compruebe si hay una regla de grupos de seguridad de red **Deny All** en la NIC de la máquina virtual o la subred que tenga una prioridad mayor que la regla predeterminada que permite los sondeos de equilibrio de carga y tráfico (los grupos de seguridad de red deben permitir la dirección IP del equilibrador de carga 168.63.129.16).
4. Si cualquiera de estas reglas bloquea el tráfico del sondeo, elimine y vuelva a configurar las reglas para permitirlo.  
5. Pruebe si la máquina virtual ahora responde a los sondeos de estado.

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>Causa 4: Otros errores de configuración del equilibrador de carga
Si parece que los motivos anteriores se han validado y resuelto correctamente, y aun así la máquina virtual de back-end sigue sin responder al sondeo de estado, compruebe manualmente la conectividad y recopile algunos seguimientos para entenderla.

**Validación y resolución**

1. Use **Psping** de una de las otras máquinas virtuales dentro de la red virtual para probar la respuesta del puerto de sondeo (ejemplo: .\psping.exe -t 10.0.0.4:3389) y registre los resultados. 
2. Use **TCPing** de una de las otras máquinas virtuales dentro de la red virtual para probar la respuesta del puerto de sondeo (ejemplo: .\tcping.exe 10.0.0.4 3389) y registre los resultados. 
3. Si no se recibe respuesta con estas pruebas de ping:
    - Ejecute un seguimiento Netsh en la máquina virtual del grupo de back-end de destino y en otra máquina virtual de prueba de la misma red virtual simultáneamente. Ahora, ejecute una prueba de PsPing durante algún tiempo, recopile seguimientos de red y detenga la prueba. 
    - Analizar la captura de red y observe si hay paquetes entrantes y salientes relacionados con la consulta de ping. 
        - Si no se observan paquetes entrantes se observan en la máquina virtual del grupo de back-end, puede haber grupos de seguridad de red o una configuración incorrecta de UDR que bloquee el tráfico. 
        - Si no se observan paquetes salientes en la máquina virtual del grupo de back-end, la máquina virtual podría tener otros problemas ajenos (por ejemplo, que la aplicación bloquee el puerto de sondeo). 
    - Compruebe si los paquetes de sondeo se fuerzan a otro destino (probablemente mediante la configuración de UDR) antes de alcanzar el equilibrador de carga. Esto puede hacer que el tráfico nunca alcance la máquina virtual de back-end. 
4. Cambiar el tipo de sondeo (por ejemplo, de HTTP a TCP) y configure el puerto correspondiente en las ACL de los grupos de seguridad de red y firewall para comprobar si el problema está relacionado con la configuración de respuesta del sondeo. Para más información acerca de la configuración del sondeo de estado, consulte [Endpoint Load Balancing health probe configuration](/archive/blogs/mast/endpoint-load-balancing-heath-probe-configuration-details) (Configuración del sondeo de estado en el punto de conexión del equilibrador de carga).

## <a name="next-steps"></a>Pasos siguientes

Si los pasos anteriores no resuelven el problema, abra una [incidencia de soporte técnico](https://azure.microsoft.com/support/options/).