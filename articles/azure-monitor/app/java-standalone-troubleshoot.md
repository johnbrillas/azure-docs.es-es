---
title: 'Solución de problemas: Application Insights de Azure Monitor para Java'
description: Solución de problemas de Application Insights de Azure Monitor para Java
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-java
ms.openlocfilehash: cf27763f857cc1fd1aad5256d0c6cecf91251caf
ms.sourcegitcommit: 48cb2b7d4022a85175309cf3573e72c4e67288f5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96855569"
---
# <a name="troubleshooting-azure-monitor-application-insights-java"></a>Solución de problemas de Application Insights de Azure Monitor para Java

En este artículo se tratan algunos de los problemas habituales a los que podría enfrentarse un usuario al instrumentar la aplicación Java mediante el agente de Java y se ofrecen los pasos para resolver estos problemas.

## <a name="self-diagnostic-log-file"></a>Archivo de registro de autodiagnóstico

De manera predeterminada, Application Insights para Java 3.0 genera un archivo de registro denominado `applicationinsights.log` en el mismo directorio donde se encuentra el archivo `applicationinsights-agent-3.0.0.jar`.

Este archivo de registro es la primera ubicación en la que buscar sugerencias sobre cualquier problema que se experimente.

## <a name="upgrade-from-application-insights-java-2x-sdk"></a>Actualización del SDK de Application Insights para Java 2.x

Vea [Actualización desde la versión 2.x del SDK](./java-standalone-upgrade-from-2x.md).

## <a name="upgrade-from-30-preview"></a>Actualización de la versión preliminar 3.0

Si va a actualizar desde la versión preliminar 3.0, revise todas las [opciones de configuración](./java-standalone-config.md) cuidadosamente, ya que la estructura JSON ha cambiado por completo en la versión de disponibilidad general 3.0.

Entre estos cambios se incluyen los siguientes:

1.  El propio nombre del archivo de configuración ha cambiado de `ApplicationInsights.json` a `applicationinsights.json`.
2.  El nodo `instrumentationSettings` ya no está presente. Todo el contenido de `instrumentationSettings` se ha movido al nivel raíz. 
3.  Los nodos de configuración, como `sampling`, `jmxMetrics`, `instrumentation` y `heartbeat`, se han movido de `preview` al nivel raíz.

## <a name="ssl-certificate-issues"></a>Problemas de certificados SSL

Si usa el almacén de claves de Java predeterminado, este ya tiene todos los certificados raíz de la entidad de certificación y no es necesario importar más certificados SSL.

Si usa un almacén de claves de Java personalizado, es posible que tenga que importar los certificados SSL de punto de conexión de Application Insights.

### <a name="some-key-terminology"></a>Alguna terminología clave:
Un *almacén de claves* es un repositorio de certificados, claves públicas y privadas. Normalmente, las distribuciones de JDK tienen un ejecutable para administrarlos: `keytool`.

El ejemplo siguiente es un comando simple para importar un certificado SSL en el almacén de claves:

`keytool -importcert -alias your_ssl_certificate -file "your downloaded SSL certificate name".cer -keystore "Your KeyStore name" -storepass "Your keystore password" -noprompt`

### <a name="steps-to-download-and-add-the-ssl-certificate"></a>Pasos para descargar y agregar el certificado SSL:

1.  Abra el explorador que prefiera y vaya a la dirección URL de `IngestionEndpoint` presente en la cadena de conexión que se usa para instrumentar la aplicación, como se muestra a continuación

    :::image type="content" source="media/java-ipa/troubleshooting/ingestion-endpoint-url.png" alt-text="Cadena de conexión a Application Insights":::

2.  Haga clic en el icono "Ver información del sitio" (candado) del explorador y luego en la opción "Certificado", como se muestra a continuación

    :::image type="content" source="media/java-ipa/troubleshooting/certificate-icon-capture.png" alt-text="Captura de certificado SSL":::

3.  Vaya a la pestaña Detalles y haga clic en Copiar en archivo.
4.  Haga clic en el botón Siguiente, seleccione el formato "X.509 codificado base 64 (.CER)" y luego Siguiente.

    :::image type="content" source="media/java-ipa/troubleshooting/certificate-export-wizard.png" alt-text="SSL Certificate ExportWizard":::

5.  Especifique el archivo en el que quiere guardar el certificado SSL. Por último, haga clic en Siguiente y termine. Debe ver el mensaje "La exportación se realizó correctamente".
6.  Una vez que tenga el certificado, es hora de importarlo en un almacén de claves de Java. Use el [comando](#some-key-terminology) anterior para importar certificados.

> [!WARNING]
> Tiene que repetir estos pasos para obtener el nuevo certificado antes de que expire el actual. Puede encontrar la información de expiración en la pestaña "Detalles" del menú emergente Certificado, como se muestra a continuación

:::image type="content" source="media/java-ipa/troubleshooting/certificate-details.png" alt-text="Detalles del certificado SSL":::
