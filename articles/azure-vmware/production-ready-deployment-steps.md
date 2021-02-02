---
title: Planificación de la implementación de Azure VMware Solution
description: En este artículo se describe el flujo de trabajo de implementación de Azure VMware Solution.  El resultado final es un entorno listo para la creación y migración de máquinas virtuales (VM).
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: 8b1d69f3f953b43177a3b1d0611b51ca2cfb1a75
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98762866"
---
# <a name="planning-the-azure-vmware-solution-deployment"></a>Planificación de la implementación de Azure VMware Solution

En este artículo, se proporciona el proceso de planeamiento para identificar y recopilar los datos que se usan durante la implementación. Cuando planee la implementación, asegúrese de documentar la información que recopila para facilitar la referencia durante la implementación.

Los procesos de este inicio rápido generan un entorno listo para producción para la creación de máquinas virtuales (VM) y la migración. 

>[!IMPORTANT]
>Antes de crear el recurso de Azure VMware Solution, siga el artículo [Habilitación del recurso de Azure VMware Solution](enable-azure-vmware-solution.md) para enviar una incidencia de soporte técnico y que le asignen los hosts. Una vez que el equipo de soporte técnico recibe su solicitud, tardan hasta cinco días laborables en confirmarla y asignar los hosts. Si tiene una nube privada de Azure VMware Solution y quiere asignar más hosts, pasará por el mismo proceso. 


## <a name="subscription"></a>Suscripción

Identifique la suscripción que piensa usar para implementar Azure VMware Solution.  Puede crear una nueva suscripción o reutilizar una existente.

>[!NOTE]
>La suscripción debe estar asociada a un plan de Contrato Enterprise de Microsoft o a un plan de Proveedor de soluciones en la nube de Azure. Para más información, consulte [Habilitación del recurso de Azure VMware Solution](enable-azure-vmware-solution.md).

## <a name="resource-group"></a>Resource group

Identifique el grupo de recursos que quiere usar para Azure VMware Solution.  Por lo general, se crea un grupo de recursos específicamente para Azure VMware Solution, pero puede usar un grupo de recursos existente.

## <a name="region"></a>Region

Identifique la región en la que desea implementar Azure VMware Solution.  Para más información, consulte la [Guía de productos Azure disponibles por región](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=azure-vmware).

## <a name="resource-name"></a>Nombre del recurso

Defina el nombre del recurso que va a usar durante la implementación.  El nombre del recurso es un nombre descriptivo en el que se le da un título a la nube privada de Azure VMware Solution.

>[!IMPORTANT]
>El nombre no puede tener más de 40 caracteres. Si el nombre supera este límite, no podrá crear direcciones IP públicas para utilizarlas con la nube privada. 

## <a name="size-hosts"></a>Hosts de tamaño

