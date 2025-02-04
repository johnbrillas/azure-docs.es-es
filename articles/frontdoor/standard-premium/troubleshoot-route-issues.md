---
title: Solución de problemas de configuración de Azure Front Door Estándar/Prémium
description: En este tutorial, aprenderá a solucionar algunos de los problemas comunes que pueden surgir con la instancia de Azure Front Door Estándar/Premium.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: 4690a513494d794377ee0c2e8cfb101e8fd66a0f
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101098259"
---
# <a name="troubleshooting-common-routing-problems-with-azure-front-door-standardpremium"></a>Solución de problemas comunes de enrutamiento con Azure Front Door Estándar/Prémium

En este artículo se describe cómo solucionar problemas comunes de enrutamiento que pueden surgir con la configuración de Azure Front Door.

## <a name="503-response-from-azure-front-door-after-a-few-seconds"></a>Respuesta 503 de Azure Front Door después de unos segundos

### <a name="symptom"></a>Síntoma

* Las solicitudes normales enviadas al back-end sin pasar a través de Azure Front Door son correctas. Al pasar a través de Azure Front Door, se producen respuestas de error 503.
* El error de Azure Front Door suele aparecer después de unos 30 segundos.

### <a name="cause"></a>Causa

La causa de este problema puede ser una de estas dos:
 
* El origen tarda más que el tiempo de expiración configurado (el valor predeterminado es 30 segundos) en recibir la solicitud de Azure Front Door.
* El tiempo que se tarda en enviar una respuesta a la solicitud desde Azure Front Door es mayor que el valor de tiempo de expiración. 

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

* Envíe la solicitud al back-end directamente (sin pasar a través de Azure Front Door). Vea cuánto tiempo tarda el back-end en responder.
* Envíe la solicitud a través de Azure Front Door y vea si se obtienen respuestas 503. Si no es así, es posible que no se trate de un problema de tiempo de expiración. Póngase en contacto con el servicio de soporte técnico.
* Si se analizan los resultados de Azure Front Door en el código de respuesta de error 503, configure el campo `sendReceiveTimeout` para su instancia de Azure Front Door. Puede ampliar el tiempo de expiración predeterminado hasta 4 minutos (240 segundos). La opción de configuración está en `Endpoint Setting` y se denomina `Origin response timeout`. 

## <a name="requests-sent-to-the-custom-domain-return-a-400-status-code"></a>Las solicitudes enviadas al dominio personalizado devuelven el código de estado 400

### <a name="symptom"></a>Síntoma

* Creó una instancia de Azure Front Door, pero una solicitud al dominio o el host de front-end devuelve un código de estado HTTP 400.
* Creó una asignación de DNS para un dominio personalizado para el host de front-end que configuró. Sin embargo, el envío de una solicitud al nombre de host de dominio personalizado devuelve un código de estado HTTP 400. No parece enrutarse al back-end que configuró.

### <a name="cause"></a>Causa

El problema se produce si no configuró una regla de enrutamiento para el dominio personalizado que se agregó como host de front-end. Es necesario agregar explícitamente una regla de enrutamiento para ese host de front-end. Esto es así incluso si ya se ha configurado una regla de enrutamiento para el host de front-end en el subdominio de Azure Front Door (*.azurefd.net).

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

Agregue una regla de enrutamiento para que el dominio personalizado dirija el tráfico al grupo de origen seleccionado.

## <a name="azure-front-door-doesnt-redirect-http-to-https"></a>Azure Front Door no redirige HTTP a HTTPS

### <a name="symptom"></a>Síntoma

Azure Front Door tiene una regla de enrutamiento que redirige HTTP a HTTPS, pero el acceso al dominio sigue manteniendo HTTP como protocolo.

### <a name="cause"></a>Causa

Este comportamiento puede producirse si no configuró correctamente las reglas de enrutamiento para Azure Front Door. Básicamente, la configuración actual no se ha especificado y puede tener reglas en conflicto.

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas


## <a name="request-to-the-frontend-host-name-returns-a-411-status-code"></a>La solicitud al nombre de host de front-end devuelve el código de estado 411

### <a name="symptom"></a>Síntoma

Ha creado una instancia de Azure Front Door Estándar/Prémium y ha configurado un host de front-end, un grupo de origen que contiene al menos un origen y una regla de enrutamiento que conecta el host de front-end con el grupo de origen. El contenido parece no estar disponible cuando una solicitud se dirige al host de front-end configurado, ya que se devuelve un código de estado HTTP 411.

Las respuestas a estas solicitudes también pueden contener una página de error HTML en el cuerpo de la respuesta que incluya una declaración explicativa. Por ejemplo: `HTTP Error 411. The request must be chunked or have a content length`.

### <a name="cause"></a>Causa

Hay varias causas posibles para este síntoma. El motivo general es que la solicitud HTTP no es totalmente compatible con RFC. 

Un ejemplo de incumplimiento es una solicitud `POST` enviada sin un encabezado `Content-Length` o `Transfer-Encoding` (por ejemplo, con `curl -X POST https://example-front-door.domain.com`). Esta solicitud no cumple los requisitos establecidos en [RFC 7230](https://tools.ietf.org/html/rfc7230#section-3.3.2). Azure Front Door la bloqueará con una respuesta HTTP 411.

Este comportamiento es independiente de la funcionalidad Web Application Firewall (WAF) de Azure Front Door. Actualmente, no hay ninguna manera de deshabilitar este comportamiento. Todas las solicitudes HTTP deben cumplir los requisitos, incluso si la funcionalidad WAF no está en uso.

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

- Compruebe que las solicitudes cumplen los requisitos establecidos en las normativas RFC exigidas.

- Tome nota de cualquier cuerpo de mensaje HTML que se devuelva en respuesta a su solicitud. Un cuerpo de mensaje suele explicar exactamente *cómo* la solicitud no es conforme.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [crear una instancia de Front Door Estándar/Prémium](create-front-door-portal.md).
