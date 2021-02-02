---
title: Variables en plantillas
description: Se describe cómo definir variables en una plantilla de Azure Resource Manager (plantilla de ARM).
ms.topic: conceptual
ms.date: 01/26/2021
ms.openlocfilehash: feecc4b5df77e6a3bf51294cb12aabf44899dde5
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98874441"
---
# <a name="variables-in-arm-template"></a>Variables en la plantilla de ARM

En este artículo se describe cómo definir y usar variables en una plantilla de Azure Resource Manager (plantilla de ARM). Las variables se utilizan para simplificar la plantilla. En lugar de repetir expresiones complicadas en toda la plantilla, defina una variable que contenga la expresión complicada. A continuación, haga referencia a esa variable en toda la plantilla tanto como sea necesario.

Resource Manager resuelve las variables antes de iniciar las operaciones de implementación. Siempre que la variable se use en la plantilla, Resource Manager la reemplaza por el valor resuelto.

## <a name="define-variable"></a>Definición de la variable

Al definir una variable, proporcione un valor o una expresión de plantilla que se resuelva como un [tipo de datos](template-syntax.md#data-types). Puede usar el valor de un parámetro u otra variable al construir la variable.

Puede usar las [funciones de plantilla](template-functions.md) en la declaración de variables, pero no puede usar la función [reference](template-functions-resource.md#reference) ni ninguna de las funciones [list](template-functions-resource.md#list). Estas funciones obtienen el estado de tiempo de ejecución de un recurso y no se pueden ejecutar antes de la implementación cuando se resuelven variables.

El ejemplo siguiente muestra una variable de definición. Crea un valor de cadena para un nombre de cuenta de almacenamiento. Usa varias funciones de plantilla para obtener un valor de parámetro y lo concatena a una cadena única.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

## <a name="use-variable"></a>Uso de la variable

En la plantilla, debe hacer referencia al valor del parámetro con la función [variables](template-functions-deployment.md#variables). En el ejemplo siguiente se muestra cómo usar la variable para una propiedad de recurso.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageName')]",
    ...
  }
]
```

## <a name="example-template"></a>Plantilla de ejemplo

La plantilla siguiente no implementa ningún recurso. Simplemente muestra algunas maneras de declarar variables.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variables.json":::

## <a name="configuration-variables"></a>Variables de configuración

Puede definir variables que contengan valores relacionados para configurar un entorno. La variable se define como un objeto con los valores. En el ejemplo siguiente se muestra un objeto que contiene valores para dos entornos: **test** y **prod**. Se pasa uno de estos valores durante la implementación.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variablesconfigurations.json":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las propiedades disponibles para variables, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](template-syntax.md).
* Para obtener recomendaciones sobre cómo crear variables, consulte [Procedimientos recomendados: variables](template-best-practices.md#variables).
* Para ver una plantilla de ejemplo que asigna reglas de seguridad a un grupo de seguridad de red, consulte las [reglas de seguridad de red](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) y el [archivo de parámetros](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json).
