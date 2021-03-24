---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: 0eeb82245a53c93af75fc3ce3f37cb588295e5b7
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102508102"
---
Las entradas del documento `deploymentconfig.json` se asignan a los parámetros de [LocalWebservice.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro del método | Description |
| ----- | ----- | ----- |
| `computeType` | N/D | El destino de proceso. Para los destinos locales, el valor tiene que ser `local`. |
| `port` | `port` | Puerto local en el que se va a exponer el punto de conexión HTTP del servicio. |

Este elemento JSON es un ejemplo de la configuración de implementación que se puede usar con la CLI:

```json
{
    "computeType": "local",
    "port": 32267
}
```