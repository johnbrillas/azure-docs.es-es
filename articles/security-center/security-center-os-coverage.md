---
title: Plataformas compatibles con Azure Security Center | Microsoft Docs
description: En este documento se proporciona una lista de plataformas compatibles con Azure Security Center.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: overview
ms.date: 03/31/2020
ms.author: memildin
ms.openlocfilehash: 88dc0760f320a99b0cbc99b7637dc34dd11dfecc
ms.sourcegitcommit: 33ac5cd254c33659f668a76a2e295fddcd5d194d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2021
ms.locfileid: "103465453"
---
# <a name="supported-platforms"></a>Plataformas compatibles 

En esta página se muestran las plataformas y los entornos compatibles con Azure Security Center.

## <a name="combinations-of-environments"></a>Combinaciones de entornos <a name="vm-server"></a>

Azure Security Center admite máquinas virtuales y servidores en diferentes tipos de entornos híbridos:

* Solo Azure
* Azure y entorno local
* Azure y otras nubes
* Azure, otras nubes y entorno local

En el caso de un entorno de Azure activado en una suscripción de Azure, Azure Security Center detectará automáticamente los recursos de IaaS que se implementan en la suscripción.

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

Security Center depende del [agente de Log Analytics](../azure-monitor/agents/agents-overview.md#log-analytics-agent). Asegúrese de que las máquinas ejecutan uno de los sistemas operativos compatibles con este agente, como se describe en las siguientes páginas:

* [Sistemas operativos Windows compatibles con el agente de Log Analytics](../azure-monitor/agents/agents-overview.md#supported-operating-systems)
* [Sistemas operativos Linux compatibles con el agente de Log Analytics](../azure-monitor/agents/agents-overview.md#supported-operating-systems)

Asegúrese también de que el agente de Log Analytics esté [configurado correctamente para enviar datos a Security Center](security-center-enable-data-collection.md#manual-agent)

Para obtener más información acerca de las características de Security Center específicas que están disponibles en Windows y Linux, consulte [Cobertura de características para máquinas](security-center-services.md).

> [!NOTE]
> Aunque Azure Defender está diseñado para proteger servidores, la mayoría de las funcionalidades de **Azure Defender para servidores** son compatibles con máquinas con Windows 10. Una característica que no se admite actualmente es la [solución EDR integrada de Security Center: Microsoft Defender para punto de conexión](security-center-wdatp.md).

## <a name="managed-virtual-machine-services"></a>Servicios de máquinas virtuales administradas <a name="virtual-machine"></a>

También se crean máquinas virtuales en una suscripción de cliente como parte de algunos servicios administrados de Azure, como Azure Kubernetes (AKS), Azure Databricks, etc. Security Center detecta también estas máquinas virtuales y el agente de Log Analytics se puede instalar y configurar si hay disponible un sistema operativo compatible.

## <a name="cloud-services"></a>Cloud Services <a name="cloud-services"></a>

También se admiten máquinas virtuales que se ejecuten en un servicio en la nube. Se supervisan los roles de trabajo y web de servicios en la nube que se ejecutan en espacios de producción. Para más información sobre Cloud Services, consulte la [información general sobre Azure Cloud Services](../cloud-services/cloud-services-choose-me.md).

También se admite la protección para las máquinas virtuales que residan en Azure Stack Hub. Para más información acerca de la integración de Security Center con Azure Stack Hub, consulte el artículo sobre [incorporación de máquinas virtuales de Azure Stack Hub a Security Center](quickstart-onboard-machines.md?pivots=azure-portal#onboard-your-azure-stack-hub-vms). 

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [Security Center recopila datos mediante el agente de Log Analytics](security-center-enable-data-collection.md).
- Obtenga información sobre cómo [Security Center administra y protege los datos](security-center-data-security.md).
- Aprenda a [planear y entender las consideraciones de diseño para adoptar Azure Security Center](security-center-planning-and-operations-guide.md).