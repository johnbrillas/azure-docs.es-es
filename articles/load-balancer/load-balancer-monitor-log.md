---
title: Supervisión de operaciones, eventos y contadores para una instancia pública de Load Balancer
titleSuffix: Azure Load Balancer
description: Obtenga información acerca de cómo habilitar el registro para Azure Load Balancer
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2020
ms.author: allensu
ms.openlocfilehash: 6742723e24df83ac8112e224f1999f116ab82c94
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96572786"
---
# <a name="azure-monitor-logs-for-the-standard-azure-load-balancer"></a>Registros de Azure Monitor para Azure Standard Load Balancer

Puede usar diferentes tipos de registros en Azure para administrar y solucionar problemas de Standard Load Balancer. Los registros se pueden transmitir a un centro de eventos o a un área de trabajo de Log Analytics. Se pueden extraer todos los registros desde Azure Blob Storage y visualizarse en distintas herramientas, como Excel y Power BI.  Puede obtener más información acerca de los diferentes tipos de registros en la lista siguiente.

* **Registros de actividad:** Puede usar [Visualización de registros de actividad para supervisar acciones sobre recursos](../azure-resource-manager/management/view-activity-logs.md) para ver toda la actividad que se está enviado a las suscripciones de Azure y sus estados. Los registros de actividad están habilitados de manera predeterminada y se pueden ver en Azure Portal. Estos registros están disponibles tanto para instancias de Load Balancer de nivel básico como para instancias de nivel estándar.
* **Métricas de Standard Load Balancer:** Puede usar este registro para consultar las métricas exportadas como registros de su instancia de Azure Standard Load Balancer. Estos registros solo están disponible para Standard Load Balancer.

