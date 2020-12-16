---
title: Activación del modo de vista previa en Azure IoT Hub
description: Obtenga información sobre cómo activar el modo de vista previa en IoT Hub, los motivos y algunas advertencias.
services: iot-hub
author: jlian
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 11/24/2020
ms.author: jlian
ms.openlocfilehash: 864870c4392b12477c321c86afd9da848120490c
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96621712"
---
# <a name="turn-on-preview-mode-for-iot-hub-to-try-select-new-features"></a>Activación del modo de vista previa para que IoT Hub intente seleccionar nuevas características

<!-- 
- We are working hard to bring you new features
- Some of these features require a brand new iot hub with preview mode on
- some features may not work at all or have unexpected behavior
- "Normal preview features" do NOT require preview mode 
- Support opt-in at creation time only
- Customer cannot opt back out post creation
- If customer wants to evaluate, they must use new hub dedicated for the preview
- Banners, documentations and all materials indicate preview quality: no GA guarantee at all
-->

Hay nuevas características en versión preliminar pública para IoT Hub, entre las que se incluyen:

- MQTT 5
- Certificado de servidor ECC
- Negociación de longitud de fragmento TLS
- Compatibilidad de la cadena de CA X509 con HTTPS/WebSocket

Estas características son mejoras en el protocolo de IoT Hub y en los niveles de autenticación, por lo que solo están disponibles por ahora para instancias **nuevas** de IoT Hub. Todavía *no* están disponibles para los centros de IoT existentes. Para obtener una vista previa de estas características, se debe crear el centro de IoT con el modo de vista previa activado.

## <a name="turn-on-preview-mode-for-a-new-iot-hub"></a>Activación del modo de vista previa en un centro de IoT nuevo

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En la página de inicio de Azure, seleccione **+ Crear un recurso** y, después, escriba *IoT Hub* en el campo **Buscar en Marketplace**.

1. Seleccione **IoT Hub** en los resultados de la búsqueda y, después, haga clic en **Crear**.

1. En la pestaña **Aspectos básicos**, rellene los campos [como lo haría habitualmente](iot-hub-create-through-portal.md), salvo **Región**. Seleccione alguna de estas regiones:
    
    - Centro de EE. UU.
    - Oeste de Europa
    - Sudeste de Asia

1. Seleccione la pestaña **Administración** y, después, expanda la sección **Configuración avanzada**.

1. Junto a **Modo de vista previa**, seleccione **Activar**. Revise detenidamente el texto de la advertencia.

    :::image type="content" source="media/iot-hub-preview-mode/turn-on-preview-mode-at-create.png" alt-text="Imagen en la que se muestra dónde seleccionar la opción de modo de vista previa al crear un nuevo centro de IoT":::

1. Seleccione **Siguiente: Revisar y crear** y, después, **Crear**.

Una vez creada, una instancia de IoT Hub en modo de vista previa muestra siempre este banner, lo que le permite usar este centro de IoT solo para fines de vista previa: 

:::image type="content" source="media/iot-hub-preview-mode/banner.png" alt-text="Imagen que muestra el banner del centro de IoT en modo de vista previa":::

## <a name="using-an-iot-hub-in-preview-mode"></a>Uso de un centro de IoT en modo de vista previa

*No* use un centro de IoT en modo de vista previa para producción. El modo de vista previa está pensado *solo* para obtener una vista previa de las características seleccionadas que aparecen en la parte superior de esta página. Otras limitaciones del modo de vista previa de IoT Hub son las siguientes:

- Algunas características de IoT Hub existentes, como filtro de IP, vínculo privado, identidad administrada, flujos de dispositivo y conmutación por error, pueden funcionar de forma inesperada o no.
- Un centro de IoT en modo de vista previa no se puede cambiar ni actualizar a un centro de IoT normal.
- No se puede garantizar el [Acuerdo de Nivel de Servicio de IoT Hub](https://azure.microsoft.com/support/legal/sla/iot-hub/v1_2/) habitual; no lo use para producción.

> [!TIP]
> El modo de vista previa no es necesario para [flujos de dispositivo](iot-hub-device-streams-overview.md) ni para el [seguimiento distribuido](iot-hub-distributed-tracing.md). Para usar las características en versión preliminar anteriores, siga la documentación de la forma habitual. 

## <a name="next-steps"></a>Pasos siguientes

- Para obtener una vista previa de la compatibilidad con MQTT 5, consulte la [información general sobre la compatibilidad de IoT Hub con MQTT 5 (versión preliminar)](iot-hub-mqtt-5.md).
- Para obtener una vista previa del certificado de servidor ECC, vea [Certificado TLS del servidor de criptografía de curva elíptica (ECC) (versión preliminar)](iot-hub-tls-support.md#elliptic-curve-cryptography-ecc-server-tls-certificate-preview).
- Para obtener una vista previa de la negociación del tamaño de fragmento de TLS, vea [Negociación de longitud de fragmento máxima de TLS (versión preliminar)](iot-hub-tls-support.md#tls-maximum-fragment-length-negotiation-preview).