---
title: 'Azure Defender para Kubernetes: ventajas y características'
description: Obtenga información sobre las ventajas y características de Azure Defender para Kubernetes.
author: memildin
ms.author: memildin
ms.date: 02/07/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 0878686e203960a0b7f33c19cc64e82319997684
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100590442"
---
# <a name="introduction-to-azure-defender-for-kubernetes"></a>Introducción a Azure Defender para Kubernetes

Azure Kubernetes Service (AKS) es un servicio que administra Microsoft y que permite desarrollar, implementar y administrar las aplicaciones en contenedores.

Azure Security Center y AKS forman una oferta de seguridad de Kubernetes nativa en la nube de protección del entorno, protección de la carga de trabajo y protección en tiempo de ejecución, tal y como se describe en [Seguridad de los contenedores en Security Center](container-security.md).

Para la detección de amenazas en los clústeres de Kubernetes, habilite **Azure Defender para Kubernetes**.

La detección de amenazas a nivel de host en los nodos de AKS de Linux está disponible si habilita [Azure Defender para servidores](defender-for-servers-introduction.md) y su agente de Log Analytics. Sin embargo, si el clúster de AKS se implementa en un conjunto de escalado de máquinas virtuales, el agente de Log Analytics no se admite actualmente.

## <a name="availability"></a>Disponibilidad

|Aspecto|Detalles|
|----|:----|
|Estado de la versión:|Disponibilidad general (GA)|
|Precios:|**Azure Defender para Kubernetes** se factura como se muestra en la [página de precios](security-center-pricing.md).|
|Roles y permisos necesarios:|**Administración de seguridad** puede descartar las alertas.<br>**El lector de seguridad** puede ver los resultados.|
|Nubes:|![Sí](./media/icons/yes-icon.png) Nubes comerciales<br>![Sí](./media/icons/yes-icon.png) Nacionales o soberanas (US Gov, China Gov, otros gobiernos)|
|||

## <a name="what-are-the-benefits-of-azure-defender-for-kubernetes"></a>¿Cuáles son las ventajas de Azure Defender para Kubernetes?

Azure Defender para Kubernetes proporciona **protección contra amenazas a nivel de clúster** al supervisar los servicios administrados por AKS mediante los registros recuperados por Azure Kubernetes Service (AKS).

Entre los ejemplos de eventos de seguridad que Azure Defender para Kubernetes supervisa se incluyen los paneles de Kubernetes expuestos y la creación de roles con privilegios elevados y de montajes confidenciales. Para la lista completa de las alertas de nivel de clúster de AKS, consulte la [tabla de referencia de alertas](alerts-reference.md#alerts-akscluster).

> [!TIP]
> Puede simular las alertas de contenedor al seguir las instrucciones de [esta entrada de blog](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270).

Además, nuestro equipo global de investigadores de seguridad supervisa constantemente el panorama de las amenazas. Agregan alertas específicas del contenedor y vulnerabilidades a medida que se detectan.

>[!NOTE]
> Igualmente, Security Center genera alertas de seguridad para las acciones y las implementaciones de Azure Kubernetes Service que se producen **después** de que haya habilitado Azure Defender para Kubernetes.




## <a name="azure-defender-for-kubernetes---faq"></a>Preguntas frecuentes de Azure Defender para Kubernetes

### <a name="can-i-still-get-aks-protections-without-the-log-analytics-agent"></a>¿Puedo obtener protecciones de AKS aún sin el agente de Log Analytics?

El plan de **Azure Defender para Kubernetes** proporciona protecciones a nivel de clúster. Si también implementa el agente de Log Analytics de **Azure Defender para servidores**, obtendrá protección contra amenazas para los nodos que se proporcionan con ese plan. Más información en [Introducción a Azure Defender para servidores](defender-for-servers-introduction.md).

Se recomienda implementar ambos para obtener la protección más completa posible.

Si decide no instalar el agente en los hosts, solo recibirá un subconjunto de las ventajas de la protección contra amenazas y las alertas de seguridad. Aún así, seguirá recibiendo alertas relacionadas con el análisis de redes y las comunicaciones con servidores malintencionados.

### <a name="does-aks-allow-me-to-install-custom-vm-extensions-on-my-aks-nodes"></a>¿Me permite AKS instalar extensiones de máquina virtual personalizadas en mis nodos de AKS?
Para que Azure Defender supervise sus nodos de AKS, deben ejecutar el agente de Log Analytics. 

AKS es un servicio administrado y, como el agente de Log Analytics es una extensión administrada por Microsoft, también es compatible con los clústeres de AKS.

### <a name="if-my-cluster-is-already-running-an-azure-monitor-for-containers-agent-do-i-need-the-log-analytics-agent-too"></a>Si el clúster ya está ejecutando un agente de Azure Monitor para contenedores, ¿necesito también el agente de Log Analytics?
Para que Azure Defender supervise sus nodos de AKS, deben ejecutar el agente de Log Analytics.

Si los clústeres ya ejecutan el agente de Azure Monitor para contenedores, puede instalar el agente de Log Analytics y los dos agentes pueden trabajar junto con otros sin problemas.

[Más información sobre el agente de Azure Monitor para contenedores](../azure-monitor/containers/container-insights-manage-agent.md).


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre la protección de Kubernetes de Security Center, incluido Azure Defender para Kubernetes. 

> [!div class="nextstepaction"]
> [Habilitación de Azure Defender](security-center-pricing.md#enable-azure-defender)

Para obtener material relacionado, consulte los siguientes artículos: 

- [Transmisión de alertas a una solución de administración de servicios de TI, SIEM o SOAR](export-to-siem.md)
- [Tabla de referencia de alertas](alerts-reference.md)
