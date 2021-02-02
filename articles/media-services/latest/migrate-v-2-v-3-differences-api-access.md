---
title: Diferencias de acceso de API entre Azure Media Services V2 y el acceso de API v3 | Microsoft Docs
description: En este artículo se describen las diferencias de acceso de API entre Azure Media Services V2 y V3.
services: media-services
documentationcenter: na
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: 746f2d61d05b5b721e86aaea9ef56e2ca0621280
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98689578"
---
# <a name="api-access-differences-between-azure-media-services-v2-to-v3-api"></a>Diferencias de acceso de API entre Azure Media Services V2 y la API v3

![logotipo de la guía de migración](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![pasos de migración 2](./media/migration-guide/steps-2.svg)

En este artículo se describen las diferencias de acceso de API entre Azure Media Services V2 y V3.

## <a name="api-access"></a>Acceso de API

Todas las cuentas de Media Services tendrán acceso a la API V3. Sin embargo, recomendamos encarecidamente el desarrollo de la migración en una cuenta nueva antes de aplicar el código actualizado a una cuenta de V2 existente. Esto se debe a que las entidades V3 no son compatibles con V2. Algunas entidades V2 como recursos son compatibles con V3.
Puede seguir usando cuentas existentes si no combina las API V2 y V3 y, después, intenta volver a V2, pero no es recomendable.

El acceso a la API V2 estará disponible hasta que se retire en 2024.

Mientras realiza la migración, puede crear una cuenta de V3 que siga teniendo acceso a V2.  La creación de la cuenta se puede realizar con:

- la API de REST y una versión anterior
- Activación de la casilla en el portal.

> [!div class="mx-imgBorder"]
> [ ![creación de cuentas en el portal](./media/migration-guide/v-3-v-2-access-account-creation-small.png) ](./media/migration-guide/v-3-v-2-access-account-creation.png#lightbox)

.NET, la CLI y otros SDK establecerán como destino la última API 2020-05-01, de modo que busque o configure las versiones de API anteriores.

> [!NOTE]
> Las nuevas cuentas creadas con la API 2020-05-01 no pueden usar las API V2.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
