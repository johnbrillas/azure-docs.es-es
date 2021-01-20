---
title: Introducción a la interoperabilidad con Teams en Azure Communication Services
titleSuffix: An Azure Communication Services quickstart
description: En este inicio rápido aprenderá a unirse a una reunión de Teams con la biblioteca cliente de Azure Communication Chat
author: askaur
ms.author: askaur
ms.date: 12/08/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 1ad6b7241c7167c6da8952e7db2797fa275b7246
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98251937"
---
# <a name="quickstart-join-your-chat-app-to-a-teams-meeting"></a>Inicio rápido: Incorporación de una aplicación de chat a una reunión de Teams

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

Comience a usar Azure Communication Services mediante la conexión de la solución de chat a Microsoft Teams con la biblioteca cliente de JavaScript. 

## <a name="prerequisites"></a>Requisitos previos 

1. Una  [implementación de Teams](https://docs.microsoft.com/deployoffice/teams-install). 
2. Una [aplicación de chat](./get-started.md) activa. 

## <a name="enable-teams-interoperability"></a>Habilitación de la interoperabilidad de Teams 

Cualquier usuario de Communication Services que se una a una reunión de Teams como invitado solo puede acceder al chat cuando se haya unido a la llamada de la reunión de Teams. Consulte la documentación de la [interoperabilidad de Teams](../voice-video-calling/get-started-teams-interop.md) para aprender a agregar un usuario de Communication Services a una llamada de reunión de Teams.

Para usar esta característica debe ser miembro de la organización propietaria de ambas entidades.

[!INCLUDE [Join Teams meetings](./includes/meeting-interop-javascript.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y quitar una suscripción a Communication Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él. Obtenga más información sobre la [limpieza de recursos](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos.

- Consulte nuestro [ejemplo de elementos principales de un chat](../../samples/chat-hero-sample.md).
- Más información sobre el [funcionamiento del chat](../../concepts/chat/concepts.md).
