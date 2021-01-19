---
title: Intervalos de direcciones IP privadas de SNAT de Azure Firewall
description: Se pueden configurar los intervalos de direcciones IP para SNAT.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 01/11/2021
ms.author: victorh
ms.openlocfilehash: 0df91680dadbc4ac19299a4df48a585a11f044e8
ms.sourcegitcommit: 3af12dc5b0b3833acb5d591d0d5a398c926919c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98072248"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Intervalos de direcciones IP privadas de SNAT de Azure Firewall

Azure Firewall proporciona SNAT automática para todo el tráfico saliente a las IP públicas. De forma predeterminada, Azure Firewall no aplica SNAT con reglas de red cuando la dirección IP de destino es un intervalo de direcciones IP privadas, de acuerdo con la normativa [RFC 1918 de IANA](https://tools.ietf.org/html/rfc1918). Las reglas de aplicación se aplican siempre mediante un [proxy transparente](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy), independientemente de la dirección IP de destino.

Esta lógica funciona bien cuando se enruta el tráfico directamente a Internet. Sin embargo, si ha se habilitado [tunelización forzada](forced-tunneling.md), al tráfico enlazado a Internet se le aplica SNAT a una de las direcciones IP privadas del firewall en AzureFirewallSubnet, ocultando el origen del firewall local.

Si su organización usa un intervalo de direcciones IP públicas para las redes privadas, Azure Firewall aplicará SNAT al tráfico para una de las direcciones IP privadas de firewall en AzureFirewallSubnet. Sin embargo, puede configurar Azure Firewall de modo que **no** aplique SNAT al intervalo de direcciones IP públicas. Por ejemplo, para especificar una dirección IP individual, puede especificarla de la siguiente manera: `192.168.1.10`. Para especificar un intervalo de direcciones IP, puede especificarlo de la siguiente manera: `192.168.1.0/24`.

- Para configurar Azure Firewall en **nunca** aplicar SNAT, independientemente de la dirección IP de destino, use **0.0.0.0/0** como intervalo de direcciones IP privadas. Con esta configuración, Azure Firewall nunca puede enrutar el tráfico directamente a Internet. 

- Para configurar el firewall en **siempre** aplicar SNAT, independientemente de la dirección IP de destino, use **255.255.255.255/32** como intervalo de direcciones IP privadas.

> [!IMPORTANT]
> El intervalo de direcciones privadas que especifique solo se aplicará a las reglas de red. Actualmente, las reglas de aplicación siempre son SNAT.

> [!IMPORTANT]
> Si desea especificar sus propios intervalos de direcciones IP privadas y mantener los intervalos de direcciones RFC 1918 de IANA predeterminados, asegúrese de que la lista personalizada todavía incluye el intervalo de RFC 1918 de IANA. 

## <a name="configure-snat-private-ip-address-ranges---azure-powershell"></a>Configuración de intervalos de direcciones IP privadas de SNAT: Azure PowerShell

Puede usar Azure PowerShell a fin de especificar intervalos de direcciones IP privadas para el firewall.

### <a name="new-firewall"></a>Nuevo firewall

En el caso de un nuevo firewall, el cmdlet de Azure PowerShell es:

```azurepowershell
$azFw = @{
    Name               = '<fw-name>'
    ResourceGroupName  = '<resourcegroup-name>'
    Location           = '<location>'
    VirtualNetworkName = '<vnet-name>'
    PublicIpName       = '<public-ip-name>'
    PrivateRange       = @("IANAPrivateRanges", "192.168.1.0/24", "192.168.1.10")
}

New-AzFirewall @azFw
```
> [!NOTE]
> La implementación de Azure Firewall con `New-AzFirewall` requiere que existan una red virtual y una dirección IP pública. Consulte [Implementación y configuración de Azure Firewall mediante Azure PowerShell](deploy-ps.md) para obtener una guía completa.

> [!NOTE]
> IANAPrivateRanges se expande a los valores predeterminados actuales en Azure Firewall mientras que los demás intervalos se agregan a él. Para mantener el valor predeterminado de IANAPrivateRanges en su especificación de intervalo privado, debe permanecer en la especificación de `PrivateRange`, tal como se muestra en los ejemplos siguientes.

Para más información, consulte [New-AzFirewall](/powershell/module/az.network/new-azfirewall?view=azps-3.3.0).

### <a name="existing-firewall"></a>Firewall existente

Para configurar un firewall existente, use los siguientes cmdlets de Azure PowerShell:

```azurepowershell
$azfw = Get-AzFirewall -Name '<fw-name>' -ResourceGroupName '<resourcegroup-name>'
$azfw.PrivateRange = @("IANAPrivateRanges","192.168.1.0/24", "192.168.1.10")
Set-AzFirewall -AzureFirewall $azfw
```

## <a name="configure-snat-private-ip-address-ranges---azure-cli"></a>Configuración de intervalos de direcciones IP privadas de SNAT: CLI de Azure

Puede usar la CLI de Azure a fin de especificar intervalos de direcciones IP privadas para el firewall.

### <a name="new-firewall"></a>Nuevo firewall

En el caso de un nuevo firewall, el comando de la CLI de Azure es:

```azurecli-interactive
az network firewall create \
-n <fw-name> \
-g <resourcegroup-name> \
--private-ranges 192.168.1.0/24 192.168.1.10 IANAPrivateRanges
```

> [!NOTE]
> La implementación de Azure Firewall con el comando `az network firewall create` de la CLI de Azure requiere pasos de configuración adicionales para crear direcciones IP públicas y la configuración de IP. Consulte [Implementación y configuración de Azure Firewall mediante la CLI de Azure](deploy-cli.md) para obtener una guía completa.

> [!NOTE]
> IANAPrivateRanges se expande a los valores predeterminados actuales en Azure Firewall mientras que los demás intervalos se agregan a él. Para mantener el valor predeterminado de IANAPrivateRanges en su especificación de intervalo privado, debe permanecer en la especificación de `PrivateRange`, tal como se muestra en los ejemplos siguientes.

### <a name="existing-firewall"></a>Firewall existente

Para configurar un firewall existente, el comando de la CLI de Azure es:

```azurecli-interactive
az network firewall update \
-n <fw-name> \
-g <resourcegroup-name> \
--private-ranges 192.168.1.0/24 192.168.1.10 IANAPrivateRanges
```

## <a name="configure-snat-private-ip-address-ranges---arm-template"></a>Configuración de intervalos de direcciones IP privadas de SNAT: plantilla de ARM

Para configurar SNAT durante la implementación de la plantilla de ARM, puede agregar lo siguiente a la propiedad `additionalProperties`:

```json
"additionalProperties": {
   "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
},
```

## <a name="configure-snat-private-ip-address-ranges---azure-portal"></a>Configuración de intervalos de direcciones IP privadas de SNAT: Azure Portal

Puede usar Azure Portal a fin de especificar intervalos de direcciones IP privadas para el firewall.

1. Seleccione el grupo de recursos y, después, el firewall.
2. En la página **Información general**, **Intervalos IP privados**, seleccione el valor predeterminado **IANA RFC 1918**.

   Se abre la página **Editar prefijos IP privados**:

   :::image type="content" source="media/snat-private-range/private-ip.png" alt-text="Editar prefijos IP privados":::

1. De forma predeterminada, **IANAPrivateRanges** está configurado.
2. Edite los intervalos de direcciones IP privadas para su entorno y, después, seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

- Obtener más información sobre [Tunelización forzada de Azure Firewall](forced-tunneling.md).