---
title: Interoperabilidad en las reuniones de Teams
titleSuffix: An Azure Communication Services concept document
description: Unirse a reuniones de Teams
author: chpalm
manager: chpalm
services: azure-communication-services
ms.author: chpalm
ms.date: 10/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: d369e976ab1ba4e33f5eb21edb92054678f9040f
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101655741"
---
# <a name="teams-interoperability"></a>Interoperabilidad de Teams

> [!IMPORTANT]
> Para habilitar o deshabilitar la [interoperabilidad de los inquilinos de equipos](../concepts/teams-interop.md), complete [este formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR21ouQM6BHtHiripswZoZsdURDQ5SUNQTElKR0VZU0VUU1hMOTBBMVhESS4u).

Azure Communication Services se puede usar para crear experiencias de reunión personalizadas con interacción con Microsoft Teams. Los usuarios de sus soluciones de Communication Services pueden interactuar con los participantes de Teams mediante la voz, el vídeo, el chat y el uso compartido de la pantalla.

La interoperabilidad de Teams permite crear aplicaciones personalizadas que conectan usuarios a reuniones de Teams. No es necesario que los usuarios de sus aplicaciones personalizadas tengan identidades de Azure Active Directory ni licencias de Teams para disfrutar de esta funcionalidad. Esto es idóneo para que los empleados (quizá ya familiarizados con Teams) y los usuarios externos (mediante una experiencia de aplicación personalizada) se reúnan con una experiencia fluida. Por ejemplo:

1. Los empleados usan Teams para programar una reunión. 
1. Los detalles de la reunión se comparten con los usuarios externos mediante una aplicación personalizada.
   * **Usar Graph API** La aplicación de Communication Services personalizada usa las API de Microsoft Graph para acceder a los detalles de la reunión que se van a compartir. 
   * **Usar otras opciones** Por ejemplo, el vínculo de la reunión se puede copiar del calendario en Microsoft Teams.
1. Los usuarios externos utilizan su aplicación personalizada para unirse a la reunión de Teams (mediante las bibliotecas cliente de llamada y chat de Communication Services).

La arquitectura general de este caso de uso tiene el siguiente aspecto: 

![Arquitectura de la interoperabilidad de Teams](./media/call-flows/teams-interop.png)

Aunque determinadas características de las reuniones de Teams, como la posibilidad de levantar la mano, el modo Juntos y las salas de trabajo en grupo, solo estarán disponibles para los usuarios de Teams, la aplicación personalizada tendrá acceso a las funcionalidades principales de audio, vídeo, chat y uso compartido de la pantalla de la reunión. El usuario de la aplicación personalizada podrá acceder al chat de reuniones mientras se encuentre en la llamada. No podrán enviar ni recibir mensajes antes de unirse a la llamada o después de dejarla. 

Cuando un usuario de Communication Services se une a la reunión de Teams, el nombre para mostrar proporcionado mediante la biblioteca cliente de llamada se mostrará a los usuarios de Teams. Por lo demás, en Teams, al usuario de Communication Services se le tratará como usuario anónimo.  La aplicación personalizada debe considerar el uso de la autenticación del usuario y otras medidas de seguridad para proteger las reuniones de Teams. Piense en las implicaciones de seguridad de permitir que usuarios anónimos unan a las reuniones y use la [guía de seguridad de Teams](/microsoftteams/teams-security-guide#addressing-threats-to-teams-meetings) para configurar las funcionalidades disponibles para los usuarios anónimos.

La interoperabilidad de Teams con Communication Services está actualmente en versión preliminar privada. Cuando esté disponible con carácter general, los usuarios de Communication Services se tratarán como "usuarios de acceso externo". Para más información sobre el acceso externo, consulte [Llamadas, chat y colaboración con personas de fuera de la organización en Microsoft Teams](/microsoftteams/communicate-with-users-from-other-organizations).

Los usuarios de Communication Services pueden unirse a reuniones de Teams programadas siempre y cuando esté habilitado que lo hagan de manera anónima en la [configuración de la reunión](/microsoftteams/meeting-settings-in-teams).

## <a name="teams-in-government-clouds-gcc"></a>Teams en las nubes de la administración pública (GCC)
Por ahora la interoperabilidad de Azure Communication Services no es compatible con las implementaciones de Teams mediante las [nubes de la administración pública (GCC) de Microsoft 365](/MicrosoftTeams/plan-for-government-gcc). 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Incorporación de una aplicación de llamadas a una reunión de Teams](../quickstarts/voice-video-calling/get-started-teams-interop.md)