---
title: Directivas IPsec de punto a sitio de Virtual WAN
titleSuffix: Azure Virtual WAN
description: Obtenga información sobre las directivas de conectividad IPsec de punto a sitio de Azure Virtual WAN.
services: virtual-wan
author: wellee
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 02/24/2021
ms.author: wellee
ms.openlocfilehash: d64748bf7719af52b76c5e4141bfbbabe80bbae2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743958"
---
# <a name="point-to-site-ipsec-policies"></a>Directivas IPsec de punto a sitio

En este artículo se muestran las combinaciones de directivas IPsec admitidas para la conectividad VPN de punto a sitio en Azure Virtual WAN.

## <a name="default-ipsec-policies"></a>Directivas IPsec predeterminadas

En la tabla siguiente se muestran los parámetros de IPsec predeterminados para las conexiones VPN de punto a sitio. Estos parámetros se eligen automáticamente mediante la puerta de enlace de VPN de punto a sitio de Virtual WAN cuando se crea un perfil de punto a sitio.

| Configuración | Parámetros |
|--- |--- |
| Cifrado IKE de fase 1 | AES256 |
| Integridad IKE de fase 1 |  SHA256 |
| Grupo DH | DHGroup24 |
| Cifrado IPsec de fase 2 | GCMAES256|
| Integridad IPsec de fase 2 | GCMAES25 |
| Grupo PFS |PFS24|

## <a name="custom-ipsec-policies"></a>Directivas IPsec personalizadas

Al trabajar con directivas IPsec personalizadas, tenga en cuenta los siguientes requisitos:

* **IKE**: En el caso de IKE de fase 1, puede seleccionar cualquier parámetro de Cifrado IKE, más cualquier parámetro de Integridad IKE, más cualquier parámetro de Grupo DH.
* **IPsec**: En el caso de IPsec de fase 2, puede seleccionar cualquier parámetro de Cifrado IPsec, más cualquier parámetro de Integridad IPsec, más PFS. Si alguno de los parámetros para el cifrado IPsec o la integridad IPsec es GCM, tanto el cifrado como la integridad IPsec deben usar el mismo algoritmo. Por ejemplo, si se elige GCMAES128 para el cifrado IPsec, se debe elegir también GCMAES128 para la integridad IPsec.  

En la tabla siguiente se muestran los parámetros de IPsec disponibles para las conexiones VPN de punto a sitio.

| Configuración | Parámetros |
|--- |--- |
| Cifrado IKE de fase 1 | AES128, AES256, GCMAES128, GCMAES256 |
| Integridad IKE de fase 1 |  SHA256, SHA384 |
| Grupo DH | DHGroup14, DHGroup24, ECP256, ECP384 |
| Cifrado IPsec de fase 2 | GCMAES128, GCMAES256, SHA256|
| Integridad IPsec de fase 2 | GCMAES128, GCMAES256 |
| Grupo PFS |PFS14, PFS24, ECP256, ECP384|

## <a name="next-steps"></a>Pasos siguientes

Consulte [Creación de una conexión de punto a sitio](virtual-wan-point-to-site-portal.md).

Para más información sobre Virtual WAN, consulte [Preguntas más frecuentes sobre Virtual WAN](virtual-wan-faq.md).
