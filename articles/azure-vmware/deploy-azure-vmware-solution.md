---
title: Implementación y configuración de Azure VMware Solution
description: Aprenda a usar la información recopilada en la fase de planeación para implementar y configurar la nube privada de Azure VMware Solution.
ms.topic: tutorial
ms.custom: contperf-fy21q3
ms.date: 02/17/2021
ms.openlocfilehash: 48b6927407a95d41603c3032f298ffc28def9693
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462463"
---
# <a name="deploy-and-configure-azure-vmware-solution"></a>Implementación y configuración de Azure VMware Solution

En este artículo, usará la información de la [sección de planeación](production-ready-deployment-steps.md) para implementar y configurar Azure VMware Solution. 

>[!IMPORTANT]
>Si aún no ha definido la información, vuelva a la [sección de planeamiento](production-ready-deployment-steps.md) antes de continuar.


## <a name="create-an-azure-vmware-solution-private-cloud"></a>Creación de una nube privada de Azure VMware Solution

Siga los requisitos previos y los pasos descritos en el tutorial sobre [creación de una nube privada de Azure VMware Solution](tutorial-create-private-cloud.md). Puede crear una nube privada de Azure VMware Solution mediante [Azure Portal](tutorial-create-private-cloud.md#azure-portal) o con la [CLI de Azure](tutorial-create-private-cloud.md#azure-cli).  

>[!NOTE]
>Puede encontrar información completa de este paso en el vídeo [Azure VMware Solution: Implementación](https://www.youtube.com/embed/gng7JjxgayI).

## <a name="create-the-jump-box"></a>Creación del jumpbox

>[!IMPORTANT]
>Si dejó la opción **Virtual Network** en blanco durante el paso de aprovisionamiento inicial en la pantalla de **Creación de una nube privada**, complete el tutorial [Configuración de redes para la nube privada de VMware](tutorial-configure-networking.md) **antes** de continuar con esta sección.  

Después de implementar Azure VMware Solution, deberá crear el jumpbox de la red virtual que se conecta a vCenter y NSX. Una vez que haya configurado los circuitos de ExpressRoute y Global Reach de ExpressRoute, el jumpbox dejará de ser necesario.  Aún así, es práctico conectarse a vCenter y NSX en Azure VMware Solution.  

:::image type="content" source="media/pre-deployment/jump-box-diagram.png" alt-text="Creación del jumpbox de Azure VMware Solution" border="false" lightbox="media/pre-deployment/jump-box-diagram.png":::

Para crear una máquina virtual en la red virtual que [identificó o creó como parte del proceso de implementación](production-ready-deployment-steps.md#attach-azure-virtual-network-to-azure-vmware-solution), siga estas instrucciones: 

[!INCLUDE [create-avs-jump-box-steps](includes/create-jump-box-steps.md)]

## <a name="connect-to-a-virtual-network-with-expressroute"></a>Conexión a una red virtual con ExpressRoute

>[!IMPORTANT]
>Si ya ha definido una red virtual en la pantalla de implementación de Azure, vaya a la sección siguiente.

Si no definió una red virtual en el paso de implementación y su intención es conectar la instancia de ExpressRoute de Azure VMware Solution a una puerta de enlace de ExpressRoute existente, siga estos pasos.

[!INCLUDE [connect-expressroute-to-vnet](includes/connect-expressroute-vnet.md)]

## <a name="verify-network-routes-advertised"></a>Comprobación de las rutas de red anunciadas

El jumpbox está en la red virtual donde Azure VMware Solution se conecta a través del circuito de ExpressRoute.  En Azure, vaya a la interfaz de red de jumpbox y [consulte las rutas efectivas](../virtual-network/manage-route-table.md#view-effective-routes).

En la lista de rutas efectivas, debería ver las redes que se crearon como parte de la implementación de Azure VMware Solution. Verá varias redes derivadas de la [red `/22` que ha definido](production-ready-deployment-steps.md#ip-address-segment-for-private-cloud-management) al [crear una nube privada](#create-an-azure-vmware-solution-private-cloud).  

:::image type="content" source="media/pre-deployment/azure-vmware-solution-effective-routes.png" alt-text="Comprobación de las rutas de red anunciadas desde Azure VMware Solution a Azure Virtual Network" lightbox="media/pre-deployment/azure-vmware-solution-effective-routes.png":::

En este ejemplo, la red 10.74.72.0/22 que se agregó durante la implementación, se deriva de las redes /24.  Si ve algo similar, puede conectarse a vCenter en Azure VMware Solution.

## <a name="connect-and-sign-in-to-vcenter-and-nsx-t"></a>Conexión e inicio de sesión en vCenter y NSX-T

Inicie sesión en el jumpbox que creó en el paso anterior. Una vez que haya iniciado sesión, abra un explorador web, vaya a vCenter y NSX-T Manager e inicie sesión.  

Puede identificar las direcciones IP y las credenciales de la consola de NSX-T Manager y vCenter en Azure Portal.  Seleccione una nube privada y, después, **Administrar** > **Identidad**.

>[!TIP]
>Seleccione **Generar una contraseña nueva** para generar las nuevas contraseñas de vCenter y NSX-T.

:::image type="content" source="media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Mostrar las direcciones URL y las credenciales del administrador de NSX y vCenter de la nube privada" border="true":::.



## <a name="create-a-network-segment-on-azure-vmware-solution"></a>Creación de un segmento de red en Azure VMware Solution

Puede usar NSX-T Manager para crear nuevos segmentos de red en el entorno de Azure VMware Solution.  Recuerde que definió las redes que quiso crear en la [sección de planeamiento](production-ready-deployment-steps.md).  Si aún no las ha definido, vuelva a la [sección de planeación](production-ready-deployment-steps.md) antes de continuar.

>[!IMPORTANT]
>Asegúrese de que el bloque de direcciones de red CIDR que definió no se superponga con ningún elemento de los entornos locales o de Azure.  

Siga el tutorial de [Creación de un segmento de red NSX-T en Azure VMware Solution](tutorial-nsx-t-network-segment.md) para crear un segmento de red NSX-T en Azure VMware Solution.

## <a name="verify-advertised-nsx-t-segment"></a>Comprobación del segmento de NSX-T anunciado

Vuelva al paso de [Comprobación de las rutas de red anunciadas](#verify-network-routes-advertised). Verá otras rutas en la lista que representan los segmentos de red que creó en el paso anterior.  

En el caso de las máquinas virtuales, asignará los segmentos creados en el paso [Creación de un segmento de red en Azure VMware Solution](#create-a-network-segment-on-azure-vmware-solution).  

Como el DNS es necesario, identifique el servidor DNS que quiere utilizar.  

- Si tiene configurado Global Reach de ExpressRoute, use el servidor DNS que quiera usar en el entorno local.  
- Si tiene un servidor DNS en Azure, úselo.  
- Si no tiene ninguno, use el servidor DNS que usa el jumpbox.

>[!NOTE]
>Este paso consiste en identificar el servidor DNS y no se realiza ninguna configuración en el mismo.

## <a name="optional-provide-dhcp-services-to-nsx-t-network-segment"></a>(Opcional) Proporcionar servicios DHCP al segmento de red NSX-T

Si tiene previsto usar DHCP en los segmentos de NSX-T, continúe con esta sección. En caso contrario, vaya a la sección [Agregar una VM en el segmento de red NSX-T](#add-a-vm-on-the-nsx-t-network-segment).  

Ahora que ha creado el segmento de red de NSX-T, puede crear y administrar DHCP en Azure VMware Solution de dos maneras:

* Si utiliza NSX-T para hospedar el servidor DHCP, deberá [crear un servidor DHCP](manage-dhcp.md#create-a-dhcp-server) y [retransmitir a ese servidor](manage-dhcp.md#create-dhcp-relay-service). 
* Si usa un servidor DHCP externo de terceros en la red, tendrá que [crear un servicio de retransmisión DHCP](manage-dhcp.md#create-dhcp-relay-service).  Para realizar esta opción, [solo debe realizar la configuración de retransmisión](manage-dhcp.md#create-dhcp-relay-service).


## <a name="add-a-vm-on-the-nsx-t-network-segment"></a>Agregar una VM en el segmento de red NSX-T

En Azure VMware Solution vCenter, implemente una máquina virtual y úsela para comprobar la conectividad de las redes de Azure VMware Solution con:

- Internet
- Instancias de Azure Virtual Network
- Local.  

Implemente la VM tal como lo haría en cualquier entorno de vSphere.  Conecte la máquina virtual a uno de los segmentos de red que creó anteriormente en NSX-T.  

>[!NOTE]
>Si configura un servidor DHCP, recibirá la configuración de red de la VM del mismo (no olvide configurar el ámbito).  Si va a realizar una configuración de forma estática, puede realizarla tal como lo haría normalmente.

## <a name="test-the-nsx-t-segment-connectivity"></a>Prueba de la conectividad del segmento NSX-T

Inicie sesión en la VM que creó en el paso anterior y compruebe la conectividad;

1. Haga ping a una dirección IP en Internet.
2. Vaya a un sitio de Internet a través del explorador web.
3. Haga ping en el jumpbox que se encuentra en Azure Virtual Network.

Azure VMware Solution ahora estará en funcionamiento y habrá establecido correctamente la conectividad con Azure Virtual Network e Internet y desde ellos.

## <a name="next-steps"></a>Pasos siguientes

En la siguiente sección, conectará Azure VMware Solution a la red local a través de ExpressRoute.
> [!div class="nextstepaction"]
> [Conexión de Azure VMware Solution al entorno local](azure-vmware-solution-on-premises.md)
