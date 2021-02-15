---
title: 'Creación de una dirección IP pública: Azure Portal'
description: Aprenda a crear una IP pública en Azure Portal
services: virtual-network
documentationcenter: na
author: blehr
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2020
ms.author: blehr
ms.openlocfilehash: 7d0c83f1ae18d36557a7a5b0222aee2905e05cb7
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550242"
---
# <a name="quickstart-create-a-public-ip-address-using-the-azure-portal"></a>Inicio rápido: Creación de una dirección IP pública mediante Azure Portal

En este artículo se muestra cómo crear un recurso de dirección IP pública mediante Azure Portal. Para más información sobre los recursos que se pueden asociar, la diferencia entre la SKU básica y estándar y otra información relacionada, consulte [Direcciones IP públicas](./public-ip-addresses.md).  En este ejemplo, nos centraremos solo en las direcciones IPv4. Para más información sobre las direcciones IPv6, consulte [IPv6 para la red virtual de Azure](./ipv6-overview.md).

# <a name="standard-sku"></a>[**SKU Estándar**](#tab/option-create-public-ip-standard-zones)

Use los pasos siguientes para crear una dirección IP pública estándar con redundancia de zona denominada **myStandardZRPublicIP**.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Crear un recurso**. 
3. En el cuadro de búsqueda, escriba *Dirección IP pública*.
4. En los resultados de la búsqueda, escriba **Dirección IP pública**. A continuación, en la página **Dirección IP pública** seleccione **Crear**.
5. En la página **Crear dirección IP pública**, escriba o seleccione la siguiente información: 

    | Configuración                 | Value                       |
    | ---                     | ---                         |
    | Versión de la dirección IP              | Seleccione IPv4                 |    
    | SKU                     | Seleccione **Estándar**.         |
    | Nivel (si se muestra *)                  | Seleccione **Regional**.         |
    | Nombre                    | Escriba *myStandardZRPublicIP*          |
    | Asignación de dirección IP   | Tenga en cuenta que se bloqueará como "estática".                                        |
    | Tiempo de espera de inactividad (minutos)  | Deje el valor en 4.        |
    | Etiqueta de nombre DNS          | Deje el valor en blanco.    |
    | Subscription            | Seleccione su suscripción.   |
    | Resource group          | Seleccione **Crear nuevo**, escriba myResourceGroup y seleccione **Aceptar**. |
    | Location                | Seleccione **Este de EE. UU. 2**.      |
    | Zona de disponibilidad       | Seleccione **Con redundancia de zona**, Ninguna zona o una zona específica (consulte la nota siguiente). |

Tenga en cuenta que estas opciones solo son selecciones válidas en regiones con [zonas de disponibilidad](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#availability-zones).  (También puede seleccionar una zona específica en estas regiones, aunque no sea resistente a errores de zona).  Para más información sobre las zonas de disponibilidad, consulte [Introducción a las zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview).

\* = el nivel está relacionado con la funcionalidad [Equilibrador de carga entre regiones](../load-balancer/cross-region-overview.md), actualmente en versión preliminar.

# <a name="basic-sku"></a>[**SKU básica**](#tab/option-create-public-ip-basic)

Use los pasos siguientes para crear una dirección IP pública estática básica denominada **myBasicPublicIP**.  Las direcciones IP públicas básicas no tienen el concepto de zonas de disponibilidad.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Crear un recurso**. 
3. En el cuadro de búsqueda, escriba *Dirección IP pública*.
4. En los resultados de la búsqueda, escriba **Dirección IP pública**. A continuación, en la página **Dirección IP pública** seleccione **Crear**.
5. En la página **Crear dirección IP pública**, escriba o seleccione la siguiente información: 

    | Configuración                 | Value                       |
    | ---                     | ---                         |
    | Versión de la dirección IP              | Seleccione IPv4                 |    
    | SKU                     | Seleccione **Estándar**.         |
    | Nombre                    | Escriba *myBasicPublicIP*          |
    | Asignación de dirección IP   | Elija **Estática** (vea la nota a continuación).                                     |
    | Tiempo de espera de inactividad (minutos)  | Deje el valor en 4.        |
    | Etiqueta de nombre DNS          | Deje el valor en blanco.    |
    | Subscription            | Seleccione su suscripción.   |
    | Resource group          | Seleccione **Crear nuevo**, escriba myResourceGroup y seleccione **Aceptar**. |
    | Location                | Seleccione **Este de EE. UU. 2**.      |

Si es admisible que la dirección IP cambie con el tiempo, se puede seleccionar la asignación de IP **dinámica**.

---

## <a name="additional-information"></a>Información adicional 

Para más información sobre los campos individuales enumerados anteriormente, consulte [Administración de direcciones IP públicas](./virtual-network-public-ip-address.md#create-a-public-ip-address).

## <a name="next-steps"></a>Pasos siguientes
- Asocie una [dirección IP pública a una máquina virtual](./associate-public-ip-address-vm.md#azure-portal).
- Obtenga más información acerca de las [direcciones IP públicas](./public-ip-addresses.md#public-ip-addresses) en Azure.
- Obtenga más información acerca de toda la [configuración de direcciones IP públicas](virtual-network-public-ip-address.md#create-a-public-ip-address).
