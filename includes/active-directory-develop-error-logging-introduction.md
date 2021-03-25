---
author: mmacy
ms.author: marsma
ms.date: 12/16/2020
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.openlocfilehash: 53198c663d318a2eb47bcb3207939bbcb1fdd59c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98763300"
---
Las aplicaciones de la biblioteca de autenticación de Microsoft (MSAL) generan mensajes de registro que pueden ayudar a diagnosticar problemas. Una aplicación puede configurar el registro con unas cuentas líneas de código y tener un control personalizado sobre el nivel de detalle y si se registran o no datos de la organización y personales. Se recomienda establecer una devolución de llamada del registro de MSAL y proporcionar una forma de que los usuarios envíen registros cuando tengan problemas de autenticación.

## <a name="logging-levels"></a>Niveles de registro

MSAL ofrece varios niveles de detalle de registro:

- Error: indica que algo no ha funcionado bien y se ha generado un error. Se usa para la depuración y la identificación de problemas.
- Advertencia: No se ha producido necesariamente un error, pero es necesario realizar un diagnóstico e identificar los problemas.
- Info: MSAL registrará los eventos de carácter informativo, no relacionados necesariamente con la depuración.
- Verbose: Predeterminada. MSAL registra todos los detalles del comportamiento de la biblioteca.

## <a name="personal-and-organizational-data"></a>Datos personales y organizativos

De forma predeterminada, el registrador de MSAL no captura datos personales u organizativos sumamente confidenciales. La biblioteca ofrece la opción de habilitar el registro de datos personales y organizativos si decide hacerlo.

En las secciones siguientes se proporcionan más detalles sobre el registro de errores de MSAL para la aplicación.