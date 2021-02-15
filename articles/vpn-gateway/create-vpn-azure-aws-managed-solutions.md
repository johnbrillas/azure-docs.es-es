---
title: Creación de una VPN entre Azure y AWS mediante soluciones administradas
description: Cómo crear una conexión VPN entre Azure y AWS mediante soluciones administradas, en lugar de VM o dispositivos.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: ricmmartins
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/03/2021
ms.author: ricmart
ms.openlocfilehash: 82161ac92566f6589ebfb64dddc221e296ec0992
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99539135"
---
# <a name="create-a-vpn-connection-between-azure-and-aws-using-managed-solutions"></a>Creación de una conexión VPN entre Azure y AWS mediante soluciones administradas

Puede establecer una conexión entre Azure y AWS mediante soluciones administradas. Anteriormente, era necesario usar un dispositivo o una VM que actuara como respondedor. Ahora, puede conectar la puerta de enlace privada virtual de AWS a Azure VPN Gateway directamente sin tener que preocuparse por la administración de los recursos de IaaS, como las máquinas virtuales. Este artículo le ayuda a crear una conexión VPN entre Azure y AWS usando solo soluciones administradas.

:::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/diagram.png" alt-text="Diagrama de la arquitectura":::

## <a name="configure-azure"></a>Configuración de Azure

### <a name="configure-a-virtual-network"></a>Configuración de una red virtual

Configurar una red virtual. Para obtener instrucciones, consulte [Inicio rápido de creación de red virtual](../virtual-network/quick-create-portal.md).

En este artículo se usan los valores de ejemplo siguientes:

* **Grupo de recursos:** rg-azure-aws
* **Región:** Este de EE. UU.
* **Nombre de la red virtual**: vnet-azure
* **Espacio de direcciones IPv4:** 172.10.0.0/16
* **Nombre de subred:** subnet-01
* **Intervalo de direcciones de subred:** 172.10.1.0/24

### <a name="create-a-vpn-gateway"></a>Creación de una puerta de enlace de VPN

Cree una puerta de enlace de VPN para la red virtual. Para obtener instrucciones, consulte: [Tutorial: Creación y administración de una puerta de enlace de VPN](tutorial-create-gateway-portal.md).

En este artículo se usan los valores y configuraciones de ejemplo siguientes:

* **Nombre de puerta de enlace:** vpn-azure-aws
* **Región:** Este de EE. UU.
* **Tipo de puerta de enlace:** VPN
* **Tipo de VPN:** basada en rutas
* **SKU:** VpnGw1
* **Generación:** Generación 1
* **Red virtual:** Debe ser la red virtual para la que quiere crear la puerta de enlace.
* **Intervalo de direcciones de subred de puerta de enlace:** 172.10.0.0/27
* **Dirección IP pública**: Crear nuevo
* **Nombre de dirección IP pública:** pip-vpn-azure-aws
* **Habilitación del modo activo-activo:** Disable
* **Configuración de BGP:** Disable

Ejemplo:

:::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/summary.png" alt-text="Resumen de puerta de enlace de red virtual":::

## <a name="configure-aws"></a>Configuración de AWS

1. Cree la Virtual Private Cloud (VPC).

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-vpc.png" alt-text="Información de creación de la VPC":::

1. Cree una subred dentro de la VPC (red virtual).

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-subnet-vpc.png" alt-text="Creación de la subred":::

1. Cree una puerta de enlace de cliente que apunte a la dirección IP pública de Azure VPN Gateway. La **Puerta de enlace del cliente** es un recurso de AWS que contiene información para AWS sobre el dispositivo de puerta de enlace del cliente que, en este caso, es Azure VPN Gateway.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-customer-gw.png" alt-text="Creación de la puerta de enlace de cliente":::

1. Cree la puerta de enlace privada virtual.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-vpg.png" alt-text="Creación de la puerta de enlace privada virtual":::

1. Conecte la puerta de enlace privada virtual a la VPC.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attach-vpg.png" alt-text="Conexión de la puerta de enlace privada virtual a la VPC":::

1. Seleccione la VPC.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attaching-vpg.png" alt-text="Adjuntar":::

1. Cree una conexión VPN de sitio a sitio.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-vpn-connection.png" alt-text="Creación de la conexiones VPN":::

1. Establezca la opción de enrutamiento en **Estático** y apunte al prefijo subnet-01 de Azure **(172.10.1.0/24)** .

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/set-static-route.png" alt-text="Definición de una ruta estática":::

1. Después de rellenar las opciones, **cree** la conexión.

1. Descargue el archivo de configuración. Para descargar la configuración correcta, cambie el proveedor, la plataforma y el software a **Genérico**, ya que Azure no es una opción válida.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/download-config.png" alt-text="Descarga de la configuración":::

1. El archivo de configuración contiene la clave precompartida y la dirección IP pública para cada uno de los dos túneles IPsec creados por AWS.

   **Túnel 1**

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-1.png" alt-text="Túnel 1":::

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-1-config.png" alt-text="Configuración del túnel 1":::

   **Túnel 2**

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-2.png" alt-text="Túnel 2":::

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-2-config.png" alt-text="Configuración del túnel 2":::

1. Una vez creados los túneles, verá algo parecido a este ejemplo.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-connection-details.png" alt-text="Detalles de conexión de VPN de AWS":::

## <a name="create-local-network-gateway"></a>Crear puerta de enlace de red local

