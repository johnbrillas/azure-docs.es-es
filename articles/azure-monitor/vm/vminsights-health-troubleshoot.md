---
title: Solución de problemas de estado de invitado de VM Insights (versión preliminar)
description: Se describen los pasos para solucionar problemas que puede seguir cuando tenga problemas con el estado de las instancias de VM Insights.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/08/2020
ms.openlocfilehash: d8b37569ebaa8e75be601a1efd65a23a61aeaa75
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051946"
---
# <a name="troubleshoot-vm-insights-guest-health-preview"></a>Solución de problemas de estado de invitado de VM Insights (versión preliminar)
En este artículo se describen los pasos para solucionar problemas que puede seguir cuando tenga problemas con el estado de las instancias de VM Insights.

## <a name="error-message-that-no-data-is-available"></a>Mensaje de error que indica que no hay datos disponibles 

![Sin datos](media/vminsights-health-troubleshoot/no-data.png)


### <a name="verify-that-the-virtual-machine-meets-configuration-requirements"></a>Compruebe que la máquina virtual cumple estos requisitos de configuración.

1. Compruebe que la máquina virtual sea una máquina virtual de Azure. Azure Arc para servidores no se admite de momento.
2. Compruebe que la máquina virtual usa un [sistema operativo](vminsights-health-enable.md?current-limitations.md) compatible.
3. Compruebe que la máquina virtual está instalada en una [región compatible](vminsights-health-enable.md?current-limitations.md).
4. Compruebe que el área de trabajo de Log Analytics está instalada en una [región compatible](vminsights-health-enable.md?current-limitations.md).

### <a name="verify-that-the-vm-is-properly-onboarded"></a>Comprobación de que la máquina virtual está incorporada correctamente
Compruebe que la extensión del agente de Azure Monitor y el agente de estado de la máquina virtual invitada se aprovisionaron correctamente en la máquina virtual. Seleccione **Extensiones** en el menú de la máquina virtual en Azure Portal y asegúrese de que se muestran los dos agentes.

![Extensiones de máquina virtual](media/vminsights-health-troubleshoot/extensions.png)

### <a name="verify-the-system-assigned-identity-is-enabled-on-the-virtual-machine"></a>Comprobación de que la identidad asignada por el sistema está habilitada en la máquina virtual
Compruebe que la identidad asignada por el sistema está habilitada en la máquina virtual. Seleccione **Identidad** en el menú de la máquina virtual en Azure Portal. Si la identidad administrada por el usuario está habilitada, independientemente del estado de la identidad administrada por el sistema, el agente de Azure Monitor no podrá comunicarse con el servicio de configuración, y la extensión de estado de invitado no funcionará.

![Identidad asignada por el sistema](media/vminsights-health-troubleshoot/system-identity.png)

### <a name="verify-data-collection-rule"></a>Comprobación de la regla de recopilación de datos
Compruebe que la regla de recopilación de datos que especifica la extensión de mantenimiento como un origen de datos está asociada a la máquina virtual.

## <a name="error-message-for-bad-request-due-to-insufficient-permissions"></a>Mensaje de error debido a una solicitud incorrecta por permisos insuficientes
Este error indica que el proveedor de recursos **Microsoft.WorkloadMonitor** no se registró en la suscripción. Consulte [Tipos y proveedores de recursos de Azure](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider) para obtener información sobre registrar a este proveedor de recursos. 

![Solicitud incorrecta](media/vminsights-health-troubleshoot/bad-request.png)

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga información general sobre la característica de estado de invitado de VM Insights](vminsights-health-overview.md).