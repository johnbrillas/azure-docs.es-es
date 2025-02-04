---
title: Private Link para el servidor único de Azure Database for PostgreSQL con Azure Portal
description: 'Obtenga información sobre cómo configurar Private Link para Azure Database for PostgreSQL: servidor único de Azure Portal'
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 01/09/2020
ms.openlocfilehash: 23b9774f28f915596abafbd8c1f6c38fc4f55748
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "95995291"
---
# <a name="create-and-manage-private-link-for-azure-database-for-postgresql---single-server-using-portal"></a>Creación y administración de Private Link para el servidor único de Azure Database for PostgreSQL con el portal

Un punto de conexión privado es el bloque de creación fundamental para el vínculo privado en Azure. Permite que los recursos de Azure, como las máquinas virtuales, se comuniquen de manera privada con recursos de vínculos privados.  En este artículo se muestra cómo usar Azure Portal para crear una máquina virtual en Azure Virtual Network y un servidor único de Azure Database for PostgreSQL con un punto de conexión privado de Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

> [!NOTE]
> La característica de vínculo privado solo está disponible para servidores de Azure Database for PostgreSQL en los planes de tarifa De uso general u Optimizado para memoria. Asegúrese de que el servidor de bases de datos esté incluido en uno de estos planes de tarifa.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-vm"></a>Creación de una máquina virtual de Azure

En esta sección, va a crear una red virtual y una subred para hospedar la máquina virtual que se usa para acceder al recurso de Private Link (un servidor PostgreSQL en Azure).

### <a name="create-the-virtual-network"></a>Crear la red virtual
En esta sección, va a crear una red virtual y una subred para hospedar la máquina virtual que se usa para acceder al recurso de Private Link.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Red virtual**.
2. En **Creación de una red virtual**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba *MyVirtualNetwork*. |
    | Espacio de direcciones | Escriba *10.1.0.0/16*. |
    | Suscripción | Seleccione su suscripción.|
    | Resource group | Seleccione **Crear nuevo**, escriba *myResourceGroup* y, después, seleccione **Aceptar**. |
    | Location | Seleccione **Oeste de Europa**.|
    | Subred: nombre | Escriba *mySubnet*. |
    | Subred: intervalo de direcciones | Escriba *10.1.0.0/24*. |
    |||
3. Deje el resto tal como está y seleccione **Crear**.

### <a name="create-virtual-machine"></a>Creación de la máquina virtual

1. En la parte superior izquierda de Azure Portal, seleccione **Crear un recurso** > **Proceso** > **Máquina virtual**.

2. En **Creación de una máquina virtual: conceptos básicos**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | **DETALLES DEL PROYECTO** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Lo creó en la sección anterior.  |
    | **DETALLES DE INSTANCIA** |  |
    | Nombre de la máquina virtual | Escriba *myVm*. |
    | Region | Seleccione **Oeste de Europa**. |
    | Opciones de disponibilidad | Deje el valor predeterminado **No se requiere redundancia de la infraestructura**. |
    | Imagen | Seleccione **Windows Server 2019 Datacenter**. |
    | Size | Deje el valor predeterminado **Estándar DS1 v2**. |
    | **CUENTA DE ADMINISTRADOR** |  |
    | Nombre de usuario | Escriba un nombre de usuario de su elección. |
    | Contraseña | Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Confirm Password | Vuelva a escribir la contraseña. |
    | **REGLAS DE PUERTO DE ENTRADA** |  |
    | Puertos de entrada públicos | Deje el valor predeterminado **Ninguno**. |
    | **AHORRE DINERO** |  |
    | ¿Ya tiene una licencia de Windows? | Deje el valor predeterminado **No**. |
    |||

1. Seleccione **Siguiente: Discos**.

1. En **Creación de una máquina virtual: Discos**, deje los valores predeterminados y seleccione **Siguiente: Redes**.

1. En **Creación de una máquina virtual: Redes**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Virtual network | Deje el valor predeterminado **MyVirtualNetwork**.  |
    | Espacio de direcciones | Deje el valor predeterminado **10.1.0.0/24**.|
    | Subnet | Deje el valor predeterminado **mySubnet (10.1.0.0/24)** .|
    | Dirección IP pública | Deje el valor predeterminado **(new) myVm-ip**. |
    | Puertos de entrada públicos | Seleccione **Permitir los puertos seleccionados**. |
    | Selección de puertos de entrada | Seleccione **HTTP** y **RDP**.|
    |||


