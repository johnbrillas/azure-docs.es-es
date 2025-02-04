---
title: 'Private Link para Azure Database for PostgreSQL: servidor único de la CLI de Azure'
description: 'Obtenga información sobre cómo configurar Private Link para Azure Database for PostgreSQL: servidor único de CLI de Azure'
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 01/09/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: b8aaebdd37f835201ef549e3f97e0c0b657e4fe9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96020133"
---
# <a name="create-and-manage-private-link-for-azure-database-for-postgresql---single-server-using-cli"></a>Creación y administración de Private Link para Azure Database for PostgreSQL: servidor único con la CLI

Un punto de conexión privado es el bloque de creación fundamental para el vínculo privado en Azure. Permite que los recursos de Azure, como las máquinas virtuales, se comuniquen de manera privada con recursos de vínculos privados. En este artículo se muestra cómo usar CLI de Azure para crear una máquina virtual en Azure Virtual Network y un servidor único de Azure Database for PostgreSQL con un punto de conexión privado de Azure.

> [!NOTE]
> La característica de vínculo privado solo está disponible para servidores de Azure Database for PostgreSQL en los planes de tarifa De uso general u Optimizado para memoria. Asegúrese de que el servidor de bases de datos esté incluido en uno de estos planes de tarifa.

## <a name="prerequisites"></a>Prerrequisitos

Para seguir esta guía, necesitará:

- Un [servidor y una base de datos de Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si, en su lugar, decide instalar y usar la CLI de Azure en un entorno local, para esta guía de inicio rápido se necesita la versión 2.0.28 de la CLI de Azure o una versión posterior. Ejecute `az --version` para buscar la versión instalada. Consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli) para obtener información sobre la instalación o actualización.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para poder crear un recurso, debe crear un grupo de recursos que hospede la red virtual. Cree un grupo de recursos con [az group create](/cli/azure/group). En este ejemplo, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *westeurope*:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-virtual-network"></a>Creación de una red virtual
Cree la red virtual con [az network vnet create](/cli/azure/network/vnet). En este ejemplo se crea una red virtual predeterminada denominada *myVirtualNetwork* con una subred denominada *mySubnet*:

```azurecli-interactive
az network vnet create \
 --name myVirtualNetwork \
 --resource-group myResourceGroup \
 --subnet-name mySubnet
```

## <a name="disable-subnet-private-endpoint-policies"></a>Deshabilitación de las directivas de punto de conexión privado 
Azure implementa los recursos en la subred de una red virtual, por lo que debe crear o actualizar la subred para deshabilitar [las directivas de red](../private-link/disable-private-endpoint-network-policy.md) del punto de conexión privado. Actualice una configuración de subred denominada *mySubnet* con [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update):

```azurecli-interactive
az network vnet subnet update \
 --name mySubnet \
 --resource-group myResourceGroup \
 --vnet-name myVirtualNetwork \
 --disable-private-endpoint-network-policies true
```
## <a name="create-the-vm"></a>Creación de la máquina virtual 
Cree la máquina virtual con az vm create. Cuando se le solicite, proporcione una contraseña que se usará como credenciales de inicio de sesión para la VM. En este ejemplo se crea una máquina virtual llamada *myVm*: 
```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image Win2019Datacenter
```

 Anote la dirección IP pública de la máquina virtual. Usará esta dirección para conectarse a la máquina virtual desde Internet en el paso siguiente.

## <a name="create-an-azure-database-for-postgresql---single-server"></a>Creación de Azure Database for PostgreSQL: servidor único 
Cree Azure Database for PostgreSQL con el comando crear servidor az postgres. Recuerde que el nombre del servidor PostgreSQL debe ser único en Azure, así que reemplace el valor del marcador de posición por sus propios valores únicos que utilizó anteriormente: 

```azurecli-interactive
# Create a server in the resource group 
az postgres server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location westeurope \
--admin-user mylogin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2
```

## <a name="create-the-private-endpoint"></a>Creación del punto de conexión privado 
Cree un punto de conexión privado para el servidor PostgreSQL en la instancia de Virtual Network: 

```azurecli-interactive
az network private-endpoint create \  
    --name myPrivateEndpoint \  
    --resource-group myResourceGroup \  
    --vnet-name myVirtualNetwork  \  
    --subnet mySubnet \  
    --private-connection-resource-id $(az resource show -g myResourcegroup -n mydemoserver --resource-type "Microsoft.DBforPostgreSQL/servers" --query "id" -o tsv) \    
    --group-id postgresqlServer \  
    --connection-name myConnection  
 ```

## <a name="configure-the-private-dns-zone"></a>Configuración de la zona DNS privada 
Cree una zona DNS privada para el dominio del servidor PostgreSQL y cree un vínculo de asociación con la instancia de Virtual Network. 

```azurecli-interactive
az network private-dns zone create --resource-group myResourceGroup \ 
   --name  "privatelink.postgres.database.azure.com" 
az network private-dns link vnet create --resource-group myResourceGroup \ 
   --zone-name  "privatelink.postgres.database.azure.com"\ 
   --name MyDNSLink \ 
   --virtual-network myVirtualNetwork \ 
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name myPrivateEndpoint --resource-group myResourceGroup --query 'networkInterfaces[0].id' -o tsv)
 
 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
# Copy the content for privateIPAddress and FQDN matching the Azure database for PostgreSQL name 
 
 
#Create DNS records 
az network private-dns record-set a create --name myserver --zone-name privatelink.postgres.database.azure.com --resource-group myResourceGroup  
az network private-dns record-set a add-record --record-set-name myserver --zone-name privatelink.postgres.database.azure.com --resource-group myResourceGroup -a <Private IP Address>
```

> [!NOTE] 
> El FQDN de la configuración de DNS del cliente no se resuelve en la dirección IP privada configurada. Tendrá que configurar una zona DNS para el FQDN configurado, como se muestra [aquí](../dns/dns-operations-recordsets-portal.md).

> [!NOTE]
> En algunos casos, Azure Database for PostgreSQL y la subred de red virtual se encuentran en distintas suscripciones. En estos casos debe garantizar las siguientes configuraciones:
> - Asegúrese de que ambas suscripciones tengan el proveedor de recursos **Microsoft.DBforPostgreSQL** registrado. Para más información, consulte [Proveedores de recursos](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

Conéctese a la máquina virtual *myVm* desde Internet de la siguiente manera:

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

3. Pruebe la conexión de Private Link del servidor PostgreSQL con cualquier cliente disponible. En el ejemplo siguiente se usa [Azure Data Studio](/sql/azure-data-studio/download?view=sql-server-ver15&preserve-view=true) para realizar la operación.

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
Cuando ya no se necesite, puede utilizar az group delete para quitar el grupo de recursos y todos los recursos que contiene: 

```azurecli-interactive
az group delete --name myResourceGroup --yes 
```

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Qué es un punto de conexión privado de Azure](../private-link/private-endpoint-overview.md)
