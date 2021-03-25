---
title: Configuración de la conectividad de Azure Synapse
description: En este artículo se muestra cómo establecer la configuración de conectividad en Azure Synapse Analytics.
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: security
ms.date: 03/15/2021
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: e0d8a8e3320b49b6fbe3e8ab66c0b4569fac9afd
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104587940"
---
# <a name="azure-synapse-analytics-connectivity-settings"></a>Configuración de conectividad de Azure Synapse Analytics

En este artículo se explicará la configuración de conectividad en Azure Synapse Analytics y cómo establecerla cuando corresponda.


## <a name="connection-policy"></a>Directiva de conexión
La directiva de conexión para Synapse SQL en Azure Synapse Analytics está establecida en *Valor predeterminado*. No es posible cambiarla en Azure Synapse Analytics. Más información [aquí](../../azure-sql/database/connectivity-architecture.md#connection-policy) sobre cómo afecta esto a las conexiones a Synapse SQL en Azure Synapse Analytics. 

## <a name="minimal-tls-version"></a>Versión mínima de TLS
Synapse SQL en Azure Synapse Analytics permite conexiones que utilizan todas las versiones de TLS. No se puede establecer la versión mínima de TLS para Synapse SQL en Azure Synapse Analytics.

## <a name="next-steps"></a>Pasos siguientes

Creación de un [área de trabajo de Azure Synapse](./synapse-workspace-ip-firewall.md)