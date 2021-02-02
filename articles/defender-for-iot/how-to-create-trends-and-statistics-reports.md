---
title: Generación de informes de tendencias y estadísticas
description: Obtenga información sobre actividad de red, estadísticas y tendencias mediante el uso de los widgets de tendencias y estadísticas de IoT.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 01/24/2021
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 8bfbacc77843c7978bcb1d364bd93f5e09381144
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811585"
---
# <a name="sensor-trends-and-statistics-reports"></a>Informes de tendencias y estadísticas del sensor

## <a name="about-sensor-trends-and-statistics-reports"></a>Acerca de los informes de tendencias y estadísticas del sensor

Puede crear gráficos de widgets y gráficos circulares para obtener información sobre estadísticas y tendencias de red. Los widgets se pueden agrupar en paneles definidos por el usuario.

> [!NOTE]
> Los administradores y analistas de seguridad pueden crear informes de tendencias y estadísticas.

El panel consta de widgets que describen gráficamente los siguientes tipos de información:

- Tráfico por puerto
- Ancho de banda de canal
- Ancho de banda total
- Conexión TCP activa
- Dispositivos:
  - Dispositivos nuevos
  - Dispositivos ocupados
  - Dispositivos por proveedor
  - Dispositivos por SO
  - Dispositivo desconectados
- Caídas de conectividad por horas
- Alertas de incidentes por tipo
- Acceso de tablas de base de datos
- Widgets de disección de protocolos
- Dirección IP y Ethernet:
  - Tráfico de direcciones IP y Ethernet por servicio de CIP
  - Tráfico de direcciones IP y Ethernet por clase de CIP
  - Tráfico de direcciones IP y Ethernet por comando
- OPC:
  - Principales operaciones de administración de OPC
  - Principales operaciones de E/S de OPC
- Siemens S7:
  - Tráfico S7 por función de control
  - Tráfico S7 por subfunción
- SRTP:
  - Tráfico SRTP por código de servicio
  - Errores SRTP por día
- SuiteLink:
  - Principales etiquetas de SuiteLink consultadas
  - Comportamiento de las etiquetas numéricas de SuiteLink
- Tráfico IEC-60870 por ASDU
- Tráfico DNP3 por función
- Tráfico MMS por servicio
- Tráfico Modbus por función
- Tráfico OPC-UA por servicio

> [!NOTE]
>  La hora en los widgets se establece según la hora del sensor.

## <a name="create-reports"></a>Crear informes

Para ver paneles y widgets:

En el menú lateral, seleccione **Tendencias y estadísticas**.

:::image type="content" source="media/how-to-generate-reports/investigation-screenshot.png" alt-text="Captura de pantalla de una investigación.":::

De forma predeterminada, los resultados se muestran para las detecciones de los últimos 7 días. Puede usar las herramientas de filtro para cambiar este intervalo. Por ejemplo, una búsqueda de texto libre.

## <a name="see-also"></a>Vea también

[Informes de evaluación de riesgos](how-to-create-risk-assessment-reports.md)
[Consultas de minería de datos del sensor](how-to-create-data-mining-queries.md)
[Informes de vectores de ataque](how-to-create-attack-vector-reports.md)