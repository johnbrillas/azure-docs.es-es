---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 10/15/2020
ms.author: alkohli
ms.openlocfilehash: c29c7abf0964edb7a5427d8c2cfe62e066d13bfb
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98901188"
---
Antes de comenzar, asegúrese de que:

* Su suscripción de Microsoft Azure debe estar habilitada para un recurso de Azure Stack Edge. Asegúrese de que ha usado una suscripción admitida como [Contrato Enterprise (EA) de Microsoft](https://azure.microsoft.com/overview/sales-number/), [Proveedor de soluciones en la nube (CSP)](/partner-center/azure-plan-lp) o [Patrocinio de Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0036p/).
* Tiene acceso de propietario o colaborador en el nivel de grupo de recursos para los recursos de la puerta de enlace de Azure Stack Edge/Azure Storage, IoT Hub y Azure Storage.

  * Para crear cualquier recurso de Azure Stack Edge, debe tener permisos de colaborador (o superiores) con ámbito en el nivel del grupo de recursos. También deberá asegurarse de que el proveedor `Microsoft.DataBoxEdge` está registrado. Para obtener información acerca de cómo registrarse, vaya a [Registro de proveedores de recursos](../articles/databox-online/azure-stack-edge-gpu-manage-access-power-connectivity-mode.md#register-resource-providers).
  * Para crear cualquier recurso de IoT Hub, asegúrese de que el proveedor Microsoft.Devices está registrado. Para obtener información acerca de cómo registrarse, vaya a [Registro de proveedores de recursos](../articles/databox-online/azure-stack-edge-gpu-manage-access-power-connectivity-mode.md#register-resource-providers).
  * Para crear un recurso de una cuenta de Storage también se necesita acceso de colaborador, o superior, con ámbito en el nivel de grupo de recursos. De forma predeterminada Azure Storage es un proveedor de recursos registrado.
* Debe tener acceso de administrador o usuario a Graph API de Azure Active Directory. Para más información, vea [Graph API de Azure Active Directory](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
* Tiene una cuenta de almacenamiento de Microsoft Azure con credenciales de acceso.