---
title: 'Tutorial: Protección de la red virtual de centro mediante Azure Firewall Manager'
description: En este tutorial aprenderá a proteger la red virtual con Azure Firewall Manager mediante Azure Portal.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: tutorial
ms.date: 03/03/2021
ms.author: victorh
ms.openlocfilehash: f631100003a4ea6e191a5bdc13a11eb9aa327268
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102212555"
---
# <a name="tutorial-secure-your-hub-virtual-network-using-azure-firewall-manager"></a>Tutorial: Protección de la red virtual de centro mediante Azure Firewall Manager

Cuando conecta la red local a una red virtual de Azure para crear una red híbrida, la capacidad de controlar el acceso a los recursos de la red de Azure es parte importante de un plan de seguridad global.

Mediante Azure Firewall Manager puede crear una red virtual de centro para proteger el tráfico de la red híbrida destinado a direcciones IP privadas, PaaS de Azure e Internet. Puede usar Azure Firewall Manager para controlar el acceso de red en una red híbrida con directivas que definan el tráfico de red permitido y denegado.

Firewall Manager también admite una arquitectura de centro virtual protegido. Para ver una comparación entre los tipos de arquitectura de red virtual de centro y centro virtual protegido, consulte [¿Cuáles son las opciones de arquitectura de Azure Firewall Manager?](vhubs-and-vnets.md)

En este tutorial se crearán tres redes virtuales:

- **VNet-Hub**: el firewall está en esta red virtual.
- **VNet-Spoke**: la red virtual Spoke representa la carga de trabajo ubicada en Azure.
- **VNet-Onprem**: la red virtual local representa una red local. En una implementación real, se puede conectar mediante una conexión VPN o ExpressRoute. Para simplificar, este tutorial usa una conexión de puerta de enlace de VPN y una red virtual ubicada en Azure para representar una red local.

