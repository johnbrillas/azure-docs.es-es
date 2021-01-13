---
title: Obtención de los puntos de conexión para el registro de una aplicación de Azure AD
titleSuffix: Microsoft identity platform
description: Aprenda a buscar los puntos de conexión de autenticación de una aplicación personalizada que esté desarrollando o registrando con Azure AD.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: 3afaf654228511a357461a9e762be0b04acc65c6
ms.sourcegitcommit: 2488894b8ece49d493399d2ed7c98d29b53a5599
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98064171"
---
# <a name="how-to-discover-endpoints"></a>Detección de puntos de conexión

Puede buscar los puntos de conexión de autenticación para su aplicación en [Azure Portal](https://portal.azure.com).

1. Inicie sesión en <a href="https://portal.azure.com/" target="_blank">Azure Portal<span class="docon docon-navigate-external x-hidden-focus"></span></a>.
1. Seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Registros de aplicaciones** y, a continuación, seleccione **Puntos de conexión** en el menú superior.

    Aparecerá la página **Puntos de conexión** y mostrará una lista de todos los puntos de conexión de autenticación del inquilino.
    
    Use el punto de conexión que coincida con el protocolo de autenticación que esté usando, junto con el **identificador de aplicación (cliente)** , para diseñar la solicitud de autenticación específica de la aplicación.

Las **nubes nacionales** (por ejemplo, Azure AD China, Alemania y US Government) tienen su propio portal de registro de aplicaciones y los puntos de conexión de autenticación de Azure AD. Obtenga más información en la [Introducción a las nubes nacionales](authentication-national-cloud.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los puntos de conexión en los diferentes entornos de Azure, consulte la [información general sobre nubes nacionales ](authentication-national-cloud.md).
