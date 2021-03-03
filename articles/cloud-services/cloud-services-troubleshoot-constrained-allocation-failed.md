---
title: Solución de problemas de ConstrainedAllocationFailed al implementar un servicio en la nube en Azure | Microsoft Docs
description: En este artículo se muestra cómo resolver una excepción ConstrainedAllocationFailed al implementar un servicio en la nube en Azure.
services: cloud-services
author: mibufo
ms.author: v-mibufo
ms.service: cloud-services
ms.topic: troubleshooting
ms.date: 02/04/2020
ms.openlocfilehash: de344bbcd89158676bacf2a8aa1743d282700b9d
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100520821"
---
# <a name="troubleshoot-constrainedallocationfailed-when-deploying-a-cloud-service-to-azure"></a>Solución de problemas de ConstrainedAllocationFailed al implementar un servicio en la nube en Azure

En este artículo, solucionará los errores de asignación por los que Azure Cloud Services no puede realizar la implementación debido a las restricciones.

Microsoft Azure realiza asignaciones cuando se dan los siguientes escenarios:

- Actualización de instancias de Cloud Services

- Adición de nuevas instancias de rol web o de rol de trabajo

- Implementación de instancias en un servicio en la nube

En ocasiones, es posible que reciba errores durante estas operaciones, incluso antes de alcanzar el límite de la suscripción de Azure.

> [!TIP]
> La información también puede ser útil si tiene pensado realizar la implementación de sus servicios.

## <a name="symptom"></a>Síntoma

En Azure Portal, vaya a su servicio en la nube y, en la barra lateral, seleccione *Registros de operaciones (clásico)* para ver los registros.

Al inspeccionar los registros del servicio en la nube, verá la siguiente excepción:

|Tipo de excepción  |Mensaje de error  |
|---------|---------|
|ConstrainedAllocationFailed     |Error "`{Operation ID}`" en operaciones de Azure con el código Compute.ConstrainedAllocationFailed. Detalles: Error en la asignación; no se pudieron satisfacer las restricciones de la solicitud. La nueva implementación del servicio solicitada está enlazada a un grupo de afinidad, su destino es una instancia de Virtual Network o hay una implementación existente bajo este servicio hospedado. Todas estas condiciones restringen la nueva implementación a recursos específicos de Azure. Inténtelo de nuevo más tarde o pruebe a reducir el tamaño de la máquina virtual o el número de instancias de rol. También puede quitar, si es posible, las restricciones mencionadas o intentar realizar la implementación en otra región.|

## <a name="cause"></a>Causa

Existe un problema de capacidad con la región o el clúster donde se va a realizar la implementación. Se produce cuando la SKU de recursos seleccionada no está disponible para la ubicación especificada.

> [!NOTE]
> Cuando se implementa el primer nodo de un servicio en la nube, se *ancla* a un grupo de recursos. Un grupo de recursos puede ser un clúster único o un grupo de clústeres.
>
> Con el tiempo, los recursos de este grupo de recursos se pueden aprovechar por completo. Si un servicio en la nube realiza una solicitud de asignación de recursos adicionales cuando no hay suficientes recursos disponibles en el grupo de recursos anclado, la solicitud producirá un [error de asignación](cloud-services-allocation-failures.md).

## <a name="solution"></a>Solución

En este escenario, debe seleccionar una región o SKU diferente donde implementar el servicio en la nube. Antes de implementar o actualizar el servicio en la nube, puede determinar qué SKU están disponibles en una región o zona de disponibilidad. Siga los procesos de la [CLI de Azure](#list-skus-in-region-using-azure-cli), de [PowerShell](#list-skus-in-region-using-powershell) o de la [API de REST](#list-skus-in-region-using-rest-api) a continuación.

### <a name="list-skus-in-region-using-azure-cli"></a>Enumeración de SKU en la región con la CLI de Azure

Puede usar el comando [az vm list-skus](https://docs.microsoft.com/cli/azure/vm.html#az_vm_list_skus).

- Use el parámetro `--location` para filtrar la salida a la ubicación que esté usando.
- Use el parámetro `--size` para buscar un nombre de tamaño parcial.
- Para más información, vea la guía sobre [Resolución del error con la SKU no disponible](../azure-resource-manager/templates/error-sku-not-available.md#solution-2---azure-cli).

    **Por ejemplo:**

    ```azurecli
    az vm list-skus --location southcentralus --size Standard_F --output table
    ```

    **Resultados de ejemplo:** ![Salida de la CLI de Azure tras la ejecución del comando "az vm list-skus --location southcentralus --size Standard_F --output table", que muestra las SKU disponibles.](./media/cloud-services-troubleshoot-constrained-allocation-failed/cloud-services-troubleshoot-constrained-allocation-failed-1.png)

#### <a name="list-skus-in-region-using-powershell"></a>Enumeración de SKU en la región con PowerShell

Puede usar el comando [Get-AzComputeResourceSku](https://docs.microsoft.com/powershell/module/az.compute/get-azcomputeresourcesku).

- Filtre los resultados por ubicación.
- Debe tener la versión más reciente de PowerShell para que funcione este comando.
- Para más información, vea la guía sobre [Resolución del error con la SKU no disponible](../azure-resource-manager/templates/error-sku-not-available.md#solution-1---powershell).

**Por ejemplo:**

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations -icontains "centralus"}
```

**Otros comandos útiles:**

Filtre las ubicaciones que contienen el tamaño (Standard_DS14_v2):

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations.Contains("centralus") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("Standard_DS14_v2")}
```

Filtre todas las ubicaciones que contengan el tamaño (V3):

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations.Contains("centralus") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("v3")} | fc
```

#### <a name="list-skus-in-region-using-rest-api"></a>Enumeración de SKU en la región con la API de REST

Puede usar la operación [Resource Skus - List](https://docs.microsoft.com/rest/api/compute/resourceskus/list). Se devuelven las SKU y las regiones disponibles en el formato siguiente:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    <Rest_of_your_file_is_located_here...>
  ]
}
    
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más soluciones de errores de asignación y comprender mejor cómo se generan:

> [!div class="nextstepaction"]
> [Errores de asignación (servicios en la nube)](cloud-services-allocation-failures.md)

Si su problema con Azure no se trata en este artículo, visite los foros de Azure en [MSDN y Stack Overflow](https://azure.microsoft.com/support/forums/). Puede publicar su problema en ellos o [@AzureSupport en Twitter](https://twitter.com/AzureSupport). También puede enviar una solicitud de soporte técnico de Azure. Para enviar una solicitud de soporte técnico, en la página de [soporte técnico de Azure](https://azure.microsoft.com/support/options/), seleccione *Obtener soporte técnico*.
