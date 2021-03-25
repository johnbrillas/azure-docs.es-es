---
title: ¿Qué es el controlador de entrada de Azure Application Gateway?
description: Este artículo ofrece una introducción sobre el controlador de entrada de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 03/02/2021
ms.author: caya
ms.openlocfilehash: 2564fd38056241fd48f58f5f6039bf64f92b6741
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101714415"
---
# <a name="what-is-application-gateway-ingress-controller"></a>¿Qué es el controlador de entrada de Application Gateway?
El controlador de entrada de Application Gateway (AGIC) es una aplicación de Kubernetes, lo que hace posible que los clientes de [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) aprovechen la tecnología nativa de Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/): el equilibrador de carga L7 para exponer software en la nube a Internet. AGIC supervisa el clúster de Kubernetes en el que se hospeda y actualiza continuamente una instancia de Application Gateway para que los servicios seleccionados se expongan a Internet.

El controlador de entrada se ejecuta en su propio pod en la instancia de AKS del cliente. AGIC supervisa un subconjunto de recursos de Kubernetes para los cambios. El estado del clúster de AKS se traduce a una configuración específica de Application Gateway y se aplica a [Azure Resource Manager (ARM)](../azure-resource-manager/management/overview.md).

## <a name="benefits-of-application-gateway-ingress-controller"></a>Ventajas del controlador de entrada de Application Gateway
AGIC ayuda a eliminar la necesidad de tener otra dirección IP pública o equilibrador de carga frente al clúster de AKS y evita varios saltos en la ruta de datos antes de que las solicitudes lleguen al clúster AKS. Application Gateway se comunica con los pods mediante su dirección IP privada directamente y no requiere servicios de NodePort o KubeProxy. Esto también aporta un mejor rendimiento a las implementaciones.

El controlador de entrada es compatible exclusivamente con las SKU de Standard_v2 y WAF_v2, que también aportan ventajas de escalado automático. Application Gateway puede reaccionar en respuesta a un aumento o una disminución de la carga y la escala de tráfico según corresponda, sin consumir ningún recurso del clúster de AKS.

El uso de Application Gateway, además de AGIC, también ayuda a proteger el clúster de AKS al proporcionar la funcionalidad de la directiva TLS y firewall de aplicaciones web (WAF).

![Azure Application Gateway + AKS](./media/application-gateway-ingress-controller-overview/architecture.png)

AGIC se configura a través del [recurso de entrada](https://kubernetes.io/docs/user-guide/ingress/) de Kubernetes, junto con los servicios, las implementaciones y los pods. Proporciona una serie de características que aprovechan el equilibrador de carga L7 nativo de Azure Application Gateway. Por mencionar algunas:
  - Enrutamiento de direcciones URL
  - Afinidad basada en cookies
  - Finalización de TLS
  - TLS de un extremo a otro
  - Soporte para sitios web públicos, privados e híbridos
  - Firewall de aplicaciones web integrado

## <a name="difference-between-helm-deployment-and-aks-add-on"></a>Diferencias entre la implementación de Helm y el complemento de AKS
Hay dos maneras de implementar AGIC para el clúster de AKS. La primera manera es mediante Helm; la segunda es mediante AKS como un complemento. La principal ventaja de implementar AGIC como un complemento de AKS es que es mucho más sencillo que la implementación mediante Helm. En el caso de una nueva instalación, puede implementar una nueva instancia de Application Gateway y un nuevo clúster de AKS con AGIC habilitado como un complemento en una línea en la CLI de Azure. El complemento es también un servicio totalmente administrado, que ofrece ventajas adicionales, como actualizaciones automáticas y mayor compatibilidad. Las dos formas de implementar AGIC (Helm y el complemento de AKS) son totalmente compatibles con Microsoft. Además, el complemento permite una mejor integración con AKS como un complemento de primera clase.

El complemento de AGIC se implementa como un pod en el clúster de AKS del cliente; sin embargo, hay algunas diferencias entre la versión de la implementación de Helm y la versión del complemento de AGIC. A continuación se muestra una lista de las diferencias entre las dos versiones: 
  - Los valores de implementación de Helm no se pueden modificar en el complemento de AKS:
    - `verbosityLevel` se establecerá en 5 de forma predeterminada
    - `usePrivateIp` se establecerá en False de forma predeterminada; esto se puede invalidar con la [anotación use-private-ip](ingress-controller-annotations.md#use-private-ip)
    - `shared` no se admite en el complemento 
    - `reconcilePeriodSeconds` no se admite en el complemento
    - `armAuth.type` no se admite en el complemento
  - AGIC implementado mediante Helm admite ProhibitedTargets, lo que significa que AGIC puede configurar la instancia de Application Gateway específicamente para los clústeres de AKS sin que afecte a otros back-ends existentes. El complemento de AGIC no es compatible actualmente con esto. 
  - Dado que el complemento de AGIC es un servicio administrado, los clientes se actualizarán automáticamente a la versión más reciente del complemento de AGIC, a diferencia de AGIC implementado mediante Helm, donde el cliente debe actualizar AGIC manualmente. 

> [!NOTE]
> Los clientes solo pueden implementar un complemento de AGIC por clúster de AKS y cada complemento de AGIC actualmente solo puede tener como destino una instancia de Application Gateway. En el caso de implementaciones que necesiten más de un AGIC por clúster o varios AGIC destinados a una instancia de Application Gateway, siga usando AGIC implementado mediante Helm. 

## <a name="next-steps"></a>Pasos siguientes
- [**Implementación Greenfield del complemento de AKS**](tutorial-ingress-controller-add-on-new.md): instrucciones sobre cómo instalar el complemento de AGIC, AKS y Application Gateway en una infraestructura de pizarra en blanco.
- [**Implementación Brownfield del complemento de AKS**](tutorial-ingress-controller-add-on-existing.md): instalación del complemento de AGIC en un clúster de AKS con una instancia de Application Gateway existente.
- [**Implementación Greenfield con Helm**](ingress-controller-install-new.md): instalación de AGIC mediante Helm, un nuevo clúster de AKS y una nueva instancia de Application Gateway en una infraestructura de pizarra en blanco.
- [**Implementación Brownfield con Helm**](ingress-controller-install-existing.md): implementación de AGIC mediante Helm en un clúster de AKS y una instancia de Application Gateway existentes.