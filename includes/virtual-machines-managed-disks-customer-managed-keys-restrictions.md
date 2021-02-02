---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/24/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 75acbb30c2bf811b7ae72d6939b9f164554fdd32
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98859532"
---
- Solo se admiten [software y claves RSA de HSM](../articles/key-vault/keys/about-keys.md) con un tamaño de 2048 bits, 3072 bits y 4096 bits, ninguna otra clave o tamaño.
    - Las claves de [HSM](../articles/key-vault/keys/hsm-protected-keys.md) requieren el nivel **premium** de los almacenes Azure Key Vault.
- Los discos creados a partir de imágenes personalizadas que están cifradas con cifrado del lado servidor y las claves administradas por el cliente deben cifrarse con las mismas claves administradas por el cliente y deben estar en la misma suscripción.
- Las instantáneas creadas a partir de discos que están cifrados con cifrado del lado servidor y claves administradas por el cliente deben cifrarse con las mismas claves administradas por el cliente.
- Todos los recursos relacionados con las claves administradas por el cliente (instancias de Azure Key Vault, conjuntos de cifrado de disco, máquinas virtuales, discos e instantáneas) deben estar en la misma suscripción y región.
- Los discos, las instantáneas y las imágenes cifrados con claves administradas por el cliente no se pueden trasladar a otro un grupo de recursos o una suscripción diferentes.
- Los discos administrados cifrados actual o anteriormente mediante Azure Disk Encryption no se pueden cifrar mediante claves administradas por el cliente.
- Solo puede crear hasta 1000 conjuntos de cifrado de disco por región y por suscripción.
- Para obtener información sobre el uso de claves administradas por el cliente con galerías de imágenes compartidas, consulte [Versión preliminar: uso de claves administradas por el cliente para el cifrado de imágenes](../articles/virtual-machines/image-version-encryption.md).
