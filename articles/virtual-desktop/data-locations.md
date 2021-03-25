---
title: 'Ubicaciones de datos para Windows Virtual Desktop: Azure'
description: Una breve introducción a las ubicaciones en las que se almacenan los datos y metadatos de Windows Virtual Desktop.
author: Heidilohr
ms.topic: conceptual
ms.custom: references_regions
ms.date: 02/17/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: 12ec71a86a5df5954c14097e6a0ec5c8a5138fc5
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "100652435"
---
# <a name="data-and-metadata-locations-for-windows-virtual-desktop"></a>Ubicaciones de datos y metadatos para Windows Virtual Desktop

>[!IMPORTANT]
>Este contenido se aplica a Windows Virtual Desktop con objetos de Windows Virtual Desktop para Azure Resource Manager. Si usa Windows Virtual Desktop (clásico) sin objetos para Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/data-locations-2019.md).

Windows Virtual Desktop está disponible actualmente para todas las ubicaciones geográficas. Los administradores pueden elegir la ubicación para almacenar los datos de usuario cuando creen las máquinas virtuales del grupo de hosts y los servicios asociados, como los servidores de archivos. Obtenga más información sobre las ubicaciones geográficas de Azure en el [mapa de centros de datos de Azure](https://azuredatacentermap.azurewebsites.net/).

>[!NOTE]
>Microsoft no controla ni limita las regiones en las que usted o sus usuarios pueden acceder a los datos específicos de la aplicación o del usuario.

>[!IMPORTANT]
>Windows Virtual Desktop almacena información de metadatos globales como nombres de inquilinos, nombres de grupos de hosts, nombres de grupos de aplicaciones y nombres principales de usuario en un centro de datos. Cada vez que un cliente crea un objeto de servicio, debe especificar una ubicación para él. La ubicación que especifique determina el lugar en que se almacenarán los metadatos del objeto. El cliente elegirá una región de Azure y los metadatos se almacenarán en la zona geográfica relacionada. Para obtener una lista de todas las regiones de Azure y las zonas geográficas relacionadas, consulte [Zonas geográficas de Azure](https://azure.microsoft.com/global-infrastructure/geographies/).

Actualmente se admite el almacenamiento de metadatos en las siguientes zonas geográficas:

- Estados Unidos (EE. UU.) (disponible con carácter general)
- Europa (EU) (versión preliminar pública) 

>[!NOTE]
> Al seleccionar una región en la que crear objetos de servicio de Windows Virtual Desktop, verá regiones en las zonas geográficas de EE. UU. y de la UE. Para asegurarse de que sabe qué región sería mejor para su implementación, eche un vistazo a [nuestro mapa de la infraestructura global de Azure](https://azure.microsoft.com/global-infrastructure/geographies/#geographies).

Los metadatos almacenados se cifran en reposo y los reflejos con redundancia geográfica se mantienen en la zona geográfica. Todos los datos del cliente, como la configuración de la aplicación y los datos de usuario, residen en la ubicación que el cliente elige y no los administra el servicio. A medida que crezca el servicio, habrá más zonas geográficas disponibles.

Los metadatos del servicio se replican dentro de la zona geográfica con fines de recuperación ante desastres.