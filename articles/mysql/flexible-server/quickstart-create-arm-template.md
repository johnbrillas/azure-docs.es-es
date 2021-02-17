---
title: 'Inicio rápido: Creación de una instancia de Azure Database for MySQL con la opción Servidor flexible mediante una plantilla de Resource Manager'
description: En este inicio rápido, aprenderá a crear una instancia de Azure Database for MySQL con la opción Servidor flexible mediante la plantilla de Resource Manager.
author: mksuni
ms.service: mysql
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: sumuth
ms.date: 10/23/2020
ms.openlocfilehash: 5be0bf21514dd92c2f980081bb57d873895bbb91
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100099938"
---
# <a name="quickstart-use-an-arm-template-to-create-an-azure-database-for-mysql---flexible-server-preview"></a>Inicio rápido: Uso de una plantilla de Resource Manager para crear una instancia de Azure Database for MySQL con la opción Servidor flexible (versión preliminar)

> [!IMPORTANT]
> Actualmente, Azure Database for MySQL: servidor flexible se encuentra en versión preliminar pública.

Azure Database for MySQL con la opción Servidor flexible (versión preliminar) es un servicio administrado que se usa para ejecutar, administrar y escalar bases de datos MySQL de alta disponibilidad en la nube. Puede usar una plantilla de Azure Resource Manager para aprovisionar un servidor flexible para implementar varios servidores o varias bases de datos en un servidor.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Prerrequisitos

Una cuenta de Azure con una suscripción activa. [cree una de forma gratuita](https://azure.microsoft.com/free/).

## <a name="review-the-template"></a>Revisión de la plantilla

Una instancia de Azure Database for MySQL con la opción Servidor flexible es el recurso primario de una o varias bases de datos dentro de una región. Proporciona el ámbito de las directivas de administración que se aplican a sus bases de datos: inicio de sesión, firewall, usuarios, roles y configuraciones.

Cree el archivo _mysql-flexible-server-template.json_ y copie este script JSON en él.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "administratorLogin": {
      "type": "String"
    },
    "administratorLoginPassword": {
      "type": "SecureString"
    },
    "location": {
      "type": "String"
    },
    "serverName": {
      "type": "String"
    },
    "serverEdition": {
      "type": "String"
    },
    "storageSizeMB": {
      "type": "Int"
    },
    "haEnabled": {
      "type": "string",
      "defaultValue": "Disabled"
    },
    "availabilityZone": {
      "type": "String"
    },
    "version": {
      "type": "String"
    },
    "tags": {
      "defaultValue": {},
      "type": "Object"
    },
    "firewallRules": {
      "defaultValue": {},
      "type": "Object"
    },
    "vnetData": {
      "defaultValue": {},
      "type": "Object"
    },
    "backupRetentionDays": {
      "type": "Int"
    }
  },
  "variables": {
    "api": "2020-07-01-preview",
    "firewallRules": "[parameters('firewallRules').rules]",
    "publicNetworkAccess": "[if(empty(parameters('vnetData')), 'Enabled', 'Disabled')]",
    "vnetDataSet": "[if(empty(parameters('vnetData')), json('{ \"subnetArmResourceId\": \"\" }'), parameters('vnetData'))]",
    "finalVnetData": "[json(concat('{ \"subnetArmResourceId\": \"', variables('vnetDataSet').subnetArmResourceId, '\"}'))]"
  },
  "resources": [
    {
      "type": "Microsoft.DBforMySQL/flexibleServers",
      "apiVersion": "[variables('api')]",
      "name": "[parameters('serverName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_D4ds_v4",
        "tier": "[parameters('serverEdition')]"
      },
      "tags": "[parameters('tags')]",
      "properties": {
        "version": "[parameters('version')]",
        "administratorLogin": "[parameters('administratorLogin')]",
        "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
        "publicNetworkAccess": "[variables('publicNetworkAccess')]",
        "DelegatedSubnetArguments": "[if(empty(parameters('vnetData')), json('null'), variables('finalVnetData'))]",
        "haEnabled": "[parameters('haEnabled')]",
        "storageProfile": {
          "storageMB": "[parameters('storageSizeMB')]",
          "backupRetentionDays": "[parameters('backupRetentionDays')]"
        },
        "availabilityZone": "[parameters('availabilityZone')]"
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-08-01",
      "name": "[concat('firewallRules-', copyIndex())]",
      "dependsOn": [
        "[concat('Microsoft.DBforMySQL/flexibleServers/', parameters('serverName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": [
            {
              "type": "Microsoft.DBforMySQL/flexibleServers/firewallRules",
              "name": "[concat(parameters('serverName'),'/',variables('firewallRules')[copyIndex()].name)]",
              "apiVersion": "[variables('api')]",
              "properties": {
                "StartIpAddress": "[variables('firewallRules')[copyIndex()].startIPAddress]",
                "EndIpAddress": "[variables('firewallRules')[copyIndex()].endIPAddress]"
              }
            }
          ]
        }
      },
      "copy": {
        "name": "firewallRulesIterator",
        "count": "[if(greater(length(variables('firewallRules')), 0), length(variables('firewallRules')), 1)]",
        "mode": "Serial"
      },
      "condition": "[greater(length(variables('firewallRules')), 0)]"
    }
  ]
}
```

Estos recursos se definen en la plantilla:

- Microsoft.DBforMySQL/flexibleServers

## <a name="deploy-the-template"></a>Implementación de la plantilla

Seleccione **Pruébelo** en el bloque de código de PowerShell siguiente para abrir [Azure Cloud Shell](../../cloud-shell/overview.md).

```azurepowershell-interactive
$serverName = Read-Host -Prompt "Enter a name for the new Azure Database for MySQL server"
$resourceGroupName = Read-Host -Prompt "Enter a name for the new resource group where the server will exist"
$location = Read-Host -Prompt "Enter an Azure region (for example, centralus) for the resource group"
$adminUser = Read-Host -Prompt "Enter the Azure Database for MySQL server's administrator account name"
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString

