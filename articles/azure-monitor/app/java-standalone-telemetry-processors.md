---
title: 'Procesadores de telemetría (versión preliminar): Azure Monitor Application Insights para Java'
description: Configuración de los procesadores de telemetría en Azure Monitor Application Insights para Java
ms.topic: conceptual
ms.date: 10/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: ba4e6b8b5e9db494ab4c0c372c2086087a2d58cb
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98133181"
---
# <a name="telemetry-processors-preview---azure-monitor-application-insights-for-java"></a>Procesadores de telemetría (versión preliminar): Azure Monitor Application Insights para Java

> [!NOTE]
> Esta característica aún sigue en la fase de versión preliminar.

Actualmente, el agente 3.0 de Java para Application Insights tiene las funcionalidades necesarias para procesar los datos de telemetría antes de que estos se exporten.

Estos son algunos casos de uso de los procesadores de telemetría:
 * Enmascaran datos confidenciales
 * Agregan dimensiones personalizadas condicionalmente
 * Actualizan el nombre de telemetría que se usa para la agregación y visualización
 * Quitan o filtran atributos de intervalo para controlar el costo de ingestas

## <a name="terminology"></a>Terminología

Antes de pasar a los procesadores de telemetría, es importante comprender qué son los seguimientos y los intervalos.

### <a name="traces"></a>Seguimientos

Los seguimientos supervisan el avance de una única solicitud, denominada `trace`, a medida que la controlan los servicios que componen una aplicación. La solicitud puede iniciarla un usuario o una aplicación. Cada unidad de trabajo de un `trace` se denomina `span`; un `trace` es un árbol de intervalos. Un `trace` está formado por un intervalo raíz único y cualquier cantidad de intervalos secundarios.

### <a name="span"></a>Intervalo

Los intervalos son objetos que representan el trabajo realizado por servicios o componentes individuales implicados en una solicitud a medida que avanza por un sistema. Cada `span` contiene un `span context`, que es un conjunto de identificadores únicos globales que representan a la solicitud única de la que forma parte cada intervalo. 

Los intervalos encapsulan lo siguiente:

