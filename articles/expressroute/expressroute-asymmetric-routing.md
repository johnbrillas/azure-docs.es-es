---
title: 'Azure ExpressRoute: Ruta asimétrica'
description: Este artículo lo guía a través de los problemas a los que se puede enfrentar con el enrutamiento asimétrico en una red que tiene varios vínculos a un destino.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 12/14/2020
ms.author: duau
ms.openlocfilehash: 0713c52c7f07db93d9ae9084062ef2c3b25a9074
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97560515"
---
# <a name="asymmetric-routing-with-multiple-network-paths"></a>Enrutamiento asimétrico con varias rutas de acceso de red
En este artículo se explica cómo el tráfico de red puede tomar distintas rutas cuando hay varias rutas de acceso disponibles entre el origen y el destino de la red.

Existen dos conceptos que debe saber para comprender el enrutamiento asimétrico. Uno es el efecto que produce tener varias rutas de acceso de red. El otro es ver cómo ciertos dispositivos, como un firewall, mantienen el estado. Estos tipos de dispositivos se denominan dispositivos con estado. Cuando se combinan estos dos factores, pueden crear un escenario en el que el dispositivo con estado anula el tráfico de red.  El tráfico se anula porque no detectó que el tráfico se originó en sí mismo.

## <a name="multiple-network-paths"></a>Varias rutas de acceso de red
Cuando una red empresarial solo tiene un vínculo a Internet a través de un proveedor de servicios de Internet, todo el tráfico hacia Internet y desde él viaja por la misma ruta de acceso. Es común que las empresas adquieran varios circuitos para crear rutas de acceso redundantes y así poder mejorar el tiempo de actividad de la red. Con este tipo de configuración, es posible que el tráfico salga de un vínculo a Internet y vuelva a través de un vínculo diferente. Este escenario se conoce comúnmente como enrutamiento asimétrico. En el enrutamiento asimétrico, el tráfico de red de regreso toma una ruta de acceso distinta a la del flujo de salida original.

