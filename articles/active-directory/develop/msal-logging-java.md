---
title: Registro de errores y excepciones en MSAL para Java
titleSuffix: Microsoft identity platform
description: Aprenda a registrar errores y excepciones en MSAL para Java.
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
ms.openlocfilehash: d3fa13a6751b2d8acf1fc99aecbf174f1823fb0b
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98954925"
---
# <a name="logging-in-msal-for-java"></a>Registro en MSAL para Java

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="msal-for-java-logging"></a>Registro en MSAL para Java

MSAL para Java le permite usar la biblioteca de registro que ya usa con la aplicación, siempre y cuando sea compatible con SLF4J. MSAL para Java usa la [fachada de registro simple para Java](http://www.slf4j.org/) (SLF4J) como fachada o abstracción simple para varios marcos de registro, tales como [java.util.logging](https://docs.oracle.com/javase/7/docs/api/java/util/logging/package-summary.html), [Logback](http://logback.qos.ch/) y [Log4j](https://logging.apache.org/log4j/2.x/). SLF4J permite al usuario conectar el marco de registro deseado en el momento de la implementación.

Por ejemplo, para usar Logback como marco de registro de la aplicación, agregue la dependencia Logback al archivo POM de Maven de la aplicación:

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
```

Agregue luego el archivo de configuración Logback:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">

</configuration>
```

SLF4J se enlaza automáticamente a Logback en el momento de la implementación. Los registros de MSAL se escribirán en la consola.

Para obtener instrucciones sobre cómo enlazar a otros marcos de registro, consulte el [manual de SLF4J](http://www.slf4j.org/manual.html).

### <a name="personal-and-organization-information"></a>Información personal y de la organización

De forma predeterminada, el registro de MSAL no captura ni registra ningún dato personal o de la organización. En el ejemplo siguiente, el registro de datos personales o de la organización está desactivado de forma predeterminada:

```java
    PublicClientApplication app2 = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
            .authority(AUTHORITY)
            .build();
```

Active el registro de datos personales y de la organización estableciendo `logPii()` en el generador de aplicaciones cliente. Si activa el registro de datos personales o de la organización, la aplicación asume la responsabilidad de controlar de forma segura datos altamente confidenciales y de cumplir los requisitos normativos.

En el ejemplo siguiente, el registro de datos personales o de la organización está habilitado:

```java
PublicClientApplication app2 = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
        .authority(AUTHORITY)
        .logPii(true)
        .build();
```

## <a name="next-steps"></a>Pasos siguientes

Para ver más ejemplos de código, consulte [Ejemplos de código de la Plataforma de identidad de Microsoft](sample-v2-code.md).