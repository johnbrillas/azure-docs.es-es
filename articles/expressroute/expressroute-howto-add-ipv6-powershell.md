---
title: 'Azure ExpressRoute: Adición de compatibilidad con IPv6 mediante Azure PowerShell'
description: Aprenda a agregar compatibilidad con IPv6 para conectarse a implementaciones de Azure mediante Azure PowerShell.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 03/02/2021
ms.author: duau
ms.openlocfilehash: 253fce7d47d694c03d470fefdf81318a6bff77b3
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102123061"
---
# <a name="add-ipv6-support-for-private-peering-using-azure-powershell-preview"></a>Adición de compatibilidad con IPv6 para el emparejamiento privado mediante Azure PowerShell (versión preliminar)

En este artículo se describe cómo agregar compatibilidad con IPv6 para conectarse a través de ExpressRoute a los recursos de Azure mediante Azure PowerShell.

> [!Note]
> Esta característica está disponible actualmente en versión preliminar en [regiones de Azure con Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-region#azure-regions-with-availability-zones). Por lo tanto, el circuito ExpressRoute se puede crear con cualquier ubicación de emparejamiento, pero las implementaciones basadas en IPv6 a las que se conecta deben encontrarse en una región con Availability Zones.

## <a name="working-with-azure-powershell"></a>Trabajo con Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="register-for-public-preview"></a>Registro para la versión preliminar pública
Antes de agregar la compatibilidad con IPv6, primero debe inscribir su suscripción. Para inscribirse, haga lo siguiente a través de Azure PowerShell:
1.  Inicie sesión en Azure y seleccione la suscripción. Tiene que hacer esto para la suscripción que contiene el circuito ExpressRoute, así como para la suscripción que contiene las implementaciones de Azure (si son diferentes).

    ```azurepowershell-interactive
    Connect-AzAccount 

    Select-AzSubscription -Subscription "<SubscriptionID or SubscriptionName>"
    ```

2. Solicite el registro de la suscripción para la versión preliminar pública con el siguiente comando:
    ```azurepowershell-interactive
    Register-AzProviderFeature -FeatureName AllowIpv6PrivatePeering -ProviderNamespace Microsoft.Network
    ```

El equipo de ExpressRoute aprobará su solicitud en un plazo de 2 a 3 días laborables.

## <a name="add-ipv6-private-peering-to-your-expressroute-circuit"></a>Adición de emparejamiento privado IPv6 al circuito ExpressRoute

1. [Cree un circuito ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-arm) o use un circuito existente. Para recuperar el circuito, ejecute el comando **Get-AzExpressRouteCircuit**:

    ```azurepowershell-interactive
    $ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
    ```

2. Para recuperar la configuración de emparejamiento privado para el circuito, ejecute **Get-AzExpressRouteCircuitPeeringConfig**:

    ```azurepowershell-interactive
    Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    ```

3. Agregue un emparejamiento privado IPv6 a la configuración de emparejamiento privado IPv4 existente. Proporcione un par de subredes IPv6 /126 que posea para el vínculo principal y los vínculos secundarios. Desde cada una de estas subredes, asignará la primera dirección IP utilizable para el enrutador, ya que Microsoft usa la segunda dirección IP utilizable para su enrutador.

    > [!Note]
    > PeerASN y VlanId deben coincidir con los de la configuración de emparejamiento privado IPv4.

    ```azurepowershell-interactive
    Set-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "3FFE:FFFF:0:CD30::/126" -SecondaryPeerAddressPrefix "3FFE:FFFF:0:CD30::4/126" -VlanId 200 -PeerAddressType IPv6

    Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
    ```

4. Una vez que la configuración se haya guardado correctamente, ejecute el comando **Get-AzExpressRouteCircuit** para volver a obtener el circuito. La respuesta debe ser similar a la del siguiente ejemplo:

    ```azurepowershell
    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : eastus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Washington DC",
                                         "BandwidthInMbps": 50
                                       }
    ExpressRoutePort                 : null
    BandwidthInGbps                  :
    Stag                             : 29
    ServiceKey                       : **************************************
    Peerings                         : [
                                         {
                                           "Name": "AzurePrivatePeering",
                                           "Etag": "W/\"facc8972-995c-4861-a18d-9a82aaa7167e\"",
                                           "Id": "/subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                           "PeeringType": "AzurePrivatePeering",
                                           "State": "Enabled",
                                           "AzureASN": 12076,
                                           "PeerASN": 100,
                                           "PrimaryPeerAddressPrefix": "192.168.15.16/30",
                                           "SecondaryPeerAddressPrefix": "192.168.15.20/30",
                                           "PrimaryAzurePort": "",
                                           "SecondaryAzurePort": "",
                                           "VlanId": 200,
                                           "ProvisioningState": "Succeeded",
                                           "GatewayManagerEtag": "",
                                           "LastModifiedBy": "Customer",
                                           "Ipv6PeeringConfig": {
                                             "State": "Enabled",
                                             "PrimaryPeerAddressPrefix": "3FFE:FFFF:0:CD30::/126",
                                             "SecondaryPeerAddressPrefix": "3FFE:FFFF:0:CD30::4/126"
                                           },
                                           "Connections": [],
                                           "PeeredConnections": []
                                         },
                                       ]
    Authorizations                   : []
    AllowClassicOperations           : False
    GatewayManagerEtag               :
    ```

## <a name="update-your-connection-to-an-existing-virtual-network"></a>Actualización de la conexión a una red virtual existente

Siga los pasos que se indican a continuación si tiene un entorno existente de recursos de Azure en una región con Availability Zones con el que quisiera usar el emparejamiento privado IPv6.

1. Recupere la red virtual a la que está conectado el circuito ExpressRoute.

    ```azurepowershell-interactive
    $vnet = Get-AzVirtualNetwork -Name "VirtualNetwork" -ResourceGroupName "ExpressRouteResourceGroup"
    ```

2. Agregue un espacio de direcciones IPv6 a la red virtual.

    ```azurepowershell-interactive
    $vnet.AddressSpace.AddressPrefixes.add("ace:daa:daaa:deaa::/64")
    Set-AzVirtualNetwork -VirtualNetwork $vnet
    ```

3. Agregue el espacio de direcciones IPv6 a la subred de la puerta de enlace. La subred IPv6 de la puerta de enlace debe ser /64 o superior.

    ```azurepowershell-interactive
    Set-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix "10.0.0.0/26", "ace:daa:daaa:deaa::/64"
    Set-AzVirtualNetwork -VirtualNetwork $vnet
    ```

4. Si tiene una puerta de enlace con redundancia de zona existente, ejecute lo siguiente para habilitar la conectividad IPv6. De lo contrario, [cree la puerta de enlace de red virtual](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-resource-manager) mediante una SKU con redundancia de zona (ErGw1AZ, ErGw2AZ, ErGw3AZ).

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name "GatewayName" -ResourceGroupName "ExpressRouteResourceGroup"
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw
    ```

## <a name="create-a-connection-to-a-new-virtual-network"></a>Creación de una conexión a una nueva red virtual

Siga los pasos que se indican a continuación si tiene previsto conectarse a un nuevo conjunto de recursos de Azure en una región con Availability Zones mediante el emparejamiento privado IPv6.

1. Cree una red virtual de doble pila con espacios de direcciones IPv4 e IPv6. Para obtener más información, consulte [Creación de una red virtual](https://docs.microsoft.com/azure/virtual-network/quick-create-portal#create-a-virtual-network).

2. [Creación de la subred de puerta de enlace de doble pila](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-resource-manager#add-a-gateway).

3. [Cree la puerta de enlace de red virtual](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-resource-manager#add-a-gateway) mediante una SKU con redundancia de zona (ErGw1AZ, ErGw2AZ, ErGw3AZ). Si tiene previsto usar FastPath, use ErGw3AZ (tenga en cuenta que esto solo está disponible para los circuitos mediante ExpressRoute Direct).

4. [Vincule la red virtual a su circuito ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-arm).

## <a name="limitations"></a>Limitaciones
Aunque la compatibilidad con IPv6 está disponible para las conexiones a las implementaciones en regiones con Availability Zones, no se admiten los siguientes casos de uso:

* Conexiones a implementaciones en Azure a través de una SKU de puerta de enlace ExpressRoute sin AZ
* Conexiones a implementaciones en regiones que no sin AZ
* Conexiones Global Reach entre circuitos ExpressRoute
* Uso de ExpressRoute con WAN virtual
* FastPath con circuitos que no son de ExpressRoute Direct
* Coexistencia con VPN Gateway

## <a name="next-steps"></a>Pasos siguientes

Para solucionar problemas de ExpressRoute, consulte los siguientes artículos:

* [Comprobación de la conectividad de ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
* [Solución de problemas de rendimiento de red](expressroute-troubleshooting-network-performance.md)
