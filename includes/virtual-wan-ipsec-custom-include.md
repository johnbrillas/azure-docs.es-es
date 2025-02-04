---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/07/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 83f0ce27172879a37de9488499e46de30b8e112c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98147663"
---
Al trabajar con directivas IPsec personalizadas, tenga en cuenta los siguientes requisitos:

* **IKE**: en el caso de IKE, puede seleccionar cualquier parámetro de Cifrado de IKE, más cualquier parámetro de Integridad de IKE, más cualquier parámetro de Grupo DH.
* **IPsec**: en el caso de IPsec, puede seleccionar cualquier parámetro de Cifrado de IPsec, más cualquier parámetro de Integridad de IPsec, más PFS. Si cualquiera de parámetros de Cifrado de IPsec o Integridad de IPsec es GCM, los parámetros de ambos valores deben ser GCM.

>[!NOTE]
> Con las directivas IPsec personalizadas, no existe el concepto de respondedor y iniciador (a diferencia de las directivas IPsec predeterminadas). Ambos lados (entorno local y puerta de enlace de VPN de Azure) usarán la misma configuración para IKE fase 1 e IKE fase 2. Se admiten los protocolos IKEv1 e IKEv2. No se admite Azure solo como respondedor.
>

**Parámetros y valores disponibles**

| Configuración | Parámetros |
|--- |--- |
| Cifrado de IKE | GCMAES256, GCMAES128, AES256, AES128 |
| Integridad de IKE | SHA384, SHA256 |
| Grupo DH | ECP384, ECP256, DHGroup24, DHGroup14 |
| Cifrado IPsec | GCMAES256, GCMAES128, AES256, AES128, None |
| Integridad de IPsec | GCMAES256, GCMAES128, SHA256 |
| Grupo PFS | ECP384, ECP256, PFS24, PFS14, None |
| Vigencia de SA |entero, mín. 300/predeterminado 27000 segundos |
