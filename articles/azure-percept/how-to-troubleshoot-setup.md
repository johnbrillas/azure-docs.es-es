---
title: Solución de problemas durante la experiencia de incorporación de Azure Percept DK
description: Vea sugerencias para la solución de algunos de los problemas más comunes que pueden surgir durante la experiencia de incorporación.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: d71cfa6ba52052e4b68175be84934c8b4294ed25
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "101660533"
---
# <a name="azure-percept-dk-onboarding-experience-troubleshooting-guide"></a>Guía de solución de problemas de la experiencia de incorporación de Azure Percept DK

Estos son algunos de los problemas que pueden surgir durante la incorporación de Azure Percept DK. Si después de seguir los pasos de esta guía el problema aún persiste, póngase en contacto con el servicio de asistencia al cliente de Azure.

|Problema|Motivo|Solución alternativa|
|:-----|:------|:----------|
|Al conectarse a las páginas de registro de la cuenta de Azure o a Azure Portal, puede iniciar sesión automáticamente con una cuenta almacenada en caché. Si no es la cuenta que desea utilizar, puede dar lugar a una experiencia incoherente con la documentación.|Esto suele deberse a un valor en el explorador para "recordar" una cuenta que ha utilizado anteriormente.|En la página de Azure, haga clic en la esquina superior derecha de la página donde se muestra el nombre de la cuenta y, a continuación, en "cerrar sesión". Después podrá iniciar sesión con la cuenta correcta.|
|La red del punto de acceso de Azure Percept DK (scz-xxxx) no aparece en la lista de redes Wi-Fi disponibles.|Normalmente, se trata de un problema temporal que se resuelve automáticamente en poco tiempo.|Espere a que aparezca la red. Si no aparece después de más de 15 minutos, reinicie el dispositivo.|
|La conexión con el punto de acceso de Azure Percept DK se desconecta con frecuencia.|Esto puede deberse a una mala conexión entre el dispositivo y el equipo host. También puede deberse a interferencias de otras conexiones Wi-Fi en el equipo host.|Asegúrese de que las antenas estén conectadas correctamente al kit de desarrollo. Si el kit de desarrollo está alejado del equipo host, intente acercarlo. Desactive cualquier otra conexión a Internet, como LTE/5G, si se están ejecutando en el equipo host.|
|El equipo host muestra una advertencia de seguridad sobre la conexión con el punto de acceso de Azure Percept DK.|Se trata de un problema conocido que se corregirá en una actualización posterior.|Es seguro continuar con la experiencia de incorporación a través del punto de acceso Wi-Fi del kit de desarrollo.|
|La red del punto de acceso de Azure Percept DK (scz-xxxx) aparece en la lista de redes, pero no se puede conectar.|Esto puede deberse a un daño temporal del punto de acceso Wi-Fi del kit de desarrollo.|Reinicie el kit de desarrollo e inténtelo de nuevo.|
|No es posible conectarse a una red Wi-Fi durante la experiencia de instalación.|La red Wi-Fi debe tener conectividad a Internet para que podamos comunicarnos con Azure. Actualmente no están admitidos el protocolo EAP [PEAP/MSCHAP], los portales cautivos ni la conectividad Enterprise EAP-TLS.|Asegúrese de que el tipo de red Wi-Fi al que se está conectando sea compatible y tenga conectividad a Internet.|