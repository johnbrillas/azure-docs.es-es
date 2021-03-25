---
title: 'Configuración de ambas opciones de preferencias de enrutamiento para una máquina virtual: Azure Portal'
description: Obtenga información acerca de cómo habilitar ambas opciones de preferencias de enrutamiento.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2021
ms.author: mnayak
ms.openlocfilehash: d9bb00077b1d96d13e15861cac477f913a002030
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678879"
---
# <a name="configure-both-routing-preference-options-for-a-virtual-machine"></a>Configuración de ambas opciones de preferencias de enrutamiento para una máquina virtual

En este artículo se muestra cómo configurar ambas opciones de [preferencias de enrutamiento](./routing-preference-overview.md) (Internet y red global de Microsoft) para una máquina virtual. Esto se consigue mediante dos interfaces de red virtual, una interfaz de red con una dirección IP pública enrutada mediante la red global de Microsoft y la otra con una dirección IP pública enrutada mediante una red del ISP.

## <a name="prerequisites"></a>Prerrequisitos

Cree una máquina virtual con una dirección IP pública según las instrucciones descritas en [Creación de una máquina virtual con una dirección IP pública estática mediante Azure Portal](./virtual-network-deploy-static-pip-arm-portal.md). La opción de preferencia de enrutamiento predeterminada para la dirección IP pública es mediante la **red global de Microsoft**. Una vez que tenga una máquina virtual con una dirección IP pública creada, agregue una segunda dirección IP pública a la máquina virtual que enruta el tráfico mediante la red del ISP de tránsito con su preferencia de enrutamiento configurada como **Internet**.

## <a name="create-a-public-ip-address-with-a-routing-preference-choice-internet"></a>Creación de una dirección IP pública con la opción preferencia de enrutamiento de Internet
1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Crear un recurso**. 
3. En el cuadro de búsqueda, escriba *Dirección IP pública*.
4. En los resultados de la búsqueda, escriba **Dirección IP pública**. A continuación, en la página **Dirección IP pública** seleccione **Crear**.
5. En las opciones de **Preferencias de enrutamiento**, seleccione **Internet**.

      ![Creación de una dirección IP pública](./media/routing-preference-portal/public-ip-new.png)

    > [!NOTE]
    > Las direcciones IP públicas se crean con una dirección IPv4 o IPv6. Sin embargo, la preferencia de enrutamiento solo admite IPV4 actualmente.

## <a name="create-a-network-interface-and-associate-the-public-ip"></a>Creación de una interfaz de red y asociación de la dirección IP pública

1. Cree una [interfaz de red](./routing-preference-overview.md) mediante Azure Portal con la configuración predeterminada. 
1. [Asocie la dirección IP pública a la interfaz de red](./associate-public-ip-address-vm.md) creada en la sección anterior. Puede tardar unos segundos en aparecer una dirección IP. Puede ver la dirección IP pública asignada a la configuración IP, como se muestra a continuación:

    ![Asociación de la dirección IP pública](./media/routing-preference-mixed-network-adapter-portal/public-ip.png)

## <a name="attach-network-interface-to-the-vm"></a>Conexión de la interfaz de red a la máquina virtual

1. [Conecte la interfaz de red creada en la sección anterior a la máquina virtual](./virtual-network-network-interface-vm.md).
2. Ahora puede ver dos interfaces de red virtual asociadas a la máquina virtual, una con una dirección IP pública que se enruta mediante la red global de Microsoft y la otra enrutada mediante una red del ISP, tal y como se muestra: ![Conexión de una interfaz de red a una máquina virtual](./media/routing-preference-mixed-network-adapter-portal/mixed-network-adapter.png) 

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre las [direcciones IP públicas con preferencia de enrutamiento](routing-preference-overview.md).
- [Configuración de la preferencia de enrutamiento de una VM](tutorial-routing-preference-virtual-machine-portal.md).
- [Configuración de la preferencia de enrutamiento para una dirección IP pública mediante PowerShell](routing-preference-powershell.md).
- Obtenga más información acerca de las [direcciones IP públicas](./public-ip-addresses.md#public-ip-addresses) en Azure.
- Obtenga más información acerca de toda la [configuración de direcciones IP públicas](virtual-network-public-ip-address.md#create-a-public-ip-address).