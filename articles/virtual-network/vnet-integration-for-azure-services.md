---
title: Integración de red virtual de servicios de Azure para el aislamiento de red
titlesuffix: Azure Virtual Network
description: En este artículo se describen los distintos métodos para integrar un servicio de Azure en una red virtual que le permite acceder de forma segura al servicio de Azure.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: NA
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 12/01/2020
ms.author: kumud
ms.openlocfilehash: a25d6d0d1990682287b5962a7bd93a0c525db133
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97882371"
---
# <a name="integrate-azure-services-with-virtual-networks-for-network-isolation"></a>Integración de servicios de Azure con redes virtuales para el aislamiento de red

La integración de Virtual Network (VNet) para un servicio de Azure le permite bloquear el acceso al servicio únicamente a la infraestructura de red virtual. La infraestructura de VNet también incluye redes virtuales del mismo nivel y redes locales.

La integración de VNet proporciona a los servicios de Azure las ventajas del aislamiento de red y puede realizarse mediante uno o varios de los métodos siguientes:
- [Implementación de instancias dedicadas del servicio en una red virtual](virtual-network-for-azure-services.md). Luego se puede acceder de forma privada a los servicios dentro de la red virtual y desde redes locales.
- Usando un [punto de conexión privado](../private-link/private-endpoint-overview.md) que le conecta de forma privada y segura a un servicio con la tecnología de [Azure Private Link](../private-link/private-link-overview.md). Un punto de conexión privado usa una dirección IP privada de su VNet y coloca el servicio de manera eficaz en la red virtual.
- Accediendo al servicio mediante puntos de conexión públicos mediante la ampliación de una red virtual al servicio a través de los [puntos de conexión de servicio](virtual-network-service-endpoints-overview.md). Los puntos de conexión de servicio permiten que los recursos de servicio se protejan en la red virtual.
- Usando [etiquetas de servicio](service-tags-overview.md) para permitir o denegar el tráfico a los recursos de Azure hacia y desde puntos de conexión de dirección IP pública.

## <a name="deploy-dedicated-azure-services-into-virtual-networks"></a>Implementación de servicios de Azure dedicados en las redes virtuales

Al implementar servicios de Azure dedicados en una red virtual, puede comunicarse con los recursos de los servicios de forma privada mediante direcciones IP privadas.

