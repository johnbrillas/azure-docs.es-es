---
title: Conexión de datos de Qualys Vulnerability Management a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar los datos de Qualys Vulnerability Management a Azure Sentinel.
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
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: 8e11c4182520e143007b46c8a7907b5e71bfb27d
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100097592"
---
# <a name="connect-your-qualys-vm-to-azure-sentinel-with-azure-function"></a>Conexión de Qualys Vulnerability Management a Azure Sentinel con Azure Functions

> [!IMPORTANT]
> El conector de datos de Qualys VM en Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El conector de Qualys Vulnerability Management (VM) permite conectar fácilmente todos los registros de la solución de seguridad [Qualys VM](https://www.qualys.com/apps/vulnerability-management/) con Azure Sentinel para ver los paneles, crear alertas personalizadas y mejorar la investigación. La integración entre Qualys VM y Azure Sentinel utiliza Azure Functions para extraer datos de registro mediante la API REST.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="configure-and-connect-qualys-vm"></a>Configuración y conexión de Qualys VM

Azure Functions puede integrar y extraer eventos y registros directamente de Qualys VM y reenviarlos a Azure Sentinel.

1. En el portal de Azure Sentinel, haga clic en **Conectores de datos** y seleccione el conector **Qualys Vulnerability Management**.

1. Seleccione **Open connector page** (Abrir página del conector).

1. Siga las instrucciones de la página **Qualys Vulnerability Management**.

## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez establecida una conexión correcta, los datos aparecen en Log Analytics en la tabla **QualysHostDetection_CL**.

## <a name="validate-connectivity"></a>Validar conectividad

Los registros pueden tardar hasta 20 minutos en empezar a aparecer en Log Analytics.

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar Qualys VM a Azure Sentinel con Azure Function Apps. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.
