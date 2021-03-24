---
title: Directiva de emparejamiento de Microsoft
titleSuffix: Azure
description: Directiva de emparejamiento de Microsoft
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: prmitiki
ms.openlocfilehash: bee41bb8e5beb4df3086ab50499cb185a83e4efe
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97592337"
---
# <a name="peering-policy"></a>Directiva de emparejamiento
Microsoft mantiene una directiva de emparejamiento selectivo diseñada para garantizar la mejor experiencia de cliente posible; asimismo, está respaldada por estándares del sector y procedimientos recomendados, escalado para al demanda futura y una colocación estratégica del emparejamiento. Como tal, Microsoft se reserva el derecho de hacer excepciones en la directiva según lo considere necesario. En las secciones siguientes se explican los requisitos generales de Microsoft de la red. Se aplican a las solicitudes de emparejamiento directo y de emparejamiento de Exchange. 

## <a name="technical-requirements"></a>Requisitos técnicos

* Una red totalmente redundante con capacidad suficiente para intercambiar tráfico sin congestión.
* El elemento del mismo nivel tendrá un número de sistema autónomo (ASN) enrutable públicamente.
* Tanto IPv4 como IPv6 se admiten y Microsoft espera establecer sesiones de ambos tipos en cada ubicación de emparejamiento.
* No se admite el uso de MD5.
* **Detalles de ASN:**

    * Microsoft administra AS8075 junto con los siguientes ASN: AS8068, AS8069, AS12076. Para obtener una lista completa de ASN con el emparejamiento de AS8075, consulte AS-SET MICROSOFT.
    * Todas las partes emparejadas con Microsoft acuerdan no aceptar rutas de AS12076 (ExpressRoute) bajo ninguna circunstancia y deben filtrar AS12076 en todos los elementos del mismo nivel.

* **Directiva de enrutamiento:**
    * El elemento del mismo nivel tendrá al menos un /24 enrutable públicamente.
    * Microsoft sobrescribirá los discriminadores de múltiples salidas (MED) recibidos.
    * Microsoft prefiere recibir etiquetas de comunidad de BGP de elementos del mismo nivel para indicar el origen de la ruta.
    * Se recomienda que los elementos del mismo nivel establezcan un prefijo máximo de 2000 (IPv4) y 500 (IPv6) rutas en las sesiones de emparejamiento con Microsoft.
    * A menos que se acuerde expresamente de antemano, se espera que los elementos del mismo nivel anuncien rutas coherentes en todas las ubicaciones en las que se emparejan con Microsoft.
    * En general, las sesiones de emparejamiento con AS8075 anunciarán todas las rutas AS-MICROSOFT. Microsoft puede anunciar algunas características específicas regionales.
    * Ninguna de las partes establecerá una ruta estática o una ruta de último recurso ni enviará tráfico de ningún otro modo a la otra parte para una ruta no anunciada a través de BGP.
    * Es necesario que el elemento del mismo nivel registre sus rutas en una base de datos de registro de enrutamiento de Internet (IRR) público con fines de filtrado, y realizará esfuerzos de buena fe para mantener esta información actualizada.      
    * Igualmente, los elementos del mismo nivel deberán adherirse a los estándares MANRS del sector para la seguridad de las rutas.  A su entera discreción, Microsoft puede elegir: 1.) no establecer el emparejamiento con empresas que no tengan rutas firmadas y registradas; 2.) quitar las rutas de RPKI no válidas; 3.) no aceptar rutas de elementos del mismo nivel establecidos que no estén registrados ni firmados. 

## <a name="operational-requirements"></a>Requisitos operativos
* Un centro de operaciones de red (NOC) disponible de manera totalmente ininterrumpida, capaz de ayudar con la resolución de todos los problemas técnicos y de rendimiento, las infracciones de seguridad, los ataques por denegación de servicio o cualquier otro abuso que se origine en el elemento del mismo nivel o en sus clientes.
* Se espera que los elementos del mismo nivel tengan un perfil completo y actualizado en [PeeringDB](https://www.peeringdb.com), incluido un correo electrónico de NOC ininterrumpido del dominio corporativo y el número de teléfono. Usamos esta información para validar los detalles del elemento del mismo nivel, como su información de NOC, sus datos de contacto técnico, su presencia en las instalaciones de emparejamiento, etc. No se aceptan cuentas personales de Yahoo, Gmail y Hotmail.

## <a name="physical-connection-requirements"></a>Requisitos de la conexión física
* Las ubicaciones en las que puede conectarse con Microsoft para el emparejamiento directo o el emparejamiento de Exchange se muestran en [PeeringDB](https://www.peeringdb.com/net/694).

* **Emparejamiento de Exchange:**
    * Los elementos del mismo nivel deben tener al menos una conexión de 10 Gb en el intercambio.
    * Se espera que los elementos del mismo nivel actualicen sus puertos cuando el uso máximo supere el 50 %.
    * Microsoft recomienda a los elementos del mismo nivel a mantener varias conexiones que pueda intercambiar para admitir escenarios de conmutación por error.

* **Emparejamiento directo:**
    * la interconexión debe realizarse a través de fibra de modo único mediante la óptica de 100 Gbps.
    * Microsoft solo establecerá el emparejamiento directo con proveedores ISP o de servicios de red.
    * Se espera que los elementos del mismo nivel actualicen sus puertos cuando el uso máximo supere el 50 % y mantengan una capacidad diversa en cada metro, ya sea en una sola ubicación o en varias ubicaciones de un metro.
    * Cada emparejamiento directo consta de dos conexiones a dos enrutadores perimetrales de Microsoft desde los enrutadores del mismo nivel ubicados en el borde del mismo nivel. Microsoft requiere sesiones de BGP duales en estas conexiones. El elemento del mismo nivel puede optar por no implementar dispositivos redundantes en su extremo.


## <a name="traffic-requirements"></a>Requisitos de tráfico

* Los elementos del mismo nivel a través del emparejamiento de Exchange deben tener como mínimo 500 Mb de tráfico y menos de 2 Gb. Se debe establecer el tráfico que supere el emparejamiento directo de 2 Gb.
* Microsoft requiere, como mínimo, 2 Gb para el emparejamiento directo. Cada ubicación de emparejamiento que se acuerde mutuamente debe admitir la conmutación por error para así garantizar que el emparejamiento permanece localizado durante un escenario de conmutación por error. 

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los pasos para configurar el emparejamiento directo con Microsoft, consulte el [Tutorial del emparejamiento directo](walkthrough-direct-all.md).
* Para más información sobre los pasos para configurar el emparejamiento de Exchange con Microsoft, consulte el [Tutorial del emparejamiento de Exchange](walkthrough-exchange-all.md).
