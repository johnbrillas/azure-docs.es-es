---
title: 'Agente de aprovisionamiento en la nube de Azure AD Connect: Actualización automática | Microsoft Docs'
description: En este artículo se describe la característica de actualización automática integrada en el agente de aprovisionamiento en la nube de Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26c0b47dd358826b843143aaf23c469d852e93b3
ms.sourcegitcommit: 8a74ab1beba4522367aef8cb39c92c1147d5ec13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98612816"
---
# <a name="azure-ad-connect-cloud-provisioning-agent-automatic-upgrade"></a>Agente de aprovisionamiento en la nube de Azure AD Connect: Actualización automática

La característica de actualización automática nos proporciona la seguridad de que la instalación del agente de aprovisionamiento en la nube de Azure Active Directory (Azure AD) Connect está siempre actualizada.

El agente se instala aquí: "Archivos de programa\Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe"

Para comprobar su versión, haga clic con el botón derecho en el archivo ejecutable y seleccione Propiedades; a continuación, seleccione los detalles.

![Versión del archivo del agente](media/how-to-automatic-upgrade/agent-1.png)

El actualizador del agente se instala aquí: "Archivos de programa\Azure AD Connect Provisioning Agent Updater\AzureADConnectAgentUpdater.exe"

Para comprobar su versión, haga clic con el botón derecho en el archivo ejecutable y seleccione Propiedades; a continuación, seleccione los detalles.

![Versión del actualizador del agente](media/how-to-automatic-upgrade/agent-2.png)

## <a name="uninstall-the-agent"></a>Desinstalación del agente
Para quitar el agente, vaya a **Desinstalar o cambiar un programa** y desinstale lo siguiente:

- **Actualizador del Agente de Microsoft Azure AD Connect**
- **Agente de aprovisionamiento de Microsoft Azure AD Connect**
- **Paquete del Agente de aprovisionamiento de Microsoft Azure AD Connect**

![Eliminación del agente](media/how-to-automatic-upgrade/agent-3.png)

## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es la sincronización en la nube de Azure AD Connect?](what-is-cloud-sync.md)

