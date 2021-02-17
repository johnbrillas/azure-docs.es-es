---
title: Adición de recursos de Azure a una solución de IoT
description: En esta guía de inicio rápido, aprenda a configurar la solución de IoT de un extremo a otro mediante Azure Defender para IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: Shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/25/2021
ms.author: shhazam
ms.openlocfilehash: 8912e8d66ae0cc1b5dba80ee9aabb0fbd288e3c6
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809045"
---
# <a name="quickstart-configure-your-azure-defender-for-iot-solution"></a>Inicio rápido: Configuración de la solución de Azure Defender para IoT

En este artículo se explica cómo realizar la configuración inicial de una solución de seguridad de IoT mediante Defender para IoT.

## <a name="what-is-defender-for-iot"></a>¿Qué es Defender para IoT?

Defender para IoT proporciona una seguridad completa, de un extremo a otro, para las soluciones de IoT que usen Azure.

Con Defender para IoT se puede supervisar toda una solución de IoT desde un solo panel, así como presentar todos los dispositivos de IoT, las plataformas de IoT y los recursos back-end de Azure.

Una vez que se habilita en IoT Hub, Defender para IoT identifica automáticamente otros servicios de Azure, que también estén conectados a IoT Hub y que estén relacionados con la solución de IoT.

Además de la detección automática de relaciones, se pueden elegir los otros grupos de recursos de Azure que se etiquetan como parte de la solución de IoT.

Las opciones que se seleccionen permiten agregar suscripciones completas, grupos de recursos o recursos individuales.

Después de definir todas las relaciones de los recursos, Defender para IoT utiliza Defender para generar alertas y recomendaciones de seguridad relativas a estos recursos.

## <a name="add-azure-resources-to-your-iot-solution"></a>Adición de recursos de Azure a una solución de IoT

Para agregar un nuevo recurso a la solución de IoT:

1. Abra **IoT Hub** en Azure Portal.

1. En **Seguridad**, seleccione **Información general** seguido de **Configuración** y, luego, elija **Recursos supervisados**.

1. Seleccione **Editar** y los recursos supervisados que pertenezcan a la solución de IoT.

1. Seleccione **Agregar**.

Felicidades. Ha agregado un nuevo grupo de recursos a la solución de IoT.

Defender para IoT ahora supervisa los grupos de recursos que acaba de agregar y muestra alertas y recomendaciones de seguridad pertinentes como parte de la solución de IoT.

## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo para aprender a crear módulos de seguridad...

> [!div class="nextstepaction"]
> [Creación de módulos de seguridad](quickstart-create-security-twin.md)