1. Seleccione **Revisar + crear**. Se le remitirá a la página **Revisar y crear**, donde Azure validará la configuración.

1. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**.

> [!NOTE]
> En algunos casos, Azure Database for PostgreSQL y la subred de red virtual se encuentran en distintas suscripciones. En estos casos debe garantizar las siguientes configuraciones:
> - Asegúrese de que ambas suscripciones tengan el proveedor de recursos **Microsoft.DBforPostgreSQL** registrado. Para más información, consulte [resource-manager-registration][resource-manager-portal].

## <a name="create-an-azure-database-for-postgresql-single-server"></a>Creación un único servidor de Azure Database for PostgreSQL

En esta sección, creará un servidor de Azure Database for PostgreSQL en Azure. 

1. En la parte superior izquierda de la pantalla en Azure Portal, seleccione **Crear un recurso** > **Bases de datos** > **Azure Database for PostgreSQL**.

1. En la **opción de implementación Azure Database for PostgreSQL**, seleccione **Único servidor** y proporcione la siguiente información:

    | Configuración | Value |
    | ------- | ----- |
    | **Detalles del proyecto** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Lo creó en la sección anterior.|
    | **Detalles del servidor** |  |
    |Nombre de servidor  | Escriba *miServidor*. Si el nombre ya existe, cree uno único.|
    | Nombre de usuario administrador| Escriba el nombre de administrador que prefiera. |
    | Contraseña | Escriba una contraseña de su elección. La contraseña debe tener al menos ocho caracteres y cumplir con los requisitos definidos. |
    | Location | Seleccione la región de Azure en la que desea que se encuentre el servidor PostgreSQL. |
    |Versión  | Seleccione la versión de la base de datos del servidor PostgreSQL que se requiere.|
    | Proceso y almacenamiento| Seleccione el plan de tarifa que sea necesario para el servidor en función de la carga de trabajo. |
    |||
 
7. Seleccione **Aceptar**. 
8. Seleccione **Revisar + crear**. Se le remitirá a la página **Revisar y crear**, donde Azure validará la configuración. 
9. Cuando reciba el mensaje Validación superada, seleccione **Crear**. 
10. Cuando reciba el mensaje Validación superada, seleccione Crear. 

## <a name="create-a-private-endpoint"></a>Creación de un punto de conexión privado

En esta sección, creará un servidor PostgreSQL y le agregará un punto de conexión privado. 

1. En la parte superior izquierda de la pantalla en Azure Portal, seleccione **Crear un recurso** > **Redes** > **Private Link**.
2. En **Private Link Center: Información general**, en la opción **Crear una conexión privada a un servicio**, seleccione **Iniciar**.

    :::image type="content" source="media/concepts-data-access-and-security-private-link/privatelink-overview.png" alt-text="Información general de Private Link":::

1. En **Crear un punto de conexión privado - Aspectos básicos**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | **Detalles del proyecto** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Lo creó en la sección anterior.|
    | **Detalles de instancia** |  |
    | Nombre | Escriba *myPrivateEndpoint*. Si el nombre ya existe, cree uno único. |
    |Region|Seleccione **Oeste de Europa**.|
    |||
5. Seleccione **Siguiente: Resource** (Siguiente: Recurso).
6. En **Create a private endpoint - Resource** (Crear un punto de conexión privado: recurso), escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    |Método de conexión  | Seleccione Connect to an Azure resource in my directory (Conectarse a un recurso de Azure en mi directorio).|
    | Subscription| Seleccione su suscripción. |
    | Tipo de recurso | Seleccione **Microsoft.DBforPostgreSQL/servers**. |
    | Recurso |Seleccione *miServidor*.|
    |Recurso secundario de destino |Seleccione *postgresqlServer*|
    |||
7. Seleccione **Siguiente: Configuration** (Siguiente: Configuración).
8. En **Crear un punto de conexión privado: Configuración**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    |**REDES**| |
    | Virtual network| Seleccione *MyVirtualNetwork*. |
    | Subnet | Seleccione *mySubnet*. |
    |**INTEGRACIÓN DE DNS PRIVADO**||
    |Integración con una zona DNS privada |Seleccione **Sí**. |
    |Zona DNS privada |Seleccione *(New)privatelink.postgres.database.azure.com* |
    |||

    > [!Note] 
    > Use la zona DNS privada predefinida para su servicio o proporcione el nombre de la zona DNS que prefiera. Consulte la [configuración de la zona DNS de los servicios de Azure](../private-link/private-endpoint-dns.md) para obtener más información.

