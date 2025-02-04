---
title: 'Preguntas más frecuentes sobre Azure Security Center: preguntas generales'
description: Preguntas más frecuentes generales sobre Azure Security Center, un producto que ayuda a evitar, detectar y responder a amenazas.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 3a8429d9dc6820b1f79c49d325872b61833f988d
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102095551"
---
# <a name="faq---general-questions-about-azure-security-center"></a>Preguntas más frecuentes: preguntas generales sobre Azure Security Center

## <a name="what-is-azure-security-center"></a>¿Qué es el Centro de seguridad de Azure?
Azure Security Center ayuda a evitar amenazas y a detectarlas y responder a ellas con más visibilidad y control sobre la seguridad de sus recursos. Proporciona administración de directivas y supervisión de la seguridad integrada en las suscripciones, ayuda a detectar las amenazas que podrían pasar desapercibidas y funciona con un amplio ecosistema de soluciones de seguridad.

Security Center usa el agente de Log Analytics para recopilar y almacenar datos. Para obtener información detallada, consulte [Recolección de datos en Azure Security Center](security-center-enable-data-collection.md).


## <a name="how-do-i-get-azure-security-center"></a>¿Cómo puedo obtener el Centro de seguridad de Azure?
Azure Security Center se habilita con la suscripción de Microsoft Azure y se accede a él desde el [Portal de Azure](https://azure.microsoft.com/features/azure-portal/). Para acceder, [inicie sesión en el portal](https://portal.azure.com), seleccione **Examinar** y desplácese al **Security Center**.


## <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>¿Qué recursos de Azure supervisa Azure Security Center?
Azure Security Center supervisa los siguientes recursos de Azure:

* Máquinas virtuales (se incluyen [Cloud Services](../cloud-services/cloud-services-choose-me.md))
* Conjuntos de escalado de máquinas virtuales
* Soluciones de asociados integradas en su suscripción de Azure, como un firewall de aplicaciones web en las máquinas virtuales y en App Service Environment
* [Los muchos servicios de PaaS de Azure que se enumeran en la información general del producto](features-paas.md)


## <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>¿Cómo puedo ver el estado de seguridad actual de los recursos de Azure?
La página **Introducción a Security Center** muestra la posición de seguridad general del entorno con la información desglosada por Computación, Redes, Almacenamiento y datos, y Aplicaciones. Cada tipo de recurso tiene un indicador que muestra las vulnerabilidades de seguridad identificadas. Al hacer clic en cada icono, se muestra una lista de problemas de seguridad que ha identificado Security Center, junto con un inventario de los recursos de la suscripción.



## <a name="what-is-a-security-initiative"></a>¿Qué es una iniciativa de seguridad?
Una iniciativa de seguridad define el conjunto de controles (directivas) recomendados para los recursos de una suscripción específica. En Azure Security Center, debe asignar iniciativas para las suscripciones de Azure de acuerdo con los requisitos de seguridad de la compañía y el tipo de aplicaciones o la confidencialidad de los datos de cada suscripción.

Las directivas de seguridad habilitadas en Azure Security Center generan la supervisión y recomendaciones de seguridad. Obtenga más información en [¿Qué son las directivas de seguridad, las iniciativas y las recomendaciones?](security-policy-concept.md)


## <a name="who-can-modify-a-security-policy"></a>¿Quién puede modificar una directiva de seguridad?
Para modificar una directiva de seguridad, debe ser **Administrador de seguridad** o **Propietario** de esa suscripción.

Para obtener información sobre cómo configurar una directiva de seguridad, consulte [Establecimiento de directivas de seguridad en el Centro de seguridad de Azure](tutorial-security-policy.md).


## <a name="what-is-a-security-recommendation"></a>¿Qué es una recomendación de seguridad?
El Centro de seguridad de Azure analiza el estado de seguridad de los recursos de Azure. Las recomendaciones se crean una vez que se identifican las posibles vulnerabilidades de seguridad. Las recomendaciones le guían en el proceso de configurar el control necesario. Algunos ejemplos son:

* Aprovisionamiento de antimalware para ayudar a identificar y eliminar software malintencionado.
* [Grupos de seguridad de red](../virtual-network/network-security-groups-overview.md) y reglas para controlar el tráfico a las máquinas virtuales.
* Aprovisionamiento de un firewall de aplicaciones web para ayudar a defenderse contra ataques dirigidos a las aplicaciones web.
* Implementación de actualizaciones del sistema que faltan.
* Resolución de las configuraciones de sistema operativo que no coinciden con las líneas base recomendadas.

Aquí solo se muestran las recomendaciones habilitadas en las directivas de seguridad.


## <a name="what-triggers-a-security-alert"></a>¿Qué desencadena una alerta de seguridad?
Azure Security Center recopila, analiza y combina automáticamente los datos de registro de los recursos de Azure, la red y soluciones de asociados como firewalls y antimalware. Cuando se detecten amenazas, se creará una alerta de seguridad. Como ejemplos se incluye la detección de:

* Máquinas virtuales en peligro que se comunican con direcciones IP malintencionadas conocidas.
* Malware avanzado detectado mediante la generación de informes de errores de Windows.
* Ataques por fuerza bruta contra máquinas virtuales.
* Alertas de seguridad de soluciones de seguridad integradas de socio, como antimalware o Firewall de aplicaciones web.


## <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>¿Qué diferencia hay entre las amenazas detectadas y advertidas por el Centro de respuestas de seguridad de Microsoft y las comunicadas por Azure Security Center?
El Centro de respuestas de seguridad de Microsoft (MSRC) lleva a cabo una selecta supervisión de seguridad de la red e infraestructura de Azure y recibe información sobre amenazas y quejas sobre abusos de terceros. Cuando MSRC se da cuenta de que un usuario ilegítimo o no autorizado ha tenido acceso a los datos del cliente o de que el uso de Azure por parte del cliente no cumple las condiciones de uso aceptable, un administrador de incidentes de seguridad notifica al cliente. La notificación suele consistir en el envío de un correo electrónico a los contactos de seguridad especificados en Azure Security Center o al propietario de la suscripción de Azure en caso de no especificarse un contacto de seguridad.

Azure Security Center es un servicio de Azure que no deja de supervisar el entorno de Azure del cliente y aplica análisis para detectar de forma automática una amplia variedad de actividades potencialmente malintencionadas. Estas detecciones aparecen como alertas de seguridad en el panel de Azure Security Center.