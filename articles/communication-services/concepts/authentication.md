---
title: Autenticación en Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Obtenga información sobre las distintas formas en que una aplicación o un servicio pueden autenticarse en Communication Services.
author: GrantMeStrength
manager: jken
services: azure-communication-services
ms.author: jken
ms.date: 07/24/2020
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: b702afe9c4359d9f8711846d93fd79df9fc2f42e
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102485968"
---
# <a name="authenticate-to-azure-communication-services"></a>Autenticación en Azure Communication Services

Se debe autenticar cada interacción del cliente con Azure Communication Services. En una arquitectura típica, se usa la [arquitectura de cliente y servidor](./client-and-server-architecture.md), las *claves de acceso* o la *identidad administrada* en el servicio de acceso de usuarios de confianza para crear usuarios y emitir tokens. Y el *token de acceso de usuario* que emite el servicio de acceso de usuarios de confianza se utiliza para que las aplicaciones cliente accedan a otros servicios de comunicación como, por ejemplo, un servicio de chat o de llamada.

El servicio de SMS de Azure Communication Services también acepta *claves de acceso* o *identidades administradas* para la autenticación. Esto ocurre normalmente en una aplicación de servicio que se ejecuta en un entorno de servicio de confianza.

Cada opción de autorización se describe brevemente a continuación:

- Autenticación con **clave de acceso** para operaciones de SMS e identidad. La autenticación con clave de acceso es adecuada para las aplicaciones de servicio que se ejecutan en un entorno de servicio de confianza. La clave de acceso se puede encontrar en el portal de Azure Communication Services. Para autenticarse con una clave de acceso, una aplicación de servicio usa la clave de acceso como credencial para inicializar las bibliotecas cliente de SMS o identidad correspondientes. Consulte [Creación y administración de tokens de acceso](../quickstarts/access-tokens.md). Como la clave de acceso forma parte de la cadena de conexión del recurso, consulte [Creación y administración de recursos de Communication Services](../quickstarts/create-communication-resource.md). La autenticación con una cadena de conexión equivale a la autenticación con clave de acceso.
- Autenticación con **identidad administrada** para operaciones de SMS e identidad. La identidad administrada (consulte el documento sobre [identidad administrada](../quickstarts/managed-identity.md)), es adecuada para las aplicaciones de servicio que se ejecutan en un entorno de servicio de confianza. Para autenticarse con una identidad administrada, una aplicación de servicio crea una credencial con el identificador y un secreto de la identidad administrada y, a continuación, inicializa las bibliotecas cliente de SMS o identidad correspondientes. Consulte [Creación y administración de tokens de acceso](../quickstarts/access-tokens.md).
- Autenticación con **token de acceso de usuario** para chat y llamadas. Los tokens de acceso de usuario permiten que las aplicaciones cliente se autentiquen en los servicios de chat y llamadas de Azure Communication Services. Estos tokens se generan en un "servicio de acceso de usuarios de confianza" que se crea. A continuación, se proporcionan a los dispositivos cliente que usan el token para inicializar las bibliotecas cliente de chat y llamadas. Para más información, consulte [Incorporación de chat a una aplicación](../quickstarts/chat/get-started.md), por ejemplo.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación y administración de recursos de Communication Services](../quickstarts/create-communication-resource.md)
> [Creación de una aplicación de identidad administrada de Azure Active Directory mediante la CLI de Azure](../quickstarts/managed-identity-from-cli.md)
> [Creación de tokens de acceso de usuario](../quickstarts/access-tokens.md)

Para más información, consulte los siguientes artículos.
- [Información sobre la arquitectura de cliente y servidor](../concepts/client-and-server-architecture.md)
