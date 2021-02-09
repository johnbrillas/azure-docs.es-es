---
title: Almacenamiento de confianza para Media Services
description: La autenticación de identidades administradas permite que Media Services acceda a la cuenta de almacenamiento que se ha configurado con un firewall o una restricción de red virtual a través del acceso de almacenamiento de confianza.
keywords: almacenamiento de confianza, identidades administradas
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 1/29/2020
ms.author: inhenkel
ms.openlocfilehash: e8d21e57f9a844b3cc0538f4805780829a1350f4
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99428595"
---
# <a name="trusted-storage-for-media-services"></a>Almacenamiento de confianza para Media Services

Al crear una cuenta de Media Services, debe asociarla a una cuenta de almacenamiento. Media Services puede acceder a esa cuenta de almacenamiento mediante la autenticación del sistema o la autenticación de identidad administrada. Media Services valida que la cuenta de Media Services y la cuenta de almacenamiento se encuentran en la misma suscripción y valida que el usuario que agrega la asociación tenga acceso a la cuenta de almacenamiento con Azure Resource Manager RBAC.

## <a name="trusted-storage-with-a-firewall"></a>Almacenamiento de confianza con un firewall

Sin embargo, si quiere usar un firewall para proteger su cuenta de almacenamiento y habilitar el almacenamiento de confianza, la autenticación de [identidad administrada](concept-managed-identities.md) es la opción preferida. Esta permite que Media Services acceda a la cuenta de almacenamiento que se ha configurado con un firewall o una restricción de red virtual a través del acceso de almacenamiento de confianza. Esta permite que Media Services acceda a la cuenta de almacenamiento que se ha configurado con un firewall o una restricción de red virtual a través del acceso de almacenamiento de confianza.

> [!NOTE]
> Debe conceder el acceso de Colaborador de datos de Storage Blob de identidad administrado de AMS para que Media Services pueda leer y escribir en la cuenta de almacenamiento.  La concesión del rol Colaborador genérico no funcionará, ya que no habilita los permisos correctos en el plano de datos.

## <a name="further-reading"></a>Información adicional

Para comprender los métodos de creación de almacenamiento de confianza con identidades administradas, lea [Identidades administradas y Media Services](concept-managed-identities.md).

Para más información acerca de los servicios de Microsoft de confianza, consulte [Configuración de redes virtuales y firewalls de Azure Storage](../../storage/common/storage-network-security.md#trusted-microsoft-services).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las funciones de las identidades administradas y su uso con las aplicaciones de Azure, consulte [Identidades administradas de Azure AD](../../active-directory/managed-identities-azure-resources/overview.md).
