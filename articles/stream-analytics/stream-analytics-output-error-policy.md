---
title: Directivas de errores de salida en Azure Stream Analytics
description: Conozca más información sobre las directivas de control de errores de salida en Azure Stream Analytics.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 19d762a55127af34e84185b11518aa6584acb5bd
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98012417"
---
# <a name="azure-stream-analytics-output-error-policy"></a>Directivas de errores de salida en Azure Stream Analytics
En este artículo se describen las directivas de control de errores de datos de salida que se pueden configurar en Azure Stream Analytics.

Las directivas de control de errores de datos de salida se aplican solo a los errores de conversión de datos que suceden cuando el evento de salida que produce un trabajo de Stream Analytics no cumple con el esquema del receptor de destino. Puede configurar esta directiva seleccionando **Reintentar** o **Anular**. En Azure Portal, cuando esté en un trabajo de Stream Analytics, en **Configurar**, seleccione **Directiva de error**.

![Ubicación de las directivas de errores de salida en Azure Stream Analytics](./media/stream-analytics-output-error-policy/stream-analytics-error-policy-locate.png)


## <a name="retry"></a>Reintento
Si se produce un error, Azure Stream Analytics volverá a intentar la escritura del evento indefinidamente hasta que esta se realice correctamente. No hay ningún tiempo de expiración para los reintentos. En última instancia, el evento que se está reintentando bloqueará todos los eventos posteriores del procesamiento. Esta opción es la directiva de control de errores de salida predeterminada.

## <a name="drop"></a>Anular
Azure Stream Analytics anulará todos los eventos de salida que produzcan un error de conversión de datos. No se pueden recuperar los eventos anulados para volver a procesarlos más adelante.


Todos los errores transitorios (por ejemplo, errores de red) se reintentan independientemente de la configuración de la directiva de control de errores de salida.


## <a name="next-steps"></a>Pasos siguientes
[Guía de solución de problemas de Azure Stream Analytics](./stream-analytics-troubleshoot-query.md)