---
title: 'Creación de una dirección IP pública: Azure Portal'
description: Aprenda a crear una IP pública en Azure Portal
services: virtual-network
author: asudbring
ms.service: virtual-network
ms.subservice: ip-services
ms.topic: how-to
ms.date: 02/22/2021
ms.author: allensu
ms.openlocfilehash: e6b7648188e2307da4ef40e0ab3daf6201f9d89d
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101694876"
---
# <a name="create-a-public-ip-address-using-the-azure-portal"></a>Creación de una dirección IP pública mediante Azure Portal

En este artículo se muestra cómo crear un recurso de dirección IP pública mediante Azure Portal. 

Para obtener más información sobre los recursos a los que se puede asociar esta dirección IP pública y la diferencia entre las SKU básica y estándar, vea [Direcciones IP públicas](./public-ip-addresses.md). 

Este artículo se centra en las direcciones IPv4. Para obtener más información sobre las direcciones IPv6, vea [¿Qué es IPv6 para Azure Virtual Network?](./ipv6-overview.md).

# <a name="standard-sku"></a>[**SKU Estándar**](#tab/option-create-public-ip-standard-zones)

Use los pasos siguientes para crear una dirección IP pública estándar con redundancia de zona denominada **myStandardZRPublicIP**.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Crear un recurso**. 
3. En el cuadro de búsqueda, escriba **Dirección IP pública**. Seleccione **Dirección IP pública** en los resultados de la búsqueda.
4. En la página **Dirección IP pública**, seleccione **Crear**.
5. En la página **Crear dirección IP pública**, escriba o seleccione la siguiente información: 

    | Configuración                 | Value                       |
    | ---                     | ---                         |
    | Versión de la dirección IP              | Seleccione IPv4                 |    
    | SKU                     | Seleccione **Estándar**.         |
    | Nivel*                   | Seleccione **Regional**.         |
    | Nombre                    | Escriba **myStandardZRPublicIP**          |
    | Asignación de dirección IP   | Tenga en cuenta que esta selección está bloqueada como "Estática".                                        |
    | Preferencia de enrutamiento      | Deje el valor predeterminado **Microsoft Network**. </br> Para obtener más información sobre la preferencia de enrutamiento, vea [¿Qué es la preferencia de enrutamiento (versión preliminar)?](./routing-preference-overview.md) |
    | Tiempo de espera de inactividad (minutos)  | Deje el valor predeterminado **4**.        |
    | Etiqueta de nombre DNS          | Deje el valor en blanco.    |
    | Subscription            | Seleccione su suscripción.   |
    | Resource group          | Seleccione **Crear nuevo** y escriba **myResourceGroup**. </br> Seleccione **Aceptar**. |
    | Location                | Seleccione **Este de EE. UU. 2**.      |
    | Zona de disponibilidad       | Seleccione **Con redundancia de zona**, Ninguna zona o una zona específica (consulte la nota siguiente). |

:::image type="content" source="./media/create-public-ip-portal/create-standard-ip.png" alt-text="Creación de una dirección IP estándar en Azure Portal" border="false":::

> [!NOTE]
> Estas selecciones son válidas en regiones con [Availability Zones](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#availability-zones). </br>
Puede seleccionar una zona específica en estas regiones, aunque no será resistente a errores de zona. </br> Para más información sobre las zonas de disponibilidad, consulte [Introducción a las zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview).

\* = el nivel está relacionado con la funcionalidad [Equilibrador de carga entre regiones](../load-balancer/cross-region-overview.md), actualmente en versión preliminar.

# <a name="basic-sku"></a>[**SKU básica**](#tab/option-create-public-ip-basic)

En esta sección, creará una dirección IP pública básica denominada **myBasicPublicIP**. 

> [!NOTE]
> Las direcciones IP públicas básicas no admiten zonas de disponibilidad.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Crear un recurso**. 
3. En el cuadro de búsqueda, escriba **Dirección IP pública**. Seleccione **Dirección IP pública** en los resultados de la búsqueda.
4. En la página **Dirección IP pública**, seleccione **Crear**.
5. En la página **Crear dirección IP pública**, escriba o seleccione la siguiente información: 

    | Configuración                 | Value                       |
    | ---                     | ---                         |
    | Versión de la dirección IP              | Seleccione IPv4                 |    
    | SKU                     | Seleccione **Básica**.         |
    | Nombre                    | Escriba *myBasicPublicIP*          |
    | Asignación de dirección IP   | Seleccione **Estática** (vea la nota a continuación).                                     |
    | Tiempo de espera de inactividad (minutos)  | Deje el valor predeterminado **4**.       |
    | Etiqueta de nombre DNS          | Deje el valor en blanco.    |
    | Subscription            | Seleccione su suscripción.   |
    | Resource group          | Seleccione **Crear nuevo** y escriba **myResourceGroup**. </br> Seleccione **Aceptar**. |
    | Location                | Seleccione **Este de EE. UU. 2**.      |

:::image type="content" source="./media/create-public-ip-portal/create-basic-ip.png" alt-text="Creación de una dirección IP estándar en Azure Portal" border="false":::

Si es admisible que la dirección IP cambie con el tiempo, se puede seleccionar la asignación de IP **Dinámica**.

---

## <a name="additional-information"></a>Información adicional 

Para más información sobre los campos individuales enumerados anteriormente, vea [Administración de direcciones IP públicas](./virtual-network-public-ip-address.md#create-a-public-ip-address).

## <a name="next-steps"></a>Pasos siguientes
- Asocie una [dirección IP pública a una máquina virtual](./associate-public-ip-address-vm.md#azure-portal).
- Obtenga más información acerca de las [direcciones IP públicas](./public-ip-addresses.md#public-ip-addresses) en Azure.
- Obtenga más información acerca de toda la [configuración de direcciones IP públicas](virtual-network-public-ip-address.md#create-a-public-ip-address).