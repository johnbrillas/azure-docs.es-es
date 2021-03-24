---
title: Declaración de recursos en plantillas
description: En este artículo se describe cómo declarar recursos para implementar en una plantilla de Azure Resource Manager (plantilla de ARM).
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 13f4a08162c40cbb36173627d88a4a8202a4ed26
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743827"
---
# <a name="resource-declaration-in-arm-templates"></a>Declaración de recursos en plantillas de ARM

Para implementar un recurso a través de una plantilla de Azure Resource Manager (plantilla de ARM), agregue una declaración de recursos. Use la matriz `resources` para una plantilla JSON, o la palabra clave `resource` para Bicep.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="set-resource-type-and-version"></a>Definición del tipo de recurso y versión

Al agregar un recurso a la plantilla, empiece por establecer el tipo de recurso y la versión de la API. Estos valores determinan las demás propiedades que están disponibles para el recurso.

En el ejemplo siguiente se muestra cómo establecer el tipo de recurso y la versión de la API para una cuenta de almacenamiento. En el ejemplo no se muestra la declaración de recursos completa.

# <a name="json"></a>[JSON](#tab/json)

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-06-01",
    ...
  }
]
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  ...
}
```

---

Para Bicep, usted establece un nombre simbólico para el recurso. En el ejemplo anterior, el nombre simbólico es `sa`. Puede usar cualquier valor como nombre simbólico, pero no puede ser el mismo que otro recurso, parámetro o variable de la plantilla. El nombre simbólico no es lo mismo que el nombre del recurso. El nombre simbólico se usa para hacer referencia fácilmente al recurso en otras partes de la plantilla.

## <a name="set-resource-name"></a>Definición del nombre del recurso

Cada recurso tiene un nombre. Al establecer el nombre del recurso, preste atención a las [reglas y restricciones de los nombres de los recursos](../management/resource-name-rules.md).

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "storageAccountName": {
    "type": "string",
    "minLength": 3,
    "maxLength": 24
  }
},
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-06-01",
    "name": "[parameters('storageAccountName')]",
    ...
  }
]
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@minLength(3)
@maxLength(24)
param storageAccountName string

resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: storageAccountName
  ...
}
```

---

## <a name="set-location"></a>Establezca una ubicación

Muchos recursos requieren una ubicación. Puede determinar si el recurso necesita una ubicación a través de IntelliSense o de la [referencia de plantillas](/azure/templates/). En el ejemplo siguiente se agrega un parámetro de ubicación que se usa para la cuenta de almacenamiento.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "storageAccountName": {
    "type": "string",
    "minLength": 3,
    "maxLength": 24
  },
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-06-01",
    "name": "[parameters('storageAccountName')]",
    "location": "[parameters('location')]",
    ...
  }
]
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@minLength(3)
@maxLength(24)
param storageAccountName string
param location string = resourceGroup().location

resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: storageAccountName
  location: location
  ...
}
```

---

Para obtener más información, consulte [Establecimiento de la ubicación del recurso en la plantilla de Resource Manager](resource-location.md).

## <a name="set-tags"></a>Definición de etiquetas

Puede aplicar etiquetas a un recurso durante la implementación. Las etiquetas le ayudan a organizar de forma lógica los recursos implementados. Para ver ejemplos de las diferentes maneras de especificar las etiquetas, consulte [Etiquetas de plantillas de ARM](../management/tag-resources.md#arm-templates).

## <a name="set-resource-specific-properties"></a>Definición de propiedades específicas de recursos

Las propiedades anteriores son genéricas para la mayoría de los tipos de recursos. Después de establecer esos valores, tiene que establecer las propiedades que son específicas del tipo de recurso que va a implementar.

Use IntelliSense o la [referencia de plantillas](/azure/templates/) para decidir qué propiedades están disponibles y cuáles son obligatorias. En el ejemplo siguiente se establecen las propiedades restantes para una cuenta de almacenamiento.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "type": "string",
      "minLength": 3,
      "maxLength": 24
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    }
  },
  "functions": [],
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "name": "[parameters('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "StorageV2",
      "properties": {
        "accessTier": "Hot"
      }
    }
  ]
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@minLength(3)
@maxLength(24)
param storageAccountName string
param location string = resourceGroup().location

resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
    tier: 'Standard'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
```

---

## <a name="next-steps"></a>Pasos siguientes

* Para implementar un recurso de manera condicional, consulte [Implementación condicional en las plantillas de ARM](conditional-resource-deployment.md).
* Para establecer dependencias de recursos, consulte [Definición del orden de implementación de recursos en plantillas de ARM](define-resource-dependency.md).
