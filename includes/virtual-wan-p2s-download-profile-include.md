---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 02/05/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: eea8cb61ce98b2394abff6995e5cc89f00a7cf46
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2021
ms.locfileid: "99628942"
---
1. En la página de su red WAN virtual, seleccione **Configuraciones de VPN de usuario**.
1. En la página **Configuraciones de VPN de usuario**, seleccione una configuración y, luego, elija **Descargar perfil de VPN de usuario de WAN virtual**. Al descargar la configuración del nivel de WAN, se obtiene un perfil de VPN de usuario basado en Traffic Manager. Para más información sobre los perfiles globales o sobre cualquier perfil basado en un centro de conectividad, consulte [Perfiles de centro de conectividad](../articles/virtual-wan/global-hub-profile.md). Los escenarios de conmutación por error se simplifican con los perfiles globales.

   
   Si, por alguna razón, alguno de los centros de conectividad no está disponible, la administración de tráfico integrada que proporciona el servicio garantiza la conectividad (a través de otro centro) a los recursos de Azure para los usuarios de punto a sitio. Siempre puede descargar una configuración de VPN específica del centro de conectividad. Para ello, vaya al centro de conectividad. En **VPN de usuario (punto a sitio)** , descargue el perfil de **VPN de usuario** del centro de conectividad virtual.
1. En la página **Descargar perfil de VPN de usuario de WAN virtual**, seleccione el **tipo de autenticación** y, luego, elija **Generar y descargar perfil**. Se generará el paquete de perfil y se descargará un archivo ZIP que contiene los valores de configuración.
