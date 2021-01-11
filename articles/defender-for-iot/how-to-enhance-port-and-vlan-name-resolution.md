---
title: Mejora de la resolución de nombres de puerto y VLAN
description: Personalice los nombres de puerto y VLAN en los sensores para enriquecer la resolución del dispositivo.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/13/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 1747af77468ebe41f59892dea4bb49ce24757f37
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97836820"
---
# <a name="enhance-port-and-vlan-name-resolution"></a>Mejora de la resolución de nombres de puerto y VLAN

Puede personalizar los nombres de puerto y VLAN en los sensores para enriquecer la resolución del dispositivo.

## <a name="customize-port-names"></a>Personalización de los nombres de puerto

Azure Defender para IoT asigna automáticamente nombres a los puertos reservados más universales, como DHCP o HTTP. Puede personalizar los nombres de otros puertos que Defender para IoT detecta. Así, puede asignar un nombre a un puerto no reservado porque ese puerto muestre una actividad inusualmente alta.

Estos nombres aparecen en estos casos:

  - Al seleccionar **Grupos de dispositivos** en el mapa de dispositivos.

  - Al crear widgets que proporcionan información de los puertos.

### <a name="view-custom-port-names-in-the-device-map"></a>Visualización de nombres de puerto personalizados en el mapa de dispositivos

Los puertos que incluyen un nombre definido por los usuarios aparecen en el mapa de dispositivos, en el grupo **Known Applications** (Aplicaciones conocidas).

:::image type="content" source="media/how-to-enrich-asset-information/applications-v2.png" alt-text="Captura de pantalla del mapa de dispositivos que muestra el grupo Known Applications (Aplicaciones conocidas).":::

### <a name="view-custom-port-names-in-widgets"></a>Visualización de nombres de puertos personalizados en los widgets

Los nombres de puerto que ha definido aparecen en los widgets que abarcan el tráfico por puerto.

:::image type="content" source="media/how-to-enrich-asset-information/traffic-v2.png" alt-text="Cobertura del tráfico.":::

Para definir nombres de puerto personalizados:

1. Seleccione **Configuración del sistema** y, luego, elija **Standard Aliases** (Alias estándar).

2. Seleccione **Add Port Alias** (Agregar alias de puerto).

    :::image type="content" source="media/how-to-enrich-asset-information/edit-aliases.png" alt-text="Adición de alias de puerto.":::

3. Escriba el número de puerto, seleccione **TCP/UDP** o elija **Ambos** y agregue el nombre.

4. Seleccione **Guardar**.

## <a name="configure-vlan-names"></a>Configuración de nombres de VLAN

Puede enriquecer los datos del inventario de dispositivos con números y etiquetas de VLAN de dispositivo. Además del enriquecimiento de datos, puede ver el número de dispositivos por VLAN, así como el ancho de banda por widgets de VLAN.

La compatibilidad con VLAN se basa en el estándar 802.1q (hasta el identificador de VLAN 4094).

Hay dos métodos disponibles para recuperar información de VLAN:

- **Detección automática**: de forma predeterminada, el sensor detecta automáticamente las VLAN. Las VLAN detectadas con el tráfico se muestran en la pantalla de configuración de VLAN, en los informes de minería de datos y en otros informes que contienen información de VLAN. No se muestran las VLAN no utilizadas. Estas VLAN no se pueden editar ni eliminar. 

  Debe agregar un nombre único a cada VLAN. Si no agrega un nombre, el número de VLAN aparece en todas las ubicaciones donde se notifique la VLAN.

- **Adición manual**: puede agregar redes VLAN manualmente. Para cada una, debe agregar un nombre único, y estas VLAN se pueden editar o eliminar.

Los nombres de VLAN pueden contener hasta 50 caracteres ASCII.

> [!NOTE]
> Estos nombres no se sincronizan entre el sensor y la consola de administración. También debe definir el nombre en la consola de administración.  
En el caso de los conmutadores Cisco, agregue la siguiente línea a la configuración del intervalo: `monitor session 1 destination interface XX/XX encapsulation dot1q`. En ese comando, *XX/XX* es el nombre y el número del puerto.

Para configurar redes VLAN:

1. En el menú lateral, seleccione **Configuración del sistema**.

2. En la ventana **Configuración del sistema**, seleccione **VLAN**.

    :::image type="content" source="media/how-to-enrich-asset-information/edit-vlan.png" alt-text="Uso de la configuración del sistema para editar las VLAN.":::

3. Agregue un nombre único junto a cada identificador de VLAN.

## <a name="next-steps"></a>Pasos siguientes

Vea la información enriquecida de los dispositivos en los informes de inventario de dispositivos y minería de datos:

- [Investigación de las detecciones de sensores en un inventario de dispositivos](how-to-investigate-sensor-detections-in-a-device-inventory.md)
- [Generación de informes](how-to-generate-reports.md)
