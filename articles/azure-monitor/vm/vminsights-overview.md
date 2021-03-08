---
title: ¿Qué es VM Insights?
description: Información general de VM Insights, que supervisa el estado y el rendimiento de las VM de Azure, y detecta y asigna automáticamente los componentes de aplicación y sus dependencias.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/22/2020
ms.openlocfilehash: 967ff9672f55af3e9c5d2e7c3c3dbc7d2ff6cb3c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101707377"
---
# <a name="overview-of-vm-insights"></a>Información general de VM Insights

VM Insights supervisa el rendimiento y el estado de las máquinas virtuales y los conjuntos de escalado de máquinas virtuales, incluidos los procesos en ejecución y las dependencias de otros recursos. Puede ayudar a ofrecer de forma eficaz un rendimiento predecible y disponibilidad de las aplicaciones vitales mediante la identificación de cuellos de botella de rendimiento y problemas de red, así como ayudar a comprender si un problema está relacionado con otras dependencias.

VM Insights admite los sistemas operativos Windows y Linux en lo siguiente:

- Azure Virtual Machines
- Conjuntos de escalado de máquinas virtuales de Azure
- Máquinas virtuales híbridas conectadas con Azure Arc
- Máquinas virtuales locales
- Máquinas virtuales hospedadas en otro entorno de nube
  

VM Insights almacena sus datos en registros de Azure Monitor, lo que le permite ofrecer una agregación y un filtrado eficaces, y analizar las tendencias de los datos en el tiempo. Puede ver estos datos directamente en una sola VM desde la máquina virtual, o puede usar a Azure Monitor para ofrecer una vista agregada de varias VM.

![Perspectiva de conclusiones de la máquina virtual desde Azure Portal](media/vminsights-overview/vminsights-azmon-directvm.png)


## <a name="pricing"></a>Precios
No hay ningún costo directo por VM Insights, pero se le cobrará por la actividad en el área de trabajo de Log Analytics. En función de los precios que se publican en la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/), VM Insights se factura según lo siguiente:

- Datos ingeridos de agentes y almacenados en el área de trabajo.
- Datos de estado de mantenimiento recopilados del estado de invitado (versión preliminar)
- Las reglas de alertas basadas en el registro y los datos de estado.
- Las notificaciones enviadas desde reglas de alertas.

El tamaño del registro varía en función de las longitudes de cadena de los contadores de rendimiento y puede aumentar con el número de discos lógicos y adaptadores de red asignados a la máquina virtual. Si ya usa Service Map, el único cambio que verá son los datos de rendimiento adicionales que se envían al tipo de datos `InsightsMetrics` de Azure Monitor.


## <a name="configuring-vm-insights"></a>Configuración de VM Insights
Los pasos para configurar VM Insights son los siguientes. Siga cada vínculo para obtener instrucciones detalladas sobre cada paso:

- [Creación de un área de trabajo de Log Analytics.](./vminsights-configure-workspace.md#create-log-analytics-workspace)
- [Incorporación de la solución VMInsights al área de trabajo.](./vminsights-configure-workspace.md#add-vminsights-solution-to-workspace)
- [Instalación de agentes en la máquina virtual y el conjunto de escalado de máquinas virtuales que se van a supervisar.](./vminsights-enable-overview.md)



## <a name="next-steps"></a>Pasos siguientes

- Vea [Información general sobre la habilitación de Azure Monitor para VM](./vminsights-enable-overview.md) a fin de obtener los requisitos y métodos para habilitar la supervisión de las máquinas virtuales.
