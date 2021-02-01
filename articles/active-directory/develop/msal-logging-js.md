---
title: Registro de errores y excepciones en MSAL.js
titleSuffix: Microsoft identity platform
description: Aprenda a registrar errores y excepciones en MSAL.js.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/25/2021
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 8604a38bc310cc884c2b5e99efe7a47ae5e787d7
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98763303"
---
# <a name="logging-in-msaljs"></a>Registro en MSAL.js

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="configure-logging-in-msaljs"></a>Configuración del registro en MSAL.js

Puede habilitar el registro en MSAL.js (JavaScript) si pasa un objeto de registrador durante la configuración para crear una instancia de `UserAgentApplication`. El objeto de registrador tiene las siguientes propiedades:

- `localCallback`: una instancia de devolución de llamada que el desarrollador puede proporcionar para consumir y publicar registros de una manera personalizada. Implemente el método localCallback según cómo quiera redirigir los registros.
- `level` (opcional): el nivel de registro configurable. Los niveles de registro admitidos son: `Error`, `Warning`, `Info` y `Verbose`. El valor predeterminado es `Info`.
- `piiLoggingEnabled` (opcional): si se establece en true, registra los datos personales y de la organización. De forma predeterminada, se establece en false para que la aplicación no registre los datos personales. Los registros de datos personales nunca se escriben en salidas predeterminadas, como consola, Logcat o NSLog.
- `correlationId` (opcional): un identificador único, que se usa para asignar la solicitud con la respuesta para fines de depuración. El valor predeterminado es RFC4122 version 4 guid (128 bits).

```javascript
function loggerCallback(logLevel, message, containsPii) {
   console.log(message);
}

var msalConfig = {
    auth: {
        clientId: "<Enter your client id>",
    },
    system: {
        logger: new Msal.Logger(
            loggerCallback , {
                level: Msal.LogLevel.Verbose,
                piiLoggingEnabled: false,
                correlationId: '1234'
            }
        )
    }
}

var UserAgentApplication = new Msal.UserAgentApplication(msalConfig);
```

## <a name="next-steps"></a>Pasos siguientes

Para ver más ejemplos de código, consulte [Ejemplos de código de la Plataforma de identidad de Microsoft](sample-v2-code.md).