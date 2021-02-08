---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/31/2021
ms.author: glenga
ms.openlocfilehash: 9a5c143927f549124cb6e69a4c8eb4829709a9e9
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99493494"
---
+ Un [grupo de recursos](../articles/azure-resource-manager/management/overview.md), que es un contenedor lógico de recursos relacionados.
+ Una [cuenta de Azure Storage](../articles/storage/common/storage-account-create.md) estándar, que mantiene información de estado y de otro tipo sobre los proyectos.
+ Un plan de consumo, que define el host subyacente para su aplicación de funciones sin servidor. 
+ Una aplicación de funciones, que proporciona el entorno para ejecutar el código de función. Una aplicación de funciones permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos en el mismo plan de hospedaje.
+ Una instancia de Application Insights conectada a la aplicación de funciones, que realiza un seguimiento del uso de la función sin servidor.