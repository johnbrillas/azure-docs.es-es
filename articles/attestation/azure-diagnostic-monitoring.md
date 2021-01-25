---
title: 'Supervisión de diagnósticos de Azure: Azure Attestation'
description: Supervisión de diagnósticos de Azure para Azure Attestation
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 40bc76f839cf6757b8f874112504249e611c3e1a
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98606010"
---
# <a name="setting-up-diagnostics-with-trusted-platform-module-tpm-endpoint-of-azure-attestation"></a>Configuración de diagnósticos con el punto de conexión del Módulo de plataforma segura (TPM) de Azure Attestation

Los [registros de plataforma](/azure/azure-monitor/platform/platform-logs-overview) de Azure, como los registros de recursos y los registros de actividad de Azure, proporcionan información de diagnóstico y auditoría detallada sobre los recursos de Azure y la plataforma de Azure de la que dependen. Las [métricas de plataforma](/azure/azure-monitor/platform/data-platform-metrics) se recopilan de forma predeterminada y suelen almacenarse en la base de datos de métricas de Azure Monitor. En este artículo, se explica cómo crear y establecer la configuración de diagnóstico para enviar métricas y registros de plataforma a diferentes destinos. 

El servicio de punto de conexión de TPM está habilitado con la configuración de diagnóstico y se puede usar para supervisar la actividad. Para configurar [Azure Monitor](/azure/azure-monitor/overview) para el punto de conexión de servicio de TPM con PowerShell, siga estos pasos. 

Configure el servicio Azure Attestation. 

[Configuración de Azure Attestation con Azure PowerShell](/azure/attestation/quickstart-powershell#:~:text=%20Quickstart%3A%20Set%20up%20Azure%20Attestation%20with%20Azure,Register%20Microsoft.Attestation%20resource%20provider.%20Register%20the...%20More%20)

```powershell

 Connect-AzAccount 

 Set-AzContext -Subscription <Subscription id> 

 $attestationProviderName=<Name of the attestation provider> 

 $attestationResourceGroup=<Name of the resource Group> 

 $attestationProvider=Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroup 

 $storageAccount=New-AzStorageAccount -ResourceGroupName $attestationProvider.ResourceGroupName -Name <Storage Account Name> -SkuName Standard_LRS -Location <Location> 

 Set-AzDiagnosticSetting -ResourceId $ attestationProvider.Id -StorageAccountId $ storageAccount.Id -Enabled $true 

```
Los registros de actividad se pueden encontrar en la sección Contenedores de la cuenta de almacenamiento. Puede encontrar información detallada en [Tutorial: Recopilación y análisis de registros de recurso desde un recurso de Azure](/azure/azure-monitor/learn/tutorial-resource-logs).
