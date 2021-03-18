---
title: Entidades de certificación permitidas para Azure Front Door Standard/Premium (versión preliminar)
description: En este artículo se enumeran todas las entidades de certificación que se permiten al crear su propio certificado.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: troubleshooting
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: e31ad0734630fbd0b4960c26f8d562cea66ae675
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102434873"
---
# <a name="allowed-certificate-authorities-for-azure-front-door-standardpremium-preview"></a>Entidades de certificación permitidas para Azure Front Door Standard/Premium (versión preliminar)

Cuando habilite la característica HTTPS con su propio certificado para un dominio personalizado de Azure Front Door Standard/Premium. Necesita una entidad de certificación (CA) permitida para crear el certificado TLS/SSL. De lo contrario, si usa una entidad de certificación no permitida o un certificado autofirmado, la solicitud se rechazará.

[!INCLUDE [cdn-front-door-allowed-ca](../../../includes/cdn-front-door-allowed-ca.md)]
