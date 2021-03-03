---
title: Residencia de datos
description: Residencia de datos e información sobre los servidores habilitados para Azure Arc.
ms.topic: reference
ms.date: 02/16/2021
ms.custom: references_regions
ms.openlocfilehash: d4764772473bbf2e5aafe2607a9462c9a6a15203
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100559502"
---
# <a name="azure-arc-enabled-servers-data-residency"></a>Servidores habilitados para Azure Arc: Residencia de datos

En este artículo se explica el concepto de residencia de datos y cómo se aplica a los servidores habilitados para Azure Arc.

Los servidores habilitados para Azure Arc están **[disponibles](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc)** en **Estados Unidos, Europa, Reino Unido, Australia y Asia Pacífico**.

## <a name="data-residency"></a>Residencia de datos

Los servidores habilitados para Azure Arc almacenan los valores de configuración de la [extensión de máquina virtual de Azure](manage-vm-extensions.md) (es decir, los valores de propiedad). Es necesario especificar la extensión antes de intentar habilitarla en la máquina conectada. Por ejemplo, cuando se habilita la extensión de máquina virtual de Log Analytics, se solicita el **id. de área de trabajo** y la **clave principal** de Log Analytics.

También se recopila información de metadatos sobre la máquina conectada. Concretamente:

* Nombre, tipo y versión del sistema operativo
* Nombre del equipo
* Nombre de dominio completo (FQDN) del equipo
* Versión del agente Connected Machine
* Active Directory y nombre de dominio completo (FQDN) de DNS
* UUID (IDENTIFICADOR DE BIOS)
* Latido del agente de Connected Machine
* Versión del agente Connected Machine
* Clave pública para la identidad administrada
* Estado y detalles de cumplimiento de las directivas (si usa directivas de configuración de invitado de Azure Policy)

Los servidores habilitados para Arc permiten especificar la región en la que se almacenarán los datos. Microsoft puede replicar en otras regiones para obtener resistencia de datos, pero no replica ni mueve datos fuera de la geografía. Estos datos se almacenan en la región donde se ha configurado el recurso de máquina de Azure Arc. Por ejemplo, si la máquina se registra en Arc en la región Este de EE. UU., estos datos se almacenan en la región de EE. UU.

Para obtener más información sobre la resistencia regional y la compatibilidad con el cumplimiento, consulte [Geografía de Azure](https://azure.microsoft.com/global-infrastructure/geographies/).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el diseño de la [resistencia de Azure](/azure/architecture/reliability/architect).
