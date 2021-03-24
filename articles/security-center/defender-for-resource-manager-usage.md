---
title: Respuesta a las alertas de Azure Defender para Resource Manager
description: Obtenga información sobre los pasos necesarios para responder a las alertas de Azure Defender para Resource Manager
author: memildin
ms.author: memildin
ms.date: 12/07/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 54790795aab8aac247e17198159130d7139dd38c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96754547"
---
# <a name="respond-to-azure-defender-for-resource-manager-alerts"></a>Respuesta a las alertas de Azure Defender para Resource Manager

Cuando reciba una alerta de Azure Defender para Resource Manager, se recomienda investigar y responder a la alerta, tal y como se describe a continuación. Azure Defender para Resource Manager protege todos los recursos conectados, por lo que, incluso si está familiarizado con la aplicación o el usuario que desencadenó la alerta, es importante comprobar la situación relativa a cada alerta.  


## <a name="step-1-contact"></a>Paso 1. Contacto

1. Póngase en contacto con el propietario del recurso para determinar si se esperaba el comportamiento o era intencionado.
1. Si se espera la actividad, descarte la alerta.
1. Si no se espera la actividad, trate las cuentas de usuario, suscripciones y máquinas virtuales relacionadas como elementos en peligro y mitíguelo tal como se describe en el siguiente paso.

## <a name="step-2-immediate-mitigation"></a>Paso 2. Mitigación inmediata 

1. Corrija las cuentas de usuario que están en peligro:
    - Si no está familiarizado con ellas, elimínelas, ya que es posible que las haya creado un actor de amenaza
    - Si está familiarizado, cambie sus credenciales de autenticación
    - Use registros de actividad de Azure para revisar todas las actividades realizadas por el usuario e identifique las que sean sospechosas

1. Corrija las suscripciones que están en peligro:
    - Quite los runbooks con los que no esté familiarizado de la cuenta de Automation en peligro
    - Revise los permisos de IAM de la suscripción y quite los permisos para las cuentas de usuario que resulten familiares
    - Revise todos los recursos de Azure en la suscripción y elimine los que no estén familiarizados
    - Revise e investigue las alertas de seguridad de la suscripción en Azure Security Center
    - Use registros de actividad de Azure para revisar todas las actividades realizadas en la suscripción e identifique las que sean sospechosas

1. Corrija las máquinas virtuales que están en peligro
    - Cambie las contraseñas de todos los usuarios
    - Ejecute un examen antimalware en la máquina
    - Restablezca la imagen inicial de las máquinas desde un origen sin malware


## <a name="next-steps"></a>Pasos siguientes

En esta página se explica el proceso de respuesta a una alerta de Azure Defender para Resource Manager. Para obtener información relacionada, consulte las páginas siguientes:

- [Introducción a Azure Defender para Resource Manager](defender-for-resource-manager-introduction.md)
- [Eliminación de alertas de Azure Defender](alerts-suppression-rules.md)
- [Exportación continua de alertas y recomendaciones de seguridad](continuous-export.md)