* El nombre del intervalo.
* Un elemento `SpanContext` inmutable que identifica de forma única al intervalo.
* Un intervalo primario en forma de `Span`, `SpanContext`o NULL.
* `SpanKind`.
* Una marca de tiempo de inicio.
* Una marca de tiempo de finalización.
* [`Attributes`](#attributes)
* Una lista de eventos con marca de tiempo.
* Objeto `Status`.

Por lo general, el ciclo de vida de un intervalo es similar al siguiente:

* Un servicio recibe una solicitud. El contexto del intervalo se extrae de los encabezados de solicitud, si está disponible.
* Un nuevo intervalo se crea como elemento secundario del contexto del intervalo extraído. Si no existe ninguno, se crea un nuevo intervalo raíz.
* El servicio controla la solicitud. Se agregan atributos y eventos adicionales al intervalo que son útiles para comprender el contexto de la solicitud, como el nombre de host del equipo que controla la solicitud, o los identificadores de cliente.
* Se pueden crear nuevos intervalos para representar el trabajo realizado por los subcomponentes del servicio.
* Cuando el servicio realiza una llamada remota a otro servicio, el contexto de intervalo actual se serializa y se reenvía al siguiente servicio al insertar el contexto de intervalo en los encabezados o en el sobre de mensaje.
* El trabajo que está realizando el servicio finaliza correctamente o no. El estado del intervalo se establece correctamente y dicho intervalo se marca como finalizado.

### <a name="attributes"></a>Atributos

`Attributes` son una lista de cero o más pares clave-valor que están encapsulados en un elemento `span`. Un elemento Attribute debe tener las siguientes propiedades:

La clave de atributo, que debe ser una cadena que no sea NULL ni esté vacía.
El valor del atributo, que puede ser:
* Un valor de tipo primitivo: cadena, booleano, punto flotante de precisión doble (IEEE 754-1985) o un entero de 64 bits con signo.
* Una matriz de valores de tipo primitivo. La matriz debe ser homogénea; es decir, no debe contener valores de tipos diferentes. En el caso de los protocolos que no admiten de manera nativa valores de matriz, tales valores deben representarse como cadenas JSON.

## <a name="supported-processors"></a>Procesadores compatibles:
 * Procesador de atributos
 * Procesador de intervalos

## <a name="to-get-started"></a>Inicio

Cree un archivo de configuración denominado `applicationinsights.json` y colóquelo en el mismo directorio que `applicationinsights-agent-***.jar`, con la siguiente plantilla.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        ...
      },
      {
        "type": "attribute",
        ...
      },
      {
        "type": "span",
        ...
      }
    ]
  }
}
```

## <a name="includeexclude-spans"></a>Intervalos de inclusión o exclusión

El procesador de atributos y el procesador de intervalos exponen la opción de proporcionar un conjunto de propiedades de un intervalo con el que coincidir, con el fin de determinar si el intervalo debe incluirse o excluirse del procesador de telemetría. Para configurar esta opción, en `include` o `exclude`, al menos son necesarios un elemento `matchType` y uno de estos dos: `spanNames` o `attributes`. La configuración de inclusión o exclusión se admite para tener más de una condición especificada. Todas las condiciones especificadas deben evaluarse en true para que se produzca una coincidencia. 

**Campo obligatorio**: 
* `matchType` controla cómo se interpretan los elementos de las matrices `spanNames` y `attributes`. Los valores posibles son `regexp` o `strict`. 

**Campos opcionales**: 
* `spanNames` debe coincidir con al menos uno de los elementos. 
* `attributes` especifica la lista de atributos con la que coincidir. Todos estos atributos deben coincidir exactamente para que se produzca una coincidencia.

> [!NOTE]
> Si se especifican `include` y `exclude`, se comprobarán las propiedades `include` antes que las propiedades `exclude`.

#### <a name="sample-usage"></a>Ejemplo de uso

```json

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
```
Para más información, consulte la documentación sobre [ejemplos del procesador de telemetría](./java-standalone-telemetry-processors-examples.md).

## <a name="attribute-processor"></a>Procesador de atributos 

El procesador de atributos modifica los atributos de un intervalo. Opcionalmente, admite la capacidad de incluir o excluir intervalos. Toma una lista de acciones que se realizan en el orden especificado en el archivo de configuración. Las acciones admitidas son:

### `insert`

Inserta un nuevo atributo en intervalos en los que la clave aún no existe.   

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "value1",
        "action": "insert"
      },
    ]
  }
]
```
Se requieren los siguientes elementos para la acción `insert`:
  * `key`
  * uno de estos valores: `value`o `fromAttribute`
  * `action`:`insert`

### `update`

actualiza un atributo en intervalos en los que la clave existe

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "newValue",
        "action": "update"
      },
    ]
  }
]
```
Se requieren los siguientes elementos para la acción `update`.
  * `key`
  * uno de estos valores: `value`o `fromAttribute`
  * `action`:`update`


### `delete` 

elimina un atributo de un intervalo

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "delete"
      },
    ]
  }
]
```
Se requieren los siguientes elementos para la acción `delete`.
  * `key`
  * `action`: `delete`

### `hash`

aplica el algoritmo hash (SHA1) a un valor de atributo existente

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "hash"
      },
    ]
  }
]
```
Se requieren los siguientes elementos para la acción `hash`.
* `key`
* `action` : `hash`

### `extract`

> [!NOTE]
> Esta característica solo está disponible en la versión 3.0.1 y versiones posteriores.

Usa una regla de expresión regular para extraer valores de la clave de entrada a las claves de destino especificadas en la regla. Si ya existe una de las claves de destino, se anulará. Se comporta de manera similar a la configuración `toAttributes` del [procesador de intervalo](#extract-attributes-from-span-name) con el atributo existente como origen.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "pattern": "<regular pattern with named matchers>",
        "action": "extract"
      },
    ]
  }
]
```
Se requieren los siguientes elementos para la acción `extract`.
* `key`
* `pattern`
* `action` : `extract`

Para más información, consulte la documentación sobre [ejemplos del procesador de telemetría](./java-standalone-telemetry-processors-examples.md).

## <a name="span-processors"></a>Procesadores de intervalos

