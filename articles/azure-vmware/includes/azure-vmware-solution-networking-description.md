---
title: Redes y conectividad de Azure VMware Solution
description: Descripción de redes y conectividad de Azure VMware Solution.
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: 96dd93f1db5dc3ddcbb883313e19c6aed8a256da
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462596"
---
<!-- Used in introduction.md and concepts-networking.md -->

Azure VMware Solution ofrece un entorno de nube privada accesible desde recursos locales y basados en Azure. Servicios como Azure ExpressRoute y las conexiones VPN o Azure Virtual WAN proporcionan la conectividad. Estos servicios requieren intervalos de direcciones de red y puertos de firewall específicos para habilitar los servicios.

Cuando se implementa una nube privada, se crean redes privadas para la administración, el aprovisionamiento y vMotion. Use estas redes privadas para acceder a vCenter y NSX-T Manager, así como para la implementación o vMotion de máquinas virtuales.  

Global Reach de ExpressRoute se usa para conectar las nubes privadas a entornos locales. La conexión requiere una red virtual con un circuito ExpressRoute en el entorno local en su suscripción.

Las máquinas virtuales (VM) implementadas en la nube privada son accesibles a través de Internet mediante la funcionalidad de IP pública de Azure Virtual WAN.  De forma predeterminada, el acceso a Internet está deshabilitado para las nuevas nubes privadas. Para más información, consulte [Uso de la funcionalidad de IP pública en Azure VMware Solution](../public-ip-usage.md).

