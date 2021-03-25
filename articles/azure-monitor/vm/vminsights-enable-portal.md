---
title: Habilitar Azure Monitor para VM para una sola máquina virtual o un conjunto de escalado de máquinas virtuales mediante Azure Portal
description: Obtenga información sobre cómo habilitar VM Insights en una sola máquina virtual o un conjunto de escalado de máquinas virtuales de Azure mediante Azure Portal.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 076fcab7b0747a7993407edd65f9d08efc27309f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102035592"
---
# <a name="enable-azure-monitor-for-single-virtual-machine-or-virtual-machine-scale-set-in-the-azure-portal"></a>Habilitar Azure Monitor para VM para una sola máquina virtual o un conjunto de escalado de máquinas virtuales mediante Azure Portal
En este artículo, se describe cómo habilitar VM Insights para una máquina virtual o un conjunto de escalado de máquinas virtuales mediante Azure Portal. Este procedimiento se puede usar para:

- Máquina virtual de Azure
- Conjunto de escalado de máquinas virtuales de Azure
- Máquina virtual híbrida conectada con Azure Arc

## <a name="prerequisites"></a>Requisitos previos

- [Cree y configure un área de trabajo de Log Analytics](./vminsights-configure-workspace.md). Como alternativa, puede crear un área de trabajo durante este proceso.
- Vea [Sistemas operativos admitidos](./vminsights-enable-overview.md#supported-operating-systems) para asegurarse de que el sistema operativo de la máquina virtual o el conjunto de escalado de máquinas virtuales que va a habilitar son compatibles. 

## <a name="enable-vm-insights"></a>Habilitar VM Insights

En Azure Portal, seleccione **Máquinas virtuales**, **Conjuntos de escalado de máquinas virtuales** o **Servidores: Azure Arc** y seleccione un recurso de la lista. En la sección **Supervisión** del menú, seleccione **Insights** y, a continuación, **Habilitar**. En el ejemplo siguiente, se muestra una máquina virtual de Azure, pero el menú es similar para el conjunto de escalado de máquinas virtuales o Azure Arc.

![Habilitación de VM Insights para una VM](media/vminsights-enable-portal/enable-vminsights-vm-portal.png)

Si la máquina virtual aún no está conectada a un área de trabajo de Log Analytics, se le pedirá que seleccione una. Si anteriormente no ha [creado un área de trabajo](../logs/quick-create-workspace.md), puede seleccionar una predeterminada para la ubicación en la que se implementa la máquina virtual o el conjunto de escalado de máquinas virtuales en la suscripción. Se creará y configurará el área de trabajo si aún no existe. Si selecciona un área de trabajo existente, se configurará en VM Insights si aún no lo estaba.

> [!NOTE]
> Si selecciona un área de trabajo que no se configuró anteriormente en VM Insights, se le agregará el paquete de administración *VMInsights*. Esto se aplicará a cualquier agente que ya esté conectado al área de trabajo, independientemente de si está habilitado o no en VM Insights. Los datos de rendimiento se recopilarán de estas máquinas virtuales y se almacenarán en la tabla *InsightsMetrics*.

![Selección del área de trabajo](media/vminsights-enable-portal/select-workspace.png)

Recibirá mensajes de estado a medida que se lleve a cabo la configuración.

>[!NOTE]
>Si usa un modelo de actualización manual para el conjunto de escalado de máquinas virtuales, actualice las instancias para completar la configuración. Puede iniciar las actualizaciones desde la página **Instancias**, en la sección **Configuración**.

![Habilitación del procesamiento de la implementación de la supervisión de VM Insights](media/vminsights-enable-portal/onboard-vminsights-vm-portal-status.png)



## <a name="next-steps"></a>Pasos siguientes

* Para visualizar las dependencias de las aplicaciones detectadas, vea [Uso de la característica de asignación de VM Insights](vminsights-maps.md). 
* Consulte [Ver el rendimiento de las máquinas virtuales de Azure](vminsights-performance.md) para identificar cuellos de botella, el uso general y el rendimiento de la máquina virtual.
