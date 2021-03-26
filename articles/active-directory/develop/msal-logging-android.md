---
title: Registro de errores y excepciones en MSAL para Android
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo registrar errores y excepciones en MSAL para Android.
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
ms.openlocfilehash: ce0929adbb2b0213cfd4ee61fe795a2d5f33c648
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98954891"
---
# <a name="logging-in-msal-for-android"></a>Inicio de sesión en MSAL para Android

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="logging-in-msal-for-android-using-java"></a>Registro en MSAL para Android con Java

Active el registro al crear la aplicación mediante la creación de una devolución de llamada de inicio de sesión. La devolución de llamada toma estos parámetros:

- `tag` es una cadena que la biblioteca pasa a la devolución de llamada. Se asocia a la entrada de registro y se puede usar para ordenar los mensajes de registro.
- `logLevel` le permite decidir el nivel de registro deseado. Los niveles de registro admitidos son: `Error`, `Warning`, `Info` y `Verbose`.
- `message` es el contenido de la entrada de registro.
- `containsPII` especifica si se registran los mensajes que contienen datos personales o datos de la organización. De forma predeterminada, se establece en false para que la aplicación no registre datos personales. Si `containsPII` es `true`, este método recibirá los mensajes dos veces: una con el parámetro `containsPII` igual a `false` y el `message` sin datos personales y una segunda con el parámetro `containsPii` igual a `true` y el mensaje podría contener datos personales. En algunos casos (cuando el mensaje no contiene datos personales), el mensaje será el mismo.

```java
private StringBuilder mLogs;

mLogs = new StringBuilder();
Logger.getInstance().setExternalLogger(new ILoggerCallback()
{
   @Override
   public void log(String tag, Logger.LogLevel logLevel, String message, boolean containsPII)
   {
      mLogs.append(message).append('\n');
   }
});
```

De forma predeterminada, el registrador de MSAL no capturará ninguna información de identificación personal ni información de identificación de la organización.
Para habilitar el registro de información de identificación personal o de información de identificación de la organización:

```java
Logger.getInstance().setEnablePII(true);
```

Para deshabilitar el registro de datos personales y de datos de la organización:

```java
Logger.getInstance().setEnablePII(false);
```

De forma predeterminada, el registro se deshabilita en logcat. Para habilitar las siguientes opciones:

```java
Logger.getInstance().setEnableLogcatLog(true);
```

## <a name="next-steps"></a>Pasos siguientes

Para ver más ejemplos de código, consulte [Ejemplos de código de la Plataforma de identidad de Microsoft](sample-v2-code.md).