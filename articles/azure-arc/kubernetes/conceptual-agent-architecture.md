---
title: Arquitectura de agentes de Kubernetes habilitado para Azure Arc
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: En este artículo se proporciona información general de los agentes de Kubernetes habilitado para Azure Arc.
keywords: Kubernetes, Arc, Azure, containers
ms.openlocfilehash: e1f51f066598b59501b30704cb1475dd5332160e
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100561224"
---
# <a name="azure-arc-enabled-kubernetes-agent-architecture"></a>Arquitectura de agentes de Kubernetes habilitado para Azure Arc

[Kubernetes](https://kubernetes.io/) se puede usar para implementar de forma coherente cargas de trabajo en contenedores en entornos híbridos y de varias nubes. Kubernetes habilitado para Azure Arc se puede usar como un plano de control centralizado para administrar de forma coherente directivas, gobernanza y seguridad en estos entornos heterogéneos. En este artículo se proporciona:

* Información general sobre la arquitectura de conexión de clústeres a Azure Arc.
* El patrón de conectividad que siguen los agentes.
* Una descripción de los datos que se intercambian entre el entorno de los clústeres y Azure.

## <a name="deploy-agents-to-your-cluster"></a>Implementación de agentes en un clúster

La mayoría de los centros de seguridad locales exigen estrictas reglas de red que evitan la comunicación entrante en el firewall que se usa en el límite de la red. Kubernetes habilitado para Azure Arc usa estas restricciones y solo habilita puntos de conexión de salida para la comunicación saliente y no requiere ningún puerto de entrada en el firewall. Los agentes de Kubernetes habilitado para Azure Arc inician las conexiones salientes.

![Introducción a la arquitectura](./media/architectural-overview.png)

Siga estos pasos para conectar un clúster a Azure Arc:

1. Cree un clúster de Kubernetes en la infraestructura que prefiera (VMware vSphere, Amazon Web Services, Google Cloud Platform, etc.). 

    > [!NOTE]
    > Los clientes deben crear y administrar el ciclo de vida del clúster de Kubernetes por ellos mismos, ya que actualmente Kubernetes habilitado para Azure Arc solo admite la conexión de clústeres de Kubernetes existentes a Azure Arc.  

1. Inicie el registro de Azure Arc para el clúster mediante la CLI de Azure.
    * La CLI de Azure usa Helm para implementar el gráfico Helm del agente en el clúster.
    * Los nodos de clúster inician una comunicación saliente con [Microsoft Container Registry](https://github.com/microsoft/containerregistry) y extraen las imágenes necesarias para crear los siguientes agentes en el espacio de nombres `azure-arc`:

        | Agente | Descripción |
        | ----- | ----------- |
        | `deployment.apps/clusteridentityoperator` | Actualmente, Kubernetes habilitado para Azure Arc solo admite [identidades asignadas por el sistema](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). clusteridentityoperator hace que la primera comunicación saliente sea necesaria para capturar el certificado de Managed Service Identity (MSI) que usan otros agentes para la comunicación con Azure. |
        | `deployment.apps/config-agent` | Comprueba en el clúster conectado los recursos de configuración de control de código fuente aplicados en el clúster y actualiza el estado de cumplimiento. |
        | `deployment.apps/controller-manager` | Un operador de operadores que organiza las interacciones entre los componentes de Azure Arc. |    
        | `deployment.apps/metrics-agent` | Recopila métricas de otros agentes de Arc para asegurarse de que presentan un rendimiento óptimo. |
        | `deployment.apps/cluster-metadata-operator` | Recopila metadatos del clúster (la versión del clúster, el número de nodos y la versión del agente de Azure Arc). |
        | `deployment.apps/resource-sync-agent` | Sincroniza estos metadatos mencionados con Azure. |
        | `deployment.apps/flux-logs-agent` | Recopila registros de los operadores de Flux implementados como parte de la configuración de control de código fuente. |
    
1. Una vez que todos los pods de los agentes de Kubernetes habilitado para Azure Arc estén en estado `Running`, compruebe que el clúster está conectado a Azure Arc. Debería ver lo siguiente:
    * Un recurso de Kubernetes habilitado para Azure Arc en [Azure Resource Manager](../../azure-resource-manager/management/overview.md). Se realiza un seguimiento de este recurso en Azure como una proyección del clúster de Kubernetes administrado por el cliente, no del propio clúster de Kubernetes real.
    * Los metadatos del clúster, como la versión de Kubernetes, la versión del agente y el número de nodos, aparecen en el recurso de Kubernetes habilitado para Azure Arc como metadatos.

## <a name="data-exchange-between-cluster-environment-and-azure"></a>Intercambio de datos entre el entorno del clúster y Azure

| Tipo de datos | Escenario | Modo de comunicación |
| --------- | -------- | ------------------ |
| Versión del clúster de Kubernetes | Metadatos del clúster | Inserciones del agente en Azure |
| Número de nodos del clúster | Metadatos del clúster | Inserciones del agente en Azure |
| Versión del agente | Metadatos del clúster | Inserciones del agente en Azure |
| Tipo de distribución de Kubernetes | Metadatos del clúster | Inserciones de la CLI de Azure en Azure |
| Tipo de infraestructura (AWS/GCP/vSphere/...) | Metadatos del clúster | Inserciones de la CLI de Azure en Azure |
| Número de nodos de vCPU en el clúster | Facturación | Inserciones de la CLI de Azure en Azure |
| Latido de agente | Estado de los recursos | Inserciones del agente en Azure |
| Consumo de recursos (memoria/CPU) por agentes | Diagnósticos y compatibilidad | Inserciones del agente en Azure |
| Registros de todos los contenedores de agentes | Diagnósticos y compatibilidad | Inserciones del agente en Azure |
| Actualización de disponibilidad del agente | Actualización del agente | Extracción del agente de Azure |
| Estado deseado de configuración: URL del repositorio de Git, parámetros del operador de Flux, clave privada, contenido de hosts conocidos, nombre de usuario de HTTPS, token/contraseña | Configuración | Extracción del agente de Azure |
| Estado de instalación del operador de Flux | Configuración | Inserciones del agente en Azure |
| Asignaciones de Azure Policy que necesitan el cumplimiento de Gatekeeper en el clúster | Azure Policy | Extracción del agente de Azure |
| Auditoría y estado de cumplimiento de las directivas del clúster | Azure Policy | Inserciones del agente en Azure |
| Métricas y registros de las cargas de trabajo del cliente | Azure Monitor | Inserciones del agente en el recurso del área de trabajo de Log Analytics de la suscripción y del inquilino del cliente |

## <a name="connectivity-status"></a>Estado de conectividad

| Status | Descripción |
| ------ | ----------- |
| Connecting | El recurso Kubernetes habilitado para Azure Arc se creó en Azure Resource Manager, pero el servicio aún no ha recibido el latido del agente. |
| Conectado | El servicio Kubernetes habilitado para Azure Arc ha recibido un latido del agente en los últimos 15 minutos. |
| Sin conexión | El recurso Kubernetes habilitado para Azure Arc se conectó anteriormente, pero el servicio no ha recibido ningún latido de agente durante 15 minutos. |
| Expirada | El certificado de Managed Service Identity (MSI) tiene una ventana de expiración de 90 días después de su emisión. Una vez que este certificado expira, el recurso se considera `Expired` y todas las características, como la configuración, la supervisión y la directiva, dejan de funcionar en este clúster. [Aquí](./faq.md#how-to-address-expired-azure-arc-enabled-kubernetes-resources) encontrará más información sobre cómo abordar la expiración de los recursos de Kubernetes habilitado para Azure Arc. |

## <a name="understand-connectivity-modes"></a>Información sobre los modos de conectividad

| Modo de conectividad | Descripción |
| ----------------- | ----------- |
| Completamente conectado | Los agentes siempre pueden ponerse en contacto con Azure. La experiencia es ideal en este caso, ya que hay poco retraso en la propagación de las configuraciones (para GitOps), la aplicación de directivas (en Azure Policy y Gatekeeper) y la recopilación de métricas y registros de cargas de trabajo (en Azure Monitor). |
| Con conexión parcial | El certificado de MSI que ha extraído `clusteridentityoperator` tiene una validez de 90 días antes de que expire el certificado. Una vez que el certificado expira, el recurso Kubernetes habilitado para Azure Arc deja de funcionar. Elimine y vuelva a crear tanto el recurso de Kubernetes habilitado para Azure Arc como los agentes para que todas las características de Arc funcionen en el clúster. Durante los 90 días, se recomienda que los usuarios conecten el clúster al menos una vez cada 30 días. |
| Escenario desconectado | Actualmente, Kubernetes habilitado para Azure Arc no admite clústeres de Kubernetes en entornos desconectados sin acceso a Azure. Si esta funcionalidad le interesa, envíe o vote dicha idea en el [foro de UserVoice de Azure Arc](https://feedback.azure.com/forums/925690-azure-arc).

## <a name="next-steps"></a>Pasos siguientes

* [Conexión de un clúster a Azure Arc](./connect-cluster.md)
* [Introducción conceptual a las configuraciones](./conceptual-configurations.md)