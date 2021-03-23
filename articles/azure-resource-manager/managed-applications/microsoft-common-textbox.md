---
title: Elemento de la interfaz de usuario de TextBox
description: Describe el elemento de la interfaz de usuario Microsoft.Common.TextBox para Azure Portal. Se usa para agregar texto sin formato.
author: tfitzmac
ms.topic: conceptual
ms.date: 03/03/2021
ms.author: tomfitz
ms.openlocfilehash: 8e4cfcc7fe46c19bf1e58ee5eadf1e0bb8c7bd8e
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102124336"
---
# <a name="microsoftcommontextbox-ui-element"></a>Elemento de interfaz de usuario Microsoft.Common.TextBox

Elemento de interfaz de usuario (UI) que se puede usar para agregar texto sin formato. El elemento `Microsoft.Common.TextBox` es un campo de entrada de una sola línea, pero admite la entrada de varias líneas con la propiedad `multiLine`.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario

El elemento `TextBox` utiliza un cuadro de texto de una o varias líneas.

:::image type="content" source="media/managed-application-elements/microsoft-common-textbox.png" alt-text="Cuadro de texto de una sola línea del elemento Microsoft.Common.TextBox.":::

:::image type="content" source="media/managed-application-elements/microsoft-common-textbox-multi-line.png" alt-text="Cuadro de texto de varias líneas del elemento Microsoft.Common.TextBox.":::

## <a name="schema"></a>Schema

```json
{
  "name": "nameInstance",
  "type": "Microsoft.Common.TextBox",
  "label": "Name",
  "defaultValue": "contoso123",
  "toolTip": "Use only allowed characters",
  "placeholder": "",
  "multiLine": false,
  "constraints": {
    "required": true,
    "validations": [
      {
        "regex": "^[a-z0-9A-Z]{1,30}$",
        "message": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
      },
      {
        "isValid": "[startsWith(steps('resourceConfig').nameInstance, 'contoso')]",
        "message": "Must start with 'contoso'."
      }
    ]
  },
  "visible": true
}
```

## <a name="sample-output"></a>Salida de ejemplo

```json
"contoso123"
```

## <a name="remarks"></a>Observaciones

- Utilice la propiedad `toolTip` para mostrar texto encima del elemento cuando el cursor del mouse se desplaza sobre el símbolo de información.
- La propiedad `placeholder` es el texto de ayuda que desaparece cuando el usuario comienza a editar. Si se definen tanto `placeholder` como `defaultValue`, `defaultValue` tiene prioridad y se muestra.
- La propiedad `multiLine` es un valor booleano, `true` o `false`. Para usar un cuadro de texto de varias líneas, establezca la propiedad en `true`. Si no se requiere un cuadro de texto de varias líneas, establezca la propiedad en `false` o exclúyala. En el caso de las nuevas líneas, la salida JSON muestra `\n` para el avance de línea. El cuadro de texto de varias líneas acepta `\r` para un retorno de carro (CR) y `\n` para un avance de línea (LF). Por ejemplo, un valor predeterminado puede incluir `\r\n` para especificar CRLF.
- Si `constraints.required` está establecido en `true`, el cuadro de texto debe contener un valor para que la validación sea correcta. El valor predeterminado es `false`.
- La propiedad `validations` es una matriz en la que se agregan condiciones para comprobar el valor proporcionado en el cuadro de texto.
- La propiedad `regex` es un patrón de expresión regular de JavaScript. Si se especifica, el valor del cuadro de texto debe coincidir con el patrón para que la validación sea correcta. El valor predeterminado es `null`. Para obtener más información sobre la sintaxis de regex, consulte la [Referencia rápida de expresiones regulares](/dotnet/standard/base-types/regular-expression-language-quick-reference).
- La propiedad `isValid` contiene una expresión que se evalúa como `true` o `false`. Dentro de la expresión, se define la condición que determina si el cuadro de texto es válido.
- La propiedad `message` es una cadena que se muestra cuando el valor del cuadro de texto no supera la validación.
- Es posible especificar un valor para `regex` cuando `required` está establecido en `false`. En este escenario, no se requiere un valor para que la validación del cuadro de texto sea correcta. Si se especifica uno, debe coincidir con el patrón de expresión regular.

## <a name="examples"></a>Ejemplos

En los ejemplos se muestra cómo usar un cuadro de texto de una sola línea y uno de varias líneas.

### <a name="single-line"></a>Una línea

En el ejemplo siguiente se usa un cuadro de texto con el control [Microsoft.Solutions.ArmApiControl](microsoft-solutions-armapicontrol.md) para comprobar la disponibilidad de un nombre de recurso.

```json
"basics": [
  {
    "name": "nameApi",
    "type": "Microsoft.Solutions.ArmApiControl",
    "request": {
      "method": "POST",
      "path": "[concat(subscription().id, '/providers/Microsoft.Storage/checkNameAvailability?api-version=2019-06-01')]",
      "body": "[parse(concat('{\"name\": \"', basics('txtStorageName'), '\", \"type\": \"Microsoft.Storage/storageAccounts\"}'))]"
    }
  },
  {
    "name": "txtStorageName",
    "type": "Microsoft.Common.TextBox",
    "label": "Storage account name",
    "constraints": {
      "validations": [
        {
          "isValid": "[not(equals(basics('nameApi').nameAvailable, false))]",
          "message": "[concat('Name unavailable: ', basics('txtStorageName'))]"
        }
      ]
    }
  }
]
```

### <a name="multi-line"></a>Varias líneas

En este ejemplo se usa la propiedad `multiLine` para crear un cuadro de texto de varias líneas con texto de marcador de posición.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {
        "name": "demoTextBox",
        "type": "Microsoft.Common.TextBox",
        "label": "Multi-line text box",
        "defaultValue": "",
        "toolTip": "Use 1-60 alphanumeric characters, hyphens, spaces, periods, and colons.",
        "placeholder": "This is a multi-line text box:\nLine 1.\nLine 2.\nLine 3.",
        "multiLine": true,
        "constraints": {
          "required": true,
          "validations": [
            {
              "regex": "^[a-z0-9A-Z -.:\n]{1,60}$",
              "message": "Only 1-60 alphanumeric characters, hyphens, spaces, periods, and colons are allowed."
            }
          ]
        },
        "visible": true
      }
    ],
    "steps": [],
    "outputs": {
      "textBox": "[basics('demoTextBox')]"
    }
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

- Para ver una introducción sobre cómo crear definiciones de UI, consulte [CreateUiDefinition.json para la experiencia de creación de aplicaciones administradas de Azure](create-uidefinition-overview.md).
- Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
