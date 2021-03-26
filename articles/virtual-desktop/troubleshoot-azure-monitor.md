---
title: Solución de problemas de Azure Monitor para Windows Virtual Desktop (versión preliminar) - Azure
description: Solución de problemas con Azure Monitor para Windows Virtual Desktop.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 12/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: c335c1cf7e5319b812345714dbdc6b87ddc4e81b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101709179"
---
# <a name="troubleshoot-azure-monitor-for-windows-virtual-desktop-preview"></a>Solución de problemas de Azure Monitor para Windows Virtual Desktop (versión preliminar)

>[!IMPORTANT]
>Azure Monitor para Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se presentan problemas conocidos y soluciones para problemas comunes en Azure Monitor para Windows Virtual Desktop (versión preliminar).

## <a name="issues-with-configuration-and-setup"></a>Problemas con la configuración y la instalación

Si el libro de configuración no funciona correctamente para automatizar la instalación, puede usar estos recursos para configurar el entorno manualmente:

- Para habilitar manualmente los diagnósticos o acceder al área de trabajo de Log Analytics, consulte [Envío de diagnósticos de Windows Virtual Desktop a Log Analytics](diagnostics-log-analytics.md).
- Para instalar manualmente la extensión de Log Analytics en un host, consulte [Extensión de máquina virtual de Log Analytics para Windows](../virtual-machines/extensions/oms-windows.md).
- Para configurar una nueva área de trabajo de Log Analytics, consulte [Creación de un área de trabajo de Log Analytics en Azure Portal](../azure-monitor/logs/quick-create-workspace.md).
- Para obtener más información sobre cómo leer contadores de rendimiento, consulte [Configuración de contadores de rendimiento](../azure-monitor/agents/data-sources-performance-counters.md).
- Para configurar eventos para un área de trabajo de Log Analytics, consulte [Recopilación de orígenes de datos del registro de eventos de Windows con el agente de Log Analytics](../azure-monitor/agents/data-sources-windows-events.md).

## <a name="my-data-isnt-displaying-properly"></a>Los datos no se muestran correctamente

Si los datos no se muestran correctamente, compruebe la configuración y los permisos, y que las direcciones IP requeridas estén desbloqueadas. 

- En primer lugar, asegúrese de que ha rellenado todos los campos del libro de configuración tal como se describe en [Uso de Azure Monitor para Windows Virtual Desktop para la supervisión de implementaciones](azure-monitor.md). Si faltan contadores o eventos, los datos asociados a ellos no se mostrarán en Azure Portal.

- Compruebe los permisos de acceso y póngase en contacto con los propietarios de los recursos para solicitar los permisos que faltan; cualquier usuario que supervise Windows Virtual Desktop necesita los siguientes permisos:

    - Acceso de lectura a las suscripciones de Azure que contienen los recursos de Windows Virtual Desktop
    - Acceso de lectura a los grupos de recursos de la suscripción que contienen los hosts de la sesión de Windows Virtual Desktop 
    - Acceso de lectura al área de trabajo de Log Analytics

- Es posible que tenga que abrir los puertos de salida en el firewall del servidor para permitir que Azure Monitor envíe datos al portal. Consulte [Puertos de salida](../azure-monitor/app/ip-addresses.md). 

- ¿No ve los datos de la actividad reciente? Puede esperar 15 minutos y actualizar la fuente. Azure Monitor tiene un período de latencia de 15 minutos para rellenar los datos de registro. Para más información, consulte [Tiempo de la ingesta de datos de registro en Azure Monitor](../azure-monitor/logs/data-ingestion-time.md).

Si no le falta información, pero los datos todavía no se muestran correctamente, puede haber un problema en la consulta o en los orígenes de datos. Revise los problemas conocidos y las limitaciones. 

## <a name="i-want-to-customize-azure-monitor-for-windows-virtual-desktop"></a>Quiero personalizar Azure Monitor para Windows Virtual Desktop

Azure Monitor para Windows Virtual Desktop usa los libros de Azure Monitor. Los libros permiten guardar una copia de la plantilla de libro de Windows Virtual Desktop y realizar sus propias personalizaciones.

Por diseño, las plantillas de libro personalizadas no adoptarán automáticamente las actualizaciones del grupo de productos. Para obtener más información, consulte [Solución de problemas de conclusiones basadas en libros](../azure-monitor/insights/troubleshoot-workbooks.md) y la [información general sobre libros](../azure-monitor/visualize/workbooks-overview.md).

## <a name="i-cant-interpret-the-data"></a>No puedo interpretar los datos

Obtenga más información sobre los términos de datos en el [glosario de Azure Monitor para Windows Virtual Desktop](azure-monitor-glossary.md).

## <a name="the-data-i-need-isnt-available"></a>Los datos que necesito no están disponibles

Si desea supervisar más contadores de rendimiento o eventos, puede habilitarlos para que los envíen al área de trabajo de Log Analytics y los supervisen en diagnósticos de host: explorador del host. 

- Para agregar contador de rendimiento, consulte [Configuración de contadores de rendimiento](../azure-monitor/agents/data-sources-performance-counters.md#configuring-performance-counters).
- Para agregar eventos de Windows, consulte [Configuración de registros de eventos de Windows](../azure-monitor/agents/data-sources-windows-events.md#configuring-windows-event-logs)

¿No encuentra un punto de datos para diagnosticar un problema? ¡Envíenos sus comentarios!

- Para obtener información sobre cómo dejar comentarios, consulte [Información general, comentarios y soporte técnico para la solución de problemas de Windows Virtual Desktop](troubleshoot-set-up-overview.md).
- También puede dejar comentarios sobre Windows Virtual Desktop en el [concentrador de comentarios de Windows Virtual Desktop](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app) o en nuestro [foro UserVoice](https://windowsvirtualdesktop.uservoice.com/forums/921118-general).

## <a name="known-issues-and-limitations"></a>Limitaciones y problemas conocidos

Estos son los problemas y las limitaciones que conocemos actualmente, y estamos trabajando para solucionarlos:

- Solo puede supervisar un grupo de hosts a la vez. 

- Para guardar los parámetros de configuración favoritos, debe guardar una plantilla personalizada del libro. Las plantillas personalizadas no adoptarán automáticamente las actualizaciones del grupo de productos.

- Algunos mensajes de error no están escritos de manera descriptiva y no todos los mensajes de error están descritos en la documentación.

- El contador de rendimiento de sesiones totales puede contar sesiones en exceso en un número reducido, y puede parecer que las sesiones totales superan el límite máximo de sesiones.

- El número de sesiones disponibles no refleja las directivas de escalado en el grupo de hosts. 
    
- Aunque es poco frecuente, es posible que el evento de finalización de una conexión falte, lo que puede afectar a algunos objetos visuales como las conexiones con el tiempo y al estado de conexión del usuario.  
    
- El libro de configuración solo admite la configuración de hosts en la misma región que su grupo de recursos. 

- El tiempo de conexión incluye el tiempo que tardan los usuarios en escribir sus credenciales. Esto se correlaciona con la experiencia, pero en algunos casos puede mostrar picos falsos. 
    

## <a name="next-steps"></a>Pasos siguientes

Si no está seguro de cómo interpretar los datos o quiere obtener más información sobre los términos comunes, consulte el [glosario de Azure Monitor para Windows Virtual Desktop](azure-monitor-glossary.md). Si quiere obtener información sobre cómo configurar y usar Azure Monitor para Windows Virtual Desktop, consulte [Uso de Azure Monitor para Windows Virtual Desktop para la supervisión de implementaciones](azure-monitor.md).