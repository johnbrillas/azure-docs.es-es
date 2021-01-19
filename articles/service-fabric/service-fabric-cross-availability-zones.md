---
title: Implementación de un clúster en Availability Zones
description: Aprenda a crear de un clúster de Azure Service Fabric en Availability Zones.
author: peterpogorski
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: f729c00d3b78631a32013ec9453302584cecbd16
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97962438"
---
# <a name="deploy-an-azure-service-fabric-cluster-across-availability-zones"></a>Implementación de un clúster de Azure Service Fabric en Availability Zones
Availability Zones de Azure es una oferta de alta disponibilidad que protege las aplicaciones y los datos de los errores del centro de datos. Una zona de disponibilidad es una ubicación física única equipada con alimentación independiente, refrigeración y redes dentro de una región de Azure.

Service Fabric admite clústeres que se distribuyen en Availability Zones mediante la implementación de tipos de nodos que están anclados a zonas específicas. Esto garantizará la alta disponibilidad de las aplicaciones. Azure Availability Zones solo está disponible en algunas regiones. Consulte [Azure Availability Zones Overview](../availability-zones/az-overview.md) (Información general de Azure Availability Zones).

Están disponibles plantillas de ejemplo: [Plantilla de Service Fabric en zona de disponibilidad](https://github.com/Azure-Samples/service-fabric-cluster-templates)

## <a name="recommended-topology-for-primary-node-type-of-azure-service-fabric-clusters-spanning-across-availability-zones"></a>Topología recomendada para el tipo de nodo principal de los clústeres de Azure Service Fabric que se distribuyen en Availability Zones
Un clúster de Service Fabric distribuido en Availability Zones garantiza el estado de alta disponibilidad del clúster. Para distribuir un clúster de Service Fabric entre zonas, debe crear un tipo de nodo principal en cada zona de disponibilidad admitida por la región. Esto distribuirá los nodos de inicialización uniformemente entre todos los tipos de nodos principales.

La topología recomendada para el tipo de nodo principal requiere los recursos que se indican a continuación:

* El nivel de confiabilidad del clúster establecido en Platinum.
* Tres tipos de nodo marcados como principales.
    * Cada tipo de nodo debe asignarse a su propio conjunto de escalado de máquinas virtuales ubicado en distintas zonas.
    * Cada conjunto de escalado de máquinas virtuales debe tener al menos cinco nodos (durabilidad Silver).
* Un único recurso de IP pública mediante la SKU estándar.
* Un único recurso de Load Balancer mediante la SKU estándar.
* Un NSG al que hace referencia la subred en la que implementa los conjuntos de escalado de máquinas virtuales.

>[!NOTE]
> La propiedad de grupo de colocación única del conjunto de escalado de máquinas virtuales debe establecerse en true, ya que Service Fabric no admite un conjunto de escalado de máquinas virtuales único que se distribuya entre zonas.

 ![Diagrama que muestra la arquitectura de la zona de disponibilidad de Azure Service Fabric.][sf-architecture]

## <a name="networking-requirements"></a>Requisitos de red
### <a name="public-ip-and-load-balancer-resource"></a>Recurso de IP pública y Load Balancer
Para habilitar la propiedad de zonas en un recurso de conjunto de escalado de máquinas virtuales, los recursos de Load Balancer y de IP a los que hace referencia ese conjunto de escalado de máquinas virtuales deben utilizar ambos una SKU *estándar*. La creación de un recurso de Load Balancer o IP sin la propiedad SKU creará una SKU básica, que no admite Availability Zones. Una instancia de Load Balancer de SKU estándar bloqueará todo el tráfico procedente del exterior de forma predeterminada; para permitir el tráfico exterior, debe implementarse un NSG en la subred.

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat('LB','-', parameters('clusterName')]",
    "location": "[parameters('computeLocation')]",
    "sku": {
        "name": "Standard"
    }
}
```

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName')]", 
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', parameters('subnet0Name')))]"
    ],
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
        {
            "name": "[parameters('subnet0Name')]",
            "properties": {
                "addressPrefix": "[parameters('subnet0Prefix')]",
                "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', parameters('subnet0Name')))]"
              }
            }
          }
        ]
    },
    "sku": {
        "name": "Standard"
    }
}
```

