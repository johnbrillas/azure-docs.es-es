---
title: 'Inicio rápido: envío de un mensaje de texto'
titleSuffix: An Azure Communication Services quickstart
description: Obtenga información acerca de cómo enviar un mensaje de texto mediante Azure Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.custom: tracking-python, devx-track-js
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: ed246cfc3929e2893087a99b7876138859d4667a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103488338"
---
# <a name="quickstart-send-an-sms-message"></a>Inicio rápido: Envío de un mensaje SMS

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]


[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

> [!IMPORTANT]
> Se pueden enviar y recibir SMS de números de teléfono de EE. UU. Los números de teléfono ubicados en otras zonas geográficas no se admiten aún en SMS de Communication Services.
> Para más información, consulte **[Tipos de número de teléfono](../../concepts/telephony-sms/plan-solution.md)** .

::: zone pivot="programming-language-csharp"
[!INCLUDE [Send SMS with .NET client library](./includes/send-sms-net.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Send SMS with JavaScript client library](./includes/send-sms-js.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Send SMS with Python client library](./includes/send-sms-python.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Send SMS with Java client library](./includes/send-sms-java.md)]
::: zone-end

## <a name="troubleshooting"></a>Solución de problemas

Para solucionar problemas relacionados con la entrega de SMS, puede [habilitar los informes de entrega con Event Grid](./handle-sms-events.md) para capturar los detalles de la entrega.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y quitar una suscripción a Communication Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él. Obtenga más información sobre la [limpieza de recursos](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha aprendido a enviar mensajes de texto mediante Azure Communication Services.

> [!div class="nextstepaction"]
> [Suscripción a eventos SMS](./handle-sms-events.md)

> [!div class="nextstepaction"]
> [Tipos de número de teléfono](../../concepts/telephony-sms/plan-solution.md)

> [!div class="nextstepaction"]
> [Más información acerca de SMS](../../concepts/telephony-sms/concepts.md)
