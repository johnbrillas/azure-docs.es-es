---
title: Actualización de datos de inteligencia sobre amenazas
description: El paquete de datos de inteligencia sobre amenazas se proporciona con cada nueva versión de Defender para IoT o, si es necesario, entre versiones.
ms.date: 12/14/2020
ms.topic: how-to
ms.openlocfilehash: 386d59d33c4f9695b8fc5243dab345321cff4a5f
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104784057"
---
# <a name="threat-intelligence-research-and-packages"></a>Investigación y paquetes de inteligencia sobre amenazas

Los equipos de seguridad de Microsoft llevan a cabo operaciones de inteligencia sobre amenazas e investigación de vulnerabilidades de ICS. Estos equipos incluyen MSTIC (Microsoft Threat Intelligence Center, Centro de inteligencia sobre amenazas de Microsoft), DART (Microsoft Detection and Response Team, Equipo de detección y respuesta de Microsoft), DCU (Digital Crimes Unit, Unidad de delitos digitales) y la sección 52 (expertos del dominio de IoT/OT/ICS que realizan el seguimiento de malware, campañas y adversarios de ingeniería inversa de cero días específicos de ICS).

Los equipos proporcionan detección de seguridad, análisis y respuesta a los siguientes elementos de Microsoft:

- Infraestructura y servicios en la nube.
- Productos y dispositivos tradicionales.
- Recursos corporativos internos.

Los equipos de seguridad ofrecen las siguientes ventajas:

- Protección contra amenazas conocidas y pertinentes.
- Información que ayuda a evaluar y establecer las prioridades.
- Comprensión del contexto completo de las amenazas antes de que se produzcan.
- Datos más pertinentes, precisos y procesables.

Esta inteligencia aporta información contextual para enriquecer el análisis de la plataforma de Microsoft y es compatible con los servicios administrados de la empresa para la respuesta a incidentes y la investigación de vulneraciones. Los paquetes de inteligencia sobre amenazas contienen firmas (incluidas las firmas de malware), CVE y otro contenido de seguridad.

Puede descargar paquetes desde la página **Updates** (Actualizaciones) del portal de Azure Defender para IoT.

:::image type="content" source="media/how-to-work-with-threat-intelligence-packages/download-screen.png" alt-text="Descarga de actualizaciones desde el portal de Azure Defender para IoT.":::

## <a name="update-threat-intelligence-data"></a>Actualización de datos de inteligencia sobre amenazas

Los paquetes de inteligencia sobre amenazas se proporcionan con cada nueva actualización de la versión de Defender para IoT o, si es necesario, entre versiones.

Los paquetes que descargue desde el portal de Defender para IoT pueden cargarse manualmente en sensores individuales. Si la consola de administración local administra los sensores, puede descargar los paquetes de inteligencia sobre amenazas en la consola de administración e incluirlos en varios sensores simultáneamente.

Para actualizar un paquete en un solo sensor:

1. Vaya a la página **Updates** (Actualizaciones) de Azure Defender para IoT.

2. Descargue y guarde el paquete de **Threat Intelligence** (Inteligencia sobre amenazas).

3. Inicie sesión en la consola del sensor.

4. En el menú lateral, seleccione **System Settings** (Configuración del sistema).

5. Seleccione **Threat Intelligence Data** (Datos de inteligencia sobre amenazas) y, a continuación, seleccione **Update** (Actualizar).

6. Cargue el nuevo paquete.

Para actualizar un paquete en varios sensores simultáneamente:

1. Vaya a la página **Updates** (Actualizaciones) de Azure Defender para IoT.

2. Descargue y guarde el paquete de **Threat Intelligence** (Inteligencia sobre amenazas).

3. Inicie sesión en la consola de administración.

4. En el menú lateral, seleccione **System Settings** (Configuración del sistema).

5. En la sección **Sensor Engine Configuration** (Configuración del motor de sensores), seleccione los sensores que deben recibir los paquetes actualizados.  

6. En la sección **Select Threat Intelligence Data** (Seleccionar datos de inteligencia sobre amenazas), seleccione el signo más ( **+** ).

7. Cargar el paquete.

## <a name="see-also"></a>Consulte también

[Actualización de versiones](how-to-manage-sensors-from-the-on-premises-management-console.md#update-versions)
