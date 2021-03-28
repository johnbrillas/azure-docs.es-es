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
ms.openlocfilehash: 1d52b017f94785f5fb25a25f127ae52d96e97d8b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104578760"
---
# <a name="logging-in-msal-for-python"></a>Inicio de sesión en MSAL para Python

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="msal-for-python-logging"></a>Registro de MSAL para Python

El registro en MSAL para Python aprovecha el [módulo de registro de la biblioteca estándar de Python](https://docs.python.org/3/library/logging.html). Puede configurar el registro de MSAL tal como se indica a continuación (y ver cómo funciona en [username_password_sample](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.0.0/sample/username_password_sample.py#L31L32)):

### <a name="enable-debug-logging-for-all-modules"></a>Habilitar el registro de depuración para todos los módulos

De forma predeterminada, el registro de cualquier script de Python está desactivado. Si desea habilitar el registro detallado de **todos** los módulos de Python en el script, use `logging.basicConfig` con un nivel `logging.DEBUG`:

```python
import logging

logging.basicConfig(level=logging.DEBUG)
```

En la salida estándar se imprimirán todos los mensajes de registro proporcionados en el módulo de registro.

### <a name="configure-msal-logging-level"></a>Configuración del nivel de registro de MSAL

Puede configurar el nivel de registro de MSAL del proveedor de registro de Python mediante el método `logging.getLogger()` con el nombre de registrador `"msal"`:

```python
import logging

logging.getLogger("msal").setLevel(logging.WARN)
```

### <a name="configure-msal-logging-with-azure-app-insights"></a>Configuración del registro de MSAL con Azure App Insights

Los registros de Python se asignan a un controlador de registro, que de manera predeterminada es `StreamHandler`. Para enviar registros de MSAL a una instancia de Application Insights con una clave de instrumentación, use la instancia `AzureLogHandler`proporcionada por la biblioteca `opencensus-ext-azure`.

Para instalar `opencensus-ext-azure`, agregue el paquete `opencensus-ext-azure` de PyPI a la instalación de dependencias o PIP:

```console
pip install opencensus-ext-azure
```

A continuación, cambie el controlador predeterminado del proveedor de registro `"msal"` a una instancia de `AzureLogHandler` con una clave de instrumentación establecida en la variable de entorno `APP_INSIGHTS_KEY`:

```python
import logging
import os

from opencensus.ext.azure.log_exporter import AzureLogHandler

APP_INSIGHTS_KEY = os.getenv('APP_INSIGHTS_KEY')

logging.getLogger("msal").addHandler(AzureLogHandler(connection_string='InstrumentationKey={0}'.format(APP_INSIGHTS_KEY))
```

### <a name="personal-and-organizational-data-in-python"></a>Datos personales y organizativos en Python

MSAL para Python no registra datos personales ni datos de la organización. No hay ninguna propiedad para activar o desactivar el registro de datos personales o de la organización.

Puede usar el registro de Python estándar para registrar lo que quiera, pero usted es responsable de controlar los datos confidenciales de forma segura y de cumplir los siguientes requisitos normativos.

Para más información sobre el registro en Python, consulte los [Procedimientos de registro](https://docs.python.org/3/howto/logging.html#logging-basic-tutorial) de Python.

## <a name="next-steps"></a>Pasos siguientes

Para ver más ejemplos de código, consulte [Ejemplos de código de la Plataforma de identidad de Microsoft](sample-v2-code.md).
