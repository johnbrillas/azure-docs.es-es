---
title: Almacenamiento y uso de secretos en aplicaciones de Azure Service Fabric Mesh
description: Service Fabric Mesh admite secretos como recursos de Azure. Aquí se muestra cómo almacenar y administrar secretos con las aplicaciones de Service Fabric Mesh.
author: erikadoyle
ms.author: edoyle
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: 85b4bb6ee4d76a7115c9ebdd1466049afe6683c6
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2021
ms.locfileid: "99625539"
---
# <a name="service-fabric-mesh-application-secrets"></a>Secretos en la aplicación Service Fabric Mesh

> [!IMPORTANT]
> Se ha retirado la versión preliminar de Azure Service Fabric Mesh. Ya no se permitirán nuevas implementaciones a través de la API de Service Fabric Mesh. La compatibilidad con las implementaciones existentes continuará hasta el 28 de abril de 2021.
> 
> Para más información, consulte [Retirada de la versión preliminar de Azure Service Fabric Mesh](https://azure.microsoft.com/updates/azure-service-fabric-mesh-preview-retirement/).

Service Fabric Mesh admite secretos como recursos de Azure. Un secreto de Service Fabric Mesh puede ser cualquier información de texto confidencial, como cadenas de conexión de almacenamiento, contraseñas u otros valores que deben almacenarse y transmitirse de forma segura.

![Información general de los secretos de Mesh][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Recursos de los secretos de Mesh
Un secreto de la aplicación Mesh consta de:
* Un recurso **Secretos**, que es un contenedor que almacena secretos de texto. Los secretos contenidos dentro del recurso **Secretos** se almacenan y se transmiten de forma segura.
* Uno o varios recursos **Secretos o valores** que se almacenan en el contenedor de recursos **Secretos**. Cada recurso **Secretos o valores** se distingue por un número de versión.

## <a name="next-steps"></a>Pasos siguientes 
Para más información acerca de los secretos de Service Fabric Mesh, consulte:
- [Administración de secretos en aplicaciones de Service Fabric Mesh](service-fabric-mesh-howto-manage-secrets.md)
- [Introducción al modelo de recursos de Service Fabric](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