1. Seleccione **Revisar + crear**. Se le remitirá a la página **Revisar y crear**, donde Azure validará la configuración. 
2. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**. 

    :::image type="content" source="media/concepts-data-access-and-security-private-link/show-postgres-private-link.png" alt-text="Private Link creado":::

    > [!NOTE] 
    > El FQDN de la configuración de DNS del cliente no se resuelve en la dirección IP privada configurada. Tendrá que configurar una zona DNS para el FQDN configurado, como se muestra [aquí](../dns/dns-operations-recordsets-portal.md).

## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Conéctese a una máquina virtual mediante Escritorio remoto (RDP)


Después de crear **myVm**, conéctese a ella desde Internet como se indica a continuación: 

1. En la barra de búsqueda del portal, escriba *myVm*.

1. Seleccione el botón **Conectar**. Después de seleccionar el botón **Conectar**, se abre **Conectar a máquina virtual**.

1. Seleccione **Descargar archivo RDP**. Azure crea un archivo de Protocolo de Escritorio remoto ( *.rdp*) y lo descarga en su equipo.

1. Abra el archivo *downloaded.rdp*.

    1. Cuando se le pida, seleccione **Conectar**.

    1. Escriba el nombre de usuario y la contraseña que especificó al crear la VM.

        > [!NOTE]
        > Es posible que tenga que seleccionar **Más opciones** > **Usar otra cuenta** para especificar las credenciales que escribió al crear la máquina virtual.

1. Seleccione **Aceptar**.

1. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe una advertencia de certificado, seleccione **Sí** o **Continuar**.

1. Una vez que aparezca el escritorio de la máquina virtual, minimícelo para volver a su escritorio local.

## <a name="access-the-postgresql-server-privately-from-the-vm"></a>Acceso al servidor PostgreSQL de forma privada desde la VM

1. En el Escritorio remoto de *myVm*, abra PowerShell.

2. Escriba  `nslookup mydemopostgresserver.privatelink.postgres.database.azure.com`. 

    Recibirá un mensaje similar a este:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mydemopostgresserver.privatelink.postgres.database.azure.com
    Address:  10.1.3.4
    ```

3. Pruebe la conexión de Private Link del servidor PostgreSQL con cualquier cliente disponible. En el ejemplo siguiente se ha usado [Azure Data Studio](/sql/azure-data-studio/download?view=sql-server-ver15&preserve-view=true) para realizar la operación.

4. En **Nueva conexión**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Tipo de servidor| Seleccione **PostgreSQL**.|
    | Nombre de servidor| Seleccione *mydemopostgresserver.privatelink.postgres.database.azure.com*. |
    | Nombre de usuario | Escriba el nombre de usuario como username@servername, que se proporciona durante la creación del servidor PostgreSQL. |
    |Contraseña |Escriba una contraseña proporcionada durante la creación del servidor PostgreSQL. |
    |SSL|Seleccione **Requerido**.|
    ||

5. Seleccione Conectar.

6. Examine las bases de datos en el menú izquierdo.

7. (Opcional) Cree o consulte la información del servidor PostgreSQL.

8. Cierre la conexión de escritorio remoto a myVm.

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando haya terminado de usar el punto de conexión privado, el servidor PostgreSQL y la máquina virtual, elimine el grupo de recursos y todos los recursos que contiene:

1. Escriba *myResourceGroup* en el cuadro **Buscar** de la parte superior del portal y seleccione  *myResourceGroup*  en los resultados.
2. Seleccione **Eliminar grupo de recursos**.
3. Escriba myResourceGroup en **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En esta guía paso a paso ha creado una máquina virtual en una red virtual, una instancia de Azure Database for PostgreSQL (único servidor) y un punto de conexión privado para acceso privado. Se ha conectado a una máquina virtual desde Internet y se ha comunicado de forma segura con el servidor PostgreSQL mediante Private Link. Para obtener más información sobre los puntos de conexión privados, vea [¿Qué es un punto de conexión privado de Azure?](../private-link/private-endpoint-overview.md).

<!-- Link references, to text, Within this same GitHub repo. -->
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md