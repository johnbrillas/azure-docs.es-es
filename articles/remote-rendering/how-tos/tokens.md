---
title: Obtención de tokens de acceso de servicio
description: Describe cómo crear tokens para acceder a las API REST de ARR
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: how-to
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 9721685fc3ccd2c1c80b55e9118d6d347cc97a9c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97830707"
---
# <a name="get-service-access-tokens"></a>Obtención de tokens de acceso de servicio

El acceso a las API REST de ARR solo se concede a los usuarios autorizados. Para demostrar la autorización, debe enviar un *token de acceso* junto con las solicitudes de REST. Estos tokens los emite el *Servicio de token seguro (STS)* en Exchange para una clave de cuenta. Los tokens tienen una **duración de 24 horas** y, por tanto, se pueden emitir a los usuarios sin concederles acceso completo al servicio.

En este artículo se describe cómo crear dicho token de acceso.

## <a name="prerequisites"></a>Prerrequisitos

[Cree una cuenta de ARR](create-an-account.md), si aún no tiene ninguna.

## <a name="token-service-rest-api"></a>API REST del servicio Table

Para crear tokens de acceso, el *Servicio de token seguro* proporciona una única API REST. La dirección URL del servicio STS depende del dominio de cuenta de la cuenta de representación remota. Tiene el formato https://sts.[dominio de cuenta], p. ej., `https://sts.southcentralus.mixedreality.azure.com`

### <a name="get-token-request"></a>Solicitud "Get token"

| URI | Método |
|-----------|:-----------|
| /accounts/**accountId**/token | GET |

| Encabezado | Value |
|--------|:------|
| Authorization | "Bearer **accountId**:**accountKey**" |

Reemplace *accountId* y *accountKey* por los datos correspondientes.

### <a name="get-token-response"></a>Respuesta "Get token"

| status code | carga de JSON | Comentarios |
|-----------|:-----------|:-----------|
| 200 | AccessToken: string | Correcto |

| Encabezado | Propósito |
|--------|:------|
| MS-CV | Este valor se puede usar para realizar un seguimiento de la llamada en el servicio. |

## <a name="getting-a-token-using-powershell"></a>Obtención de un token mediante PowerShell

El código de PowerShell siguiente muestra cómo enviar la solicitud de REST necesaria al STS. Después, muestra el token en el símbolo del sistema de PowerShell.

```PowerShell
$accountId = "<account_id_from_portal>"
$accountKey = "<account_key_from_portal>"
$accountDomain = "<account_domain_from_portal>

[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;
$webResponse = Invoke-WebRequest -Uri "https://sts.$accountDomain/accounts/$accountId/token" -Method Get -Headers @{ Authorization = "Bearer ${accountId}:$accountKey" }
$response = ConvertFrom-Json -InputObject $webResponse.Content

Write-Output "Token: $($response.AccessToken)"
```

El script simplemente muestra el token en la salida, desde donde puede copiar y pegarlo. En un proyecto real, debe automatizar este proceso.

## <a name="next-steps"></a>Pasos siguientes

* [Scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md)
* [API de front-end de Azure](../how-tos/frontend-apis.md)
* [API REST de administración de sesión](../how-tos/session-rest-api.md)