> [!IMPORTANT]
> **Los registros de eventos de sondeo de estado o de alerta de Load Balancer no funcionan en la actualidad y aparecen listados en los [problemas conocidos de Azure Load Balancer](whats-new.md#known-issues).** 

> [!IMPORTANT]
> Los registros solo están disponibles para los recursos implementados en el modelo de implementación del Administrador de recursos. No puede usar los registros de recursos del modelo de implementación clásica. Para más información sobre estos modelos de implementación, consulte [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/management/deployment-models.md) (Descripción de la implementación de Resource Manager y la implementación clásica).

## <a name="enable-logging"></a>Habilitar registro

El registro de actividades se habilita automáticamente para todos los recursos de Resource Manager. Habilite el registro de eventos y de sondeos de estado para iniciar la recopilación de los datos disponibles a través de esos registros. Para habilitar el registro, realice los siguientes pasos.

Inicie sesión en [Azure Portal](https://portal.azure.com). Si aún no tiene un equilibrador de carga, [cree uno](./quickstart-load-balancer-standard-public-portal.md) antes de continuar.

1. En el portal, haga clic en **Grupos de recursos**.
2. Seleccione el grupo de recursos **\<resource-group-name>** donde esté el equilibrador de carga.
3. Seleccione el equilibrador de carga.
4. Seleccione **Registro de actividades** > **Configuración de diagnóstico**.
5. En el panel **Configuración de diagnóstico**, en **Configuración de diagnóstico**, seleccione **+ Agregar configuración de diagnóstico**.
6. En el panel de creación **Configuración de diagnóstico**, escriba **myLBDiagnostics** en el campo **Nombre**.
7. Tiene tres opciones para la **Configuración de diagnóstico**.  Puede elegir una, dos o las tres y configurar cada una de ellas según sus requisitos:
   * **Archivar en una cuenta de almacenamiento**
   * **Transmisión a un centro de eventos**
   * **Enviar a Log Analytics**

    ### <a name="archive-to-a-storage-account"></a>Archivar en una cuenta de almacenamiento
    Necesitará una cuenta de almacenamiento ya creada para este proceso.  Para crear una cuenta de almacenamiento, consulte [Creación de una cuenta de almacenamiento](../storage/common/storage-account-create.md?tabs=azure-portal).

    1. Active la casilla junto a **Archivar en una cuenta de almacenamiento**.
    2. Seleccione **Configurar** para abrir el panel **Selección de una cuenta de almacenamiento**.
    3. Seleccione la **Suscripción** donde se creó la cuenta de almacenamiento en el cuadro desplegable.
    4. Seleccione el nombre de la cuenta de almacenamiento en **Cuenta de almacenamiento** en el cuadro desplegable.
    5. Seleccione Aceptar.

    ### <a name="stream-to-an-event-hub"></a>Transmitir a un centro de eventos
    Necesitará un centro de eventos ya creado para este proceso.  Para crear un centro de eventos, consulte [Inicio rápido: Creación de un centro de eventos mediante Azure Portal](../event-hubs/event-hubs-create.md).

    1. Active la casilla situada junto a **Transmitir a un centro de eventos**.
    2. Seleccione **Configurar** para abrir el panel **Select event hub** (Selección del centro de eventos).
    3. Seleccione la **Suscripción** donde se creó el centro de eventos en el cuadro desplegable.
    4. **Seleccione el espacio de nombres del centro de eventos** en el cuadro desplegable.
    5. **Seleccione el nombre de la directiva del centro de eventos** en el cuadro desplegable.
    6. Seleccione Aceptar.

    ### <a name="send-to-log-analytics"></a>Enviar a Log Analytics
    Deberá tener un área de trabajo de Log Analytics creada y configurada para este proceso.  Para crear un área de trabajo de Log Analytics, consulte [Creación de un área de trabajo de Log Analytics en Azure Portal](../azure-monitor/learn/quick-create-workspace.md).

    1. Active la casilla junto a **Enviar a Log Analytics**.
    2. Seleccione la **Suscripción** donde se encuentra el área de trabajo de Log Analytics en el cuadro desplegable.
    3. Seleccione el **área de trabajo de Log Analytics** en el cuadro desplegable.


8.  En la sección **METRIC** del panel **Configuración de diagnóstico**, active la casilla junto a: **AllMetrics**

9. Compruebe que todo esté correcto y haga clic en **Guardar** en la parte superior del panel de creación de **Configuración de diagnóstico**.

## <a name="activity-log"></a>Registro de actividades

El registro de actividad se genera de manera predeterminada. Se puede configurar para que se exporte a nivel de suscripción [siguiendo las instrucciones de este artículo](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log). Obtenga más información sobre estos registros en el artículo [Visualización de registros de actividad para supervisar acciones sobre recursos](../azure-resource-manager/management/view-activity-logs.md).

### <a name="view-and-analyze-the-activity-log"></a>Visualización y análisis del registro de actividades

Puede ver y analizar los datos del registro de actividades mediante el uso de cualquiera de los métodos siguientes:

* **Herramientas de Azure:** puede recuperar información de los registros de actividad mediante Azure PowerShell, la interfaz de la línea de comandos (CLI) de Azure, la API REST de Azure o Azure Portal. En el artículo [Operaciones de auditoría con el Administrador de recursos](../azure-resource-manager/management/view-activity-logs.md) se detallan instrucciones paso a paso de cada método.
* **Power BI:** si todavía no tiene una cuenta de [Power BI](https://powerbi.microsoft.com/pricing), puede probarlo gratis. Con la [integración de los registros de auditoría de Azure para Power BI](https://powerbi.microsoft.com/integrations/azure-audit-logs/) puede analizar los datos con los paneles preconfigurados o puede personalizar las vistas para que se adapten a sus necesidades.

## <a name="metrics-as-logs"></a>Métricas como registros
Mediante el uso de la funcionalidad de exportación de métricas a registros proporcionada por Azure Monitor, es posible exportar las métricas de Load Balancer. Estas métricas generarán una entrada de registro para cada intervalo de muestreo de un minuto.

La exportación de métricas a registros está habilitada a nivel de cada recurso. Puede habilitar estos registros en la hoja de configuración de diagnóstico, filtrando por grupo de recursos y seleccionando la instancia de Load Balancer para la que desea habilitar la exportación de métricas. Cuando esté activa la página de configuración de diagnóstico de Load Balancer, seleccione AllMetrics para exportar las métricas válidas como registros.

Consulte la sección [Limitaciones](#limitations) de este artículo para conocer las limitaciones de la exportación de métricas.

### <a name="view-and-analyze-metrics-as-logs"></a>Visualización y análisis de las métricas como registros
Después de habilitar AllMetrics en la configuración de diagnóstico de Standard Load Balancer, si usa un centro de eventos o un área de trabajo de Log Analytics, estos registros se rellenarán en la tabla AzureMonitor. Si va a exportar al almacenamiento, conéctese a la cuenta de almacenamiento y recupere las entradas del registro JSON para los registros de eventos y de sondeos de estado. Cuando descargue los archivos JSON, se pueden convertir a CSV y consultarlos en Excel, Power BI o cualquier otra herramienta de visualización de datos. 

> [!TIP]
> Si está familiarizado con Visual Studio y con los conceptos básicos de cambio de los valores de constantes y variables de C#, puede usar las [herramientas convertidoras de registros](https://github.com/Azure-Samples/networking-dotnet-log-converter) que encontrará en GitHub.

## <a name="stream-to-an-event-hub"></a>Transmitir a un centro de eventos
Cuando se transmite información de diagnóstico a un centro de eventos, se puede usar para el análisis de registros centralizado en una herramienta SIEM de terceros con la integración de Azure Monitor. Para más información, consulte [Transmisión de datos de supervisión de Azure a un centro de eventos](../azure-monitor/platform/stream-monitoring-data-event-hubs.md#partner-tools-with-azure-monitor-integration).

## <a name="send-to-log-analytics"></a>Enviar a Log Analytics
Los recursos de Azure pueden hacer que su información de diagnóstico se envíe directamente a un área de trabajo de Log Analytics, donde se pueden ejecutar consultas complejas en la información para la solución de problemas y el análisis.  Para más información, consulte [Recopilación de registros de recursos de Azure en el área de trabajo de Log Analytics en Azure Monitor](../azure-monitor/platform/resource-logs.md#send-to-log-analytics-workspace)

## <a name="limitations"></a>Limitaciones
Actualmente, existen las siguientes limitaciones cuando se usa la característica de exportación de métricas a registros para Load Balancer:
* Las métricas se muestran actualmente mediante nombres internos cuando se exportan como registros. Puede encontrar la asignación en la tabla siguiente
* No se conserva la dimensionalidad de las métricas. Por ejemplo, con métricas como DipAvailability (estado del sondeo de mantenimiento), no podrá dividir o ver por dirección IP de back-end
* Los puertos SNAT usados y las métricas de puertos SNAT asignados no están disponibles actualmente para exportarse como registros

## <a name="next-steps"></a>Pasos siguientes
* [Revise las métricas disponibles de Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics)
* [Cree y pruebe consultas siguiendo las instrucciones de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)
* Proporcione comentarios sobre este artículo o la funcionalidad de Load Balancer mediante los vínculos siguientes