![Implementación de servicios de Azure dedicados en las redes virtuales](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

La implementación de un servicio de Azure dedicado en una red virtual ofrece las siguientes funcionalidades:
- Los recursos dentro de la red virtual pueden comunicarse entre sí de forma privada a través de direcciones IP privadas. Ejemplo: transferencia directa de datos entre HDInsight y SQL Server que se ejecutan en una máquina virtual, en la red virtual.
- Los recursos locales pueden acceder a recursos de una red virtual mediante direcciones IP privadas a través de VPN de sitio a sitio (VPN Gateway) o ExpressRoute.
- Las redes virtuales pueden estar emparejadas para habilitar que los recursos de las redes virtuales se comuniquen entre sí mediante direcciones IP privadas.
- Normalmente, las instancias de servicio en una red virtual están totalmente administradas por el servicio de Azure. Esto incluye la supervisión del estado de los recursos y el escalado con carga.
- Las instancias de los servicios se implementan en una subred de una red virtual. El acceso de red entrante y saliente para la subred se debe abrir a través de grupos de seguridad de red, siguiendo las instrucciones proporcionadas por el servicio.
- Ciertos servicios también imponen restricciones en la subred donde se implementan, los cuales limitan la aplicación de directivas, las rutas o la combinación de máquinas virtuales y recursos de servicio dentro de la misma subred. Compruebe las restricciones específicas de cada servicio porque podrían cambiar con el tiempo. Algunos ejemplos de estos servicios son Azure NetApp Files, Dedicated HSM, Azure Container Instances y App Service.
- Opcionalmente, los servicios pueden requerir una subred delegada como un identificador explícito de que una subred puede hospedar un servicio determinado. Mediante la delegación, los servicios obtienen permisos explícitos para crear recursos específicos del servicio en la subred delegada.
- Vea un ejemplo de una respuesta de API REST en una red virtual con una subred delegada. Puede ver una lista completa de los servicios que usan el modelo de subred delegada a través de la API de Available Delegations.

Para obtener una lista de los servicios que se pueden implementar en una red virtual, consulte [Implementación de servicios de Azure dedicados en las redes virtuales](virtual-network-for-azure-services.md).

## <a name="private-link-and-private-endpoints"></a>Private Link con puntos de conexión privados

Puede usar puntos de conexión privados para permitir la entrada de eventos directamente desde su red virtual al recurso de Azure de forma segura a través de un vínculo privado sin tener que ir a la red pública de Internet. Un punto de conexión privado es una interfaz de red especial para un servicio de Azure en la red virtual. Cuando se crea un punto de conexión privado para el recurso de Azure, este proporciona conectividad segura entre los clientes de la red virtual y el recurso de Azure. Al punto de conexión privado se le asigna una dirección IP del intervalo de direcciones IP de su red virtual. La conexión entre el punto de conexión privado y el servicio de Azure usa un vínculo privado seguro.

En el ejemplo siguiente se muestra el acceso privado de un punto de conexión privado de recurso de Event Grid que proporciona conectividad segura entre los clientes de una red virtual y el recurso de Event Grid.

![Acceso privado del recurso de base de SQL mediante un punto de conexión privado](./media/network-isolation/architecture-diagram.png)

Para obtener más información sobre el Private Link y una lista de los servicios de Azure que se admiten, consulte [¿Qué es Azure Private Link?](../private-link/private-link-overview.md)

## <a name="service-endpoints"></a>Puntos de conexión del servicio
El punto de conexión de servicio de VNet proporciona conectividad directa y segura con los servicios de Azure por medio de una ruta optimizada a través de la red troncal de Azure. Los puntos de conexión permiten proteger los recursos de servicio de Azure críticos únicamente para las redes virtuales. Los puntos de conexión de servicio permiten a las direcciones IP privadas de la red virtual alcanzar el punto de conexión de un servicio de Azure sin necesidad de una dirección IP pública en la red virtual.

![Protección de servicios de Azure para las redes virtuales](./media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

Para obtener más información, vea [Puntos de conexión de servicio de red virtual](virtual-network-service-endpoints-overview.md).

## <a name="service-tags"></a>Etiquetas de servicio

Una etiqueta de servicio representa un grupo de prefijos de direcciones IP de un servicio de Azure determinado. Mediante etiquetas de servicio, puede definir controles de acceso a la red en [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) o [Azure Firewall](https://docs.microsoft.com/azure/firewall/service-tags). Al especificar el nombre de la etiqueta de servicio (por ejemplo, AzureEventGrid) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente.

![Permitir o denegar el tráfico mediante etiquetas de servicio](./media/network-isolation/service-tags.png)

Puede usar etiquetas de servicio para lograr el aislamiento de red y proteger los recursos de Azure de Internet general mientras accede a los servicios de Azure que tienen puntos de conexión públicos. Cree reglas de grupo de seguridad de red de entrada y salida para denegar el tráfico hacia y desde **Internet** y permitir el tráfico desde y hacia **AzureCloud** u otras [etiquetas de servicio disponibles](service-tags-overview.md#available-service-tags) de servicios específicos de Azure.

Para más información sobre las etiquetas de servicio y los servicios de Azure compatibles con ellas, consulte [Etiquetas de servicio de red virtual](service-tags-overview.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [integrar la aplicación con una red de Azure](../app-service/web-sites-integrate-with-vnet.md).
- Obtenga información sobre cómo [restringir el acceso a los recursos mediante etiquetas de servicio](tutorial-restrict-network-access-to-resources.md).
- Obtenga información sobre cómo [conectarse de forma privada a una cuenta de Azure Cosmos mediante Azure Private Link](../private-link/create-private-endpoint-cosmosdb-portal.md).