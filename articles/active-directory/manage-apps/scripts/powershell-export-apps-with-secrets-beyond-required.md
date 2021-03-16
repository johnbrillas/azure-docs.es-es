---
title: 'Ejemplo de PowerShell: Exportación de aplicaciones con secretos y certificados que expiran después de la fecha requerida al inquilino de Azure Active Directory.'
description: Ejemplo de PowerShell que exporta todas las aplicaciones con secretos y certificados que expiran después de la fecha requerida para las aplicaciones especificadas al inquilino de Azure Active Directory.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 03/09/2021
ms.author: kenwith
ms.reviewer: mifarca
ms.openlocfilehash: 3572f481cc2cbcb1df73b33eb2543e32256ad9fb
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102583331"
---
# <a name="export-apps-with-secrets-and-certificates-expiring-beyond-the-required-date"></a>Exportación de aplicaciones con secretos y certificados que expiran después de la fecha requerida

Este ejemplo de script de PowerShell exporta todos los secretos de la aplicación y los certificados que expiran después de la fecha requerida para las aplicaciones especificadas desde el directorio a un archivo CSV.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

El ejemplo requiere el [módulo Azure AD V2 de PowerShell para Graph](/powershell/azure/active-directory/install-adv2) (Azure AD) o la [versión preliminar del módulo Azure AD V2 de PowerShell para Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (versión preliminar de Azure AD).

## <a name="sample-script"></a>Script de ejemplo

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-apps-with-secrets-beyond-required.ps1 "Exports all apps with secrets and certificates expiring beyond the required date for the specified apps in your directory.")]

## <a name="script-explanation"></a>Explicación del script

El comando "Add-Member" es responsable de la creación de las columnas en el archivo CSV.
En el caso de que prefiera que la exportación sea no interactiva, puede modificar la variable "$Path" directamente en PowerShell mediante una ruta de acceso del archivo CSV.

| Get-Help | Notas |
|---|---|
| [Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0&preserve-view=true) | Recupera una aplicación del directorio. |
| [Get-AzureADApplicationOwner](/powershell/module/azuread/Get-AzureADApplicationOwner?view=azureadps-2.0&preserve-view=true) | Recupera los propietarios de una aplicación del directorio. |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el módulo de Azure AD PowerShell, consulte [Información general del módulo de Azure AD PowerShell](/powershell/azure/active-directory/overview).

Para obtener otros ejemplos de PowerShell para la administración de aplicaciones, consulte [Ejemplos de Azure AD PowerShell para la administración de aplicaciones](../app-management-powershell-samples.md).
