---
title: 'Ejemplos de procesadores de telemetría: Azure Monitor Application Insights para Java'
description: Ejemplos que ilustran a los procesadores de telemetría en Azure Monitor Application Insights para Java
ms.topic: conceptual
ms.date: 12/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: 9b29c9611359c97c4097ad0b90ee2673bb28f37c
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98696319"
---
# <a name="telemetry-processors-examples---azure-monitor-application-insights-for-java"></a>Ejemplos de procesadores de telemetría: Azure Monitor Application Insights para Java

## <a name="includeexclude-samples"></a>Ejemplos de inclusión o exclusión

### <a name="1-include-spans"></a>1. Intervalos de inclusión

A continuación se muestra cómo incluir intervalos para este procesador de atributos. Este procesador no procesará el resto de los intervalos cuyas propiedades no coincidan.

A continuación se indican las condiciones que se deben cumplir para lograr una coincidencia:
* El nombre del intervalo debe ser igual a "spanA" o "spanB". 

Los siguientes son intervalos que coinciden con las propiedades include y se aplican acciones del procesador.
* Span1 Name: 'spanA' Attributes: {env: dev, test_request: 123, credit_card: 1234}
* Span2 Name: 'spanB' Attributes: {env: dev, test_request: false}
* Span3 Name: 'spanA' Attributes: {env: 1, test_request: dev, credit_card: 1234}

El siguiente intervalo no coincide con las propiedades include y no se aplican las acciones del procesador.
* Span4 Name: 'spanC' Attributes: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "include": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="2-exclude-spans"></a>2. Intervalos de exclusión

A continuación se muestra cómo excluir intervalos para este procesador de atributos. Este procesador no procesará todos los intervalos cuyas propiedades coincidan.

A continuación se indican las condiciones que se deben cumplir para lograr una coincidencia:
* El nombre del intervalo debe ser igual a "spanA" o "spanB". 

Los siguientes son intervalos que coinciden con las propiedades exclude y no se aplican las acciones del procesador.
* Span1 Name: 'spanA' Attributes: {env: dev, test_request: 123, credit_card: 1234}
* Span2 Name: 'spanB' Attributes: {env: dev, test_request: false}
* Span3 Name: 'spanA' Attributes: {env: 1, test_request: dev, credit_card: 1234}

El siguiente intervalo no coincide con las propiedades exclude y se aplican las acciones del procesador.
* Span4 Name: 'spanC' Attributes: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="3-excludemulti-spans"></a>3. Intervalos ExcludeMulti

A continuación se muestra cómo excluir intervalos para este procesador de atributos. Este procesador no procesará todos los intervalos cuyas propiedades coincidan.

A continuación se indican las condiciones que se deben cumplir para lograr una coincidencia:
* Debe existir un atributo ("env", "dev") en el intervalo para una coincidencia.
* Siempre que haya un atributo con la clave "test_request" en el intervalo, habrá una coincidencia.

Los siguientes son intervalos que coinciden con las propiedades exclude y no se aplican las acciones del procesador.
* Span1 Name: 'spanB' Attributes: {env: dev, test_request: 123, credit_card: 1234}
* Span2 Name: 'spanA' Attributes: {env: dev, test_request: false}

El siguiente intervalo no coincide con las propiedades exclude y se aplican las acciones del procesador.
* Span3 Name: 'spanB' Attributes: {env: 1, test_request: dev, credit_card: 1234}
* Span4 Name: 'spanC' Attributes: {env: dev, dev_request: false}


```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ],
          "attributes": [
            {
              "key": "env",
              "value": "dev"
            },
            {
              "key": "test_request"
            }
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="4-selective-processing"></a>4. Procesamiento selectivo

A continuación, se muestra cómo especificar el conjunto de propiedades de intervalo para indicar los intervalos a los que se debe aplicar este procesador. Las propiedades `include` indican cuáles deben incluirse y las propiedades `exclude` filtran aún más los intervalos que no se deben procesar.

Con la siguiente configuración, los siguientes intervalos coinciden con las propiedades y se aplican las acciones del procesador:

* Span1 Name: 'spanB' Attributes: {env: production, test_request: 123, credit_card: 1234, redact_trace: "false"}
* Span2 Name: 'spanA' Attributes: {env: staging, test_request: false, redact_trace: true}

Los siguientes intervalos no coinciden con las propiedades de inclusión y no se aplican acciones del procesador:
* Span3 Name: 'spanB' Attributes: {env: production, test_request: true, credit_card: 1234, redact_trace: false}
* Span4 Name: 'spanC' Attributes: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "include": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ]
        },
        "exclude": {
          "matchType": "strict",
          "attributes": [
            {
              "key": "redact_trace",
              "value": "false"
            }
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          },
          {
            "key": "duplicate_key",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```
## <a name="attribute-processor-samples"></a>Ejemplos de procesador de atributos

