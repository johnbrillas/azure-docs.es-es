---
title: Registro de errores y excepciones en MSAL para Python
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo registrar errores y excepciones en MSAL para Python.
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
ms.openlocfilehash: 8488325613b05d54b352a19a06860e08f1779877
ms.sourcegitcommit: 1a98b3f91663484920a747d75500f6d70a6cb2ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99063121"
---
# <a name="logging-in-msal-for-python"></a>Inicio de sesión en MSAL para Python

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="msal-for-python-logging"></a>Registro de MSAL para Python

El registro en MSAL para Python usa el mecanismo de registro estándar de Python; por ejemplo, `logging.info("msg")` Puede configurar el registro de MSAL como se indica a continuación (y verlo en acción en [username_password_sample](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.0.0/sample/username_password_sample.py#L31L32)):

### <a name="enable-debug-logging-for-all-modules"></a>Habilitar el registro de depuración para todos los módulos

De forma predeterminada, el registro de cualquier script de Python está desactivado. Si quiere habilitar el registro de depuración para todos los módulos de su script de Python completo, use:

```python
logging.basicConfig(level=logging.DEBUG)
```

### <a name="silence-only-msal-logging"></a>Silenciar solo el registro de MSAL

Para silenciar solo el registro de la biblioteca MSAL, mientras se habilita el registro de depuración en el resto de módulos del script de Python, desactive el registrador usado por MSAL para Python:

```Python
logging.getLogger("msal").setLevel(logging.WARN)
```

### <a name="personal-and-organizational-data-in-python"></a>Datos personales y organizativos en Python

MSAL para Python no registra datos personales ni datos de la organización. No hay ninguna propiedad para activar o desactivar el registro de datos personales o de la organización.

Puede usar el registro de Python estándar para registrar lo que quiera, pero usted es responsable de controlar los datos confidenciales de forma segura y de cumplir los siguientes requisitos normativos.

Para más información sobre el registro en Python, consulte los [Procedimientos de registro](https://docs.python.org/3/howto/logging.html#logging-basic-tutorial) de Python.

## <a name="next-steps"></a>Pasos siguientes

Para ver más ejemplos de código, consulte [Ejemplos de código de la Plataforma de identidad de Microsoft](sample-v2-code.md).
