---
title: Adición de una identidad administrada a un tipo de nodo de clúster administrado de Service Fabric (versión preliminar)
description: En este artículo se muestra cómo agregar una identidad administrada a un tipo de nodo de clúster administrado de Service Fabric
ms.topic: how-to
ms.date: 11/24/2020
ms.custom: references_regions
ms.openlocfilehash: 00e679b07a44b799b6ac6677201bb59eeddcd6cf
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96839731"
---
# <a name="add-a-managed-identity-to-a-service-fabric-managed-cluster-node-type-preview"></a>Adición de una identidad administrada a un tipo de nodo de clúster administrado de Service Fabric (versión preliminar)

Cada tipo de nodo de un clúster administrado de Service Fabric se encuentra respaldado por un conjunto de escalado de máquinas virtuales. Para permitir el uso de identidades administradas con un tipo de nodo de clúster administrado, se ha agregado una propiedad `vmManagedIdentity` a las definiciones de tipo de nodo que contienen una lista de identidades que se pueden usar, `userAssignedIdentities`. La funcionalidad refleja cómo se pueden usar las identidades administradas en los clústeres no administrados, como el uso de una identidad administrada con la [extensión del conjunto de escalado de máquinas virtuales de Azure Key Vault](https://docs.microsoft.com/azure/virtual-machines/extensions/key-vault-windows).


Para ver un ejemplo de una implementación de clústeres administrados de Service Fabric que usa la identidad administrada en un tipo de nodo, consulte [esta plantilla](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/SF-Managed-Standard-SKU-1-NT-MI). Para ver una lista de las regiones admitidas, consulte las [preguntas más frecuentes sobre clústeres administrados](https://docs.microsoft.com/azure/service-fabric/faq-managed-cluster#what-regions-are-supported-in-the-preview).

> [!NOTE]
> Actualmente, solo se admiten identidades asignadas por el usuario para esta característica.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar:

* Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Si tiene previsto usar PowerShell, [instale](https://docs.microsoft.com/cli/azure/install-azure-cli) la CLI de Azure para ejecutar los comandos de referencia de la CLI.

## <a name="create-a-user-assigned-managed-identity"></a>Crear una identidad administrada asignada por el usuario 

Una identidad administrada asignada por el usuario se puede definir en la sección de recursos de una plantilla de Azure Resource Manager (ARM) para su creación tras la implementación:

```JSON
{ 
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities", 
    "name": "[parameters('userAssignedIdentityName')]", 
    "apiVersion": "2018-11-30", 
    "location": "[resourceGroup().location]"  
},
```

O crearse a través de PowerShell:

```powershell
az group create --name <resourceGroupName> --location <location>
az identity create --name <userAssignedIdentityName> --resource-group <resourceGroupName>
```

## <a name="add-a-role-assignment-with-service-fabric-resource-provider"></a>Adición de una asignación de roles con el proveedor de recursos de Service Fabric

Agregue una asignación de roles a la identidad administrada con la aplicación de proveedor de recursos de Service Fabric. Esta asignación permite al proveedor de recursos de Service Fabric asignar la identidad al conjunto de escalado de máquinas virtuales del clúster administrado. 

Se deben usar los valores siguientes cuando corresponda:

|Nombre|El valor del proveedor de recursos de Service Fabric correspondiente|
|----|-------------------------------------|
|Identificador de aplicación|74cb6831-0dbb-4be1-8206-fd4df301cdc2|
|Id. de objeto|fbc587f2-66f5-4459-a027-bcd908b9d278|


|El nombre de la definición de roles|Id. de definición de roles|
|----|-------------------------------------|
|Operador de identidad administrada|f1a07417-d97a-45cb-824c-7a7467783830
|



Esta asignación de roles se puede definir en la sección de recursos mediante el identificador de objeto y el identificador de definición de roles:

```JSON
{
    "type": "Microsoft.Authorization/roleAssignments", 
    "apiVersion": "2020-04-01-preview",
    "name": "[parameters('vmIdentityRoleNameGuid')]",
    "scope": "[concat('Microsoft.ManagedIdentity/userAssignedIdentities', '/', parameters('userAssignedIdentityName'))]",
    "dependsOn": [ 
        "[concat('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName'))]"
    ], 
    "properties": {
        "roleDefinitionId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'f1a07417-d97a-45cb-824c-7a7467783830')]",
        "principalId": "fbc587f2-66f5-4459-a027-bcd908b9d278" 
    } 
}, 
```

O crearse a través de PowerShell mediante el identificador de la aplicación y el identificador de la definición de roles:

```powershell
New-AzRoleAssignment -ApplicationId 74cb6831-0dbb-4be1-8206-fd4df301cdc2 -RoleDefinitionName "Managed Identity Operator" -Scope "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<userAssignedIdentityName>"
```

O el identificador de objeto y el identificador de definición de roles:

```powershell
New-AzRoleAssignment -PrincipalId fbc587f2-66f5-4459-a027-bcd908b9d278 -RoleDefinitionName "Managed Identity Operator" -Scope "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<userAssignedIdentityName>"
```

## <a name="add-managed-identity-properties-to-node-type-definition"></a>Adición de las propiedades de identidad administrada a la definición de tipo de nodo

Por último, agregue las propiedades `vmManagedIdentity` y `userAssignedIdentities` a la definición de tipo de nodo del clúster administrado:

```json

"properties": {
    "isPrimary" : true,
    "vmInstanceCount": 5,
    "dataDiskSizeGB": 100,
    "vmSize": "Standard_D2",
    "vmImagePublisher" : "MicrosoftWindowsServer",
    "vmImageOffer" : "WindowsServer",
    "vmImageSku" : "2019-Datacenter",
    "vmImageVersion" : "latest",
    "vmManagedIdentity": {
        "userAssignedIdentities": [
            "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('userAssignedIdentityName'))]"
        ]
    }
}
```

Después de la implementación, la identidad administrada creada se ha agregado al conjunto de escalado de máquinas virtuales del tipo de nodo designado y se puede usar como se espera, al igual que en cualquier clúster no administrado.

## <a name="troubleshooting"></a>Solución de problemas

No se puede agregar correctamente una asignación de roles con el siguiente error en la implementación:

:::image type="content" source="media/how-to-managed-identity-managed-cluster-vmss/role-assignment-error.png" alt-text="Error de implementación de Azure Portal que muestra el cliente con el identificador de objeto o aplicación de SFRP que no tiene permiso para realizar la actividad de administración de identidades":::

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Implementación de una aplicación en un clúster administrado de Service Fabric](https://docs.microsoft.com/azure/service-fabric/tutorial-managed-cluster-deploy-app) 
