---
title: ¿Qué es el diagnóstico del inicio de sesión de Azure AD? | Microsoft Docs
description: Proporciona información general sobre el diagnóstico del inicio de sesión de Azure AD.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: e2b3d8ce-708a-46e4-b474-123792f35526
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 12/15/2020
ms.author: markvi
ms.reviewer: tspring
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6aedf41fbf1ed0d70467a2efe97431fdecaa4fa
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97585929"
---
# <a name="what-is-sign-in-diagnostic-in-azure-ad"></a>¿Qué es el diagnóstico del inicio de sesión de Azure AD?

Azure AD proporciona un modelo de seguridad flexible para controlar las acciones que pueden realizar los usuarios con los recursos administrados. El acceso a estos recursos no solo se controla por **quién** hace uso de ellos, sino también por **cómo** se accede a ellos. Normalmente, la flexibilidad conlleva un grado de complejidad debido al número de opciones de configuración que tiene. La complejidad hace que pueda aumentar el riesgo de errores.

Los administradores de TI, necesitan una solución que proporcione el nivel adecuado de conocimiento de las actividades que tienen lugar en el sistema, con el fin de que si se produce algún problema, se pueda diagnosticar y resolver fácilmente. El diagnóstico del inicio de sesión de Azure AD es un ejemplo de este tipo de solución. Use el diagnóstico para analizar tanto lo que ha sucedido durante un inicio de sesión como las acciones que puede realizar para resolver los problemas sin necesidad de que intervenga el soporte técnico de Microsoft.

En este artículo se proporciona información general no solo sobre las funciones de la solución, sino también sobre cómo se puede usar.


## <a name="requirements"></a>Requisitos

El diagnóstico del inicio de sesión está disponible en todas las ediciones de Azure AD.<br> Para usarlo, es preciso ser administrador global de Azure AD.

## <a name="how-it-works"></a>Funcionamiento

En Azure AD, la respuesta a un intento de inicio de sesión está ligada a **quién** es el usuario y a **cómo** accede al inquilino. Por ejemplo, los administradores habitualmente pueden configurar todos los aspectos de su inquilino cuando inician sesión desde la red corporativa. Sin embargo, existe la posibilidad de que se les bloquee al iniciar sesión con la misma cuenta desde una red que no sea de confianza.
 
Dada la mayor flexibilidad del sistema para responder a un intento de inicio de sesión, podrían acabar en escenarios en los que tengan que solucionar problemas de inicio de sesión. El diagnóstico de inicio de sesión es una característica que:

- Analiza los datos de los inicios de sesión. 

- Muestra lo que ha sucedido y recomendaciones sobre cómo resolver problemas. 

El diagnóstico de inicio de sesión de Azure AD está diseñado para habilitar el diagnóstico automático de errores en el inicio de sesión. Para completar el proceso de diagnóstico, es preciso:

![Proceso de diagnóstico de inicio de sesión](./media/overview-sign-in-diagnostics/process.png)
 
1. **Definir** el ámbito de los eventos de inicio de sesión que interesan.

2. **Seleccionar** el inicio de sesión que se desea revisar.

3. **Examinar** el resultado del diagnóstico.

4. **Realizar** las acciones pertinentes.

 
### <a name="define-scope"></a>Definición del ámbito

El objetivo de este paso es definir el ámbito de los inicios de sesión que desea investigar. Dicho ámbito se basa en un usuario o un identificador (correlationId, requestId) y en un intervalo de tiempo. Para reducir aún más el ámbito, también puede especificar un nombre de aplicación. Azure AD usa la información del ámbito para buscar los eventos adecuados.  

### <a name="select-sign-in"></a>Selección de inicio de sesión  

En función de los criterios de búsqueda, Azure AD recupera todos los inicios de sesión que coincidan y los presenta en una vista de lista del resumen de la autenticación. 

![Resumen de autenticación](./media/overview-sign-in-diagnostics/authentication-summary.png)
 
Las columnas que se muestran en esta vista se pueden personalizar.

### <a name="review-diagnostic"></a>Revisión del diagnóstico 

Azure AD proporciona el resultado del diagnóstico de evento de inicio de sesión seleccionado. 

![Resultados del diagnóstico](./media/overview-sign-in-diagnostics/diagnostics-results.png)

 
El resultado comienza con una valoración, que explica sucintamente lo que ha sucedido. Dicha explicación le ayuda a comprender el comportamiento del sistema. 

Como paso siguiente, recibirá un resumen de las directivas de acceso condicional relacionadas que se aplicaron al inicio de sesión seleccionado. Esta parte se completa con los pasos de corrección recomendados para resolver el problema. Como no siempre es posible resolver problemas sin ayuda adicional, un paso recomendado podría ser abrir una incidencia de soporte técnico. 

