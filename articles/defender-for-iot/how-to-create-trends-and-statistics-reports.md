---
title: Generación de informes de tendencias y estadísticas
description: Obtenga información sobre actividad de red, estadísticas y tendencias mediante el uso de los widgets de tendencias y estadísticas de IoT.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 01/24/2021
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: c28e2f1c24d39ceb915be9f4f6f222d70de9ee73
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2021
ms.locfileid: "100522230"
---
# <a name="sensor-trends-and-statistics-reports"></a>Informes de tendencias y estadísticas del sensor

## <a name="about-sensor-trends-and-statistics-reports"></a>Acerca de los informes de tendencias y estadísticas del sensor

Puede crear gráficos de widgets y gráficos circulares para obtener información sobre estadísticas y tendencias de red. Los widgets se pueden agrupar en paneles definidos por el usuario.

> [!NOTE]
> Los administradores y analistas de seguridad pueden crear informes de tendencias y estadísticas.

El panel consta de widgets que describen gráficamente los siguientes tipos de información:

- Tráfico por puerto
- Tráfico principal por puerto
- Ancho de banda de canal
- Ancho de banda total
- Conexión TCP activa
- Ancho de banda principal por VLAN
- Dispositivos:
  - Dispositivos nuevos
  - Dispositivos ocupados
  - Dispositivos por proveedor
  - Dispositivos por SO
  - Número de dispositivos por VLAN
  - Dispositivo desconectados
- Caídas de conectividad por horas
- Alertas de incidentes por tipo
- Acceso de tablas de base de datos
- Widgets de disección de protocolos
- DELTAV
  - Distribución de operaciones de DeltaV ROC
  - Eventos de DeltaV ROC por nombre
  - Eventos de DeltaV por hora
- AMS
  - Tráfico de AMS por puerto de servidor
  - Tráfico de AMS por comando
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
- VLAN
  - Número de dispositivos por VLAN
  - Ancho de banda principal por VLAN
- 60870-5-104
  - Tráfico IEC-60870 por ASDU
- BACNET
  - Servicios BACnet
- DNP3
  - Tráfico DNP3 por función
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

## <a name="next-steps"></a>Pasos siguientes

[Informes de evaluación de riesgos](how-to-create-risk-assessment-reports.md)
[Consultas de minería de datos del sensor](how-to-create-data-mining-queries.md)
[Informes de vectores de ataque](how-to-create-attack-vector-reports.md)