New-AzResourceGroup -Name $resourceGroupName -Location $location # Use this command when you need to create a new resource group for your deployment
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
    -TemplateFile "D:\Azure\Templates\EngineeringSite.json
    -serverName $serverName `
    -administratorLogin $adminUser `
    -administratorLoginPassword $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

Siga estos pasos para verificar si el servidor se creó en Azure.

### <a name="azure-portal"></a>Azure portal

1. En [Azure Portal](https://portal.azure.com), busque y seleccione los servidores de **Azure Database for MySQL**.
1. En la lista base de datos, seleccione el nuevo servidor. Aparece la página de **información general**  del nuevo servidor de Azure Database for MySQL.

### <a name="powershell"></a>PowerShell

Tendrá que escribir el nombre del nuevo servidor para ver los detalles de su instancia de Azure Database for MySQL con la opción Servidor flexible.

```azurepowershell-interactive
$serverName = Read-Host -Prompt "Enter the name of your Azure Database for MySQL server"
Get-AzResource -ResourceType "Microsoft.DBforMySQL/flexibleServers" -Name $serverName | ft
Write-Host "Press [ENTER] to continue..."
```

### <a name="cli"></a>CLI

Tendrá que escribir el nombre y el grupo de recursos del nuevo servidor para ver los detalles de su instancia de Azure Database for MySQL con la opción Servidor flexible.

```azurecli-interactive
echo "Enter your Azure Database for MySQL server name:" &&
read serverName &&
echo "Enter the resource group where the Azure Database for MySQL server exists:" &&
read resourcegroupName &&
az resource show --resource-group $resourcegroupName --name $serverName --resource-type "Microsoft.DbForMySQL/flexibleServers"
```
## <a name="exporting-arm-template-from-the-portal"></a>Exportación de una plantilla de ARM desde el portal
Puede [exportar una plantilla de ARM](../../azure-resource-manager/templates/export-template-portal.md) desde Azure Portal. Hay dos maneras de exportar una plantilla:

- [Exportar desde el grupo de recursos o el recurso](../../azure-resource-manager/templates/export-template-portal.md#export-template-from-a-resource). Esta opción genera una plantilla nueva a partir de los recursos existentes. La plantilla exportada es una "instantánea" del estado actual del grupo de recursos. Puede exportar un grupo de recursos completo o recursos específicos dentro de ese grupo de recursos.
- [Exportar antes de la implementación o desde el historial](../../azure-resource-manager/templates/export-template-portal.md#export-template-before-deployment). Esta opción recupera una copia exacta de una plantilla usada para la implementación.

Al exportar la plantilla, en la sección ```"parameters":{ }``` de la plantilla observará que ```administratorLogin``` y ```administratorLoginPassword``` no se incluyen por motivos de seguridad. **DEBE** agregar estos parámetros a la plantilla antes de implementarla o, de lo contrario, se producirá un error en la plantilla.

```
"administratorLogin": {
      "type": "String"
    },
"administratorLoginPassword": {
      "type": "SecureString"
    },
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Mantenga este grupo de recursos, el servidor y la base de datos única si desea ir a los artículos de [Pasos siguientes](#next-steps). Los pasos siguientes muestran cómo conectarse a la base de datos y consultarla mediante distintos métodos.

Para eliminar el grupo de recursos:

### <a name="azure-portal"></a>Azure portal

1. En [Azure Portal](https://portal.azure.com), busque la opción **Grupos de recursos** y selecciónela.
1. En la lista de los grupos de recursos, elija el nombre del grupo de recursos.
1. En la página de **información general** del grupo de recursos, seleccione **Eliminar grupo de recursos**.
1. En el cuadro de diálogo de confirmación, escriba el nombre del grupo de recursos y seleccione **Eliminar**.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

### <a name="cli"></a>CLI

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```
---

## <a name="next-steps"></a>Pasos siguientes

Para ver un tutorial paso a paso que le guíe en el proceso de creación de una plantilla de Resource Manager, consulte:

> [!div class="nextstepaction"]
> [Creación e implementación de la primera plantilla de Resource Manager](../../azure-resource-manager/templates/template-tutorial-create-first-template.md)

Para obtener un tutorial paso a paso para compilar una aplicación con App Service mediante MySQL, consulte:

> [!div class="nextstepaction"]
>[Compilación de una aplicación web PHP (Laravel) con MySQL](tutorial-php-database-app.md)
