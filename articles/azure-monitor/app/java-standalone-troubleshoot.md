---
title: Solución de problemas de Application Insights de Azure Monitor para Java
description: Obtenga información sobre cómo solucionar problemas del agente de Java para Application Insights de Azure Monitor
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-java
ms.openlocfilehash: 2876abd3749c9e56cef462e41b8268135f82cd12
ms.sourcegitcommit: c7153bb48ce003a158e83a1174e1ee7e4b1a5461
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98232223"
---
# <a name="troubleshooting-guide-azure-monitor-application-insights-for-java"></a>Guía de solución de problemas: Application Insights de Azure Monitor para Java

En este artículo se tratan algunos de los problemas comunes que podría encontrar al instrumentar una aplicación Java mediante el agente de Java para Application Insights. También se explican los pasos para resolver estos problemas. Application Insights es una característica del servicio de plataforma Azure Monitor.

## <a name="check-the-self-diagnostic-log-file"></a>Comprobación del archivo de registro de autodiagnóstico

De forma predeterminada, el agente para Java 3.0 para Application Insights genera un archivo de registro denominado `applicationinsights.log` en el mismo directorio que contiene el archivo `applicationinsights-agent-3.0.1.jar`.

Este archivo de registro es la primera ubicación en la que buscar sugerencias sobre cualquier problema que se experimente.

## <a name="jvm-fails-to-start"></a>JVM no se inicia

Si JVM no se inicia con el mensaje "Error al abrir el archivo ZIP o falta el manifiesto JAR", intente volver a descargar el archivo JAR del agente, porque puede que se haya dañado durante la transferencia de archivos.

## <a name="upgrade-from-the-application-insights-java-2x-sdk"></a>Actualización del SDK de Application Insights para Java 2.x

Si ya está usando el SDK de Application Insights para Java 2.x en la aplicación, puede seguir usándolo. El agente para Java 3.0 lo detectará. Para obtener más información, consulte [Actualización desde el SDK para Java 2.x](./java-standalone-upgrade-from-2x.md).

## <a name="upgrade-from-application-insights-java-30-preview"></a>Actualización de Application Insights para Java 3.0 (versión preliminar)

Si va a actualizar desde el agente para Java 3.0 (versión preliminar), revise detenidamente todas las [opciones de configuración](./java-standalone-config.md). La estructura JSON ha cambiado completamente en la versión 3.0 de disponibilidad general (GA).

Entre estos cambios se incluyen los siguientes:

-  El propio nombre del archivo de configuración ha cambiado de `ApplicationInsights.json` a `applicationinsights.json`.
-  El nodo `instrumentationSettings` ya no está presente. Todo el contenido de `instrumentationSettings` se ha movido al nivel raíz. 
-  Los nodos de configuración, como `sampling`, `jmxMetrics`, `instrumentation` y `heartbeat`, se han movido de `preview` al nivel raíz.

## <a name="some-logging-is-not-auto-collected"></a>Algunos registros no se recopilan automáticamente

Los registros solo se capturan si cumple primero el umbral configurado de las plataformas de registro, y si el segundo también cumple el umbral configurado de Application Insights.

La mejor manera de saber si una instrucción de registro determinada cumple el umbral configurado de las plataformas de registro es confirmar que se muestra en el registro de aplicaciones normal (por ejemplo, archivo o consola).

Para obtener más información, consulte la [configuración de registro de recopilación automática](./java-standalone-config.md#auto-collected-logging).

## <a name="import-ssl-certificates"></a>Importación de certificados SSL

Si utiliza el almacén de claves de Java predeterminado, ya tendrá todos los certificados raíz de CA. No debe importar más certificados SSL.

Si usa un almacén de claves de Java personalizado, es posible que tenga que importar los certificados SSL de punto de conexión de Application Insights.

### <a name="key-terminology"></a>Terminología clave
Un *almacén de claves* es un repositorio de certificados, claves públicas y privadas. Normalmente, las distribuciones de Java Development Kit tienen un ejecutable para administrarlas: `keytool`.

El ejemplo siguiente es un comando simple para importar un certificado SSL en el almacén de claves:

`keytool -importcert -alias your_ssl_certificate -file "your downloaded SSL certificate name".cer -keystore "Your KeyStore name" -storepass "Your keystore password" -noprompt`

### <a name="steps-to-download-and-add-an-ssl-certificate"></a>Pasos para descargar y agregar un certificado SSL

1.  Abra el explorador que prefiera y vaya a la dirección URL de `IngestionEndpoint` presente en la cadena de conexión que se usa para instrumentar la aplicación.

    :::image type="content" source="media/java-ipa/troubleshooting/ingestion-endpoint-url.png" alt-text="Captura de pantalla que muestra una cadena de conexión de Application Insights.":::

2.  Seleccione el icono de **Ver información del sitio** (candado) en el explorador y, a continuación, seleccione la opción **Certificado**.

    :::image type="content" source="media/java-ipa/troubleshooting/certificate-icon-capture.png" alt-text="Captura de pantalla de la opción Certificado en Ver información del sitio.":::

3.  Vaya a la pestaña **Detalles** y seleccione **Copiar en archivo**.
4.  Seleccione el botón **Siguiente**, luego el formato **X.509 codificado base 64 (.CER)** formato y, a continuación, vuelva a seleccionar **Siguiente**.

    :::image type="content" source="media/java-ipa/troubleshooting/certificate-export-wizard.png" alt-text="Captura de pantalla del Asistente para exportación de certificados, con un formato seleccionado.":::

5.  Especifique el archivo en el que quiere guardar el certificado SSL. Después, seleccione **Siguiente** > **Finalizar**. Debería ver el mensaje "La exportación se realizó correctamente".
6.  Una vez que tenga el certificado, es hora de importarlo en un almacén de claves de Java. Use el [comando anterior](#key-terminology) para importar certificados.

> [!WARNING]
> Tiene que repetir estos pasos para obtener el nuevo certificado antes de que expire el actual. Puede encontrar la información de expiración en la pestaña **Detalles** del cuadro de diálogo **Certificado**.
>
> :::image type="content" source="media/java-ipa/troubleshooting/certificate-details.png" alt-text="Captura de pantalla que muestra los detalles del certificado SSL.":::