![Red con varias rutas de acceso](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

Aún así, el enrutamiento asimétrico suele producirse cuando se accede a Internet. También se produce cuando se introduce una combinación de varias rutas de acceso. Como primer ejemplo, tiene una ruta de acceso a Internet y una ruta de acceso privada que va al mismo destino. Como segundo ejemplo, tiene varias rutas de acceso privadas que también van al mismo destino.

Cada enrutador que hay a lo largo de la ruta de acceso entre el origen y el destino calculará la mejor ruta de acceso para llegar al destino. Así pues, el enrutador se basa en dos factores principales para determinar la mejor ruta de acceso posible:

* El enrutamiento entre redes externas se basa en un protocolo de enrutamiento llamado Border Gateway Protocol (BGP). BGP toma anuncios de vecinos, ejecuta una serie de pasos con ellos para determinar la mejor ruta de acceso al destino deseado. Y almacena la mejor ruta de acceso en su tabla de enrutamiento.
* La longitud de una máscara de subred asociada con una ruta influye en las rutas de acceso de los enrutamientos. Si un enrutador recibe varios anuncios de la misma dirección IP, este seleccionará la ruta de acceso que tenga la máscara de subred más larga, ya que se considera una ruta más específica.

## <a name="stateful-devices"></a>Dispositivos con estado
Los enrutadores miran el encabezado IP de un paquete para fines de enrutamiento. Algunos dispositivos miran de forma aún más profunda dentro del paquete. Normalmente, estos dispositivos consultan los encabezados de la capa 4 (Protocolo de control de transmisión o TCP, o Protocolo de datagramas de usuario o UDP) o incluso los de la capa 7 (capa de aplicación). Estas variantes de dispositivos son dispositivos de seguridad o dispositivos de optimización de ancho de banda. 

Un firewall es un ejemplo común de dispositivo con estado. Un firewall permite o rechaza los paquetes que van a pasar a través de sus interfaces en función de varios criterios. Estos criterios incluyen, entre otros, el protocolo, el puerto TCP/UDP y los encabezados de dirección URL. Este nivel de inspección de paquetes supone una pesada carga de procesamiento en el dispositivo. 

Para mejorar el rendimiento, el firewall inspecciona el primer paquete de un flujo. Si se permite que el paquete continúe a través de las interfaces, la información de flujo se mantiene en su tabla de estado. A continuación, todos los paquetes posteriores relacionados con este flujo se permiten en función de la determinación inicial. Un paquete que forma parte de un flujo existente puede llegar al firewall del cual no se originó. Puesto que no tiene información de estado anterior sobre el flujo inicial, el firewall anula el paquete.

## <a name="asymmetric-routing-with-expressroute"></a>Enrutamiento asimétrico con ExpressRoute
Cuando se conecta a Microsoft a través de Azure ExpressRoute, se producen los siguientes cambios en la red:

* Tiene varios vínculos con Microsoft. Un vínculo es la conexión a Internet existente y, el otro, está relacionado con la conexión de ExpressRoute. Parte del tráfico destinado a Microsoft puede pasar por la conexión a Internet y volver a la conexión de ExpressRoute. También puede ocurrir esto mismo cuando el tráfico pasa por ExpressRoute, pero vuelve a la ruta de acceso de Internet.
* Ha recibido más direcciones IP específicas del circuito de ExpressRoute. Por tanto, cuando el tráfico de la red va a Microsoft para obtener los servicios que se ofrecen a través de ExpressRoute, los enrutadores siempre preferirán la conexión de ExpressRoute.

Para comprender el efecto que estos dos cambios tienen en una red, veamos algunos ejemplos. Supongamos que tiene un circuito a Internet y que consume todos los servicios de Microsoft a través de Internet. El tráfico de la red hacia y desde Microsoft recorre el mismo vínculo de Internet y pasa a través del firewall. A continuación, el firewall registra el flujo cuando ve el primer paquete. Cada uno de los paquetes de esa conversación se permite porque el flujo existe en la tabla de estado.

![Enrutamiento asimétrico con ExpressRoute](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)

A continuación, se presenta un circuito de ExpressRoute que consume los servicios que ofrece Microsoft a través de ExpressRoute. Todos los demás servicios de Microsoft se consumen a través de Internet. Así pues, debe implementar un firewall independiente en su perímetro que se conecte a ExpressRoute. Microsoft anuncia prefijos más específicos en la red a través de ExpressRoute para ciertos servicios. La infraestructura de enrutamiento elige ExpressRoute como ruta de acceso preferida para estos prefijos. 

Si no anuncia sus direcciones IP públicas a Microsoft a través de ExpressRoute, Microsoft se comunicará con las direcciones IP públicas a través de Internet. El tráfico enviado desde la red a Microsoft usa la conexión de ExpressRoute, pero el tráfico de retorno desde Microsoft usará la ruta de acceso a Internet. Cuando el firewall del perímetro ve un paquete de respuesta de un flujo que no conoce, anulará esos paquetes.

Si decide anunciar el mismo grupo de traducción de direcciones de red (NAT) para ExpressRoute y para Internet, verá problemas similares con los clientes de la red en las direcciones IP privadas. Las solicitudes de servicios como Windows Update pasarán por Internet porque las direcciones IP de esos servicios no se anuncian a través de ExpressRoute. Sin embargo, el tráfico devuelto volverá a través de ExpressRoute. Dado que Microsoft ha recibido una dirección IP con la misma máscara de subred de Internet y de ExpressRoute, la ruta de acceso preferida será siempre ExpressRoute. Si un firewall u otro dispositivo con estado en el perímetro de la red está orientado a la conexión de ExpressRoute y no tiene información anterior sobre un flujo, se anularán esos paquetes.

## <a name="asymmetric-routing-solutions"></a>Soluciones de enrutamiento asimétrico
Tiene dos opciones disponibles para solucionar el problema del enrutamiento asimétrico. Una es a través del enrutamiento y la otra es mediante el uso de NAT basada en el origen (SNAT).

### <a name="routing"></a>Enrutamiento
Asegúrese de que las direcciones IP públicas se anuncian en los vínculos de red de área extensa (WAN) adecuados. Por ejemplo, si quiere usar Internet para el tráfico de autenticación y ExpressRoute para el tráfico de correo electrónico, no anuncie las direcciones IP públicas de los Servicios de federación de Active Directory (AD FS) a través de ExpressRoute. De forma similar, asegúrese de no exponer un servidor local de AD FS a direcciones IP que el enrutador reciba a través de ExpressRoute. Las rutas recibidas a través de ExpressRoute son más específicas, por lo que ExpressRoute será la ruta preferida para el tráfico de autenticación a Microsoft. Si no está atento a la manera en que se realiza el enrutamiento en la red, pueden surgir problemas de enrutamiento asimétrico.

Si quiere usar ExpressRoute para la autenticación, asegúrese de que va a anunciar direcciones IP públicas de AD FS a través de ExpressRoute sin NAT. Cuando se configura de esta manera, el tráfico que se origina en Microsoft se dirigirá a la instancia local del servidor de AD FS y pasará a través de ExpressRoute. El tráfico de retorno de la red que vaya a Microsoft usará ExpressRoute porque es la ruta preferida a través de Internet.

### <a name="source-based-nat"></a>NAT basada en origen
Otra manera de resolver el problema de enrutamiento asimétrico es usar SNAT. Por ejemplo, digamos que decide no anunciar la dirección IP pública de un servidor local de Protocolo simple de transferencia de correo (SMTP) a través de ExpressRoute. En su lugar, tiene previsto usar Internet para este tipo de comunicación. Por lo tanto, una solicitud que se haya creado en Microsoft y que vaya al servidor SMTP local, atravesará Internet. Realice una SNAT con la solicitud entrante a una dirección IP interna. El tráfico de retorno desde el servidor SMTP irá al firewall perimetral (que usa para NAT) en lugar de pasar a través de ExpressRoute. Como resultado, el tráfico devuelto tomará la ruta de acceso a Internet.

![Configuración de red con NAT basada en origen](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="asymmetric-routing-detection"></a>Detección de enrutamiento asimétrico
Traceroute es la mejor manera de asegurarse de que el tráfico de la red recorra la ruta de acceso prevista. Si espera que el tráfico del servidor SMTP local a Microsoft tome la ruta de acceso de Internet, realice el seguimiento de ruta previsto del servidor SMTP a Microsoft 365. El resultado validará que el tráfico sale de la red hacia Internet y no hacia ExpressRoute.