>[!NOTE]
> No es posible hacer un cambio en contexto de SKU en los recursos de IP pública y de Load Balancer. Si va a migrar desde recursos existentes que tienen una SKU básica, consulte la sección sobre la migración de este artículo.

### <a name="virtual-machine-scale-set-nat-rules"></a>Reglas NAT del conjunto de escalado de máquinas virtuales
Las reglas NAT de entrada de Load Balancer deben coincidir con los grupos NAT del conjunto de escalado de máquinas virtuales. Cada conjunto de escalado de máquinas virtuales debe tener un único grupo NAT de entrada.

```json
{
"inboundNatPools": [
    {
        "name": "LoadBalancerBEAddressNatPool0",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "50999",
            "frontendPortRangeStart": "50000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool1",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "51999",
            "frontendPortRangeStart": "51000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool2",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "52999",
            "frontendPortRangeStart": "52000",
            "protocol": "tcp"
        }
    }
    ]
}
```

### <a name="standard-sku-load-balancer-outbound-rules"></a>Reglas de salida de Load Balancer de SKU estándar
Standard Load Balancer y la IP pública estándar presentan capacidades nuevas y comportamientos diferentes en la conectividad de salida en comparación con el uso de SKU básicas. Si quiere conectividad saliente al trabajar con las SKU de nivel Estándar, debe definirlas con las direcciones IP públicas estándar o con la instancia pública de Load Balancer estándar. Para más información, consulte [Conexiones de salida](../load-balancer/load-balancer-outbound-connections.md) y [Azure Standard Load Balancer](../load-balancer/load-balancer-overview.md).

>[!NOTE]
> La plantilla estándar hace referencia a un NSG que permite todo el tráfico de salida de forma predeterminada. El tráfico de entrada se limita a los puertos necesarios para las operaciones de administración de Service Fabric. Las reglas de NSG pueden modificarse para satisfacer sus requisitos.

>[!NOTE]
> Cualquier clúster de Service Fabric que haga uso de la SKU estándar para SLB deberá asegurarse de que cada tipo de nodo tiene una regla que permite el tráfico saliente en el puerto 443. Esto es necesario para completar la configuración del clúster, produciéndose un error en cualquier implementación sin esta regla.


### <a name="enabling-zones-on-a-virtual-machine-scale-set"></a>Habilitación de zonas en un conjunto de escalado de máquinas virtuales
Para habilitar una zona, debe incluir los tres valores siguientes en el recurso de conjunto de escalado de máquinas virtuales.

* El primer valor es la propiedad **zones**, que especifica en qué zona de disponibilidad se va a implementar el conjunto de escalado de máquinas virtuales.
* El segundo valor es la propiedad "singlePlacementGroup", que se debe establecer en true.
* El tercer valor es la propiedad "faultDomainOverride" en la extensión de conjunto de escalado de máquinas virtuales de Service Fabric. El valor de esta propiedad debe incluir solo la zona en la que se colocará este conjunto de escalado de máquinas virtuales. Ejemplo: "faultDomainOverride": "az1" Todos los recursos del conjunto de escalado de máquinas virtuales se deben colocar en la misma región porque los clústeres de Azure Service Fabric no admiten la distribución entre regiones.

```json
{
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType1Name')]",
    "location": "[parameters('computeLocation')]",
    "zones": ["1"],
    "properties": {
        "singlePlacementGroup": "true",
    },
    "virtualMachineProfile": {
    "extensionProfile": {
    "extensions": [
    {
    "name": "[concat(parameters('vmNodeType1Name'),'_ServiceFabricNode')]",
    "properties": {
        "type": "ServiceFabricNode",
        "autoUpgradeMinorVersion": false,
        "publisher": "Microsoft.Azure.ServiceFabric",
        "settings": {
            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
            "nodeTypeRef": "[parameters('vmNodeType1Name')]",
            "dataPath": "D:\\\\SvcFab",
            "durabilityLevel": "Silver",
            "certificate": {
                "thumbprint": "[parameters('certificateThumbprint')]",
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
            "systemLogUploadSettings": {
                "Enabled": true
            },
            "faultDomainOverride": "az1"
        },
        "typeHandlerVersion": "1.0"
    }
}
```

