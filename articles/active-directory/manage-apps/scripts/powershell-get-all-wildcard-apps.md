---
title: 'Ejemplo de PowerShell: Enumeración de aplicaciones de Application Proxy con caracteres comodín'
description: Ejemplo de PowerShell en el que se enumeran todas las aplicaciones de Azure Active Directory (Azure AD) Application Proxy que usan caracteres comodín.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: ae9131e8e98126bc87a3d88156e182332409d7de
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99253720"
---
# <a name="get-all-application-proxy-apps-using-wildcard-publishing"></a>Obtención de todas las aplicaciones de Application Proxy mediante la publicación con caracteres comodín

En este ejemplo del script de PowerShell se enumeran todas las aplicaciones de Azure Active Directory (Azure AD) Application Proxy que se publican con caracteres comodín.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

El ejemplo requiere el [módulo Azure AD V2 de PowerShell para Graph](/powershell/azure/active-directory/install-adv2) (Azure AD) o la [versión preliminar del módulo Azure AD V2 de PowerShell para Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (versión preliminar de Azure AD).

## <a name="sample-script"></a>Script de ejemplo

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-wildcard-apps.ps1 "Get all Application Proxy apps using wildcards")]

## <a name="script-explanation"></a>Explicación del script

| Get-Help | Notas |
|---|---|
|[Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | Obtiene una entidad de servicio. |
|[Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication) | Obtiene una aplicación de Azure AD. |
|[Get-AzureADApplicationProxyApplication](/powershell/module/azuread/get-azureadapplicationproxyapplication) | Recupera una aplicación configurada para Application Proxy en Azure AD. |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el módulo de Azure AD PowerShell, consulte [Información general del módulo de Azure AD PowerShell](/powershell/azure/active-directory/overview).

Para obtener otros ejemplos de PowerShell para Application Proxy, consulte [Ejemplos de Azure AD PowerShell para Azure AD Application Proxy](../application-proxy-powershell-samples.md).
