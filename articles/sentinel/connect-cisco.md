---
title: Conexión de datos de Cisco con Azure Sentinel | Microsoft Docs
description: Aprenda a conectar el dispositivo Cisco ASA a Azure Sentinel para ver los paneles, crear alertas personalizadas y mejorar la investigación.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 62029b5c-29d3-4336-8a22-a9db8214eb7e
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: e8a64dd3e47384ba2bf7579f8052177252634622
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "85566031"
---
# <a name="connect-cisco-asa-to-azure-sentinel"></a>Conexión de Cisco ASA a Azure Sentinel



En este artículo se explica cómo conectar el dispositivo de Cisco ASA a Azure Sentinel. El conector de datos de Cisco ASA permite conectar fácilmente los registros de Cisco ASA con Azure Sentinel para ver paneles, crear alertas personalizadas y mejorar la investigación. El uso de Cisco ASA en Azure Sentinel le proporcionará más información sobre el uso de Internet de su organización y mejorará sus capacidades de operación de seguridad. 



## <a name="forward-cisco-asa-logs-to-the-syslog-agent"></a>Reenviar los registros de Cisco ASA al agente de Syslog

Cisco ASA no es compatible con CEF, por lo que los registros se envían como Syslog y el agente de Azure Sentinel sabe cómo analizarlos como si fueran registros CEF. Configure Cisco ASA para reenviar mensajes de Syslog a su área de trabajo de Azure a través del agente de Syslog:

1. Vaya a [Send Syslog messages to an external Syslog server](https://aka.ms/asi-syslog-cisco-forwarding) (Enviar mensajes de Syslog a un servidor de Syslog externo) y siga las instrucciones para configurar la conexión. Use estos parámetros cuando se le solicite:
    - Establezca **port** en 514 o el puerto que estableció en el agente.
    - Establezca **syslog_ip** en la dirección IP del agente.

1. Para usar el esquema correspondiente en Log Analytics para los eventos de Cisco, busque `CommonSecurityLog`.

1. Diríjase al [PASO 3: Validación de conectividad](connect-cef-verify.md).




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos de Cisco ASA a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.