### <a name="take-action"></a>Realizar acción 
En este momento, debe tener la información necesaria para corregir el problema.


## <a name="scenarios"></a>Escenarios

En esta sección se proporciona información general sobre los escenarios de diagnóstico descritos. Se implementan los siguientes escenarios: 
 
- Bloqueado por acceso condicional

- Acceso condicional con error

- Autenticación multifactor de acceso condicional

- Autenticación multifactor de otros requisitos

- Se requiere una prueba de autenticación multifactor

- Se requiere una prueba de autenticación multifactor, pero el intento de inicio de sesión del usuario no es desde una ubicación segura

- Inicio de sesión correcto


### <a name="blocked-by-conditional-access"></a>Bloqueado por acceso condicional

Este escenario se basa en un inicio de sesión bloqueado por una directiva de acceso condicional.

![Bloquear acceso](./media/overview-sign-in-diagnostics/block-access.png)

La sección de diagnóstico de este escenario muestra detalles sobre el inicio de sesión del usuario y las directivas aplicadas.


### <a name="failed-conditional-access"></a>Acceso condicional con error

Este escenario suele ser el resultado de un error del inicio de sesión debido a que no se cumplieron los requisitos de una directiva de acceso condicional. Los ejemplos comunes son:

![Requerir controles](./media/overview-sign-in-diagnostics/require-controls.png)

- Requerir un dispositivo unido a Azure AD híbrido

- Requerir aplicación cliente aprobada

- Requerir la directiva de protección de aplicaciones   


La sección de diagnóstico de este escenario muestra detalles sobre el inicio de sesión del usuario y las directivas aplicadas.


### <a name="mfa-from-conditional-access"></a>Autenticación multifactor de acceso condicional

Este escenario se basa en una directiva de acceso condicional que tiene el requisito para iniciar sesión mediante la autenticación multifactor establecida.

![Requerir autenticación multifactor](./media/overview-sign-in-diagnostics/require-mfa.png)

La sección de diagnóstico de este escenario muestra detalles sobre el inicio de sesión del usuario y las directivas aplicadas.



### <a name="mfa-from-other-requirements"></a>Autenticación multifactor de otros requisitos

Este escenario se basa en un requisito de autenticación multifactor que una directiva de acceso condicional no ha exigido. Por ejemplo, autenticación multifactor usuario a usuario.


![Requerir autenticación multifactor a todos los usuarios](./media/overview-sign-in-diagnostics/mfa-per-user.png)


La intención de este escenario de diagnóstico es proporcionar más detalles sobre:

- El origen de la interrupción de la autenticación multifactor. 
- El resultado de la interacción del cliente.

Además, en esta sección también se proporcionan todos los detalles sobre el intento de inicio de sesión del usuario. 


### <a name="mfa-proof-up-required"></a>Se requiere una prueba de autenticación multifactor

Este escenario se basa en inicios de sesión que se han interrumpido por solicitudes de configuración de la autenticación multifactor. Esta configuración se conoce también como "comprobación de la autenticación multifactor".

La prueba de autenticación multifactor se produce cuando se exige a un usuario que utilice la autenticación multifactor, pero aún no se ha configurado, o un administrador ha configurado el usuario para configurarlo.

La intención de este escenario de diagnóstico es informar de que la interrupción de la autenticación multifactor se debe a que hay que configurarla, así como recomendar al usuario que la complete.

### <a name="mfa-proof-up-required-from-a-risky-sign-in"></a>Prueba de autenticación multifactor necesaria desde un inicio de sesión de riesgo

Este escenario es el resultado de inicios de sesión que se han interrumpido por una solicitud para configurar la autenticación multifactor desde un información de inicio de sesión. 

La intención de este escenario de diagnóstico es proporcionar la información de que la interrupción de la autenticación se tenía que configurar, proporcionar la recomendación de que el usuario complete la prueba, pero hacerlo desde una ubicación de red que no parece de riesgo. Por ejemplo, si una red corporativa se define como una ubicación con nombre, intente realizar la prueba desde la red corporativa.


### <a name="successful-sign-in"></a>Inicio de sesión correcto

Este escenario se basa en inicios de sesión que no se han interrumpido por el acceso condicional o la autenticación multifactor.

La intención de este escenario de diagnóstico es proporcionar una visión general de lo que el usuario especificó durante el inicio de sesión, en caso de que se esperara que se apliquen una o varias directivas de acceso condicional, o bien que se esperara que una autenticación multifactor configurada interrumpa el inicio de sesión del usuario.



## <a name="next-steps"></a>Pasos siguientes

* [¿Qué son los informes de Azure Active Directory?](overview-reports.md)
* [¿Qué es la supervisión de Azure Active Directory?](overview-monitoring.md)
