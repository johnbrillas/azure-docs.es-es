---
title: Uso de identidades administradas en Communication Services
titleSuffix: An Azure Communication Services quickstart
description: Las identidades administradas le permiten autorizar a Azure Communication Services el acceso desde aplicaciones que se ejecutan en máquinas virtuales de Azure, aplicaciones de funciones y otros recursos.
services: azure-communication-services
author: peiliu
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 2/24/2021
ms.author: peiliu
ms.reviewer: mikben
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: 2aea4daddb9cccbf7f268c596fd121357cc17b6d
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102486647"
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
