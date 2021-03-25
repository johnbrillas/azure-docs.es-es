---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 11/22/2020
ms.topic: include
ms.openlocfilehash: 2112cde42ee00b78a82962ee46f53110068977c0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98187097"
---
## <a name="availability"></a>Disponibilidad

|Aspecto|Detalles|
|----|:----|
|Estado de la versión:|Disponible con carácter general|
|Precios:|**Azure Defender para registros de contenedor** se factura como se muestra en la [página de precios](../articles/security-center/security-center-pricing.md).|
|Registros e imágenes compatibles:|Imágenes de Linux en registros de ACR accesibles desde la red pública de Internet con acceso al shell|
|Registros e imágenes no compatibles:|Imágenes de Windows<br>Registros "privados"<br>Registros con acceso limitado con un firewall, un punto de conexión de servicio o puntos de conexión privados, como Azure Private Link.<br>Imágenes excesivamente minimalistas, como las imágenes [base de Docker](https://hub.docker.com/_/scratch/) o imágenes "sin distribución" que solo contienen una aplicación y sus dependencias en tiempo de ejecución sin un administrador de paquetes, shell o sistema operativo.|
|Roles y permisos necesarios:|**Lector de seguridad** y [roles y permisos de Azure Container Registry](../articles/container-registry/container-registry-roles.md)|
|Nubes:|:::image type="icon" source="../articles/security-center/media/icons/yes-icon.png" border="false"::: Nubes comerciales<br>:::image type="icon" source="../articles/security-center/media/icons/yes-icon.png" border="false"::: US Gov y China Gov: actualmente solo se admite el examen en la característica de inserción. Más información en [¿Cuándo se examinan las imágenes?](../articles/security-center/defender-for-container-registries-introduction.md#when-are-images-scanned)|
|||