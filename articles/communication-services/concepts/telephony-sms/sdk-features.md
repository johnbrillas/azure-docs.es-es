---
title: Información general de la biblioteca cliente de SMS para Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Proporciona información general sobre la biblioteca cliente de SMS y sus ofertas.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 11ab634ed4b25c5fd8c0079263094c393e9dcbe6
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103496599"
---
# <a name="sms-client-library-overview"></a>Información general de la biblioteca cliente de SMS

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]


[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

Las bibliotecas cliente de SMS de Azure Communication Services se pueden usar para agregar mensajería SMS a las aplicaciones.

## <a name="sms-client-library-capabilities"></a>Funcionalidades de la biblioteca cliente de SMS

En la lista siguiente se presenta el conjunto de características que están disponibles actualmente en nuestras bibliotecas cliente.

| Grupo de características | Capacidad                                                                            | JS  | Java | .NET | Python |
| ----------------- | ------------------------------------------------------------------------------------- | --- | ---- | ---- | ------ |
| Funcionalidades principales | Enviar y recibir mensajes SMS </br> *Se admiten los emojis Unicode*                        | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Recibir informes de entrega para los mensajes enviados                                            | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Todos los juegos de caracteres (compatibilidad con lenguaje/Unicode)                                         | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Compatibilidad con mensajes largos (hasta 2048 caracteres)                                           | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Concatenación automática de mensajes largos                                                   | ✔️   | ✔️    | ✔️    | ✔️      |
| Events            | Usar Event Grid para configurar webhooks para recibir mensajes entrantes e informes de entrega | ✔️   | ✔️    | ✔️    | ✔️      |
| Número de teléfono      | Números de teléfono gratuitos                                                                     | ✔️   | ✔️    | ✔️    | ✔️      |
| Reglamentario        | Control de deshabilitación de envío                                                                      | ✔️   | ✔️    | ✔️    | ✔️      |
| Supervisión        | Supervisar el uso de los mensajes enviados y recibidos                                          | ✔️   | ✔️    | ✔️    | ✔️      |
| Llamadas RTC      | Agregar funcionalidades de llamadas RTC al número de teléfono gratuito habilitado para SMS                    | ✔️   | ✔️    | ✔️    | ✔️      |

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción al envío de SMS](../../quickstarts/telephony-sms/send.md)

Puede que los siguientes documentos le resulten interesantes:

- Familiarización con los [conceptos de SMS](../telephony-sms/concepts.md) generales
- Obtención de un [número de teléfono](../../quickstarts/telephony-sms/get-phone-number.md) compatible con SMS
- [Tipos de número de teléfono en Azure Communication Services](../telephony-sms/plan-solution.md)
