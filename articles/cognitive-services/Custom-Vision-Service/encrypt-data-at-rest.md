---
title: Cifrado de datos en reposo de Custom Vision
titleSuffix: Azure Cognitive Services
description: Microsoft ofrece claves de cifrado administradas por Microsoft y también le permite administrar las suscripciones de Cognitive Services con sus propias claves, llamadas claves administradas por el cliente (CMK). En este artículo, encontrará información sobre el cifrado de datos en reposo de Custom Vision y cómo habilitar y administrar CMK.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: egeaney
ms.openlocfilehash: 822a4249b6ed054f36605d0367803da68bab090b
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "100652265"
---
# <a name="custom-vision-encryption-of-data-at-rest"></a>Cifrado de datos en reposo de Custom Vision

Custom Vision de Azure cifra automáticamente los datos al guardarlos en la nube. El cifrado de Custom Vision protege los datos y le ayuda a satisfacer los requisitos de cumplimiento normativo y seguridad de la organización.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Las claves que administre el cliente solo serán aquellos recursos disponibles que se hayan creado después del 11 de mayo de 2020. Para usar CMK con Custom Vision, tendrá que crear un nuevo recurso de Custom Vision. Una vez creado el recurso, puede usar Azure Key Vault para configurar la identidad administrada.

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una lista completa de los servicios que admiten CMK, consulte [Claves administradas por el cliente para Cognitive Services](../encryption/cognitive-services-encryption-keys-portal.md).
* [¿Qué es Azure Key Vault?](../../key-vault/general/overview.md)
* [Formulario de solicitud de claves administradas por el cliente de Cognitive Services](https://aka.ms/cogsvc-cmk)