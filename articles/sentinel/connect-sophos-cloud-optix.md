---
title: Conexión datos de Sophos Cloud Optix con Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo usar el conector de datos de Sophos Cloud Optix para extraer los registros de <PRODUCT NAME> en Azure Sentinel. Vea los datos de <PRODUCT NAME> en los libros, cree alertas y mejore la investigación.
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
ms.date: 01/26/2021
ms.author: yelevin
ms.openlocfilehash: c66205ffd9bd5a742d645cbf2f9251a44329a16e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743558"
---
# <a name="connect-your-sophos-cloud-optix-to-azure-sentinel"></a>Conexión de Sophos Cloud Optix con Azure Sentinel

> [!IMPORTANT]
> El conector de Sophos Cloud Optix está actualmente en **VERSIÓN PRELIMINAR**. Consulte [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para conocer los términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

El conector de Sophos Cloud Optix permite conectar fácilmente todos los registros de la solución de seguridad de Sophos Cloud Optix con Azure Sentinel para ver los paneles, crear alertas personalizadas y mejorar la investigación,  Gracias a esta capacidad, dispondrá de más conclusiones sobre la seguridad de la nube y la postura de cumplimiento de su organización, y podrá mejorar las capacidades de las operaciones de seguridad en la nube. La integración entre Sophos Cloud Optix y Azure Sentinel usa la API REST.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="configure-and-connect-sophos-cloud-optix"></a>Configuración y conexión de Sophos Cloud Optix

Sophos Cloud Optix puede integrar y exportar los registros directamente en Azure Sentinel.

1. En el portal de Azure Sentinel, haga clic en **Conectores de datos** y seleccione el conector **Sophos Cloud Optix(versión preliminar)** .

1. Seleccione **Open connector page** (Abrir página del conector).

1. Copie y guarde el **Id. del área de trabajo** y la **Clave principal** de la página del conector.

1. Siga las instrucciones de Sophos para [realizar la integración con Microsoft Azure Sentinel](https://docs.sophos.com/pcg/optix/help/en-us/pcg/optix/tasks/IntegrateAzureSentinel.html) (a partir del tercer paso).

## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez que se establece una conexión correcta, los datos aparecen en **Registros**, en la sección **Registros personalizados**, en la tabla *SophosCloudOptix_CL*.

Para consultar los datos de Sophos Cloud Optix, escriba `SophosCloudOptix_CL` en la ventana de consulta. Consulte la pestaña **Pasos siguientes** de la página de conectores y la [documentación de Sophos](https://docs.sophos.com/pcg/optix/help/en-us/pcg/optix/concepts/ExampleAzureSentinelQueries.html) para ver algunos ejemplos de consulta útiles.

## <a name="validate-connectivity"></a>Validar conectividad

Los registros pueden tardar hasta 20 minutos en empezar a aparecer en Log Analytics. 

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar Sophos Cloud Optix con Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.
