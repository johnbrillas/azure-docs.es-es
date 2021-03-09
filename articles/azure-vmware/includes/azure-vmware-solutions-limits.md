---
title: Límites de Azure VMware Solution
description: Limitaciones de Azure VMware Solution.
ms.topic: include
ms.date: 03/04/2021
ms.openlocfilehash: 99c27694a300284cfd99b8411306c90ad4d8a12e
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102193759"
---
<!-- Used in /azure/azure-resource-manager/management/azure-subscription-service-limits.md -->

En la tabla siguiente se describen los límites máximos para Azure VMware Solution.

| **Recurso** | **Límite** |
| :-- | :-- |
| Clústeres por nube privada | 4 |
| Número mínimo de nodos por clúster | 3 |
| Número máximo de nodos por clúster | 16 |
| Nodos por nube privada | 64 |
| vCenter por nube privada | 1  |
| Emparejamientos con sitios de HCX | 3 con Advanced Edition, 10 con Enterprise Edition |
| Máx. de SDDC vinculados a AVS ExpressRoute | 4 |
| Velocidad de puerto de AVS ExpressRoute | 10 Gbps | 
| Direcciones IP públicas expuestas a través de vWAN | 100 |
| Límites de capacidad de vSAN | 75 % del total que se puede usar (el 25 % restante se mantiene disponible para el contrato de nivel de servicio)  |

En el caso de otros límites específicos de VMware, use la [herramienta de configuración máxima de VMware](https://configmax.vmware.com/).
