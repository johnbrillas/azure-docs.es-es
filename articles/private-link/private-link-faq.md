---
title: Preguntas frecuentes sobre Azure Private Link
description: Obtenga información sobre Azure Private Link.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: conceptual
ms.date: 10/05/2019
ms.author: allensu
ms.openlocfilehash: 4e81d8f88a7c01b6d302bcdaa88559159bed04ea
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101709416"
---
# <a name="azure-private-link-frequently-asked-questions-faq"></a>Preguntas frecuentes sobre Azure Private Link

## <a name="private-link"></a>Private Link

### <a name="what-is-azure-private-endpoint-and-azure-private-link-service"></a>¿Qué es el punto de conexión privado de Azure y el servicio Azure Private Link?

- **[Punto de conexión privado de Azure](private-endpoint-overview.md)** : Un punto de conexión privado de Azure es una interfaz de red que le conecta de forma privada y segura a un servicio con la tecnología de Azure Private Link. Puede usar puntos de conexión privados para conectarse a un servicio PaaS de Azure que admita Private Link o a su propio servicio Private Link.
- **[Servicio Azure Private Link](private-link-service-overview.md)** : el servicio Azure Private Link es un servicio creado por un proveedor de servicios. Actualmente, se puede asociar un servicio Private Link a la configuración de IP de front-end de una instancia de Standard Load Balancer. 

### <a name="how-is-traffic-being-sent-when-using-private-link"></a>¿Cómo se envía el tráfico al usar Private Link?
El tráfico se envía de forma privada mediante la red troncal de Microsoft. No recorre Internet. Azure Private Link no almacena datos de clientes.
 
### <a name="what-is-the-difference-between-service-endpoints-and-private-endpoints"></a>¿Cuál es la diferencia entre puntos de conexión de servicio y puntos de conexión privados?
- Los puntos de conexión privados conceden acceso de red a recursos específicos detrás de un servicio determinado, lo que proporciona una segmentación granular. El tráfico puede llegar al recurso de servicio desde el entorno local sin usar puntos de conexión públicos.
- Un punto de conexión de servicio sigue siendo una dirección IP enrutable públicamente.  Un punto de conexión privado es una dirección IP privada en el espacio de direcciones de la red virtual en donde se configura el punto de conexión privado.

### <a name="what-is-the-relationship-between-private-link-service-and-private-endpoint"></a>¿Cuál es la relación entre el servicio Private Link y el punto de conexión privado?
Varios tipos de recursos de Private Link admiten el acceso a través de un punto de conexión privado. Los recursos incluyen los servicios PaaS de Azure y su propio servicio Private Link. Es una relación de uno a varios. 

Un servicio Private Link recibe conexiones de varios puntos de conexión privados. Un punto de conexión privado se conecta a un servicio Private Link.    

### <a name="do-i-need-to-disable-network-policies-for-private-link"></a>¿Tengo que deshabilitar las directivas de red para Private Link?
Sí. Tanto el punto de conexión privado como el servicio de Private Link tienen que deshabilitar las directivas de red para funcionar correctamente. Ambos tienen propiedades independientes entre sí.

## <a name="private-endpoint"></a>Punto de conexión privado 
 
### <a name="can-i-create-multiple-private-endpoints-in-same-vnet-can-they-connect-to-different-services"></a>¿Puedo crear varios puntos de conexión privados en la misma red virtual? ¿Se pueden conectar a diferentes servicios? 
Sí. Puede tener varios puntos de conexión privados en la misma red virtual o subred. Pueden conectarse a diferentes servicios.  
 
### <a name="do-i-require-a-dedicated-subnet-for-private-endpoints"></a>¿Necesito una subred dedicada para los puntos de conexión privados? 
No. No necesita una subred dedicada para los puntos de conexión privados. Puede elegir una dirección IP de punto de conexión privado desde cualquier subred de la red virtual donde se implemente su servicio.  
 
### <a name="can-a-private-endpoint-connect-to-private-link-services-across-azure-active-directory-tenants"></a>¿Puede el punto de conexión privado conectarse a los servicios Private Link a través de inquilinos de Azure Active Directory? 
Sí. Los puntos de conexión privados pueden conectarse a servicios de Private Link o a un PaaS de Azure a través de inquilinos de Azure Active Directory. Los puntos de conexión privados que se conectan entre los inquilinos requieren una aprobación de solicitud manual. 
 
### <a name="can-private-endpoint-connect-to-azure-paas-resources-across-azure-regions"></a>¿Puede el punto de conexión privado conectarse a recursos de PaaS a través de las regiones de Azure?
Sí. Los puntos de conexión privados pueden conectarse a recursos de PaaS de Azure a través de las regiones de Azure.

