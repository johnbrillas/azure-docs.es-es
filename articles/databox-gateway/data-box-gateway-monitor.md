---
title: Supervisión del dispositivo Azure Data Box Gateway| Microsoft Docs
description: Describe cómo usar Azure Portal y la interfaz de usuario web local para supervisar su instancia de Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: how-to
ms.date: 10/20/2020
ms.author: alkohli
ms.openlocfilehash: ed05f3d60f8ba4fbb06327136c7a117ae1d1d2db
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96581130"
---
# <a name="monitor-your-azure-data-box-gateway"></a>Supervisión de Azure Data Box Gateway

En este artículo se describe cómo supervisar Azure Data Box Gateway. Para supervisar el dispositivo, puede usar Azure Portal o la interfaz de usuario web local. Use Azure Portal para ver los eventos de dispositivo, configurar y administrar las alertas, y ver las métricas.

En este artículo aprenderá a:

> [!div class="checklist"]
>
> * Ver los eventos de dispositivo y las alertas correspondientes
> * Ver las métricas de transacción y de capacidad del dispositivo
> * Configurar y administrar las alertas

## <a name="view-device-events"></a>Ver los eventos de dispositivo

[!INCLUDE [data-box-gateway-view-device-events](../../includes/data-box-gateway-view-device-events.md)]

## <a name="view-metrics"></a>Visualización de métricas

[!INCLUDE [data-box-gateway-view-metrics](../../includes/data-box-gateway-view-metrics.md)]

### <a name="metrics-on-your-device"></a>Métricas sobre el dispositivo

En esta sección se describen las métricas de supervisión sobre el dispositivo. Las métricas pueden ser:

* Métricas de capacidad. Las métricas de capacidad están relacionadas con la capacidad del dispositivo.

* Métricas de transacción. Las métricas de transacción están relacionadas con las operaciones de lectura y de escritura en Azure Storage.

En la tabla siguiente se muestra una lista completa de las métricas:

|Métricas de capacidad                     |Descripción  |
|-------------------------------------|-------------|
|**Capacidad disponible**               | Se refiere al tamaño de los datos que se pueden escribir en el dispositivo. En otras palabras, esta es la capacidad que puede estar disponible en el dispositivo. <br></br>Puede liberar la capacidad del dispositivo mediante la eliminación de la copia local de los archivos que tienen una copia tanto en el dispositivo como en la nube.        |
|**Capacidad total**                   | Hace referencia al número total de bytes en el dispositivo para escribir datos. También se conoce como el tamaño total de la memoria caché local. <br></br> Ahora puede aumentar la capacidad de un dispositivo virtual existente mediante la adición de un disco de datos. Agregue un disco de datos a través de la administración del hipervisor para la VM y, después, reinicie la VM. El bloque de almacenamiento local del dispositivo de puerta de enlace se expandirá para adaptarse al disco de datos recién agregado. <br></br>Para obtener más información, vaya a [Add a hard drive for Hyper-V virtual machine](https://www.youtube.com/watch?v=EWdqUw9tTe4) (Adición de un disco duro para la máquina virtual de Hyper-V). |

|Métricas de transacciones              | Descripción         |
|-------------------------------------|---------|
|**Bytes cargados en la nube (dispositivo)**    | Suma de todos los bytes cargados en todos los recursos compartidos en el dispositivo        |
|**Bytes cargados en la nube (recurso compartido)**     | Bytes cargados por recurso compartido. Puede ser: <br></br> Valor medio: suma de todos los bytes cargados por uno o varios recursos compartidos  <br></br>Valor máx.: número máximo de bytes cargados desde un recurso compartido <br></br>Valor mín.: número mínimo de bytes cargados desde un recurso compartido      |
|**Rendimiento de descarga en la nube (recurso compartido)**| Bytes descargados por recurso compartido. Puede ser: <br></br> Valor medio: suma de todos los bytes leídos o descargados en uno o varios recursos compartidos <br></br> Valor máx.: número máximo de bytes descargados desde un recurso compartido<br></br> Valor mín.: número mínimo de bytes descargados desde un recurso compartido  |
|**Rendimiento de lectura en la nube**            | Suma de todos los bytes leídos desde la nube en todos los recursos compartidos en el dispositivo     |
|**Rendimiento de carga en la nube**          | Suma de todos los bytes escritos en la nube en todos los recursos compartidos en el dispositivo     |
|**Rendimiento de carga en la nube (recurso compartido)**  | Suma de todos los bytes escritos en la nube desde uno o varios recursos compartidos. Se trata de un valor medio, máx. y mín. por recurso compartido      |
|**Rendimiento de lectura (red)**           | Incluye el rendimiento de la red del sistema para todos los bytes leídos desde la nube. Esta vista puede incluir datos no restringidos a los recursos compartidos. <br></br>La división mostrará el tráfico a través de todos los adaptadores de red en el dispositivo. Esto incluye los adaptadores que no están conectados ni habilitados.      |
|**Rendimiento de escritura (red)**       | Incluye el rendimiento de la red del sistema para todos los bytes escritos en la nube. Esta vista puede incluir datos no restringidos a los recursos compartidos. <br></br>La división mostrará el tráfico a través de todos los adaptadores de red en el dispositivo. Esto incluye los adaptadores que no están conectados ni habilitados.          |

## <a name="manage-alerts"></a>Administrar alertas

[!INCLUDE [data-box-gateway-manage-alerts](../../includes/data-box-gateway-manage-alerts.md)]

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [administrar el ancho de banda](data-box-gateway-manage-bandwidth-schedules.md).