![Red híbrida](media/tutorial-hybrid-portal/hybrid-network-firewall.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una directiva de firewall
> * Crear las redes virtuales
> * Configuración e implementación del firewall
> * Creación y conexión de las puertas de enlace de VPN
> * Emparejar las redes virtuales de tipo hub-and-spoke
> * Creación de las rutas
> * Creación de las máquinas virtuales
> * Probar el firewall


## <a name="prerequisites"></a>Requisitos previos

Una red híbrida usa el modelo de arquitectura radial para enrutar el tráfico entre redes virtuales de Azure y redes locales. La arquitectura radial tiene los siguientes requisitos:

- Establezca **AllowGatewayTransit** al emparejar VNet-Hub con VNet-Spoke. En una arquitectura de red radial, el tránsito de una puerta de enlace permite que las redes virtuales de radio compartan la puerta de enlace de VPN en el centro, en lugar de implementar puertas de enlace de VPN en todas las redes virtuales de radio. 

   Además, las rutas a las redes virtuales conectadas a la puerta de enlace o a las redes locales se propagarán automáticamente a las tablas de enrutamiento de las redes virtuales emparejadas mediante el tránsito de la puerta de enlace. Para más información, consulte [Configuración del tránsito de la puerta de enlace de VPN para el emparejamiento de red virtual](../vpn-gateway/vpn-gateway-peering-gateway-transit.md).

- Establezca **UseRemoteGateways** al emparejar VNet-Spoke con VNet-Hub. Si se establece **UseRemoteGateways** y también se establece **AllowGatewayTransit** en emparejamiento remoto, la red virtual de radio usa puertas de enlace de la red virtual remota para el tránsito.
- Para enrutar el tráfico de la subred de radio a través del firewall de centro, necesita una ruta definida por el usuario (UDR) que apunte al firewall con la opción **Virtual network gateway route propagation** (Deshabilitar la propagación de rutas de la puerta de enlace de red virtual) deshabilitada. Esta opción evita la distribución de rutas a las subredes de radio. Esto evita que las rutas aprendidas entren en conflicto con la UDR.
- Configure una ruta definida por el usuario en la subred de la puerta de enlace del centro que apunte a la dirección IP del firewall como próximo salto para las redes de radio. No se requiere ninguna ruta definida por el usuario en la subred de Azure Firewall, ya que obtiene las rutas de BGP.

Consulte la sección [Creación de rutas](#create-the-routes) en este tutorial para ver cómo se crean estas rutas.

>[!NOTE]
>Azure Firewall debe tener conectividad directa a Internet. Si AzureFirewallSubnet aprende una ruta predeterminada a la red local mediante BGP, debe reemplazarla por una UDR 0.0.0.0/0 con el valor **NextHopType** establecido como **Internet** para mantener la conectividad directa a Internet.
>
>Azure Firewall puede configurarse para admitir la tunelización forzada. Para más información, consulte [Tunelización forzada de Azure Firewall](../firewall/forced-tunneling.md).

>[!NOTE]
>El tráfico entre redes virtuales emparejadas directamente se enruta directamente aunque una ruta definida por el usuario apunte a Azure Firewall como puerta de enlace predeterminada. Para enviar tráfico de subred a subred al firewall en este escenario, una UDR debe contener el prefijo de red de la subred de destino de forma explícita en ambas subredes.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-firewall-policy"></a>Creación de una directiva de firewall

1. Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).
2. En la barra de búsqueda de Azure Portal, escriba **Firewall Manager** y presione **Entrar**.
3. En la página de Azure Firewall Manager, seleccione **Ver directivas de Azure Firewall**.

   ![Directiva de firewall](media/tutorial-hybrid-portal/firewall-manager-policy.png)

1. Seleccione **Crear una directiva de Azure Firewall**.
1. Seleccione su suscripción y, como grupo de recursos, elija **Crear nuevo** y cree un grupo de recursos llamado **FW-Hybrid-Test**.
2. Como nombre de la directiva, escriba **Pol-Net01**.
3. Como región, seleccione **Este de EE. UU**.
1. Seleccione **Siguiente: Configuración DNS**.
1. Seleccione **Siguiente: Inspección de TLS (versión preliminar)** .
1. Seleccione **Siguiente: Reglas**.
1. Seleccione **Agregar una colección de reglas**.
1. En **Nombre**, escriba **RCNet01**.
1. En **Tipo de colección de reglas**, seleccione **Red**.
1. En **Prioridad**, escriba **100**.
1. En **Acción**, seleccione **Permitir**.
1. En **Reglas**, como **Nombre**, escriba **AllowWeb**.
1. Como **Origen**, escriba **192.168.1.0/24**.
1. En **Protocolo**, seleccione **TCP**.
1. En **Puertos de destino**, escriba **80**.
1. En **Tipo de destino**, seleccione **Dirección IP**.
1. En **Destino**, escriba **10.6.0.0/16**.
1. En la siguiente fila de la regla, escriba la siguiente información:
 
    En Nombre, escriba **AllowRDP**.<br>
    En Origen, escriba **192.168.1.0/24**.<br>
    En Protocolo, seleccione **TCP**.<br>
    En Puertos de destino, escriba **3389**.<br>
    En Tipo de destino, seleccione **Dirección IP**.<br>
    En Destino, escriba **10.6.0.0/16**.

1. Seleccione **Agregar**.
2. Seleccione **Revisar + crear**.
3. Revise los valores y, luego, seleccione **Crear**.

## <a name="create-the-firewall-hub-virtual-network"></a>Crear la red virtual del centro de firewall

> [!NOTE]
> El tamaño de la subred AzureFirewallSubnet es /26. Para más información sobre el tamaño de la subred, consulte [Preguntas más frecuentes sobre Azure Firewall](../firewall/firewall-faq.yml#why-does-azure-firewall-need-a--26-subnet-size).

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. Busque **Red virtual** y, después, seleccione **Red virtual**.
1. Seleccione **Crear**.
1. En **Suscripción**, seleccione la suscripción.
1. En **Grupo de recursos**, seleccione **FW-Hybrid-Test**.
1. En **Nombre**, escriba **Vnet-Hub**.
1. En **Región**, seleccione **Este de EE. UU.** .
1. Seleccione **Siguiente: Direcciones IP**.

1. En **Espacio de direcciones IPv4**, escriba **10.5.0.0/16**.
1. En **Nombre de subred**, seleccione **predeterminada**.
1.  Cambie el valor de **Nombre de subred** a **AzureFirewallSubnet**. El firewall esté en esta subred y el nombre de la subred **debe** ser AzureFirewallSubnet.
1. En **Intervalo de direcciones de subred**, escriba **10.5.0.0/26**. 
1. Acepte los restantes valores predeterminados y seleccione **Guardar**.
1. Seleccione **Revisar + crear**.
1. Seleccione **Crear**.

## <a name="create-the-spoke-virtual-network"></a>Crear la red virtual de tipo hub-and-spoke

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. Busque **Red virtual** y, después, seleccione **Red virtual**.
1. Seleccione **Crear**.
1. En **Suscripción**, seleccione la suscripción.
1. En **Grupo de recursos**, seleccione **FW-Hybrid-Test**.
1. En **Nombre**, escriba **VNet-Spoke**.
1. En **Región**, seleccione **Este de EE. UU.** .
1. Seleccione **Siguiente: Direcciones IP**.

1. En **Espacio de direcciones IPv4**, escriba **10.6.0.0/16**.
1. En **Nombre de subred**, seleccione **predeterminada**.
1. Cambie el valor de **Nombre de subred** a **SN-Workload**.
1. En **Intervalo de direcciones de subred**, escriba **10.6.0.0/24**. 
1. Acepte los restantes valores predeterminados y seleccione **Guardar**.
1. Seleccione **Revisar + crear**.
1. Seleccione **Crear**.


## <a name="create-the-on-premises-virtual-network"></a>Crear la red virtual local

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. Busque **Red virtual** y, después, seleccione **Red virtual**.
1. Seleccione **Crear**.
1. En **Suscripción**, seleccione la suscripción.
1. En **Grupo de recursos**, seleccione **FW-Hybrid-Test**.
1. En **Nombre**, escriba **VNet-OnPrem**.
1. En **Región**, seleccione **Este de EE. UU.** .
1. Seleccione **Siguiente: Direcciones IP**.

1. En **Espacio de direcciones IPv4**, escriba **192.168.0.0/16**.
1. En **Nombre de subred**, seleccione **predeterminada**.
1. Cambie el valor de **Nombre de subred** a **SN-Corp**.
1. En **Intervalo de direcciones de subred**, escriba **192.168.1.0/24**. 
1. Acepte los restantes valores predeterminados y seleccione **Guardar**.
2. Haga clic en **Agregar subred**.
3. En **Nombre de subred**, escriba **GatewaySubnet**.
4. En **Intervalo de direcciones de subred**, escriba **192.168.2.0/24**.
5. Seleccione **Agregar**.
1. Seleccione **Revisar y crear**.
1. Seleccione **Crear**.




## <a name="configure-and-deploy-the-firewall"></a>Configuración e implementación del firewall

Cuando las directivas de seguridad están asociadas a un centro de conectividad, se hace referencia a ellas como *red virtual de centro de conectividad*.

Convierta la red virtual **VNet-Hub** en una *red virtual de centro* y protéjala con Azure Firewall.

1. En la barra de búsqueda de Azure Portal, escriba **Firewall Manager** y presione **Entrar**.
3. En la página de Azure Firewall Manager, en **Add security to virtual networks** (Agregar seguridad a redes virtuales), seleccione **View hub virtual networks** (Ver redes virtuales de centro).
1. En **Redes virtuales**, active la casilla **VNet-hub** (Centro de red virtual).
1. Seleccione **Manage Security** (Administrar seguridad) y, después, seleccione **Deploy a Firewall with Firewall Policy** (Implementar un firewall con directiva de firewall).
1. En la página **Convertir redes virtuales**, en **Directiva de firewall**, active la casilla de **Pol-Net01**.
1. Seleccione **Siguiente: Revisar y confirmar**.
1. Revise los detalles y, luego, seleccione **Confirmar**.


   Esto tarda unos minutos en implementarse.
7. Una vez finalizada la implementación, vaya al grupo de recursos **FW-Hybrid-Test** y seleccione el firewall.
9. Anote la dirección **Firewall private IP** (IP privada de firewall) de la página **Información general**. Se usará más adelante al crear la ruta predeterminada.

## <a name="create-and-connect-the-vpn-gateways"></a>Creación y conexión de las puertas de enlace de VPN

Las redes virtuales de centro y local están conectadas mediante puertas de enlace VPN.

### <a name="create-a-vpn-gateway-for-the-hub-virtual-network"></a>Creación de una puerta de enlace VPN para la red virtual de centro

Cree la puerta de enlace VPN para la red virtual de centro. Las configuraciones de red a red requieren un VpnType RouteBased. La creación de una puerta de enlace de VPN suele tardar 45 minutos o más, según la SKU de la puerta de enlace de VPN seleccionada.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de texto de búsqueda, escriba **puerta de enlace de red virtual** y presione **Entrar**.
3. Seleccione **Puerta de enlace de red virtual** y, después, **Crear**.
4. En **Nombre**, escriba **GW-hub**.
5. En **Región**, seleccione **(EE. UU.) Este de EE. UU.** .
6. En **Tipo de puerta de enlace** seleccione **VPN**.
7. En **Tipo de VPN** seleccione **Basada en rutas**.
8. En **SKU**, seleccione **Básico**.
9. En **Red virtual**, seleccione **VNet-hub**.
10. En **Dirección IP pública**, seleccione **Crear nuevo** y escriba **VNet-hub-GW-pip** como nombre.
11. Acepte el resto de valores predeterminados y seleccione **Revisar y crear**.
12. Revise la configuración y, después, seleccione **Crear**.

### <a name="create-a-vpn-gateway-for-the-on-premises-virtual-network"></a>Creación de una puerta de enlace VPN para la red virtual local

Cree la puerta de enlace VPN para la red virtual local. Las configuraciones de red a red requieren un VpnType RouteBased. La creación de una puerta de enlace de VPN suele tardar 45 minutos o más, según la SKU de la puerta de enlace de VPN seleccionada.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de texto de búsqueda, escriba **puerta de enlace de red virtual** y presione **Entrar**.
3. Seleccione **Puerta de enlace de red virtual** y, después, **Crear**.
4. En **Nombre**, escriba **GW-Onprem**.
5. En **Región**, seleccione **(EE. UU.) Este de EE. UU.** .
6. En **Tipo de puerta de enlace** seleccione **VPN**.
7. En **Tipo de VPN** seleccione **Basada en rutas**.
8. En **SKU**, seleccione **Básico**.
9. En **Red virtual**, selecciona **VNet-Onprem**.
10. En **Dirección IP pública**, seleccione **Crear nuevo** y escriba **VNet-Onprem-GW-pip** como nombre.
11. Acepte el resto de valores predeterminados y seleccione **Revisar y crear**.
12. Revise la configuración y, después, seleccione **Crear**.

### <a name="create-the-vpn-connections"></a>Creación de las conexiones

Ahora puede crear las conexiones de VPN entre las puertas de enlace de centro y local.

En este paso va a crear la conexión entre la red virtual de centro y la red virtual local. Verá una clave compartida a la que se hace referencia en los ejemplos. Puede utilizar sus propios valores para la clave compartida. Lo importante es que la clave compartida coincida en ambas conexiones. La conexión tarda un tiempo en crearse.

1. Abra el grupo de recursos **FW-Hybrid-Test** y seleccione la puerta de enlace **GW-hub**.
2. Seleccione **Conexiones** en la columna izquierda.
3. Seleccione **Agregar**.
4. En el nombre de la conexión, escriba **Hub-to-Onprem**.
5. En **Tipo de conexión**, seleccione **De VNet a VNet**.
6. En **Segunda puerta enlace de red virtual**, seleccione **GW-Onprem**.
7. En **Clave compartida (PSK)** , escriba **AzureA1b2C3**.
8. Seleccione **Aceptar**.

Cree la conexión de red entre la red virtual local y la red virtual de centro. Este paso es similar al anterior, excepto que se crea una conexión desde la red virtual local a la red virtual de centro. Asegúrese de que coincidan las claves compartidas. Después de unos minutos, se habrá establecido la conexión.

1. Abra el grupo de recursos **FW-Hybrid-Test** y seleccione la puerta de enlace **GW-Onprem**.
2. Seleccione **Conexiones** en la columna izquierda.
3. Seleccione **Agregar**.
4. En el nombre de la conexión, escriba **Onprem-to-Hub**.
5. En **Tipo de conexión**, seleccione **De VNet a VNet**.
6. En **Segunda puerta enlace de red virtual**, seleccione **GW-hub**.
7. En **Clave compartida (PSK)** , escriba **AzureA1b2C3**.
8. Seleccione **Aceptar**.


#### <a name="verify-the-connection"></a>Comprobación de la conexión

Después de unos cinco minutos o más, el estado de ambas conexiones debe ser **conectado**.

![Conexiones de puerta de enlace](media/secure-hybrid-network/gateway-connections.png)

## <a name="peer-the-hub-and-spoke-virtual-networks"></a>Emparejar las redes virtuales de tipo hub-and-spoke

Ahora, empareje las redes virtuales de tipo hub-and-spoke.

1. Abra el grupo de recursos **FW-Hybrid-Test** y seleccione la red virtual **VNet-hub**.
2. En la columna izquierda, seleccione **Emparejamientos**.
3. Seleccione **Agregar**.
4. En **Esta red virtual**:
 
   
   |Nombre del valor  |Value  |
   |---------|---------|
   |Nombre del vínculo de emparejamiento| HubtoSpoke|
   |Tráfico hacia la red virtual remota|   Permitir (predeterminado)      |
   |Tráfico reenviado desde la red virtual remota    |   Permitir (predeterminado)      |
   |Servidor de ruta o puerta de enlace de la red virtual    |  Usar la puerta de enlace de esta red virtual       |
    
5. En **Red virtual remota**:

   |Nombre del valor  |Value  |
   |---------|---------|
   |Nombre del vínculo de emparejamiento | SpoketoHub|
   |Modelo de implementación de red virtual| Resource Manager|
   |Suscripción|\<your subscription\>|
   |Virtual network| VNet-Spoke
   |Tráfico hacia la red virtual remota     |   Permitir (predeterminado)      |
   |Tráfico reenviado desde la red virtual remota    |   Permitir (predeterminado)      |
   |Puerta de enlace de red virtual     |  Usar la puerta de enlace de la red virtual remota       |

5. Seleccione **Agregar**.

   :::image type="content" source="media/secure-hybrid-network/firewall-peering.png" alt-text="Emparejamiento de Vnet":::

## <a name="create-the-routes"></a>Creación de las rutas

A continuación, cree un par de rutas:

- Una ruta desde la subred de puerta de enlace de concentrador a la subred de radio mediante la dirección IP del firewall.
- Una ruta predeterminada desde la subred de radio mediante la dirección IP del firewall.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de texto de búsqueda, escriba **tabla de rutas** y presione **Entrar**.
3. Seleccione **Tabla de rutas**.
4. Seleccione **Crear**.
1. Seleccione **FW-Hybrid-Test** en grupo de recursos.
1. En **Región**, seleccione **Este de EE. UU.** .
1. En el nombre, escriba **UDR-Hub-Spoke**.
1. Seleccione **Revisar + crear**.
1. Seleccione **Crear**.
1. Una vez creada la tabla de rutas, selecciónela para abrir la página de la tabla de rutas.
1. Seleccione **Rutas** en la columna izquierda.
1. Seleccione **Agregar**.
1. En el nombre de ruta, escriba **ToSpoke**.
1. En el prefijo de dirección, escriba **10.6.0.0/16**.
1. En el tipo del próximo salto, seleccione **Aplicación virtual**.
1. En la dirección del próximo salto, escriba la dirección IP privada del firewall que anotó anteriormente.
1. Seleccione **Aceptar**.

Ahora asocie la ruta a la subred.

1. En la página **UDR-Hub-Spoke - Routes**, seleccione **Subredes**.
2. Seleccione **Asociar**.
4. En **Red virtual**, seleccione **VNet-hub**.
5. En **Subred**, seleccione **GatewaySubnet**.
6. Seleccione **Aceptar**.

Ahora, cree la ruta predeterminada desde la subred radial.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de texto de búsqueda, escriba **tabla de rutas** y presione **Entrar**.
3. Seleccione **Tabla de rutas**.
5. Seleccione **Crear**.
7. Seleccione **FW-Hybrid-Test** en grupo de recursos.
8. En **Región**, seleccione **Este de EE. UU.** .
1. En el nombre, escriba **UDR-DG**.
4. En **Propagate gateway routes** (Propagar rutas de puerta de enlace), seleccione **No**.
1. Seleccione **Revisar + crear**.
1. Seleccione **Crear**.
1. Una vez creada la tabla de rutas, selecciónela para abrir la página de la tabla de rutas.
1. Seleccione **Rutas** en la columna izquierda.
1. Seleccione **Agregar**.
1. En el nombre de ruta, escriba **ToHub**.
1. En el prefijo de dirección, escriba **0.0.0.0/0**.
1. En el tipo del próximo salto, seleccione **Aplicación virtual**.
1. En la dirección del próximo salto, escriba la dirección IP privada del firewall que anotó anteriormente.
1. Seleccione **Aceptar**.

Ahora asocie la ruta a la subred.

1. En la página **UDR-DG - Routes**, seleccione **Subredes**.
2. Seleccione **Asociar**.
4. En **Red virtual**, seleccione **VNet-spoke**.
5. En **Subred**, seleccione **SN-Workload**.
6. Seleccione **Aceptar**.

## <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Ahora, cree la carga de trabajo de tipo hub-and-spoke y las máquinas virtuales locales, y colóquelas en las subredes adecuadas.

### <a name="create-the-workload-virtual-machine"></a>Creación de la máquina virtual de cargas de trabajo

Cree una máquina virtual en la red virtual radial, que ejecute IIS, sin ninguna dirección IP pública.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En la página **Popular**, seleccione **Windows Server 2016 Datacenter**.
3. Especifique estos valores para la máquina virtual:
    - **Grupo de recursos**: seleccione **FW-Hybrid-Test**.
    - **Nombre de máquina virtual**: *VM-Spoke-01*.
    - **Región** -  *(EE. UU.) Este de EE. UU.*
    - **Nombre de usuario**: escriba un nombre de usuario.
    - **Contraseña**: escriba una contraseña.

4. Seleccione **Siguiente: Discos**.
5. Acepte los valores predeterminados y seleccione **Siguiente: Redes**.
6. Seleccione **VNet-Spoke** para la red virtual y la subred es **SN-Workload**.
8. En **Puertos de entrada públicos**, seleccione **Permitir los puertos seleccionados** y, después, seleccione **HTTP (80)** y **RDP (3389)** .
1. Seleccione **Siguiente: Administración**.
1. En **Diagnósticos de arranque**, seleccione **Deshabilitado**.
1. Seleccione **Revisar y crear**, revise la configuración en la página de resumen y seleccione **Crear**.

### <a name="install-iis"></a>Instalación de IIS

1. En Azure Portal, abra Cloud Shell y asegúrese de que está establecido en **PowerShell**.
2. Ejecute el siguiente comando para instalar IIS en la máquina virtual; cambie la ubicación si es necesario:

   ```azurepowershell-interactive
   Set-AzVMExtension `
           -ResourceGroupName FW-Hybrid-Test `
           -ExtensionName IIS `
           -VMName VM-Spoke-01 `
           -Publisher Microsoft.Compute `
           -ExtensionType CustomScriptExtension `
           -TypeHandlerVersion 1.4 `
           -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell      Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
           -Location EastUS
   ```

### <a name="create-the-on-premises-virtual-machine"></a>Creación de la máquina virtual local

Se trata de una máquina virtual con una dirección IP pública a la que se puede conectar mediante Escritorio remoto. Desde allí, puede conectarse al servidor local a través del firewall.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En la página **Popular**, seleccione **Windows Server 2016 Datacenter**.
3. Especifique estos valores para la máquina virtual:
    - **Grupo de recursos**: seleccione el existente y, después, **FW-Hybrid-Test**.
    - **Nombre de máquina virtual** - *VM-Onprem*.
    - **Región** -  *(EE. UU.) Este de EE. UU.*
    - **Nombre de usuario**: escriba un nombre de usuario.
    - En **Contraseña**, escriba su contraseña.

4. Seleccione **Siguiente: Discos**.
5. Acepte los valores predeterminados y seleccione **Siguiente: Redes**.
6. Seleccione **VNet-Onprem** como red virtual y **SN-Corp** como subred.
7. En **Puertos de entrada públicos**, seleccione **Permitir los puertos seleccionados** y, después, seleccione **RDP (3389)** .
8. Seleccione **Siguiente: Administración**.
9. En **Diagnósticos de arranque**, seleccione **Deshabilitar**.
10. Seleccione **Revisar y crear**, revise la configuración en la página de resumen y seleccione **Crear**.

## <a name="test-the-firewall"></a>Probar el firewall

1. En primer lugar, anote la dirección IP privada de la máquina virtual VM-Spoke-01 que aparece en la página de información general de esta máquina virtual.

2. En Azure Portal, conéctese a la máquina virtual **VM-Onprem**.
<!---2. Open a Windows PowerShell command prompt on **VM-Onprem**, and ping the private IP for **VM-spoke-01**.

   You should get a reply.--->
3. Abra un explorador web en **VM-Onprem** y vaya a http://\<VM-spoke-01 private IP\>.

   Debería ver la página web de **VM-spoke-01**: ![Página web VM-Spoke-01](media/secure-hybrid-network/vm-spoke-01-web.png)

4. En la máquina virtual **VM-Onprem**, abra un escritorio remoto a **VM-spoke-01** en la dirección IP privada.

   Se realizará la conexión y podrá iniciar sesión.

Con ello, ha comprobado que las reglas de firewall funcionan:

<!---- You can ping the server on the spoke VNet.--->
- Puede examinar el servidor web en la red virtual de tipo hub-and-spoke.
- Puede conectarse al servidor en la red virtual de tipo hub-and-spoke mediante RDP.

A continuación, cambie la acción de recopilación de la regla de red del firewall a **Deny** (Denegar) para comprobar que las reglas del firewall funcionan según lo previsto.

1. Abra el grupo de recursos **FW-Hybrid-Test** y seleccione la directiva de firewall **Pol-Net01**.
2. En **Configuración**, seleccione **Rule Collections** (colecciones de reglas).
1. Seleccione la colección de reglas **RCNet01**.
1. En **Acción de colección de reglas**, seleccione **Denegar**.
1. Seleccione **Guardar**.

Antes de probar las reglas modificadas, cierre los escritorios remotos y exploradores existentes en **VM-Onprem**. Una vez finalizada la actualización de la colección de reglas, vuelva a ejecutar las pruebas. Esta vez, todas deberían mostrar un error al conectarse.

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede conservar los recursos relacionados con el firewall para investigarlos más adelante o, si ya no los necesita, eliminar el grupo de recursos **FW-Hybrid-Test** para eliminarlos todos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Protección de una red WAN virtual mediante Azure Firewall Manager](secure-cloud-network.md)