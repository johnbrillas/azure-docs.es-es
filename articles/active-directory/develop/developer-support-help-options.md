---
title: Opciones de ayuda y soporte técnico para los desarrolladores de aplicaciones de Azure AD
description: Infórmese sobre cómo obtener ayuda y soporte técnico para preguntas relacionadas con el desarrollo, y para resolver los problemas que pueden surgir al crear aplicaciones que se integran con las identidades de Microsoft (Azure Active Directory y cuenta Microsoft)
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/23/2019
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: bce9479d063d091eb4fa68d2452d8a4218d45db9
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2021
ms.locfileid: "99219950"
---
# <a name="support-and-help-options-for-developers"></a>Opciones de ayuda y soporte técnico para desarrolladores

Si está empezando a integrarse con Azure Active Directory (Azure AD), las identidades de Microsoft o Microsoft Graph API, o si está implementando una nueva característica en una aplicación, hay ocasiones en las que puede necesitar ayuda de la comunidad de usuarios o querer saber las opciones de soporte técnico que tienen como desarrollador. Este artículo le ayuda a conocer estas opciones, entre ellas:

> [!div class="checklist"]
> * Cómo buscar si la pregunta sobre su problema no se ha contestado en la comunidad o si existe documentación para la característica que está intentando implementar
> * En algunos casos, puede que lo que desea es usar nuestras herramientas de soporte técnico para ayudarle a depurar un problema específico
> * Si no encuentra la respuesta que necesita, puede formular una pregunta en *Microsoft Q&A*
> * Si tiene algún problema con una de nuestras bibliotecas de autenticación, genere un *problema de GitHub*
> * Por último, si necesita hablar con alguien puede abrir una solicitud de soporte técnico

## <a name="search"></a>Search

Si tiene alguna pregunta relacionada con el desarrollo, puede que encuentre la respuesta en nuestra documentación, los [ejemplos de GitHub](https://github.com/azure-samples) o las respuestas a las preguntas de [Microsoft Q&A](https://docs.microsoft.com/answers/products/).

### <a name="scoped-search"></a>Búsqueda restringida

Para obtener resultados más rápido, limite su búsqueda a [Microsoft Q&A](https://docs.microsoft.com/answers/products/), la documentación y los ejemplos de código utilizando la consulta siguiente en su motor de búsqueda favorito:

```
{Your Search Terms} (site:http://www.docs.microsoft.com/answers/products/ OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/graph)
```

Donde *{Your Search Terms}* son sus palabras clave de búsqueda.

## <a name="use-the-development-support-tools"></a>Uso de las herramientas de soporte técnico de desarrollo

| Herramienta  | Descripción  |
|---------|---------|
| [jwt.ms](https://jwt.ms) | Pegue un identificador o un token de acceso para descodificar los valores y nombres de notificaciones. |
| [Explorador de Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer)| Esta herramienta le permite realizar las solicitudes y ver las respuestas en Microsoft Graph API. |

## <a name="post-a-question-to-microsoft-qa"></a>Publicación de una pregunta en Microsoft Q&A

[Microsoft Q&A](https://docs.microsoft.com/answers/products/) es el canal preferido para las preguntas relacionadas con el desarrollo. Aquí, los miembros de la comunidad de desarrolladores y los miembros del equipo de Microsoft se implican directamente para ayudarle a solucionar problemas.

Si no encuentra una respuesta a su pregunta a través de la búsqueda, envíe una nueva pregunta a [Microsoft Q&A](https://docs.microsoft.com/answers/products/). Use una de las siguientes etiquetas cuando formule preguntas para ayudar a la comunidad a identificar y responder su pregunta más rápidamente:

|Componente o área  | Etiquetas |
|---------|---------|
| Biblioteca ADAL | [[adal]](https://docs.microsoft.com/answers/topics/azure-ad-adal-deprecation.html) |
| Biblioteca MSAL     | [[msal]](https://docs.microsoft.com/answers/topics/azure-ad-msal.html) |
| Middleware OWIN  | [[azure-active directory]](https://docs.microsoft.com/answers/topics/azure-active-directory.html) |
| [B2B de Azure](../external-identities/what-is-b2b.md)  | [[azure-ad-b2b]](https://docs.microsoft.com/answers/topics/azure-ad-b2b.html) |
| [B2C de Azure](https://azure.microsoft.com/services/active-directory-b2c/)  | [[azure-ad-b2c]](https://docs.microsoft.com/answers/topics/azure-ad-b2c.html) |
| [Microsoft Graph API](https://developer.microsoft.com/graph/) | [[azure-ad-graph]](https://docs.microsoft.com/answers/topics/azure-ad-graph.html) |
| Cualquier otra área relacionada con temas de autenticación o autorización | [[azure-active directory]](https://docs.microsoft.com/answers/topics/azure-active-directory.html) |

Las siguientes publicaciones de [Microsoft Q&A](https://docs.microsoft.com/answers/products/) contienen sugerencias sobre cómo formular preguntas y agregar código fuente. Seguir estas directrices aumentan las posibilidades de que los miembros de la comunidad evalúen y respondan a su pregunta rápidamente:

* [Cómo se puede formular una buena pregunta](https://docs.microsoft.com/answers/articles/24951/how-to-write-a-quality-question.html)
* [Cómo crear un ejemplo mínimo, completo y comprobable](https://docs.microsoft.com/answers/articles/24907/how-to-write-a-quality-answer.html)

## <a name="create-a-github-issue"></a>Creación de un problema de GitHub

Si encuentra un error o un problema relacionado con nuestras bibliotecas, genere un problema en los repositorios de GitHub. Dado que nuestra bibliotecas son de código abierto, también puede enviar una solicitud de incorporación de cambios.

Para ver una lista de bibliotecas y sus repositorios de GitHub, consulte lo siguiente:

* [Bibliotecas de autenticación de Azure Active Directory (ADAL)](../azuread-dev/active-directory-authentication-libraries.md) y repositorios de GitHub
* [Bibliotecas de autenticación de Microsoft (MSAL)](reference-v2-libraries.md) y repositorios de GitHub

## <a name="open-a-support-request"></a>Apertura de una solicitud de soporte técnico

Si necesita hablar con alguien, puede abrir una solicitud de soporte técnico. Si es un cliente de Azure, tiene varias opciones de soporte técnico. Para comparar los planes, consulte [esta página](https://azure.microsoft.com/support/plans/). El soporte técnico Developer también está disponible para los clientes de Azure. Para obtener información sobre cómo adquirir planes de soporte técnico de Developer, consulte [esta página](https://azure.microsoft.com/support/plans/developer/).

* Si ya tiene un plan de soporte técnico de Azure, [abra una solicitud de soporte aquí](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

* Si no es un cliente de Azure, también puede abrir una solicitud de soporte técnico con Microsoft a través de [nuestro soporte comercial](https://support.serviceshub.microsoft.com/supportforbusiness).

También puede intentar un [agente virtual](https://support.microsoft.com/contactus/?ws=support) para obtener soporte técnico o formular alguna pregunta.