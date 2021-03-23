---
title: 'Opciones de configuración: Application Insights de Azure Monitor para Java'
description: Configuración de Application Insights de Azure Monitor para Java
ms.topic: conceptual
ms.date: 11/04/2020
author: MS-jgol
ms.custom: devx-track-java
ms.author: jgol
ms.openlocfilehash: 32b1558bf4af2ee151fef33a8c0cbe7df82f1e84
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102201760"
---
# <a name="configuration-options---azure-monitor-application-insights-for-java"></a>Opciones de configuración: Application Insights de Azure Monitor para Java

> [!WARNING]
> **Si va actualizar desde la versión preliminar 3.0**
>
> Revise todas las opciones de configuración con cuidado, ya que la estructura JSON ha cambiado por completo, además del nombre de archivo, que es en minúsculas.

## <a name="connection-string-and-role-name"></a>Cadena de conexión y nombre de rol

La cadena de conexión y el nombre de rol son las opciones de configuración más comunes que se necesitan para empezar:

```json
{
  "connectionString": "InstrumentationKey=...",
  "role": {
    "name": "my cloud role name"
  }
}
```

La cadena de conexión es obligatoria y el nombre del rol es importante cada vez que se envían datos de diferentes aplicaciones al mismo recurso de Application Insights.

Encontrará más detalles y opciones de configuración adicionales a continuación.

## <a name="configuration-file-path"></a>Ruta del archivo de configuración

De forma predeterminada, Application Insights Java 3.0 espera que el archivo de configuración se denomine `applicationinsights.json` y que se encuentre en el mismo directorio que `applicationinsights-agent-3.0.2.jar`.

Puede especificar la ruta de acceso a su propio archivo de configuración mediante

* la variable de entorno `APPLICATIONINSIGHTS_CONFIGURATION_FILE`, o
* la propiedad del sistema Java `applicationinsights.configuration.file`.

Si especifica una ruta de acceso relativa, se resolverá de forma relativa al directorio en el que se encuentra `applicationinsights-agent-3.0.2.jar`.

## <a name="connection-string"></a>Cadena de conexión

Se requiere la cadena de conexión. Puede encontrar la cadena de conexión en el recurso de Application Insights:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Cadena de conexión a Application Insights":::


```json
{
  "connectionString": "InstrumentationKey=..."
}
```

También puede establecer la cadena de conexión mediante la variable de entorno `APPLICATIONINSIGHTS_CONNECTION_STRING`.

Si no se establece la cadena de conexión, se deshabilitará el agente de Java.

## <a name="cloud-role-name"></a>Nombre del rol en la nube

Este se usa para etiquetar el componente en el mapa de aplicación.

Si quiere establecer el nombre del rol de nube, haga lo siguiente:

```json
{
  "role": {   
    "name": "my cloud role name"
  }
}
```

Si no se establece el nombre del rol en la nube, se usará el nombre del recurso de Application Insights para etiquetar el componente en el mapa de aplicación.

También puede establecer el nombre de rol en la nube mediante la variable de entorno `APPLICATIONINSIGHTS_ROLE_NAME`.

## <a name="cloud-role-instance"></a>Instancia de rol en la nube

La instancia de rol de la nube tiene como valor predeterminado el nombre del equipo.

Si quiere establecer la instancia de rol de nube en un valor diferente en lugar del nombre del equipo, use lo siguiente:

```json
{
  "role": {
    "name": "my cloud role name",
    "instance": "my cloud role instance"
  }
}
```

También puede establecer la instancia de rol en la nube mediante la variable de entorno `APPLICATIONINSIGHTS_ROLE_INSTANCE`.

## <a name="sampling"></a>muestreo

El muestreo resulta útil si necesita reducir costos.
Este se realiza como una función en el id. de operación (también conocido como id. de seguimiento), por lo que el mismo id. de operación siempre dará como resultado la misma decisión de muestreo. Esto garantiza que no se realizará el muestreo de algunas partes de una transacción distribuida mientras se muestrean otras.

Por ejemplo, si establece el muestreo en 10 %, solo verá el 10 % de sus transacciones, pero cada una de ese 10 % tendrá los detalles completos de las transacciones de un extremo a otro.

A continuación se muestra un ejemplo de cómo establecer el muestreo para capturar aproximadamente **1/3 de todas las transacciones**. Asegúrese de establecer la velocidad de muestreo correcta para su caso de uso:

```json
{
  "sampling": {
    "percentage": 33.333
  }
}
```

También puede establecer el porcentaje de muestreo mediante la variable de entorno `APPLICATIONINSIGHTS_SAMPLING_PERCENTAGE`.

> [!NOTE]
> Para el porcentaje de muestreo, elija un porcentaje que esté cerca de 100/N, donde N es un número entero. Actualmente el muestreo no es compatible con otros valores.

## <a name="jmx-metrics"></a>Métricas JMX

Si quiere recopilar algunas métricas de JMX adicionales:

