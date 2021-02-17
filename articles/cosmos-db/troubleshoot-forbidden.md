---
title: Solución de problemas de excepciones de solicitudes prohibidas de Azure Cosmos DB
description: Aprenda a diagnosticar y corregir excepciones de solicitudes prohibidas.
author: ealsur
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.date: 02/05/2021
ms.author: maquaran
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 0854e5d2c323df695460908c45714673756328c2
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99971895"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-forbidden-exceptions"></a>Diagnóstico y solución de problemas de excepciones de solicitudes prohibidas de Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

El código de estado HTTP 403 significa que se prohíbe que se complete la solicitud.

## <a name="firewall-blocking-requests"></a>Solicitudes de bloqueo de firewall
En este escenario, es habitual ver errores como los siguientes:

```
Request originated from client IP {...} through public internet. This is blocked by your Cosmos DB account firewall settings.
```

```
Request is blocked. Please check your authorization token and Cosmos DB account firewall settings
```

### <a name="solution"></a>Solución
Compruebe que la [configuración actual del firewall](how-to-configure-firewall.md) sea correcta e incluya las direcciones IP o redes a las que está intentando conectarse.
Si las ha actualizado recientemente, tenga en cuenta que los cambios pueden tardar **hasta 15 minutos en aplicarse**.

## <a name="next-steps"></a>Pasos siguientes
* Configuración de [firewall de IP](how-to-configure-firewall.md).
* Configuración del acceso desde [redes virtuales](how-to-configure-vnet-service-endpoint.md).
* Configuración del acceso desde [puntos de conexión privados](how-to-configure-private-endpoints.md).