En Azure, la puerta de enlace de red local es un recurso de Azure que normalmente representa una ubicación local. Se rellena con la información usada para conectarse al dispositivo VPN local. Sin embargo, en esta configuración, la puerta de enlace de red local se crea y rellena con la información de conexión de la puerta de enlace privada virtual de AWS. Para obtener más información sobre las puertas de enlace de red local de Azure, consulte [Configuración de Azure VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#lng).

Cree una puerta de enlace de red local en Azure. Para conocer los pasos, consulte [Creación de una puerta de enlace de la red local](tutorial-site-to-site-portal.md#LocalNetworkGateway).

Especifique los siguientes valores:

* **Nombre:** En el ejemplo, se usa lng-azure-aws.
* **Punto de conexión:** Dirección IP
* **Dirección IP**: La dirección IP pública de la puerta de enlace privada virtual de AWS y el prefijo CIDR de VPC. Puede encontrar la dirección IP pública en el archivo de configuración que descargó anteriormente.

AWS crea dos túneles IPsec con fines de alta disponibilidad. En el siguiente ejemplo se muestra la dirección IP pública del túnel 1 de IPsec.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/local-network-gateway.png" alt-text="Puerta de enlace de red local":::

## <a name="create-vpn-connection"></a>Creación de la conexiones VPN

En esta sección, creará la conexión VPN entre la puerta de enlace de red virtual de Azure y la puerta de enlace de AWS.

1. Cree la conexión a Azure. Para conocer los pasos para crear la conexión, consulte [Creación de una conexión VPN](tutorial-site-to-site-portal.md#CreateConnection).

   En el ejemplo siguiente, la clave compartida se obtuvo del archivo de configuración que descargó anteriormente. En este ejemplo, usamos los valores del túnel 1 de IPsec creados por AWS y descritos en el archivo de configuración.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-connection.png" alt-text="Objeto de conexión de Azure":::

1. Vea la conexión. Después de unos minutos, se establece la conexión.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/connection-established.png" alt-text="Conexión en funcionamiento":::

1. Compruebe que el túnel 1 de IPsec en AWS esté establecido en **UP** (Activo).

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-connection-established.png" alt-text="Comprobación de que el túnel de AWS está en UP":::

1. Edite la tabla de enrutamiento asociada a la VPC.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/edit-aws-route.png" alt-text="Edición de la ruta":::

1. Agregue la ruta a la subred de Azure. Esta ruta se desplazará a través de la puerta de enlace de VPC.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/save-aws-route.png" alt-text="Almacenamiento de la configuración de enrutamiento":::

## <a name="configure-second-connection"></a>Configuración de la segunda conexión

En esta sección, creará una segunda conexión para garantizar la alta disponibilidad.

1. Cree otra puerta de enlace de red local que apunte a la dirección IP pública del túnel 2 de IPsec en AWS. Esta es la puerta de enlace en espera.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-lng-standby.png" alt-text="Creación de la puerta de enlace de red local":::

1. Cree la segunda conexión VPN de Azure a AWS.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-connection-standby.png" alt-text="Creación de la conexión de la puerta de enlace de red local en espera":::

1. Vea las conexiones de Azure VPN Gateway.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-tunnels.png" alt-text="Estado de la conexión de Azure":::

1. Vea las conexiones de AWS. En este ejemplo, puede ver que ya se han establecido las conexiones.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-tunnels.png" alt-text="Estado de la conexión de AWS":::

## <a name="to-test-connections"></a>Para probar las conexiones

1. Agregue una **puerta de enlace a Internet** a la VPC en AWS. La puerta de enlace a Internet es una conexión lógica entre una VPN de Amazon e Internet. Este recurso le permite conectarse a través de la VM de prueba desde la dirección IP pública de AWS a través de Internet. Este recurso no es necesario para la conexión VPN. Solo se usa para probar.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-igw.png" alt-text="Creación de la puerta de enlace a Internet":::

1. Seleccione **Attach to VPC** (Adjuntar a la VPC).

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attach-igw.png" alt-text="Conexión de la puerta de enlace a Internet a la VPC":::

1. Seleccione una VPC y **Attach Internet gateway** (Adjuntar puerta de enlace a Internet).

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attach-igw-2.png" alt-text="Conexión de la puerta de enlace":::

1. Cree una ruta para permitir conexiones a **0.0.0.0/0** (Internet) a través de la puerta de enlace a Internet.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/allow-internet-igw.png" alt-text="Configuración de la ruta a través de la puerta de enlace":::

1. En Azure, la ruta se crea automáticamente. Para comprobar la ruta desde la VM de Azure, puede seleccionar **VM > Redes > Interfaz de red > Rutas eficaces**. Verá dos rutas, una ruta por conexión.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-effective-routes.png" alt-text="Comprobación de las rutas eficaces":::

1. Puede realizar esta comprobación desde una máquina virtual Linux en Azure. El resultado tendrá un aspecto similar al siguiente ejemplo.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-overview.png" alt-text="Información general de Azure desde una VM Linux":::

1. También puede hacer la comprobación desde una máquina virtual Linux en AWS. El resultado tendrá un aspecto similar al siguiente ejemplo.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-overview.png" alt-text="Información general de AWS desde una VM Linux":::

1. Pruebe la conectividad desde la VM de Azure.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-ping.png" alt-text="Prueba de ping desde Azure":::

1. Pruebe la conectividad desde la VM de AWS.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-ping.png" alt-text="Prueba de ping desde AWS":::

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la compatibilidad de AWS con IKEv2, consulte el [artículo de AWS](https://aws.amazon.com/about-aws/whats-new/2019/02/aws-site-to-site-vpn-now-supports-ikev2/).

* Para más información sobre cómo crear una VPN multinube a escala, consulte el vídeo [Creación de la mejor VPN multinube a escala](https://www.youtube.com/watch?v=p7h-frLDFE0).
