---
title: Comparación de la sintaxis de las plantillas de Azure Resource Manager en JSON y Bicep
description: Compara las plantillas de Azure Resource Manager desarrolladas con JSON y Bicep, y muestra cómo realizar la conversión entre los dos lenguajes.
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 29c2b9948957ebc10a26f22f0fe3daf383dfe5ba
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102036221"
---
# <a name="comparing-json-and-bicep-for-templates"></a>Comparación de JSON y Bicep para plantillas

En este artículo se compara la sintaxis de Bicep con la sintaxis de JSON para las plantillas de Azure Resource Manager (plantillas de ARM). En la mayoría de los casos, Bicep proporciona una sintaxis menos detallada que el equivalente en JSON.

## <a name="syntax-equivalents"></a>Equivalentes de sintaxis

Si está familiarizado con el uso de JSON para desarrollar plantillas de ARM, use la siguiente tabla para obtener información sobre los equivalentes de sintaxis para Bicep.

| Escenario | Bicep | JSON |
| -------- | ------------ | ----- |
| Crear una expresión | `func()` | `"[func()]"` |
| Obtener un valor de parámetro | `exampleParameter` | `[parameters('exampleParameter'))]` |
| Obtener un valor de variable | `exampleVar` | `[variables('exampleVar'))]` |
| Concatenación de cadenas | `'${namePrefix}-vm'` | `[concat(parameters('namePrefix'), '-vm')]` |
| Establecer una propiedad de recurso | `sku: '2016-Datacenter'` | `"sku": "2016-Datacenter",` |
| Devolver el operador lógico AND | `isMonday && isNovember` | `[and(parameter('isMonday'), parameter('isNovember'))]` |
| Obtener el id. de recurso en la plantilla | `nic1.id` | `[resourceId('Microsoft.Network/networkInterfaces', variables('nic1Name'))]` |
| Obtener la propiedad del recurso en la plantilla | `diagsAccount.properties.primaryEndpoints.blob` | `[reference(resourceId('Microsoft.Storage/storageAccounts', variables('diagStorageAccountName'))).primaryEndpoints.blob]` |
| Establecer un valor condicionalmente | `isMonday ? 'valueIfTrue' : 'valueIfFalse'` | `[if(parameters('isMonday'), 'valueIfTrue', 'valueIfFalse')]` |
| Dividir una solución en varios archivos | Usar módulos | Usar plantillas vinculadas |
| Establecer el ámbito de destino de la implementación | `targetScope = 'subscription'` | `"$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#"` |
| Establecer la dependencias | Puede confiar en la detección automática de dependencias o establecer manualmente las dependencias con `dependsOn: [ stg ]`. | `"dependsOn": ["[resourceId('Microsoft.Storage/storageAccounts', 'parameters('storageAccountName'))]"]` |
| Declaración de recursos | `resource vm 'Microsoft.Compute/virtualMachines@2020-06-01' = {...}` | `"resources": [ { "type": "Microsoft.Compute/virtualMachines", "apiVersion": "2020-06-01", ... } ]` |

## <a name="recommendations"></a>Recomendaciones

* Cuando sea posible, evite usar las funciones [reference](template-functions-resource.md#reference) y [resourceId](template-functions-resource.md#resourceid) en el archivo Bicep. Cuando haga referencia a un recurso en la misma implementación de Bicep, use el id. de recursos en su lugar. Por ejemplo, si ha implementado un recurso en el archivo Bicep con `stg` como id. de recurso, use una sintaxis como `stg.id` o `stg.properties.primaryEndpoints.blob` para obtener los valores de las propiedades. Al usar el id. de recursos, se crea una dependencia implícita entre los recursos. No es necesario establecer explícitamente la dependencia con la propiedad dependsOn.
* Haga un uso coherente de mayúsculas y minúsculas para los identificadores. Si no está seguro de qué tipo de uso de mayúsculas aplicar, pruebe con la combinación de mayúsculas y minúsculas. Por ejemplo, `param myCamelCasedParameter string`.
* Agregue una descripción a un parámetro solo cuando la descripción proporcione información esencial a los usuarios. Puede usar comentarios de tipo `//` para incluir información.

## <a name="decompile-json-to-bicep"></a>Descompilación de JSON a Bicep

La CLI de Bicep incluye un comando para descompilar cualquier plantilla de ARM existente en un archivo Bicep. Para descompilar un archivo JSON, use: `bicep decompile "path/to/file.json"`.

Este comando proporciona un punto inicial para la creación de Bicep, pero el comando no funciona para todas las plantillas. Es posible que se produzca un error en el comando o que deba corregir los problemas después de la descompilación. Actualmente, las plantillas anidadas solo se pueden descompilar si usan el ámbito de evaluación de expresión "inner" (interno).

Puede exportar la plantilla para un grupo de recursos y, a continuación, pasarla directamente al comando de descompilación de Bicep. En el ejemplo siguiente se muestra cómo descompilar una plantilla exportada.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli
az group export --name "your_resource_group_name" > main.json
bicep decompile main.json
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Export-AzResourceGroup -ResourceGroupName "your_resource_group_name" -Path ./main.json
bicep decompile main.json
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Exporte la plantilla](export-template-portal.md) a través del portal. Use `bicep decompile <filename>` en el archivo descargado.

---

## <a name="build-json-from-bicep"></a>Compilación de JSON a partir de Bicep

La CLI de Bicep también incluye un comando para convertir Bicep en JSON. Para compilar un archivo JSON, use: `bicep build "path/to/file.json"`.

## <a name="side-by-side-view"></a>Vista en paralelo

[Bicep Playground](https://aka.ms/bicepdemo) le permite ver los archivos JSON y Bicep equivalentes en paralelo. Puede seleccionar una plantilla de ejemplo para ver ambas versiones. O bien, seleccione `Decompile` para cargar su propia plantilla JSON y ver el archivo Bicep equivalente.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre Bicep, consulte el [Tutorial de Bicep](./bicep-tutorial-create-first-bicep.md).
