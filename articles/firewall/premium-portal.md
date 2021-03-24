---
title: Versión preliminar de Azure Firewall Prémium en Azure Portal
description: Aprenda sobre la versión preliminar de Azure Firewall Prémium en Azure Portal.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 02/16/2021
ms.author: victorh
ms.openlocfilehash: 3d56fc73faeb0d48ba7e5b21449c61d6213afa40
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100549485"
---
# <a name="azure-firewall-premium-preview-in-the-azure-portal"></a>Versión preliminar de Azure Firewall Prémium en Azure Portal

> [!IMPORTANT]
> Azure Firewall Prémium se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

 La versión preliminar de Azure Firewall Prémium es un firewall de próxima generación con funcionalidades que son necesarias en entornos de alta confidencialidad y regulados. Contiene las características siguientes:

- **Inspección de TLS**: descifra el tráfico saliente, procesa los datos y, luego, los cifra y los envía al destino.
- **IDPS**: sistema de detección y prevención de intrusiones en la red (IDPS) que permite supervisar la actividad malintencionada de la red, registrar información sobre esta actividad, notificarla y, opcionalmente, intentar bloquearla.
- **Filtrado de direcciones URL**: extiende la funcionalidad de filtrado de nombres de dominio completos de Azure Firewall para tener en cuenta una dirección URL completa. Por ejemplo, `www.contoso.com/a/c` en lugar de `www.contoso.com`.
- **Categorías web**: los administradores pueden permitir o denegar el acceso de los usuarios a categorías de sitios web como, por ejemplo, sitios web de apuestas, de redes sociales u otros.

Para más información, consulte [Características de Azure Firewall Prémium](premium-features.md).

## <a name="deploy-the-firewall"></a>Implementación del firewall

La implementación de una versión preliminar de Azure Firewall Prémium es similar a la implementación de la versión estándar de Azure Firewall:

:::image type="content" source="media/premium-portal/premium-portal.png" alt-text="Implementación en el portal":::

En **Firewall tier** (Nivel de firewall), seleccione **Premium (preview)** (Prémium [versión preliminar]) y, en **Directiva de firewall**, seleccione una directiva prémium existente o cree una.

## <a name="configure-the-premium-policy"></a>Configuración de la directiva prémium

La configuración de una directiva de firewall prémium es similar a la configuración de una directiva de firewall estándar. Con una directiva prémium, puede configurar las características prémium:

:::image type="content" source="media/premium-portal/premium-policy.png" alt-text="Implementación de directivas prémium":::

### <a name="rule-configuration"></a>Configuración de reglas

Al configurar reglas de aplicación en una directiva prémium, puede configurar características prémium adicionales:

:::image type="content" source="media/premium-portal/premium-application-rule.png" alt-text="Regla prémium":::

## <a name="next-steps"></a>Pasos siguientes

Para ver en acción las características en versión preliminar de Azure Firewall Prémium, consulte [Implementación y configuración de la versión preliminar de Azure Firewall Prémium](premium-deploy.md).