---
title: Creación de suscripciones de Azure mediante programación
description: Este artículo le ayudará a comprender las opciones disponibles para crear suscripciones de Azure mediante programación.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 01/13/2021
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: a51a158700df52ced5288cf2c2f82ebcdf3de54d
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254028"
---
# <a name="create-azure-subscriptions-programatically"></a>Creación de suscripciones de Azure mediante programación

Este artículo le ayudará a comprender las opciones disponibles para crear suscripciones de Azure mediante programación.

Mediante el uso de varias API REST, puede crear una suscripción para los siguientes tipos de contrato de Azure:

- Contrato Enterprise (EA)
- Contrato de cliente de Microsoft (MCA).
- Microsoft Partner Agreement (MPA)

No es posible crear suscripciones mediante programación para otros tipos de contratos con las API REST.

Los requisitos y los detalles para crear suscripciones varían según el tipo de contrato y la versión de API. Consulte los siguientes artículos que hacen referencia a su situación:

API más recientes:

- [Creación de suscripciones de EA](programmatically-create-subscription-enterprise-agreement.md)
- [Creación de suscripciones de MCA](programmatically-create-subscription-microsoft-customer-agreement.md)
- [Creación de suscripciones de MPA](programmatically-create-subscription-microsoft-partner-agreement.md)

Si todavía usa las [API de versión preliminar](programmatically-create-subscription-preview.md), puede seguir creando suscripciones con ellas. 

Además, puede [crear suscripciones con una plantilla de ARM](create-subscription-template.md). La plantilla de ARM permite automatizar el proceso de creación de suscripciones mediante las API REST. 

## <a name="next-steps"></a>Pasos siguientes

* Después de crear una suscripción, puede otorgar esa capacidad a otros usuarios y entidades de servicio. Para más información, vea [Concesión de acceso para crear suscripciones de EA (versión preliminar)](grant-access-to-create-subscription.md).
* Para más información sobre la administración de grandes cantidades de suscripciones mediante grupos de administración, consulte [Organización de los recursos con grupos de administración de Azure](../../governance/management-groups/overview.md).
