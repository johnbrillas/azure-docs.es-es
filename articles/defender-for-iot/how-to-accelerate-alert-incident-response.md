---
title: Aceleración de los flujos de trabajo de alertas
description: Mejore los flujos de trabajo de alertas e incidentes.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/02/2020
ms.service: azure
ms.topic: how-to
ms.openlocfilehash: 14d7a0de1cd29b8c07f90c759a4d423d7186fdb9
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97836540"
---
# <a name="accelerate-alert-workflows"></a>Aceleración de los flujos de trabajo de alertas

En este artículo se describe cómo acelerar los flujos de trabajo de alertas mediante comentarios de alertas, grupos de alertas y reglas de alerta personalizadas en Azure Defender para IoT.  Estas herramientas le ayudan a:

- Analizar y administrar el gran volumen de eventos de alerta detectados en la red.

- Localizar y controlar actividad de red específica.

## <a name="accelerate-incident-workflows-by-using-alert-comments"></a>Aceleración de los flujos de trabajo de incidentes mediante comentarios de alertas

Trabaje con comentarios de alertas para mejorar la comunicación entre los usuarios y los equipos durante la investigación de un evento de alerta.

:::image type="content" source="media/how-to-work-with-alerts-sensor/suspicion-of malicious-activity-screen.png" alt-text="Captura de pantalla que muestra actividad malintencionada.":::

Use los comentarios de alertas para mejorar:

- **Pasos del flujo de trabajo**: proporcione pasos de mitigación de alertas.

- **Seguimiento del flujo de trabajo**: notifique los pasos que se realizaron.

- **Guía del flujo de trabajo**: proporcione recomendaciones, conclusiones o advertencias sobre el evento.

:::image type="content" source="media/how-to-work-with-alerts-sensor/alert-comment-screen.png" alt-text="Captura de pantalla que muestra comentarios de alertas.":::

La lista de opciones disponibles aparece en cada alerta. Los usuarios pueden seleccionar uno o varios mensajes.

Para agregar comentarios de alertas:

1. En el menú lateral, seleccione **Configuración del sistema**.

2. En la ventana **Configuración del sistema**, seleccione **Alert Comments** (Comentarios de alertas).

3. En el cuadro **Agregar comentarios**, escriba el texto del comentario. Puede usar un máximo de 50 caracteres. No se permiten comas.

4. Seleccione **Agregar**.

## <a name="accelerate-incident-workflows-by-using-alert-groups"></a>Aceleración de los flujos de trabajo de incidentes mediante grupos de alertas

Los grupos de alertas permiten a los equipos del Centro de operaciones de seguridad (SOC) ver y filtrar las alertas en sus soluciones de Administración de eventos e información de seguridad (SIEM) y, después, administrarlas en función de las directivas de seguridad de la empresa y las prioridades de esta. Por ejemplo, las alertas sobre las nuevas detecciones se organizan en un grupo de detección. Este grupo incluye alertas que se ocupan de la detección de nuevos dispositivos, nuevas VLAN, nuevas cuentas de usuario, nuevas direcciones MAC, etc.

Los grupos de alertas se aplican al crear reglas de reenvío para las siguientes soluciones de asociados:

  - Servidores de Syslog

  - QRadar

  - ArcSight

:::image type="content" source="media/how-to-work-with-alerts-sensor/create-forwarding-rule.png" alt-text="Captura de pantalla de la creación de una regla de reenvío.":::

El grupo de alertas correspondiente aparece en las soluciones de salida de asociados. 

### <a name="requirements"></a>Requisitos

El grupo de alertas aparecerá en las soluciones de asociados compatibles con los siguientes prefijos:

  - **cat** para QRadar, ArcSight, Syslog CEF, Syslog LEEF

  - **Alert Group** para mensajes de texto de Syslog

  - **alert_group** para objetos de Syslog

Estos campos deben configurarse en la solución de asociado para mostrar el nombre del grupo de alertas. Si no hay ninguna alerta asociada a un grupo de alertas, el campo de la solución de asociado mostrará **ND**.

### <a name="default-alert-groups"></a>Grupos de alertas predeterminados

Los siguientes grupos de alertas se definen automáticamente:
|  |  |  |
|--|--|--|
| Comportamiento de comunicación anómalo | Alertas personalizadas | Acceso remoto |
| Comportamiento de comunicación HTTP anómalo | Detección | Comandos de reinicio y detención |
| Authentication | Cambio de firmware | Explorar |
| Comportamiento de comunicación no autorizado | Comandos ilegales | Tráfico del sensor |
| Anomalías de ancho de banda | Acceso a Internet | Sospecha de malware |
| Desbordamiento de búfer | Errores de operación | Sospecha de actividad malintencionada |
| Errores de comando | Problemas operativos |  |
| Cambios en la configuración | Programar |  |

Los grupos de alertas están predefinidos. Para más información acerca de las alertas asociadas a grupos de alertas y sobre la creación de grupos de alertas personalizados, póngase en contacto con [Soporte técnico de Microsoft](https://support.microsoft.com/supportforbusiness/productselection?sapId=82c88f35-1b8e-f274-ec11-c6efdd6dd099).

## <a name="customize-alert-rules"></a>Personalización de reglas de alertas

Puede agregar reglas de alertas personalizadas basadas en la información que detectan los sensores individuales. Por ejemplo, defina una regla que indique a un sensor que desencadene una alerta basada en una dirección IP de origen, una dirección IP de destino o un comando (dentro de un protocolo). Cuando el sensor detecta el tráfico definido en la regla, se genera una alerta o un evento.

El mensaje de alerta indica que una regla definida por el usuario ha desencadenado la alerta.

:::image type="content" source="media/how-to-work-with-alerts-sensor/customized-alerts-screen.png" alt-text="Captura de pantalla que muestra alertas personalizadas.":::

Para crear una regla de alertas personalizada:

1. Seleccione **Alertas personalizadas** en el menú lateral de un sensor.
1. Seleccione el signo más ( **+** ) para crear una regla.

   :::image type="content" source="media/how-to-work-with-alerts-sensor/user-defined-rule.png" alt-text="Captura de pantalla que muestra una regla definida por el usuario.":::

1. Defina un nombre de regla.
1. Seleccione una categoría o un protocolo en el panel **Categorías**.
1. Defina una dirección MAC o IP de origen y de destino específicas o elija cualquier dirección.
1. Agregue una condición. Una lista de condiciones y sus propiedades es única para cada categoría. Puede seleccionar más de una condición para cada alerta.
1. Indica si la regla desencadena una **alarma** o un **evento**.
1. Asigne un nivel de gravedad a la alerta.
1. Indique si la alerta incluirá un archivo PCAP.
1. Seleccione **Guardar**.

La regla se agrega a la lista de **reglas de alertas personalizadas**, en la que puede revisar los parámetros básicos de la regla, la última vez que se desencadenó la regla, etc. También puede habilitar y deshabilitar la regla desde la lista.

:::image type="content" source="media/how-to-work-with-alerts-sensor/customized-alerts-screen.png" alt-text="Captura de pantalla de una regla personalizada agregada por el usuario.":::

### <a name="see-also"></a>Consulte también

[Visualización de la información proporcionada en las alertas](how-to-view-information-provided-in-alerts.md)

[Administración del evento de alertas](how-to-manage-the-alert-event.md)