### <a name="enabling-multiple-primary-node-types-in-the-service-fabric-cluster-resource"></a>Habilitación de varios tipos de nodos principales en el recurso de clúster de Service Fabric
Para establecer uno o varios tipos de nodos como principales en un recurso de clúster, establezca la propiedad "isPrimary" en "true". Al implementar un clúster de Service Fabric en Availability Zones, debe tener tres tipos de nodo en distintas zonas.

```json
{
    "reliabilityLevel": "Platinum",
    "nodeTypes": [
    {
        "name": "[parameters('vmNodeType0Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt0applicationEndPort')]",
            "startPort": "[parameters('nt0applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt0fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt0ephemeralEndPort')]",
            "startPort": "[parameters('nt0ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt0fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType1Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt1applicationEndPort')]",
            "startPort": "[parameters('nt1applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt1fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt1ephemeralEndPort')]",
            "startPort": "[parameters('nt1ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt1fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt1InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType2Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt2applicationEndPort')]",
            "startPort": "[parameters('nt2applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt2ephemeralEndPort')]",
            "startPort": "[parameters('nt2ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt2InstanceCount')]"
    }
    ],
}
```

## <a name="migrate-to-using-availability-zones-from-a-cluster-using-a-basic-sku-load-balancer-and-a-basic-sku-ip"></a>Migrar a usar Availability Zones desde un clúster mediante Load Balancer de SKU básica y una IP de SKU básica
Para migrar un clúster, que estaba usando una instancia de Load Balancer y una IP con una SKU básica, primero debe crear un recurso de Load Balancer e IP completamente nuevo mediante la SKU estándar. No es posible actualizar estos recursos en contexto.

El nuevo LB y la IP se deben hacer referencia en los nuevos tipos de nodo zona de disponibilidad que le gustaría utilizar. En el ejemplo anterior, se agregaron tres nuevos recursos de conjunto de escalado de máquinas virtuales en las zonas 1, 2 y 3. Estos conjuntos de escalado de máquinas virtuales hacen referencia a los recursos de LB e IP recién creados y están marcados como tipos de nodo principal en el recurso de clúster de Service Fabric.

Para empezar, deberá agregar los nuevos recursos a la plantilla de Resource Manager existente. Estos recursos incluyen:
* Un recurso de IP pública mediante la SKU estándar.
* Un recurso de Load Balancer mediante la SKU estándar.
* Un NSG al que hace referencia la subred en la que implementa los conjuntos de escalado de máquinas virtuales.
* Tres tipos de nodo marcados como principales.
    * Cada tipo de nodo debe asignarse a su propio conjunto de escalado de máquinas virtuales ubicado en distintas zonas.
    * Cada conjunto de escalado de máquinas virtuales debe tener al menos cinco nodos (durabilidad Silver).

Puede encontrar un ejemplo de estos recursos en la [plantilla de ejemplo](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/10-VM-Ubuntu-2-NodeType-Secure).

```powershell
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName $ResourceGroupName `
    -TemplateFile $Template `
    -TemplateParameterFile $Parameters
```

Una vez que ha terminado la implementación de los recursos, puede empezar a deshabilitar los nodos en el tipo de nodo principal del clúster original. Como los nodos están deshabilitados, los servicios del sistema se migrarán al nuevo tipo de nodo principal que se había implementado en el paso anterior.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Write-Host "Connected to cluster"

