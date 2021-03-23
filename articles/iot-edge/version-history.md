---
title: 'Navegación e historial de versiones de IoT Edge: Azure IoT Edge'
description: Descubra las novedades de IoT Edge con información sobre las nuevas características y funcionalidades de las versiones más recientes.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9b06a9d1065fe27a7ac38d45c2a0aaba635b40eb
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102044857"
---
# <a name="azure-iot-edge-versions-and-release-notes"></a>Versiones de Azure IoT Edge y notas de la versión

Azure IoT Edge es un producto creado a partir del proyecto de IoT Edge de código abierto hospedado en GitHub. Todas las versiones nuevas están disponibles en el [proyecto de Azure IoT Edge](https://github.com/Azure/azure-iotedge). Las contribuciones y los informes de errores se pueden realizar en el [proyecto de IoT Edge de código abierto](https://github.com/Azure/iotedge).

## <a name="documented-versions"></a>Versiones documentadas

La documentación de IoT Edge de este sitio está disponible para dos versiones diferentes del producto, de modo que pueda elegir el contenido que se aplica a su entorno de IoT Edge. Actualmente, las versiones compatibles son:

* **IoT Edge 1.1 (LTS)** es la primera versión de compatibilidad a largo plazo (LTS) de IoT Edge. La documentación de esta versión cubre todas las características y capacidades de todas las versiones anteriores hasta 1.1. Esta versión de la documentación será estable a través de la duración admitida de la versión 1.1 y no reflejará las nuevas características publicadas en versiones posteriores. La versión 1.1 es la última versión disponible con carácter general de IoT Edge.
* **IoT Edge 1.2 (versión preliminar)** incluye contenido adicional para las características y funcionalidades que se encuentran en la versión preliminar más reciente, [1.2-rc4](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc4).

Para más información sobre las versiones de IoT Edge, consulte [Sistemas compatibles con Azure IoT Edge](support.md).

## <a name="version-history"></a>Historial de versiones

En esta tabla se proporciona el historial de versiones recientes de los paquetes de IoT Edge, y se destacan las actualizaciones de documentación realizadas para cada versión.

| Notas de la versión y recursos | Tipo | Date | Aspectos destacados |
| ------------------------ | ---- | ---- | ---------- |
| [1.2-rc4](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1) | Vista previa | Marzo de 2021 | Se introdujeron nuevos paquetes de IoT Edge, con nuevos pasos de instalación y configuración. Para obtener más información, consulte [Actualización de 1.0 o 1.1 a 1.2](how-to-update-iot-edge.md#special-case-update-from-10-or-11-to-12).
| [1.1](https://github.com/Azure/azure-iotedge/releases/tag/1.1.0) | Soporte técnico a largo plazo (LTS) | Febrero de 2021 | [Plan de soporte técnico a largo plazo y actualizaciones de sistemas compatibles](support.md) |
| [1.2-rc1](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1) | Versión preliminar | Noviembre de 2020 | [Dispositivos de IoT Edge detrás de puertas de enlace](how-to-connect-downstream-iot-edge-device.md?view=iotedge-2020-11&preserve-view=true)<br>[Agente MQTT de IoT Edge](how-to-publish-subscribe.md?view=iotedge-2020-11&preserve-view=true) |
| [1.0.10](https://github.com/Azure/azure-iotedge/releases/tag/1.0.10) | Stable | Octubre de 2020 | [Método directo UploadSupportBundle](how-to-retrieve-iot-edge-logs.md#upload-support-bundle-diagnostics)<br>[Carga de métricas en tiempo de ejecución](how-to-access-built-in-metrics.md)<br>[Enrutamiento de prioridad y período de vida](module-composition.md#priority-and-time-to-live)<br>[Orden de inicio de módulos](module-composition.md#configure-modules)<br>[Aprovisionamiento manual de X.509](how-to-register-device.md) |
| [1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) | Stable | Marzo de 2020 | [Aprovisionamiento automático de X.509 con DPS](how-to-auto-provision-x509-certs.md)<br>[Método directo RestartModule](how-to-edgeagent-direct-method.md#restart-module)<br>[Comando support-bundle](troubleshoot.md#gather-debug-information-with-support-bundle-command) |

## <a name="next-steps"></a>Pasos siguientes

* [Consulte todas las versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases)
* [Cree o revise solicitudes de características en el foro de comentarios](https://feedback.azure.com/forums/907045-azure-iot-edge)