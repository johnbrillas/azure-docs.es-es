---
title: Información general de las etiquetas del servicio Azure Firewall
description: Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: 83e9a96573bbc72e0afff61cc0f151f95b081e30
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97031586"
---
# <a name="azure-firewall-service-tags"></a>Etiquetas de servicio de Azure Firewall

Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad. No puede crear su propia etiqueta de servicio, ni especificar qué direcciones IP se incluyen dentro de una etiqueta. Microsoft administra los prefijos de direcciones que incluye la etiqueta de servicio y actualiza automáticamente esta a medida que las direcciones cambian.

Las etiquetas de servicio de Azure Firewall se pueden usar en el campo de destino de las reglas de red. Puede usarlas en lugar de direcciones IP específicas.

## <a name="supported-service-tags"></a>Etiquetas de servicio admitidas

Consulte [Etiquetas de servicio de red virtual](../virtual-network/service-tags-overview.md#available-service-tags) para obtener una lista de etiquetas de servicio que están disponibles para su uso en las reglas de red del firewall de Azure.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reglas de Azure Firewall, consulte [Lógica de procesamiento de reglas de Azure Firewall](rule-processing.md).
