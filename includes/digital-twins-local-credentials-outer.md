---
author: baanders
description: Archivo de inclusión para configurar la autenticación local de DefaultAzureCredential en los ejemplos de Azure Digital Twins, con introducción
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: dcef8030c47e771e5cd771dac09b5f96e3d38abd
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102473846"
---
### <a name="set-up-local-azure-credentials"></a>Configuración de credenciales locales de Azure

En este ejemplo se usa [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) (parte de la biblioteca de `Azure.Identity`) para autenticar a los usuarios mediante la instancia de Azure Digital Twins cuando la ejecuta en la máquina local. Para más información sobre las distintas formas en que una aplicación cliente puede autenticarse mediante Azure Digital Twins, consulte [*Procedimiento: Escritura de código de autenticación de aplicación*](../articles/digital-twins/how-to-authenticate-client.md).

[!INCLUDE [Azure Digital Twins: local credentials prereq (inner)](digital-twins-local-credentials-inner.md)]