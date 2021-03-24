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
ms.openlocfilehash: ce1a4808833cbd897da17f9ad75af346538d23d1
ms.sourcegitcommit: 66ce33826d77416dc2e4ba5447eeb387705a6ae5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2021
ms.locfileid: "103472798"
---
# <a name="azure-synapse-analytics-connectivity-settings"></a>Configuración de conectividad de Azure Synapse Analytics

En este artículo se explicará la configuración de conectividad en Azure Synapse Analytics y cómo establecerla cuando corresponda.


## <a name="connection-policy"></a>Directiva de conexión
La directiva de conexión para Synapse SQL en Azure Synapse Analytics está establecida en *Valor predeterminado*. No es posible cambiarla en Azure Synapse Analytics. Más información [aquí](https://docs.microsoft.com/azure/azure-sql/database/connectivity-architecture#connection-policy) sobre cómo afecta esto a las conexiones a Synapse SQL en Azure Synapse Analytics. 

## <a name="minimal-tls-version"></a>Versión mínima de TLS
Synapse SQL en Azure Synapse Analytics permite conexiones que utilizan todas las versiones de TLS. No se puede establecer la versión mínima de TLS para Synapse SQL en Azure Synapse Analytics.

## <a name="next-steps"></a>Pasos siguientes

Creación de un [área de trabajo de Azure Synapse](./synapse-workspace-ip-firewall.md)
