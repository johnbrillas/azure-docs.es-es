---
title: Conexión de productos de Forcepoint a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar productos de Forcepoint a Azure Sentinel.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: eb099a786a84f9b7d0a6f0dc6e6df9c3459af295
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "77588236"
---
# <a name="connect-your-forcepoint-products-to-azure-sentinel"></a>Conecte productos de Forcepoint a Azure Sentinel

> [!IMPORTANT]
> El conector de datos de productos de Forcepoint en Azure Sentinel se encuentra actualmente en versión preliminar pública. Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


En este artículo se explica cómo conectar los productos de Forcepoint a Azure Sentinel. 

Los conectores de datos de Forcepoint permiten conectar Forcepoint Cloud Access Security Broker y los registros de Forcepoint Next Generation Firewall con Azure Sentinel. De esta forma es posible exportar registros definidos por el usuario a Azure Sentinel en tiempo real. El conector proporciona mayor visibilidad de las actividades de los usuarios grabadas por productos de Forcepoint. También permite una mayor correlación de los datos de las cargas de trabajo de Azure y otras fuentes, y mejora la funcionalidad de supervisión con los libros dentro de Azure Sentinel.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.



## <a name="forward-forcepoint-product-logs-to-the-syslog-agent"></a>Reenvío de registros de productos de Forcepoint al agente de Syslog 

Configure el producto de Forcepoint para reenviar mensajes de Syslog en formato CEF a su área de trabajo de Azure a través del agente de Syslog.

1. Configure el producto de Forcepoint para que se integre con Azure Sentinel tal como se describe en la siguientes guías de instalación:
 - [Forcepoint CASB Integration Guide](https://frcpnt.com/casb-sentinel) (Guía de integración de Forcepoint CASB)
 - [Forcepoint CASB Integration Guide](https://frcpnt.com/ngfw-sentinel) (Guía de integración de Forcepoint NGFW)

2. Busque CommonSecurityLog para usar el esquema pertinente en Log Analytics cuando el nombre de DeviceVendor contenga "Forcepoint". 

3. Diríjase al [PASO 3: Validación de conectividad](connect-cef-verify.md).



## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar productos de Forcepoint a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).

- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.