### <a name="insert"></a>Insertar

El siguiente código inserta un nuevo atributo {"attribute1": "attributeValue1"} en los intervalos en los que la clave "attribute1" no existe.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "attribute1",
            "value": "attributeValue1",
            "action": "insert"
          }
        ]
      }
    ]
  }
}
```

### <a name="insert-from-another-key"></a>Inserción desde otra clave

El siguiente código usa el valor del atributo "anotherkey" para insertar un nuevo atributo {"newKey": valor del atributo "anotherkey"} en los intervalos en los que la clave "newKey" no existe. Si el atributo "anotherkey" no existe, no se inserta ningún atributo nuevo en los intervalos.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "newKey",
            "fromAttribute": "anotherKey",
            "action": "insert"
          }
        ]
      }
    ]
  }
}
```

### <a name="update"></a>Actualizar

El siguiente código actualiza el atributo a { "db.secret": "redacted"} y actualiza el atributo "boo" con el valor del atributo "foo". Los intervalos sin el atributo "boo" no cambiarán.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "db.secret",
            "value": "redacted",
            "action": "update"
          },
          {
            "key": "boo",
            "fromAttribute": "foo",
            "action": "update" 
          }
        ]
      }
    ]
  }
}
```

### <a name="delete"></a>Eliminar

A continuación se muestra la eliminación de un atributo con la clave "credit_card".

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="hash"></a>Hash

A continuación se muestran los valores de atributo existentes de hash.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "user.email",
            "action": "hash"
          }
        ]
      }
    ]
  }
}
```

### <a name="extract"></a>Extract

En el ejemplo siguiente se muestra cómo usar expresiones regulares para crear nuevos atributos basados en el valor de otro atributo.
Por ejemplo, con el valor http.url = 'http://example.com/path?queryParam1=value1, queryParam2=value2' se insertarán los siguientes atributos:
* httpProtocol: http
* httpDomain: example.com
* httpPath: path
* httpQueryParams: queryParam1=value1,queryParam2=value2
* El valor de http.url no cambia.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "http.url",
            "pattern": "^(?<httpProtocol>.*):\\/\\/(?<httpDomain>.*)\\/(?<httpPath>.*)(\\?|\\&)(?<httpQueryParams>.*)",
            "action": "extract"
          }
        ]
      }
    ]
  }
}
```

En el ejemplo siguiente se muestra cómo procesar intervalos que tienen un nombre de intervalo que coincide con los patrones regexp.
Este procesador quitará el atributo "token" y ofuscará el atributo "password" en intervalos donde el nombre del intervalo coincide con "auth.\*" y donde el nombre del intervalo no coincide con "login.\*".

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "include": {
          "matchType": "regexp",
          "spanNames": [
            "auth.*"
          ]
        },
        "exclude": {
          "matchType": "regexp",
          "spanNames": [
            "login.*"
          ]
        },
        "actions": [
          {
            "key": "password",
            "value": "obfuscated",
            "action": "update"
          },
          {
            "key": "token",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```


## <a name="span-processor-samples"></a>Ejemplos de procesador de intervalos

### <a name="name-a-span"></a>Asignación de un nombre para un intervalo

En el siguiente ejemplo se especifican los valores de atributo "db.svc", "operation" e "id", que formarán el nuevo nombre del intervalo, en ese orden, separado por el valor "::".
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "name": {
          "fromAttributes": [
            "db.svc",
            "operation",
            "id"
          ],
          "separator": "::"
        }
      }
    ]
  }
}
```

### <a name="extract-attributes-from-span-name"></a>Extracción de atributos del nombre del intervalo

Supongamos que el nombre del intervalo de entrada es /api/v1/document/12345678/update. La aplicación de los siguientes resultados en el nombre del intervalo de salida /api/v1/document/{documentId}/update agregará un nuevo atributo "documentId"="12345678" al intervalo.
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "name": {
          "toAttributes": {
            "rules": [
              "^/api/v1/document/(?<documentId>.*)/update$"
            ]
          }
        }
      }
    ]
  }
}
```

### <a name="extract-attributes-from-span-name-with-include-and-exclude"></a>Extracción de atributos del nombre de intervalo con include y exclude

A continuación se muestra cómo cambiar el nombre del intervalo por "{operation_website}" y agregar el atributo {Key: operation_website, Value: oldSpanName } cuando el intervalo tiene las siguientes propiedades:
- El nombre del intervalo contiene '/' en cualquier parte de la cadena.
- El nombre del intervalo no es 'donot/change '.
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "include": {
          "matchType": "regexp",
          "spanNames": [
            "^(.*?)/(.*?)$"
          ]
        },
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "donot/change"
          ]
        },
        "name": {
          "toAttributes": {
            "rules": [
              "(?<operation_website>.*?)$"
            ]
          }
        }
      }
    ]
  }
}
```