Identifique los hosts de tamaño que quiere usar al implementar Azure VMware Solution.  Para obtener una lista completa, consulte la documentación de [nubes privadas y clústeres de Azure VMware Solution](concepts-private-clouds-clusters.md#hosts).

## <a name="number-of-hosts"></a>Número de hosts

Defina el número de hosts que quiere implementar en la nube privada de Azure VMware Solution.  El número mínimo de hosts es tres y el máximo es 16 por clúster.  Para más información, consulte la documentación de [nubes privadas y clústeres de Azure VMware Solution](concepts-private-clouds-clusters.md#clusters).

Siempre puede extender el clúster más adelante si necesita aumentar el número de implementación inicial.

## <a name="vcenter-admin-password"></a>Contraseña del administrador de vCenter
Defina la contraseña del administrador de vCenter.  Durante la implementación, creará una contraseña de administrador de vCenter. La contraseña es para la cuenta de administrador de cloudadmin@vsphere.local durante la compilación de vCenter. La usará para iniciar sesión en vCenter.

## <a name="nsx-t-admin-password"></a>Contraseña de administrador de NSX-T
Defina la contraseña del administrador de NSX-T.  Durante la implementación, creará una contraseña de administrador de NSX-T. La contraseña se asigna al usuario administrador en la cuenta de NSX durante la compilación de NSX. La usará para iniciar sesión en NSX-T Manager.

## <a name="ip-address-segment"></a>Segmento de dirección IP

El primer paso para planificar la implementación es planificar la segmentación de IP.  Azure VMware Solution ingiere una red /22 que le proporciona. Después, la divide en segmentos menores y usa esos segmentos IP para vCenter, VMware HCX, NSX-T y vMotion.

Azure VMware Solution se conecta a su instancia de Microsoft Azure Virtual Network mediante un circuito ExpressRoute interno. En la mayoría de los casos, se conecta a su centro de datos mediante ExpressRoute Global Reach. 

Azure VMware Solution, el entorno de Azure existente y el entorno local (normalmente) intercambian rutas. Dicho esto, el bloque de direcciones de red CIDR /22 que define en este paso no se debe superponer a nada que ya tenga en el entorno local o en Azure.

**Ejemplo**: 10.0.0.0/22

Para más información, consulte [Lista de comprobación de planificación de la red](tutorial-network-checklist.md#routing-and-subnet-considerations).

:::image type="content" source="media/pre-deployment/management-vmotion-vsan-network-ip-diagram.png" alt-text="Identificación: segmento de dirección IP" border="false":::  

## <a name="ip-address-segment-for-virtual-machine-workloads"></a>Segmento de dirección IP para cargas de trabajo de máquina virtual

Identifique un segmento IP para crear su primera red (segmento NSX) en la nube privada.  Dicho de otra forma, debe crear un segmento de red en Azure VMware Solution para que pueda implementar máquinas virtuales en Azure VMware Solution.   

Incluso si solo tiene previsto extender las redes L2, cree un segmento de red que valide el entorno.

Recuerde que los segmentos IP creados deben ser únicos en la superficie de Azure y local.  

**Ejemplo**: 10.0.4.0/24

:::image type="content" source="media/pre-deployment/nsx-segment-diagram.png" alt-text="Identificación: segmento de dirección IP para cargas de trabajo de máquina virtual" border="false":::     

## <a name="optional-extend-networks"></a>(Opcional) Extensión de redes

Puede extender los segmentos de red del entorno local a Azure VMware Solution y, si lo hace, identificar esas redes.  

Tenga en cuenta que:

- Si tiene previsto extender las redes del entorno local, esas redes deben conectarse a [vSphere Distributed Switch (vDS)](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-B15C6A13-797E-4BCB-B9D9-5CBC5A60C3A6.html) en el entorno local de VMware.  
- Si las redes que desea extender se encuentran en [vSphere Standard Switch](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-350344DE-483A-42ED-B0E2-C811EE927D59.html), no se pueden extender.

## <a name="attach-virtual-network-to-azure-vmware-solution"></a>Conexión de una red virtual a Azure VMware Solution

En este paso, identificará una puerta de enlace de red virtual de ExpressRoute y la red virtual de Azure auxiliar que se usa para conectar el circuito de ExpressRoute de Azure VMware Solution.  El circuito de ExpressRoute facilita la conectividad hacia y desde la nube privada de Azure VMware Solution a otros servicios de Azure, recursos de Azure y entornos locales.

Puede usar una puerta de enlace de red virtual de ExpressRoute *existente* o *nueva*.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-expressroute-diagram.png" alt-text="Identificación: Azure Virtual Network para conectar Azure VMware Solution" border="false":::

### <a name="use-an-existing-expressroute-virtual-network-gateway"></a>Uso de una puerta de enlace de red virtual de ExpressRoute existente

Si usa una puerta de enlace de red virtual de ExpressRoute *existente*, el circuito de ExpressRoute de Azure VMware Solution se establece después de implementar la nube privada. En este caso, deje en blanco el campo **Red virtual**.  

Tome nota de qué puerta de enlace de red virtual de ExpressRoute va a usar y continúe con el paso siguiente.

### <a name="create-a-new-expressroute-virtual-network-gateway"></a>Creación de una nueva puerta de enlace de red virtual de ExpressRoute

Al crear una *nueva* puerta de enlace de red virtual de ExpressRoute, puede usar una red virtual de Azure existente o crear una nueva.  

- Para una red virtual de Azure existente:
   1. Compruebe que no hay ninguna puerta de enlace de red virtual de ExpressRoute preexistente en la red virtual. 
   1. Seleccione la red virtual de Azure existente en la lista **Red virtual**.

- En el caso de una nueva red virtual de Azure, puede crearla de antemano o durante la implementación. Seleccione el vínculo **Crear nueva** en la lista **Red virtual**.

En la imagen siguiente se muestra la pantalla de implementación **Crear una nube privada** con el campo **Red virtual** resaltado.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-deployment-screen-vnet-circle.png" alt-text="Captura de pantalla de la pantalla de implementación de Azure VMware Solution con el campo Red virtual resaltado.":::

>[!NOTE]
>El entorno local y Azure VMware Solution pueden ver cualquier red virtual que se vaya a usar o crear, por lo que debe asegurarse de que el segmento IP que use en esa red virtual y las subredes no se superpongan.

## <a name="vmware-hcx-network-segments"></a>Segmentos de red de VMware HCX

VMware HCX es una tecnología incluida en Azure VMware Solution. Los casos de uso principales de VMware HCX son las migraciones de cargas de trabajo y la recuperación ante desastres. Si tiene previsto realizar alguna de ellas, es mejor planificar las redes ahora.   En otro caso, puede omitirla y continuar en el paso siguiente.

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha recopilado y documentado la información necesaria, continúe con la siguiente sección para crear la nube privada de Azure VMware Solution.

> [!div class="nextstepaction"]
> [Implementación de Azure VMware Solution](deploy-azure-vmware-solution.md)
