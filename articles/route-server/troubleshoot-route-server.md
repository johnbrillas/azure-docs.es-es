---
title: Solución de problemas de Azure Route Server
description: Obtenga información sobre cómo solucionar problemas de Azure Route Server.
services: route-server
author: duongau
ms.service: route-server
ms.topic: how-to
ms.date: 03/02/2021
ms.author: duau
ms.openlocfilehash: 02dd9aa74da42f0a5d70de4513b88756a97bea1e
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678929"
---
# <a name="troubleshooting-azure-route-server-issues"></a>Solución de problemas de Azure Route Server

> [!IMPORTANT]
> Azure Route Server (versión preliminar) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="bgp-connectivity-issues"></a>Problemas de conectividad de BGP

### <a name="why-is-the-bgp-peering-between-my-nva-and-the-azure-route-server-going-up-and-down-flapping"></a>¿Por qué el emparejamiento de BGP entre mi NVA y Azure Route Server se activa y desactiva ("oscilación")?

La causa de la oscilación podría deberse a la configuración del temporizador de BGP. De manera predeterminada, el temporizador para mantener la conexión en Azure Route Server se establece en 60 segundos y el temporizador de retención en 180 segundos.

### <a name="why-can-i-ping-from-my-nva-to-the-bgp-peer-ip-on-azure-route-server-but-after-i-set-up-the-bgp-peering-between-them-i-cant-ping-the-same-ip-anymore-why-does-the-bgp-peering-goes-down"></a>¿Por qué puedo hacer ping desde mi NVA a la dirección IP del emparejamiento de BGP en Azure Route Server, pero después de configurar el emparejamiento de BGP entre ellos, ya no puedo hacer ping a la misma dirección IP? ¿Por qué deja de funcionar el emparejamiento de BGP?

En algunas NVA, debe agregar una ruta estática para la subred de Azure Route Server. Por ejemplo, si Azure Route Server se encuentra en 10.0.255.0/27 y la NVA se encuentra en 10.0.1.0/24, debe agregar la siguiente ruta a la tabla de enrutamiento en la NVA:

| Ruta | Próximo salto |
|-------|----------|
| 10.0.255.0/27 | 10.0.1.1 |

10.0.1.1 es la dirección IP de la puerta de enlace predeterminada de la subred donde se hospeda la NVA (o de forma más precisa, una de las NIC).

## <a name="bgp-route-issues"></a>Problemas de rutas de BGP

### <a name="why-does-my-nva-not-receive-routes-from-azure-route-server-even-though-the-bgp-peering-is-up"></a>¿Por qué la NVA no recibe rutas de Azure Route Server aunque el emparejamiento de BGP esté activo?

Azure Route Server utiliza el ASN 65515. Asegúrese de configurar un ASN diferente para la NVA, de modo que se pueda establecer una sesión de "eBGP" entre la NVA y Azure Route Server para que la propagación de rutas se realice automáticamente.

### <a name="the-bgp-peering-between-my-nva-and-azure-route-server-is-up-i-can-see-routes-exchanged-correctly-between-them-why-arent-the-nva-routes-in-the-effective-routing-table-of-my-vm"></a>El emparejamiento de BGP entre la NVA y Azure Route Server está activo. Puedo ver que las rutas se intercambian correctamente entre ambos. ¿Por qué no están las rutas de la NVA en la tabla de enrutamiento efectiva de la máquina virtual? 

* Si la máquina virtual está en la misma red virtual que la NVA y Azure Route Server:

     Azure Route Server expone dos direcciones IP del emparejamiento de BGP que se hospedan en dos máquinas virtuales que comparten la responsabilidad de enviar las rutas a todas las demás máquinas virtuales que se ejecutan en la red virtual. Cada una de las NVA debe configurar dos sesiones de BGP idénticas (por ejemplo, usar el mismo número AS, la misma ruta AS y anunciar el mismo conjunto de rutas) en las dos máquinas virtuales para que las máquinas virtuales de la red virtual puedan obtener información de enrutamiento coherente desde Azure Route Server. Observe el siguiente diagrama.

    ![Diagrama que muestra una aplicación virtual de red con Route Server.](./media/faq/network-virtual-appliances.png)

    Si tiene dos o más instancias de la NVA, *puede* anunciar diferentes rutas AS para la misma ruta desde diferentes instancias de la NVA si quiere designar una instancia de NVA como activa y la otra como pasiva.

* Si la máquina virtual está en una red virtual diferente de la que hospeda la NVA y Azure Route Server. Compruebe si el emparejamiento de red virtual está habilitado entre las dos redes virtuales *y* si está habilitado el uso del servidor de rutas remoto en la red virtual de la máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

[Configuración de una instancia de Azure Route Server](quickstart-configure-route-server-powershell.md)
