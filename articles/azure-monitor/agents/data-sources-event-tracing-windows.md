---
title: Recopilación de eventos de Seguimiento de eventos para Windows (ETW) para análisis en registros de Azure Monitor
description: Obtenga información sobre cómo recopilar el Seguimiento de eventos para Windows (ETW) para el análisis en registros de Azure Monitor.
services: azure-monitor
ms.subservice: logs
ms.topic: conceptual
ms.author: jamesfit
author: jimmyfit
ms.date: 01/29/2021
ms.openlocfilehash: 6239cf48794c74c5dd810fda42476df399300578
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100603222"
---
# <a name="collecting-event-tracing-for-windows-etw-events-for-analysis-azure-monitor-logs"></a>Recopilación de eventos de Seguimiento de eventos para Windows (ETW) para análisis en registros de Azure Monitor

*Seguimiento de eventos para Windows (ETW)* proporciona un mecanismo para la instrumentación de aplicaciones en modo de usuario y controladores de modo de kernel. El agente de Log Analytics se usa para [recopilar eventos de Windows](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-windows-events) escritos en los [canales de ETW](https://docs.microsoft.com/windows/win32/wes/eventmanifestschema-channeltype-complextype) administrativo y operativo. Sin embargo, en ocasiones es necesario capturar y analizar otros eventos, como los que se escriben en el canal analítico.  

## <a name="event-flow"></a>Flujo de eventos

Para recopilar correctamente [eventos ETW basados en un manifiesto](https://docs.microsoft.com/windows/win32/etw/about-event-tracing#types-of-providers) para el análisis en registros de Azure Monitor, debe usar la [extensión de Azure Diagnostics](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview) para Windows (WAD). En este escenario, la extensión de diagnóstico actúa como consumidor de ETW, y escribe eventos en Azure Storage (tablas) como almacén intermedio. Aquí se almacenará en una tabla denominada **WADETWEventTable**. Luego Log Analytics recopila los datos de la tabla de Azure Storage y los presenta como una tabla denominada **ETWEvent**.

![Flujo de eventos](./media/data-sources-event-tracing-windows/event-flow.png)

## <a name="configuring-etw-log-collection"></a>Configuración de la recopilación de registros de ETW

### <a name="step-1-locate-the-correct-etw-provider"></a>Paso 1: Búsqueda del proveedor de ETW correcto

Use cualquiera de los siguientes comandos para enumerar los proveedores de ETW en un sistema Windows de origen.

Línea de comandos:

```
logman query providers
```

PowerShell:
```
Get-NetEventProvider -ShowInstalled | Select-Object Name, Guid
```
Opcionalmente, puede elegir canalizar esta salida de PowerShell a Out-Gridview para ayudar a la navegación.

Registre el nombre del proveedor de ETW y el GUID que se alinee con el registro de análisis o de depuración que se presenta en el Visor de eventos, o con el módulo para el que quiere recopilar datos de eventos.

### <a name="step-2-diagnostics-extension"></a>Paso 2: Extensión Diagnostics

Asegúrese de que de la *extensión de Diagnostics para Windows* esté [instalada](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-windows-install#install-with-azure-portal) en todos los sistemas de origen.

### <a name="step-3-configure-etw-log-collection"></a>Paso 3: Configuración de la recopilación de registros de ETW

1. Vaya a la hoja **Configuración de diagnóstico** de la máquina virtual.

2. Seleccione la pestaña **Registros**.

3. Desplácese hacia abajo y habilite la opción **Eventos de seguimiento de eventos para Windows (ETW)** . ![Captura de pantalla de la configuración de diagnóstico](./media/data-sources-event-tracing-windows/enable-event-tracing-windows-collection.png)

4. Establezca el GUID del proveedor o la clase del proveedor en función del proveedor para el que esté configurando la recopilación.

5. Establezca el [**Nivel de registro**](https://docs.microsoft.com/windows/win32/etw/configuring-and-starting-an-event-tracing-session) según corresponda.

6. Haga clic en los puntos suspensivos junto al proveedor proporcionado y haga clic en **Configurar**.

7. Asegúrese de que la **Tabla de destino predeterminada** esté establecida en **etweventtable**.

8. Establezca un [**Filtro de palabra clave**](https://docs.microsoft.com/windows/win32/wes/defining-keywords-used-to-classify-types-of-events) de ser necesario.

9. Guarde la configuración del proveedor y del registro.

Una vez generados los eventos coincidentes, debe empezar a ver los eventos ETW aparecer en la tabla **WADetweventtable** de Azure Storage. Puede usar el Explorador de Azure Storage para confirmarlo.

### <a name="step-4-configure-log-analytics-storage-account-collection"></a>Paso 4: Configuración de la colección de cuentas de almacenamiento de Log Analytics

Siga [estas instrucciones](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-logs#collect-logs-from-azure-storage) para recopilar los registros de Azure Storage. Una vez configurados, los datos de eventos ETW deben aparecer en Log Analytics en la tabla **ETWEvent**.

## <a name="next-steps"></a>Pasos siguientes
- Use los [campos personalizados](https://docs.microsoft.com/azure/azure-monitor/platform/custom-fields) para crear una estructura en los eventos ETW.
- Obtenga información acerca de las [consultas de registros](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) para analizar los datos recopilados de soluciones y orígenes de datos.
