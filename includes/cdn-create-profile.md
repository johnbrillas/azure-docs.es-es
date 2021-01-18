---
title: archivo de inclusión
description: archivo de inclusión
services: cdn
author: SyntaxC4
ms.service: azure-cdn
ms.topic: include
ms.date: 04/30/2020
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: c6352ee9d29e4e45aa4be449046a0715fee06047
ms.sourcegitcommit: 16887168729120399e6ffb6f53a92fde17889451
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98165916"
---
## <a name="create-a-new-cdn-profile"></a>Crear un nuevo perfil de CDN

Un perfil de CDN es un contenedor para los puntos de conexión de CDN y especifica un plan de tarifa.

1. En Azure Portal, seleccione **Crear un recurso** (en la parte superior izquierda). Aparece el panel **Nuevos**.
   
1. Busque **CDN**, selecciónelo y, después, seleccione **Crear**:
   
    ![Seleccionar recurso de CDN](./media/cdn-create-profile/cdn-new-resource.png)

    Aparece el panel **Perfil de CDN**.

1. Escriba los siguientes valores:
   
    | Configuración  | Value |
    | -------- | ----- |
    | **Nombre** | Escriba *cdn-profile-123* como nombre de perfil. Este nombre debe ser único globalmente; si ya está en uso, escriba otro. |
    | **Suscripción** | Seleccione una suscripción de Azure en la lista desplegable. |
    | **Grupos de recursos** | Seleccione **Crear nuevo** y escriba *CDNQuickstart-rg* como nombre del grupo de recursos, o bien seleccione **Usar existente** y elija *CDNQuickstart-rg* si ya tiene el grupo. | 
    | **Ubicación del grupo de recursos** | Seleccione una ubicación cercana en la lista desplegable. |
    | **Plan de tarifa** | Seleccione la opción **Akamai estándar** en la lista desplegable. (el tiempo de implementación del nivel Akamai es aproximadamente un minuto. El nivel Microsoft tarda aproximadamente 10 minutos y los niveles de Verizon tardan unos 30 minutos). |
    | **Crear un punto de conexión de CDN ahora** | Déjelo sin seleccionar. |  
   
    ![Nuevo perfil de CDN](./media/cdn-create-profile/cdn-new-profile.png)

1. Seleccione **Crear** para crear el perfil.

