---
title: Carga de datos de uso en Azure Monitor
description: Carga de datos de uso de los servicios de datos habilitados para Azure Arc en Azure Monitor
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
zone_pivot_groups: client-operating-system-macos-and-linux-windows-powershell
ms.openlocfilehash: 0c72eda59f375c70274b17796ca53614ef95505b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104669515"
---
# <a name="upload-usage-data-to-azure-monitor"></a>Carga de datos de uso en Azure Monitor

Puede exportar la información de uso de forma periódica. La exportación y la carga de esta información crea y actualiza los recursos del controlador de datos, la instancia administrada de SQL y el grupo de servidores Hiperescala de PostgreSQL en Azure.

> [!NOTE] 
> Durante el período de versión preliminar, los servicios de datos habilitados para Azure Arc no suponen ningún costo.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]


> [!NOTE]
> Espere al menos 24 horas después de crear el controlador de datos de Azure Arc antes de cargar los datos de uso.

## <a name="create-service-principal-and-assign-roles"></a>Creación de una entidad de servicio y asignación de roles

Antes de continuar, asegúrese de que ha creado la entidad de servicio necesaria y de que le ha asignado un rol adecuado. Para obtener detalles, consulte:
* [Creación de una entidad de servicio](upload-metrics-and-logs-to-azure-monitor.md#create-service-principal).
* [Asignación de roles a la entidad de servicio](upload-metrics-and-logs-to-azure-monitor.md#assign-roles-to-the-service-principal)

## <a name="upload-usage-data"></a>Carga de datos de uso

La información de uso, como el inventario y el uso de recursos, se puede cargar en Azure con el siguiente método de dos pasos:

1. Inicie sesión en el controlador de datos. Escriba los valores en el símbolo del sistema. 

   ```console
   azdata login
   ```

1. Exporte los datos de uso mediante comando `azdata arc dc export`, como se indica a continuación:

   ```console
   azdata arc dc export --type usage --path usage.json
   ```
 
   Este comando crea un archivo `usage.json` con todos los recursos de datos habilitados para Azure Arc, como las instancias administradas de SQL y las instancias de Hiperescala de PostgreSQL, etc., que se crean en el controlador de datos.

2. Cargue los datos de uso mediante el comando ```azdata upload```.

   ```console
   azdata arc dc upload --path usage.json
   ```

## <a name="automating-uploads-optional"></a>Automatización de cargas (opcional)

Si quiere cargar métricas y registros de forma programada, puede crear un script y ejecutarlo en un temporizador cada pocos minutos. A continuación se muestra un ejemplo de cómo automatizar las cargas mediante un script de shell de Linux.

En el editor de código o texto que prefiera, agregue el siguiente script al archivo y guárdelo como un archivo ejecutable de script como `.sh` (Linux o Mac) o bien `.cmd`, `.bat` o `.ps1`.

```console
azdata arc dc export --type metrics --path metrics.json --force
azdata arc dc upload --path metrics.json
```

Conversión del archivo de script en ejecutable

```console
chmod +x myuploadscript.sh
```

Ejecute el script cada 20 minutos:

```console
watch -n 1200 ./myuploadscript.sh
```

También puede usar un programador de trabajos como cron o el Programador de tareas de Windows, o bien un orquestador como Ansible, Puppet o Chef.

## <a name="next-steps"></a>Pasos siguientes

[Carga de métricas en Azure Monitor](upload-metrics.md)

[Carga de registros a Azure Monitor](upload-logs.md)

[Carga de datos de facturación en Azure y visualización en Azure Portal](view-billing-data-in-azure.md)

[Visualización del recurso de controlador de datos de Azure Arc en Azure Portal](view-data-controller-in-azure-portal.md)
