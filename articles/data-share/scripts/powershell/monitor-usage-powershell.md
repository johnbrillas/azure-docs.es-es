---
title: 'Script de PowerShell: supervisión del uso de Azure Data Share'
description: Este script de PowerShell recupera las métricas de uso de un recurso compartido de datos enviado.
author: joannapea
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: e9ff7a29cba9b8e9ca058bfe742f484c5b495cd7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92221322"
---
# <a name="use-powershell-to-monitor-the-usage-of-a-sent-data-share"></a>Uso de PowerShell para supervisar el uso de un recurso compartido de datos enviado

Este script de PowerShell supervisa el uso de los datos mediante la enumeración de las sincronizaciones de un recurso compartido de datos enviado y la obtención de los detalles de una sincronización específica.

## <a name="sample-script"></a>Script de ejemplo


```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$synchronizationId = "<Azure synchronization id>"

# List synchronizations on a share
Get-AzDataShareSynchronization -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareName $dataShareName

#Get details of a specific synchronization
Get-AzDataShareSynchronizationDetails -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareName $dataShareName -SynchronizationId $synchronizationId
```


## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: 

| Get-Help | Notas |
|---|---|
| [Get-AzDataShareSynchronization](/powershell/module/az.datashare/get-azdatasharesynchronization) | Enumere las sincronizaciones de un recurso compartido. |
| [Get-AzDataShareSynchronizationDetails](/powershell/module/az.datashare/get-azdatasharesynchronizationdetail) | Obtiene los detalles de una sincronización de un recurso compartido. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/).

Encontrará más ejemplos de scripts de PowerShell para Azure Data Share en el artículo [Ejemplos de PowerShell para Azure Data Share](../../samples-powershell.md).
