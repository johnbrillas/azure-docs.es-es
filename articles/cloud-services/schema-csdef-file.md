---
title: Esquema de definición de Azure Cloud Services (clásico) (archivo .csdef) | Microsoft Docs
description: Un archivo de definición de servicio (.csdef) define un modelo de servicio para una aplicación, con los roles, puntos de conexión y valores de configuración disponibles para el servicio.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: b98534b049698ea95c6738ce3404dd5ef8ff7a28
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102502271"
---
# <a name="azure-cloud-services-classic-definition-schema-csdef-file"></a>Esquema de definición de Azure Cloud Services (clásico) (archivo .csdef)

> [!IMPORTANT]
> [Azure Cloud Services (soporte extendido)](../cloud-services-extended-support/overview.md) es un nuevo modelo de implementación basado en Azure Resource Manager para el producto Azure Cloud Services. Con este cambio, se ha modificado el nombre del modelo de implementación basado en Azure Cloud Services para Azure Service Manager a Cloud Services (clásico), y todas las implementaciones nuevas deben usar [Cloud Services (soporte extendido)](../cloud-services-extended-support/overview.md).

El archivo de definición de servicio define el modelo de servicio de una aplicación. El archivo contiene las definiciones de los roles que están disponibles para un servicio en la nube, especifica los puntos de conexión de servicio y establece los valores de configuración del servicio. Los valores de configuración se establecen en el archivo de configuración de servicio, como se describe en el [esquema de configuración de servicio en la nube (clásico)](/previous-versions/azure/reference/ee758710(v=azure.100)).

De forma predeterminada, el archivo de esquema de configuración de Diagnósticos de Azure se instala en el directorio `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas`. Reemplace `<version>` por la versión instalada del [SDK de Azure](https://www.windowsazure.com/develop/downloads/).

La extensión predeterminada del archivo de definición de servicio es. csdef.

## <a name="basic-service-definition-schema"></a>Esquema básico de definición de servicio
El archivo de definición de servicio debe contener un elemento `ServiceDefinition`. La definición de servicio debe contener al menos un rol (elemento `WebRole` o `WorkerRole`). Puede contener hasta 25 roles definidos en una única definición y se pueden mezclar tipos de rol. La definición de servicio también contiene el elemento opcional `NetworkTrafficRules` que restringe qué roles pueden comunicarse con puntos de conexión internos especificados. La definición de servicio también contiene el elemento opcional `LoadBalancerProbes` que incluye sondeos de estado de mantenimiento de puntos de conexión definidos por el cliente.

El formato básico del archivo de definición de servicio es el siguiente.

```xml
<ServiceDefinition name="<service-name>" topologyChangeDiscovery="<change-type>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" upgradeDomainCount="<number-of-upgrade-domains>" schemaVersion="<version>">
  
  <LoadBalancerProbes>
         …
  </LoadBalancerProbes>
  
  <WebRole …>
         …
  </WebRole>
  
  <WorkerRole …>
         …
  </WorkerRole>
  
  <NetworkTrafficRules>
         …
  </NetworkTrafficRules>

</ServiceDefinition>
```

## <a name="schema-definitions"></a>Definiciones de esquema
En los temas siguientes se describe el esquema:

- [Esquema LoadBalancerProbe](schema-csdef-loadbalancerprobe.md)
- [Esquema WebRole](schema-csdef-webrole.md)
- [Esquema WorkerRole](schema-csdef-workerrole.md)
- [Esquema NetworkTrafficRules](schema-csdef-networktrafficrules.md)

##  <a name="servicedefinition-element"></a><a name="ServiceDefinition"></a> Elemento ServiceDefinition
El elemento `ServiceDefinition` es el elemento de nivel superior del archivo de definición de servicio.

En la tabla siguiente se describen los atributos del elemento `ServiceDefinition`:

| Atributo               | Descripción |
| ----------------------- | ----------- |
| name                    |Necesario. El nombre del servicio. El nombre debe ser único dentro de la cuenta de servicio.|
| topologyChangeDiscovery | Opcional. Especifica el tipo de notificación de cambio de topología. Los valores posibles son:<br /><br /> -   `Blast`: envía la actualización en cuanto es posible a todas las instancias de rol. Si elige esta opción, el rol debe ser capaz de controlar la actualización de la topología sin tener que reiniciarse.<br />-   `UpgradeDomainWalk`: envía la actualización a cada instancia de rol de forma secuencial después de que la instancia anterior ha aceptado correctamente la actualización.|
| schemaVersion           | Opcional. Especifica la versión del esquema de definición de servicio. La versión del esquema permite que Visual Studio seleccione las herramientas del SDK correcto que se usarán en la validación del esquema si se instala más de una versión del SDK en paralelo.|
| upgradeDomainCount      | Opcional. Especifica el número de dominios de actualización en los que se asignan roles de este servicio. Las instancias de rol se asignan a un dominio de actualización cuando se implementa el servicio. Para más información, consulte [Actualización de una implementación o un rol de servicio en la nube](cloud-services-how-to-manage-portal.md#update-a-cloud-service-role-or-deployment), [Administración de la disponibilidad de las máquinas virtuales](../virtual-machines/availability.md) y [Qué es un modelo de servicio en la nube](./cloud-services-model-and-package.md).<br /><br /> Puede especificar hasta 20 dominios de actualización. Si no lo especifica, el número predeterminado de dominios de actualización es 5.|