```json
{
  "jmxMetrics": [
    {
      "name": "JVM uptime (millis)",
      "objectName": "java.lang:type=Runtime",
      "attribute": "Uptime"
    },
    {
      "name": "MetaSpace Used",
      "objectName": "java.lang:type=MemoryPool,name=Metaspace",
      "attribute": "Usage.used"
    }
  ]
}
```

`name` es el nombre de la métrica que se asignará a esta métrica de JMX (puede ser cualquier nombre).

`objectName` es el [nombre de objeto](https://docs.oracle.com/javase/8/docs/api/javax/management/ObjectName.html) del método JMX de MBean que desea recopilar.

`attribute` es el nombre del método JMX de MBean que desea recopilar.

Se admiten los valores de métrica JMX numéricos y booleanos. Las métricas JMX booleanas se asignan a `0` para false y `1` para true.

[//]: # "NOTA: No se documenta APPLICATIONINSIGHTS_JMX_METRICS aquí"
[//]: # "JSON insertado en env var es confuso y solo debe documentarse para el escenario de conexión sin código"

## <a name="custom-dimensions"></a>Dimensiones personalizadas

Si desea agregar dimensiones personalizadas a toda la telemetría:

```json
{
  "customDimensions": {
    "mytag": "my value",
    "anothertag": "${ANOTHER_VALUE}"
  }
}
```

`${...}` se puede usar para leer el valor de la variable de entorno especificada en el inicio.

> [!NOTE]
> A partir de la versión 3.0.2, si agrega una dimensión personalizada denominada `service.version`, el valor se almacenará en la columna `application_Version` de la tabla de registros de Application Insights en lugar de como una dimensión personalizada.

## <a name="telemetry-processors-preview"></a>Procesadores de telemetría (versión preliminar)

Esta característica se encuentra en su versión preliminar.

Permite configurar reglas que se aplicarán a la telemetría de solicitudes, dependencias y seguimientos; por ejemplo:
 * Enmascarar datos confidenciales
 * Agregan dimensiones personalizadas condicionalmente
 * Actualizar el nombre de telemetría que se usa para la agregación y visualización

Para obtener más información, consulte la documentación del [procesador de telemetría](./java-standalone-telemetry-processors.md).

## <a name="auto-collected-logging"></a>Registros recopilados automáticamente

Log4j, Logback y java.util.logging se instrumentan automáticamente y los registros creados mediante estas plataformas de registro se recopilan automáticamente.

Los registros solo se capturan si cumple primero el umbral configurado de las plataformas de registro, y si el segundo también cumple el umbral configurado de Application Insights.

El umbral predeterminado de Application Insights es `INFO`. Si quiere cambiar este nivel:

```json
{
  "instrumentation": {
    "logging": {
      "level": "WARN"
    }
  }
}
```

También puede establecer el umbral mediante la variable de entorno `APPLICATIONINSIGHTS_INSTRUMENTATION_LOGGING_LEVEL`.

A continuación se muestran los valores `level` válidos que puede especificar en el archivo `applicationinsights.json` y cómo se corresponden con los niveles de registro en diferentes plataformas de registro:

| level             | Log4j  | Logback | JUL     |
|-------------------|--------|---------|---------|
| Apagado               | Apagado    | Apagado     | Apagado     |
| FATAL             | FATAL  | ERROR   | SEVERE  |
| ERROR (o SEVERE) | ERROR  | ERROR   | SEVERE  |
| WARN (o WARNING) | WARN   | WARN    | WARNING |
| INFO              | INFO   | INFO    | INFO    |
| CONFIG            | DEBUG  | DEBUG   | CONFIG  |
| DEBUG (o FINE)   | DEBUG  | DEBUG   | FINE    |
| FINER             | DEBUG  | DEBUG   | FINER   |
| TRACE (o FINEST) | TRACE  | TRACE   | FINEST  |
| ALL               | ALL    | ALL     | ALL     |

> [!NOTE]
> Si se pasa un objeto de excepción al registrador, el mensaje de registro (y los detalles del objeto de excepción) se mostrarán en Azure Portal en la tabla `exceptions`, en lugar de la tabla `traces`.

## <a name="auto-collected-micrometer-metrics-including-spring-boot-actuator-metrics"></a>Métricas de Micrometer recopiladas automáticamente (incluidas las métricas del accionador de Spring Boot)

Si la aplicación usa [Micrometer](https://micrometer.io), las métricas que se envían al registro global de Micrometer se recopilan automáticamente.

Además, si la aplicación usa el [accionador de Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html), las métricas configuradas por dicho accionador también se recopilan automáticamente.

Para deshabilitar la recopilación automática de métricas de Micrometer (incluidas las métricas del accionador de Spring Boot):

> [!NOTE]
> Las métricas personalizadas se facturan por separado y pueden generar costos adicionales. Asegúrese de consultar la [información detallada sobre los precios](https://azure.microsoft.com/pricing/details/monitor/). Para deshabilitar las métricas de Micrometer y del accionador de Spring Boot, agregue la siguiente configuración al archivo de configuración.

```json
{
  "instrumentation": {
    "micrometer": {
      "enabled": false
    }
  }
}
```

## <a name="suppressing-specific-auto-collected-telemetry"></a>Supresión de la telemetría específica recopilada automáticamente

A partir de la versión 3.0.2, se puede suprimir la telemetría específica recopilada automáticamente mediante estas opciones de configuración:

```json
{
  "instrumentation": {
    "cassandra": {
      "enabled": false
    },
    "jdbc": {
      "enabled": false
    },
    "kafka": {
      "enabled": false
    },
    "micrometer": {
      "enabled": false
    },
    "mongo": {
      "enabled": false
    },
    "redis": {
      "enabled": false
    }
  }
}
```

## <a name="heartbeat"></a>Latido

De forma predeterminada, Application Insights Java 3.0 envía una métrica de latido cada 15 minutos. Si está usando la métrica de latido para desencadenar las alertas, puede aumentar la frecuencia de este latido:

```json
{
  "heartbeat": {
    "intervalSeconds": 60
  }
}
```

> [!NOTE]
> No puede reducir la frecuencia del latido, ya que los datos de dicho latido también se usan para supervisar el uso de Application Insights.

## <a name="http-proxy"></a>Proxy HTTP

Si su aplicación está protegida por un firewall y no puede conectarse directamente a Application Insights (consulte [Direcciones IP que emplea Application Insights](./ip-addresses.md)), puede configurar Application Insights Java 3.0 para que use un proxy HTTP:

```json
{
  "proxy": {
    "host": "myproxy",
    "port": 8080
  }
}
```

Application Insights Java 3.0 también respeta `-Dhttps.proxyHost` y `-Dhttps.proxyPort` globales si se establecen.

[//]: # "Tenga en cuenta que la compatibilidad con OpenTelemetry está en versión preliminar privada hasta que la API de OpenTelemetry alcance 1.0"

[//]: # "# # Compatibilidad con las versiones anteriores a la 1.0 de la API de OpenTelemetry."

[//]: # "La compatibilidad con las versiones anteriores a la 1.0 de la API de OpenTelemetry es opcional, porque la API de OpenTelemetry no es estable todavía."
[//]: # "Cada versión del agente solo admite versiones anteriores a la 1.0 de la API de OpenTelemetry."
[//]: # "Esta limitación no se produce una vez que se lance la API de OpenTelemetry 1.0."

[//]: # "```json"
[//]: # "{"
[//]: # "  \"preview\": {"
[//]: # "    \"openTelemetryApiSupport\": true"
[//]: # "  }"
[//]: # "}"
[//]: # "```"

## <a name="self-diagnostics"></a>Diagnóstico automático

"Diagnóstico automático" hace referencia al registro interno de Application Insights Java 3.0.

Esta funcionalidad puede ser útil para detectar y diagnosticar problemas con Application Insights.

De forma predeterminada, Application Insights Java 3.0 registra en el nivel `INFO` tanto en el archivo `applicationinsights.log` como en la consola, que se corresponde con esta configuración:

```json
{
  "selfDiagnostics": {
    "destination": "file+console",
    "level": "INFO",
    "file": {
      "path": "applicationinsights.log",
      "maxSizeMb": 5,
      "maxHistory": 1
    }
  }
}
```

`destination` puede ser uno de `file`, `console` o `file+console`.

`level` puede ser uno de `OFF`, `ERROR`, `WARN`, `INFO`, `DEBUG` o `TRACE`.

`path` incluye una ruta de acceso absoluta o relativa. Las rutas de acceso relativas se resuelven en el directorio donde se encuentra `applicationinsights-agent-3.0.2.jar`.

`maxSizeMb` es el tamaño máximo del archivo de registro antes de que se revierta.

`maxHistory` es el número de archivos de registro revertidos que se conservan (además del archivo de registro actual).

A partir de la versión 3.0.2, también puede establecer el `level` de diagnóstico automático mediante la variable de entorno `APPLICATIONINSIGHTS_SELF_DIAGNOSTICS_LEVEL`.

## <a name="an-example"></a>Un ejemplo

Este es solo un ejemplo para mostrar el aspecto de un archivo de configuración con varios componentes.
Configure opciones específicas en función de sus necesidades.

```json
{
  "connectionString": "InstrumentationKey=...",
  "role": {
    "name": "my cloud role name"
  },
  "sampling": {
    "percentage": 100
  },
  "jmxMetrics": [
  ],
  "customDimensions": {
  },
  "instrumentation": {
    "logging": {
      "level": "INFO"
    },
    "micrometer": {
      "enabled": true
    }
  },
  "proxy": {
  },
  "preview": {
    "processors": [
    ]
  },
  "selfDiagnostics": {
    "destination": "file+console",
    "level": "INFO",
    "file": {
      "path": "applicationinsights.log",
      "maxSizeMb": 5,
      "maxHistory": 1
    }
  }
}
```
