---
title: Respuesta a las alertas de Azure Defender para DNS
description: Obtenga información sobre los pasos necesarios para responder a las alertas de Azure Defender para DNS
author: memildin
ms.author: memildin
ms.date: 12/07/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 375a81127a000741ca5e0397642800794610bcda
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96754552"
---
# <a name="respond-to-azure-defender-for-dns-alerts"></a>Respuesta a las alertas de Azure Defender para DNS

Cuando reciba una alerta de Azure Defender para DNS, se recomienda investigar y responder a la alerta, tal y como se describe a continuación. Azure Defender para DNS protege todos los recursos conectados, por lo que, incluso si está familiarizado con la aplicación o el usuario que desencadenó la alerta, es importante comprobar la situación relativa a cada alerta.  


## <a name="step-1-contact"></a>Paso 1. Contacto

1. Póngase en contacto con el propietario del recurso para determinar si se esperaba el comportamiento o era intencionado.
1. Si la actividad se espera, descarte la alerta.
1. Si la actividad es inesperada, trate el recurso como si potencialmente corriera peligro y mitíguelo como se describe en el paso siguiente.

## <a name="step-2-immediate-mitigation"></a>Paso 2. Mitigación inmediata 

1. Aísle el recurso de la red para evitar el movimiento lateral.
1. Ejecute un examen de antimalware completo en el recurso, siguiendo cualquier consejo de corrección resultante.
1. Revise el software instalado y en ejecución en el recurso y quite los paquetes que no conozca o que no desee.
1. Revierta la máquina a un estado bueno conocido, vuelva a instalar el sistema operativo si fuera necesario y restaure el software de un origen que esté comprobado que no tiene malware.
1. Resuelva las recomendaciones para la máquina de Azure Security Center y corrija los problemas de seguridad resaltados para evitar infracciones futuras.


## <a name="next-steps"></a>Pasos siguientes

En esta página se explica el proceso de respuesta a una alerta de Azure Defender para DNS. Para obtener información relacionada, consulte las páginas siguientes:

- [Introducción a Azure Defender para DNS](defender-for-dns-introduction.md)
- [Eliminación de alertas de Azure Defender](alerts-suppression-rules.md)
- [Exportación continua de alertas y recomendaciones de seguridad](continuous-export.md)