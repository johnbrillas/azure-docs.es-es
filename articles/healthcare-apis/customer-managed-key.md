---
title: Configuración de claves administradas por el cliente Azure API for FHIR
description: Incorpore su propia característica clave compatible con Azure API for FHIR mediante Cosmos DB
services: healthcare-apis
author: ginalee-dotcom
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: overview
ms.date: 09/28/2020
ms.author: ginle
ms.openlocfilehash: f810769529569309656193d41f28cca201a85c07
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101719226"
---
# <a name="configure-customer-managed-keys-at-rest"></a>Configuración de claves administradas por el cliente en reposo

Cuando crea una cuenta de Azure API for FHIR, de forma predeterminada los datos se cifran mediante claves administradas por Microsoft. Ahora puede agregar una segunda capa de cifrado de datos mediante una clave que usted mismo elige y administra.

En Azure, para hacerlo se suele utilizar una clave de cifrado en la instancia de Azure Key Vault del cliente. Azure SQL, Azure Storage y Cosmos DB son algunos ejemplos que proporcionan esta funcionalidad hoy en día. Azure API for FHIR aprovecha esta compatibilidad con Cosmos DB. Al crear una cuenta, tendrá la opción de especificar el identificador URI de una clave de Azure Key Vault. Esta clave se pasará a Cosmos DB al aprovisionar la cuenta de la base de datos. Cuando se realiza una solicitud de FHIR, Cosmos DB captura su clave y la usa para cifrar o descifrar los datos. Para empezar, puede consultar los vínculos siguientes:

- [Registro del proveedor de recursos de Azure Cosmos DB para su suscripción a Azure](../cosmos-db/how-to-setup-cmk.md#register-resource-provider) 
- [Configuración de la instancia de Azure Key Vault](../cosmos-db/how-to-setup-cmk.md#configure-your-azure-key-vault-instance)
- [Adición de una directiva de acceso a una instancia de Azure Key Vault](../cosmos-db/how-to-setup-cmk.md#add-an-access-policy-to-your-azure-key-vault-instance)
- [Generación de una clave en Azure Key Vault](../cosmos-db/how-to-setup-cmk.md#generate-a-key-in-azure-key-vault)

## <a name="using-azure-portal"></a>Uso de Azure Portal

Al crear una cuenta de Azure API for FHIR en Azure Portal, puede ver la opción de configuración "Cifrado de datos" en la sección "Configuración de base de datos" de la pestaña "Configuración adicional". De forma predeterminada, se elegirá la opción de clave administrada por el servicio. 

La clave se puede elegir en KeyPicker:

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-keypicker.png" alt-text="KeyPicker":::

O bien, se puede especificar la clave de Azure Key Vault seleccionando la opción "Clave administrada por el cliente". Aquí puede escribir el identificador URI de la clave:

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-create.png" alt-text="Creación de Azure API for FHIR":::

En el caso de las cuentas de FHIR existentes, puede ver la opción de cifrado de clave (clave administrada por el cliente o por el servicio) en la hoja "Base de datos" como se indica a continuación. La opción de configuración no se puede modificar una vez elegida. Sin embargo, se puede modificar y actualizar la clave.

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-database.png" alt-text="Base de datos":::

Además, puede crear una nueva versión de la clave especificada, después de la cual los datos se cifrarán con la nueva versión sin ninguna interrupción del servicio. También puede quitar el acceso a la clave para quitar el acceso a los datos. Cuando la clave esté deshabilitada, las consultas generarán un error. Si se vuelve a habilitar, las consultas se volverán a realizar correctamente.




## <a name="using-azure-powershell"></a>Uso de Azure PowerShell

Con el URI de la clave de Azure Key Vault, puede configurar la clave administrada por el cliente con PowerShell mediante la ejecución del comando de PowerShell siguiente:

```powershell
New-AzHealthcareApisService
    -Name "myService"
    -Kind "fhir-R4"
    -ResourceGroupName "myResourceGroup"
    -Location "westus2"
    -CosmosKeyVaultKeyUri "https://<my-vault>.vault.azure.net/keys/<my-key>"
```

## <a name="using-azure-cli"></a>Uso de la CLI de Azure

Al igual que con el método de PowerShell, puede configurar la clave administrada por el cliente pasando el URI de la clave de Azure Key Vault del parámetro `key-vault-key-uri` y ejecutando el comando de la CLI siguiente: 

```azurecli-interactive
az healthcareapis service create
    --resource-group "myResourceGroup"
    --resource-name "myResourceName"
    --kind "fhir-R4"
    --location "westus2"
    --cosmos-db-configuration key-vault-key-uri="https://<my-vault>.vault.azure.net/keys/<my-key>"

```
## <a name="using-azure-resource-manager-template"></a>Uso de la plantilla de Azure Resource Manager

Con el URI de la clave de Azure Key Vault puede configurar la clave administrada por el cliente pasándolo por la propiedad **keyVaultKeyUri** del objeto **properties**.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "services_myService_name": {
            "defaultValue": "myService",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.HealthcareApis/services",
            "apiVersion": "2020-03-30",
            "name": "[parameters('services_myService_name')]",
            "location": "westus2",
            "kind": "fhir-R4",
            "properties": {
                "accessPolicies": [],
                "cosmosDbConfiguration": {
                    "offerThroughput": 400,
                    "keyVaultKeyUri": "https://<my-vault>.vault.azure.net/keys/<my-key>"
                },
                "authenticationConfiguration": {
                    "authority": "https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db47",
                    "audience": "[concat('https://', parameters('services_myService_name'), '.azurehealthcareapis.com')]",
                    "smartProxyEnabled": false
                },
                "corsConfiguration": {
                    "origins": [],
                    "headers": [],
                    "methods": [],
                    "maxAge": 0,
                    "allowCredentials": false
                }
            }
        }
    ]
}
```

Y puede implementar la plantilla con el siguiente script de PowerShell:

```powershell
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$accountLocation = "West US 2"
$keyVaultKeyUri = "https://<my-vault>.vault.azure.net/keys/<my-key>"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "deploy.json" `
    -accountName $accountName `
    -location $accountLocation `
    -keyVaultKeyUri $keyVaultKeyUri
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido a configurar las claves administradas por el cliente en reposo mediante Azure Portal, PowerShell, la CLI y la plantilla de Resource Manager. Puede consultar la sección de preguntas más frecuentes de Azure Cosmos DB para más preguntas: 
 
>[!div class="nextstepaction"]
>[Cosmos DB: procedimientos para configurar CMK](../cosmos-db/how-to-setup-cmk.md#frequently-asked-questions)