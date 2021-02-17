---
title: Uso del panel de la consola del sensor
description: El panel le permite ver rápidamente el estado de seguridad de la red. Proporciona información general de alto nivel sobre las amenazas para todo el sistema en una escala de tiempo junto con información sobre los dispositivos relacionados.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 11/03/2020
ms.topic: article
ms.service: azure
ms.openlocfilehash: eb37434213dd756ba5d7137b93a1cd37da5bb9ae
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2021
ms.locfileid: "100523641"
---
# <a name="the-dashboard"></a>El panel

El panel le permite ver rápidamente el estado de seguridad de la red. Proporciona información general de alto nivel sobre las amenazas para todo el sistema en una escala de tiempo junto con información sobre los dispositivos relacionados, como:

- Alertas con distintos niveles de gravedad:

- Crítico

- Principal

- Minor

- Advertencias

- Los dos indicadores del centro de la página muestran los paquetes por segundo (PPS) y las alertas no confirmadas (UA). **PPS** es el número de paquetes confirmados por el sistema por segundo. **UA** es el número de alertas que aún no se han confirmado.

- Lista de alertas no confirmadas con su descripción.

- Escala de tiempo con la descripción de la alerta.

:::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/dashboard-alert-screen-v2.png" alt-text="Panel":::

## <a name="viewing-the-latest-alerts"></a>Visualización de las últimas alertas

El medidor de alertas no confirmadas (UA) en el centro de la página indica el número de dichas alertas. Para ver una lista de alertas, seleccione **Más alertas** en la parte inferior de la página del panel o seleccione **Alertas** en el menú lateral.

:::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/unhandled-alerts-list.png" alt-text="Alertas no confirmadas":::

## <a name="status-boxes"></a>Cuadros de estado

En esta sección se describe cada cuadro de estado.

| Cuadro de estado y medidores | Descripción |
| -------------- | -------------- |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/critical-alert-status-box-v2.png" alt-text="Alertas críticas"::: | **Alertas críticas**: el cuadro de la parte superior central de la página indica el número de alertas críticas. Seleccione este cuadro para mostrar las descripciones de las alertas en la escala de tiempo y en la lista situada debajo de los medidores, si existen.                              |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/major-alert-status-box-v2.png" alt-text="Alertas graves"::: | **Alertas graves**: el cuadro de la parte superior derecha de la página indica el número de alertas graves. Seleccione este cuadro para mostrar las descripciones de las alertas en la escala de tiempo y en la lista situada debajo de los medidores, si existen.                                     |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/minor-alert-status-box-v2.png" alt-text="Alertas leves"::: | **Alertas leves**: el cuadro de la parte inferior izquierda de la página indica el número de alertas leves. Seleccione este cuadro para mostrar las descripciones de las alertas en la escala de tiempo y en la lista situada debajo de los medidores, si existen.                                   |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/warnings-alert-status-box-v2.png" alt-text="Alertas de advertencia"::: | **Alertas de advertencia**: el cuadro de la parte inferior central de la página indica el número de alertas de advertencia. Seleccione este cuadro para mostrar las descripciones de las alertas en la escala de tiempo y en la lista situada debajo de los medidores, si existen.                             |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/all-alert-status-box-v2.png" alt-text="Todas las alertas"::: | **Todas las alertas**: el cuadro de la parte inferior derecha de la página indica el número total de alertas críticas, graves, leves y de advertencia. Seleccione este cuadro para mostrar las descripciones de las alertas en la escala de tiempo y en la lista situada debajo de los medidores, si existen. |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/packets-per-second-gauge-v2.png" alt-text="Paquetes por segundo"::: | **Paquetes por segundo (PPS)** : la métrica de PPS es un indicador del rendimiento de la red. |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/unacknowledged-events-gauge-v2.png" alt-text="Eventos no confirmados (UA)"::: | **Eventos no confirmados**: esta métrica indica el número de eventos no confirmados.

## <a name="using-the-timeline"></a>Uso de la escala de tiempo

Las alertas se muestran a lo largo de una escala de tiempo vertical que incluye información de fecha y hora.

La escala de tiempo se muestra gráficamente:

- Alertas críticas

- Alertas graves

- Alertas leves

- Alertas de advertencia

:::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/timeline-of-events.png" alt-text="Gráfico de escala de tiempo":::

## <a name="viewing-alerts"></a>Visualización de alertas

Seleccione la flecha hacia abajo **V** en la parte inferior de un cuadro de alerta para mostrar la entrada de la alerta y la información de los dispositivos.

:::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/extended-alert-screen.png" alt-text="Entrada de alertas e información de dispositivos":::

- Seleccione el dispositivo para mostrar el mapa del modo físico. Los dispositivos afectados se resaltan.

- Haga clic en cualquier lugar del cuadro alerta para mostrar detalles adicionales relacionados con la alerta. Se mostrará un elemento emergente similar al siguiente.

- Seleccione :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/excel-icon.png" alt-text="Excel"::: para exportar un archivo CSV sobre la alerta.

- Solo administradores y analistas de seguridad: seleccione :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/approve-all-icon.png" alt-text="Acknowledge all"::: (Confirmar todo) para **confirmar todas** las alertas asociadas.

- Seleccione :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/pdf-icon.png" alt-text="PDF"::: para descargar un informe de alerta como un archivo PDF.

- Seleccione :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/pin-icon.png" alt-text="Anclar"::: para anclar o desanclar la alerta. Al seleccionar la opción para anclar, se agregará a la ventana **Pinned Alerts** (Alertas ancladas) en la pantalla **Alerts** (Alertas).

- Seleccione :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/download-icon.png" alt-text="Download"::: (Descargar) para investigar la alerta mediante la descarga del archivo PCAP relacionado que contiene un análisis del protocolo de red.

- Seleccione :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/cloud-download-icon.png" alt-text="Cloud"::: (Nube) para descargar un archivo PCAP filtrado relacionado que solo contenga los paquetes pertinentes de la alerta, lo que reduce el tamaño del archivo de salida y permite realizar un análisis más específico. Puede verlo mediante [Wireshark](https://www.wireshark.org/).

- Seleccione :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/navigate-icon.png" alt-text="Navegación"::: para ir a la escala de tiempo del evento en el momento de la alerta solicitada. Esto le permite evaluar otros eventos que pueden estar ocurriendo en torno a la alerta específica.

- Solo administradores y analistas de seguridad: cambian el estado de la alerta de no confirmada a confirmada. Seleccione Aprender para aprobar la actividad detectada.

:::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/unauthorized-internet-connectivity-detection-v3.png" alt-text="Se ha detectado una conexión a Internet no autorizada":::.

## <a name="next-steps"></a>Pasos siguientes

[Uso de alertas en el sensor](how-to-work-with-alerts-on-your-sensor.md)
