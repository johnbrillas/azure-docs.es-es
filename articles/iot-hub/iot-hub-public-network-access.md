---
title: Administración del acceso a la red pública para Azure IoT Hub
description: Documentación sobre cómo deshabilitar y habilitar el acceso a la red pública para IoT Hub
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/12/2021
ms.openlocfilehash: 27552b2c39f2d1e5d9cc1719a9cc2944e088773a
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100517134"
---
# <a name="managing-public-network-access-for-your-iot-hub"></a>Administración del acceso a la red pública para su instancia de IoT Hub

Para restringir el acceso solo al [punto de conexión privado para su instancia de IoT Hub en su red virtual](virtual-network-support.md), deshabilite el acceso a la red pública. Para ello, use Azure Portal o la API de `publicNetworkAccess`. 

## <a name="turn-off-public-network-access-using-azure-portal"></a>Desactivación del acceso a la red pública mediante Azure Portal

1. Visite [Azure Portal](https://portal.azure.com).
2. Vaya a su instancia de IoT Hub.
3. Seleccione **Redes** en el menú de la izquierda.
4. En "Allow public network access to" (Permitir acceso a la red pública para), seleccione **Deshabilitado**.
5. Seleccione **Guardar**.

:::image type="content" source="media/iot-hub-publicnetworkaccess/turn-off-public-network-access.png" alt-text="Imagen que muestra en Azure Portal dónde desactivar el acceso a la red pública" lightbox="media/iot-hub-publicnetworkaccess/turn-off-public-network-access.png":::

Para activar el acceso a la red pública, seleccione **Todas las redes** y **Guardar**.

## <a name="iot-hub-endpoint-ip-address-and-ports-after-disabling-public-network-access"></a>El punto de conexión de IoT Hub, la dirección IP y los puertos después de deshabilitar el acceso a la red pública

IoT Hub es una plataforma como servicio (PaaS) multiinquilino, por lo que diferentes clientes comparten el mismo grupo de recursos de proceso, red y hardware de almacenamiento. Los nombres de host de IoT Hub se asignan a un punto de conexión público con una dirección IP enrutable públicamente a través de Internet. Diferentes clientes comparten este punto de conexión público de IoT Hub, que es accesible para todos los dispositivos de IoT a través de redes de área extensa y de redes locales. 

La deshabilitación del acceso a la red pública se aplica sobre un recurso de IoT Hub específico, lo que garantiza el aislamiento. Para mantener el servicio activo para otros recursos del cliente usando la ruta de acceso pública, su punto de conexión público aún se puede resolver, las direcciones IP aún se pueden detectar y los puertos permanecen abiertos. Esto no es motivo de preocupación, ya que Microsoft integra varias capas de seguridad para garantizar el aislamiento completo entre los inquilinos. Para más información, consulte [Aislamiento en la nube pública de Azure](../security/fundamentals/isolation-choices.md#tenant-level-isolation).

## <a name="ip-filter"></a>Filtro IP 

Si el acceso a la red pública está deshabilitado, todas las reglas de [Filtro IP](iot-hub-ip-filtering.md) se omiten. Esto se debe a que se bloquean todas las direcciones IP de la red pública de Internet. Para usar el filtro IP, utilice la opción **Selected IP ranges** (Intervalos IP seleccionados).

## <a name="bug-fix-with-built-in-event-hub-compatible-endpoint"></a>Corrección de errores con el punto de conexión compatible con el centro de eventos integrado

Hay un error con IoT Hub en el que el [punto de conexión compatible con el centro de eventos integrado](iot-hub-devguide-messages-read-builtin.md) sigue siendo accesible a través de la red pública de Internet cuando se deshabilita el acceso a la red pública para IoT Hub. Para obtener más información y ponerse en contacto con nosotros en relación con este error, consulte [Al deshabilitar el acceso de la red pública a Azure IoT Hub, se deshabilitará el acceso al punto de conexión integrado del centro de eventos integrado](https://azure.microsoft.com/updates/iot-hub-public-network-access-bug-fix).
