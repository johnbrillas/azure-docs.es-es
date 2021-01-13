---
title: Visualización de alertas
description: Vea las alertas según las distintas categorías y use las características de búsqueda para buscar alertas de interés.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/02/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 4803691a82a97cd2be5fa3beafd4419010e7a9c9
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97837041"
---
# <a name="view-alerts"></a>Visualización de alertas

En este artículo se describe cómo ver las alertas desencadenadas por el sensor y administrarlas con las herramientas de alerta.

Puede ver las alertas según diversas categorías como, por ejemplo, las alertas que se han archivado o anclado. También puede buscar alertas de interés, como las alertas basadas en una dirección IP o MAC.  

También puede ver alertas en el panel del sensor.

Para ver las alertas:

- Seleccione **Alertas** en el menú lateral. La ventana **Alertas** muestra las alertas que ha detectado el sensor.

  :::image type="content" source="media/how-to-work-with-alerts-sensor/alerts-screen.png" alt-text="Vista de la pantalla Alertas.":::

## <a name="view-alerts-by-category"></a>Visualización de alertas por categoría

Puede ver las alertas en función de las distintas categorías de la vista principal **Alertas**. Seleccione una alerta para revisar los detalles y administrar el evento.

| Ordenar por tipo | Descripción |
|--|--|
| **Alertas importantes** | Alertas ordenadas por importancia. |
| **Alertas ancladas** | Alertas que el usuario ha anclado para su posterior análisis. Las alertas ancladas no se archivan y se almacenan durante 14 días en la carpeta anclada. |
| **Alertas recientes** | Alertas ordenadas por hora. |
| **Alertas confirmadas** | Las alertas que se han confirmado y no se han controlado, o que se han silenciado y después se han activado. |
| **Alertas archivadas** | Alertas que el sistema ha archivado automáticamente. Solo el usuario administrador puede acceder a ellas. |

## <a name="search-for-alerts-of-interest"></a>Búsqueda de alertas de interés

La vista principal de **Alertas** proporciona varias características de búsqueda que le ayudarán a encontrar alertas de interés.

:::image type="content" source="media/how-to-work-with-alerts-sensor/main-alerts-view.png" alt-text="Captura de pantalla de aprendizaje de alertas.":::

### <a name="text-search"></a>Búsqueda de texto 

Use la opción **Free Search** (Búsqueda libre) para buscar alertas por texto, números o caracteres.

Para buscar:

- Escriba el texto necesario en el campo **Free Search** (Búsqueda libre) y presione ENTRAR en el teclado.

Para borrar la búsqueda:

- Elimine el texto del campo **Free Search** (Búsqueda libre) y presione ENTRAR en el teclado.

### <a name="device-group-or-device-ip-address-search"></a>Búsqueda de grupos de dispositivos o de direcciones IP de dispositivo

Busque alertas que hagan referencia a direcciones IP o grupos de dispositivos específicos. Los grupos de dispositivos se crean en el mapa del dispositivo.

Para usar filtros avanzados:

1. Seleccione **Filtros avanzados** en la vista principal de **Alertas**.

2. Elija un grupo de dispositivos o un dispositivo.

3. Seleccione **Confirmar**.

4. Para borrar la búsqueda, seleccione **Borrar todo**.

### <a name="security-versus-operational-alert-search"></a>Seguridad frente a búsqueda de alertas operativas

Puede seleccionar entre ver alertas operativas o de seguridad:

- Las alertas de **seguridad** representan un posible tráfico de malware, anomalías de red, infracciones de directivas y de protocolos.

- Las alertas **operativas** representan anomalías de red, infracciones de directivas y de protocolos.

Si no se selecciona ninguna de las opciones, se muestran todas las alertas.

:::image type="content" source="media/how-to-work-with-alerts-sensor/alerts-security.png" alt-text="Seguridad en la pantalla Alertas.":::

## <a name="alert-window-options"></a>Opciones de la ventana Alertas

Los mensajes de alerta proporcionan las siguientes acciones:

- Seleccione :::image type="icon" source="media/how-to-work-with-alerts-sensor/acknowledge-an-alert-icon.png" border="false"::: para confirmar una alerta.

- Seleccione :::image type="icon" source="media/how-to-work-with-alerts-sensor/unacknowledge-an-alert-icon.png" border="false"::: para anular la confirmación de una alerta.

- Seleccione :::image type="icon" source="media/how-to-work-with-alerts-sensor/pin-an-alert-icon.png" border="false"::: para anclar una alerta.

- Seleccione :::image type="icon" source="media/how-to-work-with-alerts-sensor/unpin-an-alert-icon.png" border="false"::: para desanclar una alerta.

- Seleccione :::image type="icon" source="media/how-to-work-with-alerts-sensor/acknowledge-all-alerts-icon.png" border="false"::: para confirmar todas las alertas.

- Seleccione :::image type="icon" source="media/how-to-work-with-alerts-sensor/learn-and-acknowledge-all-alerts.png" border="false"::: para conocer y confirmar todas las alertas.

- Seleccione :::image type="icon" source="media/how-to-work-with-alerts-sensor/export-to-csv.png" border="false"::: para exportar la lista de alertas a un archivo CSV y seleccionar la opción de exportación. Elija **Exportación de alertas** para la opción para exportar a CSV normal. O bien, elija **Extended Alert Export** (Exportación de alertas extendida) para tener la posibilidad de agregar filas independientes y obtener información adicional sobre una alerta del archivo CSV.

- Seleccione el icono :::image type="icon" source="media/how-to-work-with-alerts-sensor/export-to-pdf.png" border="false"::: para descargar un informe de alertas como un archivo PDF.

- Seleccione el icono :::image type="icon" source="media/how-to-work-with-alerts-sensor/download-pcap.png" border="false"::: para descargar el archivo PCAP. El archivo se puede ver con Wireshark, el analizador de protocolos de red gratuito.

- Seleccione :::image type="icon" source="media/how-to-work-with-alerts-sensor/download-filtered-pcap.png" border="false"::: para descargar un archivo PCAP filtrado que solo contenga los paquetes importantes de la alerta, lo que reduce el tamaño del archivo de salida y permite realizar un análisis más específico. Puede ver el archivo mediante Wireshark.

- Seleccione el icono :::image type="icon" source="media/how-to-work-with-alerts-sensor/show-alert-in-timeline.png" border="false"::: para mostrar la alerta en la escala de tiempo del evento.

- Seleccione el icono :::image type="icon" source="media/how-to-work-with-alerts-sensor/pin-an-alert-icon.png" border="false"::: para anclar la alerta.

- Seleccione el icono :::image type="icon" source="media/how-to-work-with-alerts-sensor/unpin-an-alert-icon.png" border="false"::: para desanclar la alerta.

- Seleccione :::image type="icon" source="media/how-to-work-with-alerts-sensor/learn-icon.png" border="false"::: para aprobar el tráfico (solo pueden hacerlo los analistas de seguridad y los administradores).

- Seleccione un dispositivo para mostrarlo en el mapa del dispositivo.

## <a name="next-steps"></a>Pasos siguientes

[Administración del evento de alertas](how-to-manage-the-alert-event.md)

[Aceleración de los flujos de trabajo de alertas](how-to-accelerate-alert-incident-response.md)
