---
title: Conexión de datos de NXLog DNS Logs para Windows a Azure Sentinel | Microsoft Docs
description: Aprenda a usar el conector de datos NXLog DNS Logs para extraer eventos de DNS de Windows y exportarlos a Azure Sentinel. Visualice los datos de DNS de Windows en libros, cree alertas y mejore la investigación.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/02/2021
ms.author: yelevin
ms.openlocfilehash: 880aad438d98605d11e5a2a7c314d89bd8beb5c5
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743995"
---
# <a name="connect-your-nxlog-windows-dns-logs-to-azure-sentinel"></a>Conexión de NXLog DNS Logs para Windows a Azure Sentinel

> [!IMPORTANT]
> El conector NXLog DNS Logs se encuentra actualmente en **versión preliminar**. Consulte [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para conocer los términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

El conector [NXLog DNS Logs](https://nxlog.co/documentation/nxlog-user-guide/windows-dns-server.html) permite exportar fácilmente todos los eventos del servidor DNS de Windows a Azure Sentinel en tiempo real, lo que le proporciona información sobre la actividad del servidor de nombres de dominio en toda su organización. Emplea el [Seguimiento de eventos para Windows (ETW)](https://nxlog.co/documentation/nxlog-user-guide/windows-dns-server.html#dns_windows_etw) de alto rendimiento para recopilar eventos de servidor DNS de auditoría y de análisis en tiempo real, y admite el enriquecimiento de eventos con campos personalizados. La integración entre los NXLog DNS Logs y Azure Sentinel se facilita a través de la API REST.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="configure-and-connect-nxlog-dns-logs"></a>Configuración y conexión de NXLog DNS Logs

NXLog puede configurarse para enviar eventos en formato JSON directamente a Azure Sentinel.

1. En el portal de Azure Sentinel, haga clic en **Conectores de datos** y seleccione el conector **NXLog DNS Logs**.

1. Seleccione **Open connector page** (Abrir página del conector).

1. Siga las instrucciones paso a paso del tema de integración [Microsoft Azure Sentinel](https://nxlog.co/documentation/nxlog-user-guide/sentinel.html) de *NXLog User Guide* (Guía de usuario de NXLog) para configurar el reenvío a través de la API REST.

## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez que se establece una conexión correcta, los datos aparecen en **Registros**, en la sección **Registros personalizados**, en la tabla *DNS_Logs_CL*.

## <a name="validate-connectivity"></a>Validar conectividad

Los registros pueden tardar hasta 20 minutos en empezar a aparecer en Log Analytics.

## <a name="next-steps"></a>Pasos siguientes

En este documento, aprendió a usar NXLog para ingerir registros de DNS de Windows en Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.
