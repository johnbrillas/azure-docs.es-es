---
ms.openlocfilehash: 2e92d150851c74a84f785d1f5f0ebe2e5870a54e
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97936771"
---


| Característica |[Plan de consumo](../articles/azure-functions/consumption-plan.md)|[Plan Premium](../articles/azure-functions/functions-premium-plan.md)|[Plan dedicado](../articles/azure-functions/dedicated-plan.md)|[Azure App Service Environment](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[Restricciones de IP de entrada y acceso al sitio privado](../articles/azure-functions/functions-networking-options.md#inbound-access-restrictions)|✅Sí|✅Sí|✅Sí|✅Sí|✅Sí|
|[Integración de redes virtuales](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌No|✅Sí (regional)|✅Sí (regional y puerta de enlace)|✅Sí| ✅Sí|
|[Desencadenadores de red virtual (no HTTP)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌No| ✅Sí |✅Sí|✅Sí|✅Sí|
|[Conexiones híbridas](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (solo Windows)|❌No|✅Sí|✅Sí|✅Sí|✅Sí|
|[Restricciones de IP de salida](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌No| ✅Sí|✅Sí|✅Sí|✅Sí|