### <a name="can-i-modify-my-private-endpoint-network-interface-nic-"></a>¿Puedo modificar mi interfaz de red (NIC) de punto de conexión privado?
Cuando se crea un punto de conexión privado, se asigna una NIC de solo lectura. Esto no se puede modificar y permanecerá durante el ciclo de vida del punto de conexión privado.

## <a name="private-link-service"></a>Servicio Private Link
 
### <a name="what-are-the-pre-requisites-for-creating-a-private-link-service"></a>¿Cuáles son los requisitos previos para crear un servicio Private Link? 
Sus back-ends de servicio deben estar en una red virtual y detrás de una instancia de Standard Load Balancer.
 
### <a name="how-can-i-scale-my-private-link-service"></a>¿Cómo puedo escalar mi servicio Private Link? 
Puede escalar su servicio Private Link de distintas formas: 
- Agregando máquinas virtuales de back-end al grupo situado detrás de su instancia de Standard Load Balancer. 
- Agregando una dirección IP al servicio Private Link. Permitimos hasta ocho direcciones IP por servicio Private Link.  
- Agregando un nuevo servicio Private Link a Standard Load Balancer. Permitimos hasta ocho servicios Private Link por equilibrador de carga.   

### <a name="what-is-natnetwork-address-translation-ip-configuration-used-in-private-link-service-how-can-i-scale-in-terms-of-available-ports-and-connections"></a>¿Qué es la configuración IP de NAT (traducción de direcciones de red) que se usa en el servicio Private Link? ¿Cómo puedo escalar en términos de puertos y conexiones disponibles? 

La configuración de IP de NAT garantiza que no hay ningún conflicto de IP entre el espacio de direcciones de origen (consumidor) y de destino (proveedor de servicios) al proporcionar NAT de origen en el tráfico de Private Link en el lado de destino (proveedor de servicios). La dirección IP de NAT se mostrará como IP de origen para todos los paquetes recibidos por su servicio e IP de destino para todos los paquetes enviados por su servicio.  La IP de NAT se puede elegir desde cualquier subred de la red virtual de un proveedor de servicios. 

Cada IP de NAT proporciona 64 000 conexiones TCP (64 000 puertos) por máquina virtual detrás de Standard Load Balancer. Con el fin de escalar y aumentar las conexiones, puede agregar nuevas direcciones IP de NAT o más máquinas virtuales detrás de Standard Load Balancer. Si lo hace, escalará la disponibilidad de puertos y permitirá más conexiones. Las conexiones se distribuirán entre las direcciones IP de NAT y las máquinas virtuales detrás de Standard Load Balancer.

### <a name="can-i-connect-my-service-to-multiple-private-endpoints"></a>¿Puedo conectar mi servicio a varios puntos de conexión privados?
Sí. Un servicio Private Link puede recibir conexiones de varios puntos de conexión privados. Sin embargo, un punto de conexión privado solo puede conectarse a un servicio Private Link.  
 
### <a name="how-should-i-control-the-exposure-of-my-private-link-service"></a>¿Cómo debo controlar la exposición de mi servicio Private Link?
Puede controlar la exposición mediante la configuración de visibilidad del servicio Private Link. La visibilidad admite tres configuraciones:

- **Ninguno**: solo las suscripciones con acceso de Azure RBAC pueden localizar el servicio. 
- **Restrictivo**: solo las suscripciones que están aprobadas y con acceso de Azure RBAC pueden localizar el servicio. 
- **Todo**: todos pueden localizar el servicio. 
 
### <a name="can-i-create-a-private-link-service-with-basic-load-balancer"></a>¿Puedo crear un servicio Private Link con un equilibrador de carga básico? 
No. No se admite ningún servicio Private Link mediante un equilibrador de carga básico.
 
### <a name="is-a-dedicated-subnet-required-for-private-link-service"></a>¿Es necesaria una subred dedicada para el servicio Private Link? 
No. El servicio Private Link no requiere una subred dedicada. Puede elegir cualquier subred de su red virtual donde se implemente su servicio.   

### <a name="im-a-service-provider-using-azure-private-link-do-i-need-to-make-sure-all-my-customers-have-unique-ip-space-and-dont-overlap-with-my-ip-space"></a>Soy un proveedor de servicios que usa Azure Private Link. ¿Debo asegurarme de que todos mis clientes tienen espacio de IP único y no se superpone con mi espacio de IP? 
No. Azure Private Link proporciona esta funcionalidad automáticamente. No se le exige tener un espacio de direcciones que no se superponga con el espacio de direcciones de su cliente. 

##  <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [Azure Private Link](private-link-overview.md)
