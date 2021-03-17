---
title: Uso de identidades administradas en Communication Services
titleSuffix: An Azure Communication Services quickstart
description: Las identidades administradas le permiten autorizar a Azure Communication Services el acceso desde aplicaciones que se ejecutan en máquinas virtuales de Azure, aplicaciones de funciones y otros recursos.
services: azure-communication-services
author: peiliu
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 03/10/2021
ms.author: peiliu
ms.reviewer: mikben
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: ffda88da451e25b79112a7adf85026158bd27acc
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103492360"
---
# <a name="use-managed-identities"></a>Uso de identidades administradas
Comience a trabajar con Azure Communication Services mediante identidades administradas. Las bibliotecas cliente de identidades y de SMS de Communication Services admiten la autenticación de Azure Active Directory (Azure AD) con [identidades administradas para recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md).

En este inicio rápido se muestra cómo autorizar el acceso a las bibliotecas cliente de identidades y de SMS desde un entorno de Azure que admita identidades administradas. También se describe cómo probar el código en un entorno de desarrollo.

::: zone pivot="programming-language-csharp"
[!INCLUDE [.NET](./includes/managed-identity-net.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/managed-identity-js.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/managed-identity-java.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/managed-identity-python.md)]
::: zone-end

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre el control de acceso basado en rol de Azure](../../../articles/role-based-access-control/index.yml).
- [Más información sobre la biblioteca de identidades de Azure para .NET](/dotnet/api/overview/azure/identity-readme)
- [Creación de tokens de acceso de usuario](../quickstarts/access-tokens.md)
- [Enviar un mensaje SMS](../quickstarts/telephony-sms/send.md)
- [Más información acerca de SMS](../concepts/telephony-sms/concepts.md)
