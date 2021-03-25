---
title: 'Inicio rápido: Interoperabilidad con Teams en Azure Communication Services'
titleSuffix: An Azure Communication Services quickstart
description: En este inicio rápido aprenderá a unirse a una reunión de Teams con el SDK de llamada de Azure Communication Services.
author: chpalm
ms.author: chpalm
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-plat-web-ios-android
ms.openlocfilehash: a9ef74c04c1f709348ae1d6dd97558ee6bedccf3
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654975"
---
# <a name="quickstart-join-your-calling-app-to-a-teams-meeting"></a>Inicio rápido: Incorporación de una aplicación de llamadas a una reunión de Teams

> [!IMPORTANT]
> Para habilitar o deshabilitar la [interoperabilidad de los inquilinos de equipos](../../concepts/teams-interop.md), complete [este formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR21ouQM6BHtHiripswZoZsdURDQ5SUNQTElKR0VZU0VUU1hMOTBBMVhESS4u).

Comience a usar Azure Communication Services mediante la conexión de la solución de llamada a Microsoft Teams con la biblioteca cliente de JavaScript.

::: zone pivot="platform-web"
[!INCLUDE [Calling with JavaScript](./includes/teams-interop-javascript.md)]
::: zone-end

::: zone pivot="platform-android"
[!INCLUDE [Calling with Android](./includes/teams-interop-android.md)]
::: zone-end

::: zone pivot="platform-ios"
[!INCLUDE [Calling with iOS](./includes/teams-interop-ios.md)]
::: zone-end

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y quitar una suscripción a Communication Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él. Obtenga más información sobre la [limpieza de recursos](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos.

- Consulte nuestro [ejemplo de elementos principales de una llamada](../../samples/calling-hero-sample.md).
- Información sobre las [funcionalidades de la biblioteca cliente de llamadas](./calling-client-samples.md)
- Más información sobre [cómo funciona la llamada](../../concepts/voice-video-calling/about-call-types.md)
