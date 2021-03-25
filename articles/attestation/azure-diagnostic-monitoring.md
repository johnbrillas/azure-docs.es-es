---
title: 'Supervisión de diagnósticos de Azure: Azure Attestation'
description: Supervisión de diagnósticos de Azure para Azure Attestation
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: d01e7817906927295591353b710afe2899aacdf1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101726485"
---
# <a name="setting-up-diagnostics-with-trusted-platform-module-tpm-endpoint-of-azure-attestation"></a>Configuración de diagnósticos con el punto de conexión del Módulo de plataforma segura (TPM) de Azure Attestation

Los [registros de plataforma](../azure-monitor/essentials/platform-logs-overview.md) de Azure, como los registros de recursos y los registros de actividad de Azure, proporcionan información de diagnóstico y auditoría detallada sobre los recursos de Azure y la plataforma de Azure de la que dependen. Las [métricas de plataforma](../azure-monitor/essentials/data-platform-metrics.md) se recopilan de forma predeterminada y suelen almacenarse en la base de datos de métricas de Azure Monitor. En este artículo, se explica cómo crear y establecer la configuración de diagnóstico para enviar métricas y registros de plataforma a diferentes destinos. 

El servicio de punto de conexión de TPM está habilitado con la configuración de diagnóstico y se puede usar para supervisar la actividad. Para configurar [Azure Monitor](../azure-monitor/overview.md) para el punto de conexión de servicio de TPM con PowerShell, siga estos pasos. 

Configure el servicio Azure Attestation. 

[Configuración de Azure Attestation con Azure PowerShell](./quickstart-powershell.md)

```powershell

 Connect-AzAccount 

 Set-AzContext -Subscription <Subscription id> 

 $attestationProviderName=<Name of the attestation provider> 

 $attestationResourceGroup=<Name of the resource Group> 

 $attestationProvider=Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroup 

 $storageAccount=New-AzStorageAccount -ResourceGroupName $attestationProvider.ResourceGroupName -Name <Storage Account Name> -SkuName Standard_LRS -Location <Location> 

 Set-AzDiagnosticSetting -ResourceId $ attestationProvider.Id -StorageAccountId $ storageAccount.Id -Enabled $true 

```
Los registros de actividad se pueden encontrar en la sección Contenedores de la cuenta de almacenamiento. Puede encontrar información detallada en [Tutorial: Recopilación y análisis de registros de recurso desde un recurso de Azure](../azure-monitor/essentials/tutorial-resource-logs.md).
