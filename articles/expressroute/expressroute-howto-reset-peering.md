---
title: 'Azure ExpressRoute: Restablecimiento de emparejamiento de circuitos'
description: Aprenda a deshabilitar y habilitar emparejamientos de un circuito de Azure ExpressRoute mediante Azure PowerShell.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: how-to
ms.date: 12/15/2020
ms.author: duau
ms.openlocfilehash: 0bde96ae5f4a9aff6f4a16a4f1544d9b39e5cb66
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97559580"
---
# <a name="reset-expressroute-circuit-peerings"></a>Restablecimiento de emparejamientos de circuitos ExpressRoute

En este artículo se describe cómo deshabilitar y habilitar los emparejamientos de un circuito de ExpressRoute mediante PowerShell. Los emparejamientos están habilitados de forma predeterminada cuando se crean. Cuando se deshabilita un emparejamiento, se cerrará la sesión de BGP en las conexiones principal y secundaria del circuito de ExpressRoute. Perderá la conectividad de este emparejamiento con Microsoft. Cuando se habilita un emparejamiento, se desactivará la sesión de BGP en las conexiones principal y secundaria del circuito ExpressRoute. La conectividad con Microsoft se restaurará para este emparejamiento. Asimismo, puede habilitar y deshabilitar el emparejamiento de Microsoft y el emparejamiento privado de Azure de forma independiente, en un circuito de ExpressRoute.

Existen dos escenarios en los que puede resultar útil restablecer los emparejamientos de ExpressRoute.
* Si quiere probar el diseño y la implementación de la recuperación ante desastres. Por ejemplo, tiene dos circuitos de ExpressRoute. Puede deshabilitar los emparejamientos de un circuido y forzar la conmutación por error del tráfico de red a otro circuito.
* Habilite la detección de reenvío bidireccional (BFD) en el emparejamiento privado de Azure o el emparejamiento de Microsoft de su circuito de ExpressRoute. BFD se habilita de forma predeterminada en el emparejamiento privado de Azure si creó el circuito de ExpressRoute después del 1 de agosto de 2018 y para el emparejamiento de Microsoft después del 10 de enero de 2020. Si el circuito se creó antes de la fecha indicada, deberá restablecer el emparejamiento para habilitar BFD. 

### <a name="working-with-azure-powershell"></a>Trabajo con Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="reset-a-peering"></a>Restablecimiento de un emparejamiento

1. Si ejecuta PowerShell de manera local, abra la consola de PowerShell con privilegios elevados y conéctese a su cuenta. Use el siguiente ejemplo para conectarse:

   ```azurepowershell
   Connect-AzAccount
   ```
2. Si tiene varias suscripciones de Azure, compruebe las suscripciones de la cuenta.

   ```azurepowershell-interactive
   Get-AzSubscription
   ```
3. Especifique la suscripción que desea usar.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
   ```
4. Ejecute los comandos siguientes para recuperar el circuito de ExpressRoute.

   ```azurepowershell-interactive
   $ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   ```
5. Identifique el emparejamiento que quiere habilitar o deshabilitar. *Peerings* es una matriz. En el ejemplo siguiente, Peerings[0] es el emparejamiento privado de Azure y Peerings[1], el emparejamiento de Microsoft.

   ```azurepowershell-interactive
   Name                             : ExpressRouteARMCircuit
   ResourceGroupName                : ExpressRouteResourceGroup
   Location                         : westus
   Id                               : /subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
   Etag                             : W/"cd011bef-dc79-49eb-b4c6-81fb6ea5d178"
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
                                     "ServiceProviderName": "Coresite",
                                     "PeeringLocation": "Los Angeles",
                                     "BandwidthInMbps": 50
                                   }
   ServiceKey                       : ########-####-####-####-############
   Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "10.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "10.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 789,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "NotConfigured",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     },
                                     {
                                       "Name": "MicrosoftPeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/MicrosoftPeering",
                                       "PeeringType": "MicrosoftPeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "3.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "3.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 345,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [
                                           "3.0.0.3/32"
                                         ],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "ValidationNeeded",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
   Authorizations                   : []
   AllowClassicOperations           : False
   GatewayManagerEtag               :
   ```
6. Ejecute el comando siguiente para cambiar el estado del emparejamiento.

   ```azurepowershell-interactive
   $ckt.Peerings[0].State = "Disabled"
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```
   El emparejamiento debe tener el estado que le haya establecido. 

## <a name="next-steps"></a>Pasos siguientes
Si necesita ayuda para solucionar un problema de ExpressRoute, consulte los artículos siguientes:
* [Comprobación de la conectividad de ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
* [Solución de problemas de rendimiento de red](expressroute-troubleshooting-network-performance.md)