El procesador de intervalos modifica el nombre del intervalo o los atributos de un intervalo en función del nombre del intervalo. Opcionalmente, admite la capacidad de incluir o excluir intervalos.

### <a name="name-a-span"></a>Asignación de un nombre para un intervalo

Se requiere la configuración siguiente como parte de la sección de nombre:

* `fromAttributes`: el valor de atributo de las claves se usa para crear un nuevo nombre en el orden especificado en la configuración. Todas las claves de atributo deben especificarse en el intervalo para que el procesador cambie su nombre.

La configuración siguiente se puede configurar de forma opcional:

* `separator`: una cadena, que se especifica, se usará para dividir los valores
> [!NOTE]
> Si el cambio de nombre depende de los atributos modificados por el procesador de atributos, asegúrese de que el procesador de intervalos se especifica después del procesador de atributos en la especificación de canalización.

```json
"processors": [
  {
    "type": "span",
    "name": {
      "fromAttributes": [
        "attributeKey1",
        "attributeKey2",
      ],
      "separator": "::"
    }
  }
] 
```

### <a name="extract-attributes-from-span-name"></a>Extracción de atributos del nombre del intervalo

Toma una lista de expresiones regulares con las que va a coincidir el nombre del intervalo y para extraer atributos de la misma en función de las subexpresiones. Se debe especificar en la sección `toAttributes`.

Se requiere la siguiente configuración:

`rules`: una lista de reglas para extraer valores de atributo del nombre del intervalo. Los valores del nombre del intervalo se reemplazan por los nombres de atributo extraídos. Cada regla de la lista es una cadena de patrón de expresión regular. El nombre del intervalo se comprueba con la expresión regular. Si la expresión regular coincide, todas las subexpresiones con nombre de la expresión regular se extraen como atributos y se agregan al intervalo. Cada nombre de subexpresión se convierte en un nombre de atributo y la parte que coincide con la subexpresión se convierte en el valor de atributo. La parte coincidente del nombre del intervalo se reemplaza por el nombre de atributo extraído. Si los atributos ya existen en el intervalo, se sobrescribirán. El proceso se repite para todas las reglas en el orden en que se especifican. Cada regla subsiguiente funciona en el nombre del intervalo que es la salida después de procesar la regla anterior.

```json

"processors": [
  {
    "type": "span",
    "name": {
      "toAttributes": {
        "rules": [
          "rule1",
          "rule2",
          "rule3"
        ]
      }
    }
  }
]

```

## <a name="list-of-attributes"></a>Lista de atributos

A continuación se muestra una lista de algunos atributos de intervalo comunes que se pueden usar en los procesadores de telemetría.

### <a name="http-spans"></a>Intervalos HTTP

| Atributo  | Tipo | Descripción | 
|---|---|---|
| `http.method` | string | Método de solicitud HTTP.|
| `http.url` | string | URL de solicitud HTTP completa con el formato `scheme://host[:port]/path?query[#fragment]`. Normalmente, el fragmento no se transmite a través de HTTP, aunque si se conoce, debe estar incluido.|
| `http.status_code` | number | [Código de estado de respuesta HTTP](https://tools.ietf.org/html/rfc7231#section-6).|
| `http.flavor` | string | Tipo de protocolo HTTP usado. |
| `http.user_agent` | string | Valor del encabezado [User-Agent de HTTP](https://tools.ietf.org/html/rfc7231#section-5.5.3) enviado por el cliente. |

### <a name="jdbc-spans"></a>Intervalos de JDBC

| Atributo  | Tipo | Descripción  |
|---|---|---|
| `db.system` | string | Identificador del producto del sistema de administración de bases de datos (DBMS) que se va a usar. |
| `db.connection_string` | string | Cadena de conexión usada para conectarse a la base de datos. Se recomienda quitar las credenciales incrustadas.|
| `db.user` | string | Nombre de usuario para acceder a la base de datos. |
| `db.name` | string | Este atributo se usa para informar sobre el nombre de la base de datos a la que se accederá. En el caso de los comandos que cambian la base de datos, debe establecerse en la base de datos de destino (incluso si se produce un error en el comando).|
| `db.statement` | string | Instrucción de base de datos que se ejecutará.|