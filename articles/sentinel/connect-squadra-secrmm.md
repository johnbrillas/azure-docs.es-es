---
title: Conexión de datos de secRMM de Squadra Technologies a Azure Sentinel | Microsoft Docs
description: Obtenga información sobre la conexión de datos de secRMM de Squadra Technologies a Azure Sentinel.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/20/2021
ms.author: yelevin
ms.openlocfilehash: a00b4b1e81c0d644cf1475aa46dda3848fda1365
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632908"
---
# <a name="connect-your-squadra-technologies-secrmm-data-to-azure-sentinel"></a>Conexión de datos de secRMM de Squadra Technologies a Azure Sentinel. 

El conector secRMM de Squadra Technologies le permite conectar fácilmente sus registros de la solución de seguridad secRMM de Squadra Technologies a Azure Sentinel. Le permite ver paneles, crear alertas personalizadas y mejorar la investigación. Este conector le ofrece información sobre los eventos de almacenamiento extraíble USB. La integración entre secRMM de Squadra Technologies y Azure Sentinel usa la API de REST.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="configure-and-connect-squadra-technologies-secrmm"></a>Configuración y conexión de secRMM de Squadra Technologies 

El conector secRMM de Squadra Technologies puede integrar y exportar los registros directamente a Azure Sentinel.
1. En el portal de Azure Sentinel, haga clic en Data connectors (Conectores de datos), seleccione el conector secRMM de Squadra Technologies y luego Open connector page (Abrir página de conectores).

2. Siga los pasos descritos en la [guía de incorporación de Squadra Technologies para Azure Sentinel](http://www.squadratechnologies.com/StaticContent/ProductDownload/secRMM/9.9.0.0/secRMMAzureSentinelAdministratorGuide.pdf) para obtener datos de secRMM de Squadra en Azure Sentinel.   

## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez que se establece una conexión correcta, los datos aparecen en **Registros**, en la sección **CustomLogs**, en la tabla `secRMM_CL`.

Para consultar los registros secRMM de Squadra Technologies, escriba el nombre de la tabla en la parte superior de la ventana de consulta.

## <a name="validate-connectivity"></a>Validar conectividad

Los registros pueden tardar hasta 20 minutos en empezar a aparecer en Log Analytics. 

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar secRMM de Squadra Technologies a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.