$nodeNames = @("_nt0_0", "_nt0_1", "_nt0_2", "_nt0_3", "_nt0_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames) {
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Una vez que todos los nodos están deshabilitados, se ejecutarán los servicios del sistema en el tipo de nodo principal, que se extiende entre zonas. A continuación, puede quitar los nodos deshabilitados del clúster. Una vez que se quitan los nodos, puede quitar los recursos de IP original, Load Balancer y conjunto de escalado de máquinas virtuales.

```powershell
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}

$scaleSetName="nt0"
Remove-AzureRmVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force

$lbname="LB-cluster-nt0"
$oldPublicIpName="LBIP-cluster-0"
$newPublicIpName="LBIP-cluster-1"

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force
```

A continuación, debe quitar las referencias a estos recursos de la plantilla de Resource Manager que había implementado.

El último paso implicará actualizar el nombre DNS e IP pública.

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

$PublicIP = Get-AzureRmPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP

```

## <a name="preview-enable-multiple-availability-zones-in-single-virtual-machine-scale-set"></a>(Versión preliminar) Habilitación de varias zonas de disponibilidad en un conjunto de escalado de máquinas virtuales único

La solución mencionada anteriormente usa un elemento nodeType por zona de disponibilidad. La solución siguiente permitirá que los usuarios implementen 3 zonas de disponibildad en el mismo elemento nodeType.

La plantilla de ejemplo completa se encuentra [aquí](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/15-VM-Windows-Multiple-AZ-Secure).

![Arquitectura de la zona de disponibilidad de Azure Service Fabric][sf-multi-az-arch]

### <a name="configuring-zones-on-a-virtual-machine-scale-set"></a>Configuración de zonas en un conjunto de escalado de máquinas virtuales
Para habilitar zonas en un conjunto de escalado de máquinas virtuales, debe incluir los tres valores siguientes en el recurso del conjunto de escalado de máquinas virtuales.

* El primer valor es la propiedad **zones**, que especifica las zonas de disponibilidad presentes en el conjunto de escalado de máquinas virtuales.
* El segundo valor es la propiedad "singlePlacementGroup", que se debe establecer en true. **El conjunto de escalado distribuido en tres zonas de disponibilidad puede escalar hasta 300 máquinas virtuales, incluso con "singlePlacementGroup = true".**
* El tercer valor es "zoneBalance" y es un valor opcional que garantiza el equilibrio de zona estricto si se establece en true. Lea sobre el [equilibrio de zona](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones#zone-balancing).
* No es necesario que las invalidaciones FaultDomain y UpgradeDomain estén configuradas.

```json
{
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType1Name')]",
    "location": "[parameters('computeLocation')]",
    "zones": ["1", "2", "3"],
    "properties": {
        "singlePlacementGroup": "true",
        "zoneBalance": false
    }
}
```

>[!NOTE]
> * **Los clústeres de SF deben tener al menos un elemento nodeType principal. El valor de durabilityLevel de los elementos nodeType principales debe ser Silver o superior.**
> * La zona de disponibilidad que abarca el conjunto de escalado de máquinas virtuales se debe configurar con al menos 3 zonas de disponibilidad, independientemente del valor de durabilityLevel.
> * La zona de disponibilidad que abarca el conjunto de escalado de máquinas virtuales con durabilidad Silver (o superior) debe tener al menos 15 máquinas virtuales.
> * La zona de disponibilidad que abarca el conjunto de escalado de máquinas virtuales con durabilidad Bronze debe tener al menos 6 máquinas virtuales.

### <a name="enabling-the-support-for-multiple-zones-in-the-service-fabric-nodetype"></a>Habilitación de la compatibilidad con varias zonas en el elemento nodeType de Service Fabric
El elemento nodeType de Service Fabric debe estar habilitado para admitir varias zonas de disponibilidad.

* El primer valor es **multipleAvailabilityZones**, que se debe establecer en true para el elemento nodeType.
* El segundo valor es **sfZonalUpgradeMode** y es opcional. Esta propiedad no se puede modificar si ya existe un elemento NodeType con varias zonas de disponibilidad en el clúster.
      La propiedad controla la agrupación lógica de máquinas virtuales en los dominios de actualización.
          Si el valor se establece en false (modo plano): las máquinas virtuales bajo el tipo de nodo se agruparán en el dominio de actualización y omitirán la información de la zona en 5 dominios de actualización.
          Si se omite el valor o se establece en true (modo jerárquico): las máquinas virtuales se agruparán para reflejar la distribución de zonas en hasta 15 dominios de actualización. Cada una de las 3 zonas tendrá 5 dominios de actualización.
          Esta propiedad solo define el comportamiento de actualización de la aplicación ServiceFabric y las actualizaciones de código. Las actualizaciones del conjunto de escalado de máquinas virtuales subyacente seguirán siendo paralelas en todas las zonas de disponibilidad.
      Esta propiedad no afectará la distribución del dominio de actualización para los tipos de nodo que no tienen varias zonas habilitadas.
* El tercer valor es **vmssZonalUpgradeMode = Parallel**. Se trata de una propiedad *obligatoria* que se va a configurar en el clúster, si se agrega un elemento nodeType con varias zonas de disponibilidad. Esta propiedad define el modo de actualización para las actualizaciones del conjunto de escalado de máquinas virtuales que se producirán en paralelo en todas las zonas de disponibilidad de una vez.
      Ahora mismo, esta propiedad solo se puede establecer en paralelo.
* El valor de apiVersion del recurso de clúster de Service Fabric debe ser "2020-12-01-preview" o superior.
* La versión del código del clúster debe ser "7.2.445" o superior.

```json
{
    "apiVersion": "2020-12-01-preview",
    "type": "Microsoft.ServiceFabric/clusters",
    "name": "[parameters('clusterName')]",
    "location": "[parameters('clusterLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
    ],
    "properties": {
        "SFZonalUpgradeMode": "Hierarchical",
        "VMSSZonalUpgradeMode": "Parallel",
        "nodeTypes": [
          {
                "name": "[parameters('vmNodeType0Name')]",
                "multipleAvailabilityZones": true,
          }
        ]
}
```

>[!NOTE]
> * Los recursos de Load Balancer e IP pública deben usar el SKU estándar tal como se describiró anteriormente en el artículo.
> * La propiedad "multipleAvailabilityZones" del elemento nodeType solo se puede definir en el momento de la creación de nodeType y no se pueden modificar más adelante. Por lo tanto, los elementos nodeType no se pueden configurar con esta propiedad.
> * Cuando "sfZonalUpgradeMode" se omite o se establece en "Jerárquico", las implementaciones de clúster y aplicación serán más lentas, porque habrá más dominios de actualización en el clúster. Es importante ajustar correctamente los tiempos de inactividad de la directiva de actualización para incorporar la duración del tiempo de actualización para 15 dominios de actualización.
> * Se recomienda establecer el nivel de confiabilidad del clúster en Platinum para garantizar que el clúster sobreviva al escenario con una zona caída.

>[!NOTE]
> Como procedimiento recomendado, se recomienda establecer sfZonalUpgradeMode en Jerárquico, o bien omitirlo. La implementación seguirá la distribución zonal de las máquinas virtuales que afectan a una cantidad menor de réplicas o instancias, para hacerlas más seguras.
> Use sfZonalUpgradeMode establecido en Paralelo si la velocidad de la implementación es prioridad o solo se ejecuta una carga de trabajo sin estado en el tipo de nodo con varias zonas de disponibilidad. Esto hará que el recorrido del dominio de actualización ocurra en paralelo en todas las zonas de disponibilidad.

### <a name="migration-to-the-node-type-with-multiple-availability-zones"></a>Migración al tipo de nodo con varias zonas de disponibilidad
En todos los escenarios de migración, es necesario agregar un elemento nodeType nuevo que tendrá varias zonas de disponibilidad admitidas. No se puede migrar un elemento nodeType existente para admitir varias zonas.
En [este artículo](https://docs.microsoft.com/azure/service-fabric/service-fabric-scale-up-primary-node-type ) se encuentran los pasos detallados para agregar un elemento nodeType nuevo y para agregar también los otros recursos necesarios para el elemento nodeType nuevo, como los recursos de IP y LB. En el mismo artículo se describe también cómo retirar el elemento nodeType existente una vez que se agrega al clúster el elemento nodeType con varias zonas disponibles.

* Migración desde un elemento nodeType que usa recursos IP y LB básicos: Esto ya se describe [aquí](https://docs.microsoft.com/azure/service-fabric/service-fabric-cross-availability-zones#migrate-to-using-availability-zones-from-a-cluster-using-a-basic-sku-load-balancer-and-a-basic-sku-ip) para la solución con un tipo de nodo por zona de disponibilidad. 
    Para el tipo de nodo nuevo, la única diferencia es que solo hay 1 conjunto de escalado de máquinas virtuales y 1 elemento nodeType para todas las zonas de disponibilidad, en lugar de uno de cada uno por zona de disponibilidad.
* Migración desde un elemento nodeType que usa los recursos de IP y LB de SKU estándar con NSG:   Siga el mismo procedimiento descrito anteriormente, excepto que no es necesario agregar recursos de LB, IP y NSG nuevos, y los mismos recursos se pueden volver a usar en el elemento nodeType nuevo.


[sf-architecture]: ./media/service-fabric-cross-availability-zones/sf-cross-az-topology.png
[sf-multi-az-arch]: ./media/service-fabric-cross-availability-zones/sf-multi-az-topology.png
