---
title: Lista de comprobación de certificación previa para las ofertas de módulos IoT Edge en Azure Marketplace
description: Obtenga información sobre los requisitos de certificación específicos para publicar ofertas de módulos IoT Edge en Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 03/01/2021
ms.openlocfilehash: 4782e6c0ed866c467051fc1811a60df0010a3db1
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743650"
---
# <a name="pre-certification-checklist-for-iot-edge-modules"></a>Lista de comprobación de certificación previa para los módulos IoT Edge

> [!NOTE]
> Se recomienda que los anunciantes revisen esta lista de comprobación y validen la funcionalidad del módulo antes de enviar certificaciones. Esto acelerará el proceso de certificación, lo que reducirá la necesidad de realizar cambios y reenvíos.

## <a name="validation-of-image"></a>Validación de la imagen

Una vez que la imagen del módulo Edge esté lista para enviarse, siga estos pasos para asegurarse de que la imagen funciona de la manera que Microsoft espera.

### <a name="steps-to-perform-in-the-azure-portal"></a>Pasos que se deben realizar en Azure Portal

1. Abra [Azure Portal](https://partner.microsoft.com/).
1. Cree un grupo de recursos.
1. Cree un centro de IoT Hub.
1. Cree un dispositivo IoT Edge.
1. Copie la cadena de conexión y guárdela en el bloc de notas.
1. Seleccione el conjunto **Módulos en el dispositivo perimetral creado**.
1. Agregue los datos de la instancia de ACR en la que reside la versión más reciente de la imagen.
1. Seleccione **Agregar módulo IoT Edge** y especifique lo siguiente:
    - El URI de imagen en la configuración del módulo.
    - La variable de entorno (la misma que se agregó en el Centro de partners).
    - Las opciones de creación del contenedor (las mismas que se agregaron en el Centro de partners).
    - La configuración de módulo gemelo (la misma que se agregó en el Centro de partners).
1. Agregue rutas (las mismas que se agregaron en el Centro de partners).
1. Seleccione **Revisar + crear**.

Los módulos perimetrales se implementan en el dispositivo Edge creado en Azure.

### <a name="steps-to-perform-on-the-device"></a>Pasos que se deben realizar en el dispositivo

#### <a name="device-details"></a>Detalles del dispositivo

El equipo de certificación usa el siguiente hardware para validar las imágenes de diferentes arquitecturas:

- En el caso de las imágenes x64, una VM de Azure con un tamaño de configuración Standard D2S v3 que ejecuta Ubuntu Server 18.04/Ubuntu Server 16.04.
- En el caso de las imágenes de Azure Resource Manager 32, un Raspberry Pi 3 modelo B.
- En el caso de las imágenes de Azure Resource Manager 64, una NVIDIA Jetson Nano de 4 GB.

#### <a name="steps"></a>Pasos

1. Asegúrese de que se puede acceder a los dispositivos o VM creados a través de PuTTY.
1. Descargue el [entorno de ejecución de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge) en el dispositivo.
1. Actualice la cadena de conexión que copió en el paso 5 al archivo config.yaml.
1. Reinicie el módulo Edge con `sudo systemctl restart iotedge`.
1. Compruebe si el módulo está implementado en el dispositivo con `sudo iotedge list`; debe estar en estado de ejecución.
1. Asegúrese de que los registros del módulo implementado con `sudo iotedge logs “Module Name“ -f` no tengan errores. Si hay errores conocidos, descríbalos en las **Notas para el revisor** del Centro de partners antes de enviar la oferta.

## <a name="metadata-validation"></a>Validación de los metadatos

Verifique lo siguiente:

- La etiqueta más reciente se muestra en el Centro de partners y en la instancia de Azure Container Registry.
- El requisito de hardware mínimo se agregó en la descripción de la oferta.
- El nombre de usuario y la contraseña de Azure Container Registry se actualizaron y se agregaron en el Centro de partners.
- La precisión de la **propiedad de gemelo** deseada, *si procede*.
- La precisión de las **variables de entorno** deseadas, *si procede*.
- La precisión de las **opciones de creación** deseadas, *si procede*.
- La cadena de conexión de la administración de clientes potenciales está presente.
- La directiva de privacidad está presente.
- Las condiciones de uso están presentes.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de módulos desde el marketplace comercial](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal#deploy-from-azure-marketplace)
- [Publicación del módulo Edge en el Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-iot-edge-module-creation)
- [Implementación del módulo IoT Edge](https://docs.microsoft.com/azure/iot-edge/quickstart-linux)  
