---
title: Validación de alertas en Azure Security Center | Microsoft Docs
description: Obtenga información acerca de cómo validar que las alertas de seguridad están configuradas correctamente en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/17/2021
ms.author: memildin
ms.openlocfilehash: 74323c63ab8985dee2391a546d82258dcb8d0114
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102099529"
---
# <a name="alert-validation-in-azure-security-center"></a>Validación de alertas en Azure Security Center
Este documento le ayuda a comprobar si el sistema está configurado correctamente para las alertas de Azure Security Center.

## <a name="what-are-security-alerts"></a>¿Qué son las alertas de seguridad?
Las alertas son notificaciones que Security Center genera cuando detecta amenazas en los recursos. Asigna prioridades y enumera las alertas, junto con la información necesaria para que pueda investigar rápidamente el problema. Security Center también proporciona recomendaciones sobre el modo en que puede corregir un ataque.
Para obtener más información, consulte [Alertas de seguridad en Azure Security Center](security-center-alerts-overview.md) y [Administración y respuesta a las alertas de seguridad](security-center-managing-and-responding-alerts.md).


## <a name="generate-sample-azure-defender-alerts"></a>Generación de alertas de ejemplo de Azure Defender

Si usa la nueva experiencia de alertas de versión preliminar, tal como se describe en [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md), puede crear alertas de ejemplo con unos cuantos clics en la página de alertas de seguridad de Azure Portal.

Use alertas de ejemplo para:

- evaluar el valor y las capacidades de Azure Defender.
- validar las configuraciones que haya realizado para las alertas de seguridad (como integraciones de SIEM, automatización de flujos de trabajo y notificaciones de correo electrónico).

Para crear alertas de ejemplo:

1. En la barra de herramientas de la página de alertas, seleccione **Create sample alerts** (Crear alertas de ejemplo). 
1. Seleccione la suscripción.
1. Seleccione el plan de Azure Defender correspondiente para el que quiere ver las alertas. 
1. Seleccione **Create sample alerts** (Crear alertas de ejemplo).

    :::image type="content" source="media/security-center-alert-validation/create-sample-alerts-procedures.png" alt-text="Pasos para crear alertas de ejemplo en Azure Security Center":::
    
    Aparecerá una notificación en la que se le informa de que se están creando las alertas de ejemplo:

    :::image type="content" source="media/security-center-alert-validation/notification-sample-alerts-creation.png" alt-text="Notificación que indica que se están generando las alertas de ejemplo.":::

    Después de unos minutos, las alertas aparecerán en la página alertas de seguridad. También aparecerán en cualquier otro lugar que haya configurado para recibir las alertas de seguridad de Azure Security Center (SIEM conectados, notificaciones de correo electrónico, etc.).

    :::image type="content" source="media/security-center-alert-validation/sample-alerts.png" alt-text="Alertas de ejemplo en la lista de alertas de seguridad":::

    > [!TIP]
    > Las alertas son para los recursos simulados.

## <a name="simulate-alerts-on-your-azure-vms-windows"></a>Simulación de alertas en las VM de Azure (Windows) <a name="validate-windows"></a>

Una vez que el agente de Security Center esté instalado en el equipo, siga estos pasos desde el equipo donde desee que esté el recurso atacado de la alerta:

1. Copie un archivo ejecutable (por ejemplo **calc.exe**) en el escritorio del equipo o en otro directorio que prefiera y cambie el nombre a **ASC_AlertTest_662jfi039N.exe**.
1. Abra el símbolo del sistema y ejecute este archivo con un argumento (un nombre de argumento falso), por ejemplo, ```ASC_AlertTest_662jfi039N.exe -foo```
1. Espere de cinco a diez minutos y abra Alertas de Security Center. Debería aparecer una alerta.

> [!NOTE]
> Al revisar esta alerta de prueba para Windows, asegúrese de que el campo **Arguments Auditing Enabled** (Auditoría de argumentos habilitada) aparece como **true**. Si es **false**, debe habilitar la auditoría de argumentos de línea de comandos. Para habilitarlo, utilice la línea de comandos siguiente:
>
>```reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"```

## <a name="simulate-alerts-on-your-azure-vms-linux"></a>Simulación de alertas en las VM de Azure (Linux) <a name="validate-linux"></a>

Una vez que el agente de Security Center esté instalado en el equipo, siga estos pasos desde el equipo donde desee que esté el recurso atacado de la alerta:
1. Copie un archivo ejecutable en una ubicación adecuada y cambie el nombre a **./asc_alerttest_662jfi039n**, por ejemplo:

    ```cp /bin/echo ./asc_alerttest_662jfi039n```

1. Abra el símbolo del sistema y ejecute este archivo:

    ```./asc_alerttest_662jfi039n testing eicar pipe```

1. Espere de cinco a diez minutos y abra Alertas de Security Center. Debería aparecer una alerta.


## <a name="simulate-alerts-on-kubernetes"></a>Simulación de alertas en Kubernetes <a name="validate-kubernetes"></a>

Si ha integrado Azure Kubernetes Service con Security Center, puede probar que sus alertas funcionan con el siguiente comando kubectl:

```kubectl get pods --namespace=asc-alerttest-662jfi039n```

Para obtener más información sobre cómo defender los nodos y clústeres de Kubernetes, consulte [Introducción a Azure Defender para Kubernetes](defender-for-kubernetes-introduction.md).

## <a name="next-steps"></a>Pasos siguientes
En este artículo se explicó el proceso de validación de las alertas. Ahora que conoce esta validación, intente los siguientes pasos:

* [Validación de la detección de amenazas de Azure Key Vault en Azure Security Center](https://techcommunity.microsoft.com/t5/azure-security-center/validating-azure-key-vault-threat-detection-in-azure-security/ba-p/1220336)
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): obtenga información sobre cómo administrar y responder a los incidentes de seguridad en Security Center.
* [Comprender las alertas de seguridad en Azure Security Center](./security-center-alerts-overview.md): obtenga información sobre los distintos tipos de alertas de seguridad.