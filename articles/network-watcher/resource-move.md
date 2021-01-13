---
title: Traslado de recursos de Azure Network Watcher | Microsoft Docs
description: Traslado de recursos de Azure Network Watcher entre regiones
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: damendo
ms.openlocfilehash: 4853f485e4424c3c3263a18d27834d0f9ae94918
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98019659"
---
# <a name="moving-azure-network-watcher-resources-across-regions"></a>Traslado de recursos de Azure Network Watcher entre regiones

## <a name="moving-the-network-watcher-resource"></a>Traslado del recurso de Network Watcher
El recurso de Network Watcher representa el servicio back-end para Network Watcher y está totalmente administrado por Azure. Los clientes no tienen que administrarlo. La operación de traslado no se admite en este recurso.

## <a name="moving-child-resources-of-network-watcher"></a>Mover recursos secundarios de Network Watcher
Actualmente no se admite el traslado de recursos entre regiones para ningún recurso secundario del tipo de recurso `*networkWatcher*`.

## <a name="next-steps"></a>Pasos siguientes
* Lea la [introducción a Network Watcher](./network-watcher-monitoring-overview.md).
* Consulte las [Preguntas frecuentes de Network Watcher](./frequently-asked-questions.md).