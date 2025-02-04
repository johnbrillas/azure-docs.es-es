---
title: IP Groups en Azure Firewall
description: Los grupos IP permiten agrupar y administrar direcciones IP en las reglas de Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 01/22/2021
ms.author: victorh
ms.openlocfilehash: 907ed0774db0e07a61a3ad568e6c451ea5d37b44
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102501743"
---
# <a name="ip-groups-in-azure-firewall"></a>IP Groups en Azure Firewall

IP Groups permite agrupar y administrar direcciones IP en reglas de Azure Firewall de las siguientes maneras:

- Como una dirección de origen en reglas de DNAT
- Como una dirección de origen o destino en reglas de red
- Como una dirección de origen en reglas de aplicación


Un grupo IP puede tener una sola dirección IP, varias direcciones IP o uno o varios intervalos de direcciones IP.

Los grupos de IP se puede reutilizar en reglas de DNAT, red y aplicación de Azure Firewall para varios firewalls entre regiones y suscripciones de Azure. Los nombres de grupo deben ser únicos. Puede configurar un grupo de IP en Azure Portal, la CLI de Azure o la API REST. Para ayudarle a comenzar, se proporciona una plantilla de ejemplo.

> [!NOTE]
> Los grupos de direcciones IP no están disponibles actualmente en los entornos de nube nacionales de Azure.

## <a name="sample-format"></a>Formato de ejemplo

Los siguientes ejemplos de formato de dirección IPv4 son válidos para su uso en IP Groups:

- Dirección única: 10.0.0.0
- Notación CIDR: 10.1.0.0/32
- Intervalo de direcciones: 10.2.0.0-10.2.0.31

## <a name="create-an-ip-group"></a>Creación de un grupo de IP

Un grupo de IP se puede crear mediante Azure Portal, la CLI de Azure o la API REST. Para obtener más información, consulte [Creación de grupos de IP](create-ip-group.md).

## <a name="browse-ip-groups"></a>Acceso a IP Groups
1. En la barra de búsqueda de Azure Portal, escriba **IP Groups** y selecciónelo. Puede ver la lista de los grupos de IP o puede seleccionar **Agregar** para crear uno.
2. Seleccione un grupo de IP para abrir la página de información general. Puede editar, agregar o eliminar direcciones IP o grupos de IP.

   ![Información general de grupos de IP](media/ip-groups/overview.png)

## <a name="manage-an-ip-group"></a>Administración de un grupo de IP

Puede ver todas las direcciones IP del grupo de IP y las reglas o los recursos a él asociados. Para eliminar un grupo de IP, primero debe desasociar el grupo de IP del recurso que lo usa.

1. Para ver o editar las direcciones IP, seleccione **Direcciones IP** en **Configuración** en el panel izquierdo.
2. Para agregar una o varias direcciones IP, seleccione **Agregar direcciones IP**. Se abre la página **Drag or Browse** (Arrastrar o examinar) para que las descargue; también puede escribir las direcciones manualmente.
3.    Seleccione los puntos suspensivos ( **…** ) de la derecha para editar o eliminar las direcciones IP. Para editar o eliminar varias direcciones IP, active las casillas y seleccione **Editar** o **Eliminar** en la parte superior.
4. Por último, puede exportar el archivo en formato CSV.

> [!NOTE]
> Si elimina todas las direcciones IP de un grupo de IP mientras todavía se está usando en una regla, esa regla se omite.


## <a name="use-an-ip-group"></a>Uso de un grupo de IP

Ahora puede seleccionar **IP Group** (Grupo de IP) como **Tipo de origen** o **Tipo de destino** para las direcciones IP al crear reglas de DNAT, aplicación o red de Azure Firewall.

![IP Groups en Firewall](media/ip-groups/fw-ipgroup.png)

## <a name="region-availability"></a>Disponibilidad en regiones

Los grupos IP están disponible en todas las regiones de la nube pública.

## <a name="ip-address-limits"></a>Límites de direcciones IP

Puede tener un máximo de 100 grupos de IP por firewall con un máximo de 5000 direcciones IP individuales o prefijos IP por cada grupo de IP.

## <a name="related-azure-powershell-cmdlets"></a>Cmdlets de Azure PowerShell relacionados

Se pueden usar los siguientes cmdlets de Azure PowerShell para crear y administrar grupos de IP:

- [New-AzIpGroup](/powershell/module/az.network/new-azipgroup)
- [Remove-AzIPGroup](/powershell/module/az.network/remove-azipgroup)
- [Get-AzIpGroup](/powershell/module/az.network/get-azipgroup)
- [Set-AzIpGroup](/powershell/module/az.network/set-azipgroup)
- [New-AzFirewallNetworkRule](/powershell/module/az.network/new-azfirewallnetworkrule)
- [New-AzFirewallApplicationRule](/powershell/module/az.network/new-azfirewallapplicationrule)
- [New-AzFirewallNatRule](/powershell/module/az.network/new-azfirewallnatrule)

## <a name="next-steps"></a>Pasos siguientes

- Consulte el tutorial [Implementación y configuración de Azure Firewall](tutorial-firewall-deploy-portal.md).