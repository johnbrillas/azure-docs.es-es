---
title: Tipos de punto de conexión de Traffic Manager | Microsoft Docs
description: En este artículo, se explican los diferentes tipos de puntos de conexión que pueden utilizarse con el Administrador de tráfico de Azure.
services: traffic-manager
documentationcenter: ''
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/21/2021
ms.author: duau
ms.openlocfilehash: 7686f2f97da0113704216dcab741c063a80d3136
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99051234"
---
# <a name="traffic-manager-endpoints"></a>Puntos de conexión del Administrador de tráfico

Microsoft Azure Traffic Manager permite controlar cómo se distribuye el tráfico de red a implementaciones de aplicaciones que se ejecutan en distintos centros de datos. Se configura cada implementación de aplicaciones como un "punto de conexión" en Traffic Manager. Cuando Traffic Manager recibe una solicitud de DNS, elige un punto de conexión disponible para devolverlo en la respuesta DNS. Traffic Manager basa la elección en el estado actual del punto de conexión y el método de enrutamiento del tráfico. Para obtener más información, consulte [Cómo funciona el Administrador de tráfico](traffic-manager-how-it-works.md).

El Administrador de tráfico admite tres tipos de puntos de conexión:

* **puntos de conexión de Azure** se utilizan para los servicios hospedados en Azure.
* Los **puntos de conexión externos** se usan con direcciones IPv4/IPv6, nombres de dominio completos o con servicios fuera de Azure. Estos servicios pueden estar en el entorno local o con otro proveedor de hospedaje.
* Los **puntos de conexión anidados** se usan para combinar los perfiles de Traffic Manager a fin de crear esquemas de enrutamiento de tráfico más flexibles para satisfacer los requisitos de implementaciones más complejas y de mayor envergadura.

No hay restricciones en cuanto a cómo se combinan los diferentes tipos de puntos de conexión en un único perfil de Traffic Manager. Cada perfil puede contener cualquier combinación de tipos de punto de conexión.

En las siguientes secciones se describe con más detalle cada tipo de punto de conexión.

## <a name="azure-endpoints"></a>puntos de conexión de Azure

Los puntos de conexión de Azure se utilizan para servicios basados en Azure en Traffic Manager. Se admiten los siguientes tipos de recursos de Azure:

* Servicios en la nube PaaS
* Web Apps
* Ranuras de aplicación web
* Recursos de PublicIPAddress (que pueden conectarse a las máquinas virtuales directamente o a través de una instancia de Azure Load Balancer). El recurso de publicIpAddress debe tener un nombre DNS asignado para usarse en el perfil de Traffic Manager.

Los recursos de PublicIPAddress son recursos de Azure Resource Manager. No existen en el modelo de implementación clásica y solo se admiten en las experiencias de Azure Resource Manager de Traffic Manager. Los demás tipos de punto de conexión se admiten tanto en el modelo de Resource Manager como el de implementación clásica.

Cuando se usan puntos de conexión de Azure, Traffic Manager detecta cuándo se detiene e inicia una aplicación web. Este estado se refleja en el estado del punto de conexión. Consulte [Supervisión de puntos de conexión de Traffic Manager](traffic-manager-monitoring.md#endpoint-and-profile-status) para más información. Cuando se detiene el servicio subyacente, Traffic Manager no lleva a cabo comprobaciones de estado del punto de conexión ni dirige a este el tráfico. No se producen eventos de facturación de Traffic Manager para la instancia detenida. Cuando se reinicia el servicio, se reanuda la facturación y el punto de conexión es apto para recibir tráfico. Esta detección no se aplica a los puntos de conexión de PublicIpAddress.

## <a name="external-endpoints"></a>puntos de conexión externos

Los puntos de conexión externos se usan para direcciones IPv4/IPv6, nombres de dominio completo o para servicios fuera de Azure. El uso de puntos de conexión de IPv4/IPv6 permite a Traffic Manager comprobar el estado de los puntos de conexión sin necesidad de requerir un nombre DNS para ellos. Como resultado, Traffic Manager puede responder a consultas con los registros A/AAAA cuando se devuelve ese punto de conexión en una respuesta. Los servicios fuera de Azure pueden incluir un servicio hospedado localmente o con un proveedor diferente. Los puntos de conexión externos pueden usarse de forma individual o combinados con puntos de conexión de Azure en el mismo perfil de Traffic Manager. La excepción son los puntos de conexión que se especifican como direcciones IPv4 o IPv6, que solo pueden ser puntos de conexión externos. La combinación de puntos de conexión de Azure con otros externos posibilita distintos escenarios:

* Proporcionar una mayor redundancia a una aplicación local existente en un modelo de conmutación por error de activo-activo o de activo-pasivo mediante el uso de Azure. 
* Enrutamiento del tráfico a puntos de conexión que no tienen un nombre DNS asociado. Además, reducción de la latencia de la búsqueda DNS global al eliminar la necesidad de ejecutar una segunda consulta de DNS para obtener una dirección IP de un nombre DNS devuelto.
* Reducción de la latencia de las aplicaciones para usuarios de todo el mundo y extensión de las aplicaciones locales existentes a otras ubicaciones geográficas en Azure. Para más información, consulte [Método de enrutamiento del tráfico de rendimiento de Traffic Manager](traffic-manager-routing-methods.md#performance).
* Provisión de una mayor capacidad para aplicaciones locales existentes, bien de forma continua o como una solución de ampliación a la nube para satisfacer los picos de demanda mediante Azure.

En algunos casos, resulta útil usar puntos de conexión externos para hacer referencia a los servicios de Azure. Para ver más ejemplos, consulte las [preguntas frecuentes](traffic-manager-faqs.md#traffic-manager-endpoints). Las comprobaciones de estado se facturan según la tarifa de los puntos de conexión de Azure, y no la de los puntos de conexión externos. A diferencia de los puntos de conexión de Azure, si detiene o elimina el servicio subyacente, la facturación de la comprobación de estado continúa. La facturación se detendrá cuando deshabilite o elimine el punto de conexión en Traffic Manager.

## <a name="nested-endpoints"></a>puntos de conexión anidados

Los puntos de conexión anidados combinan varios perfiles de Traffic Manager para crear esquemas de enrutamiento de tráfico flexibles a fin de satisfacer las necesidades de implementaciones más grandes y complejas. Con los puntos de conexión anidados, se agrega un perfil "secundario" como punto de conexión a un perfil "primario". Los perfiles primarios y secundarios pueden contener otros puntos de conexión de cualquier tipo, incluidos otros perfiles anidados. 

Para más información, consulte [Nested Traffic Manager profiles](traffic-manager-nested-profiles.md)(Perfiles anidados de Administrador de tráfico).

## <a name="web-apps-as-endpoints"></a>Web Apps como puntos de conexión

A la hora de configurar Web Apps como puntos de conexión en Traffic Manager, hay que tener en cuenta otros aspectos:

1. Solo se puede utilizar con Traffic Manager Web Apps del nivel de SKU "Estándar" o superiores. Si se intenta agregar una aplicación web de una SKU inferior, se producirá un error. Si se cambia el nivel de SKU de una aplicación web existente a otro inferior, Traffic Manager deja de enviar tráfico a esa aplicación web. Para más información sobre los planes admitidos, consulte [Planes de App Service](https://azure.microsoft.com/pricing/details/app-service/plans/).
2. Cuando un punto de conexión recibe una solicitud HTTP, usa el encabezado "host" de la solicitud para determinar qué aplicación web debe atenderla. El encabezado host contiene el nombre DNS utilizado para iniciar la solicitud, por ejemplo, "contosoapp.azurewebsites.net". Para usar otro nombre DNS con la aplicación web, este debe estar registrado como nombre de dominio personalizado para la aplicación. Al agregar un punto de conexión de aplicación web como punto de conexión de Azure, el nombre DNS del perfil de Traffic Manager se registra automáticamente para la aplicación. Este registro se elimina automáticamente cuando se elimina el punto de conexión.
3. Cada perfil del Administrador de tráfico puede tener, como máximo, un punto de conexión de aplicación web en cada región de Azure. Como solución alternativa para esta restricción, puede configurar una aplicación web como punto de conexión externo. Para más información, consulte las [preguntas más frecuentes](traffic-manager-faqs.md#traffic-manager-endpoints).

## <a name="enabling-and-disabling-endpoints"></a>Habilitación y deshabilitación de puntos de conexión

Deshabilitar un punto de conexión en Traffic Manager puede ser útil para quitar temporalmente tráfico de un punto de conexión que se encuentre en modo de mantenimiento o que se vaya a volver a implementar. Cuando el punto de conexión esté de nuevo en funcionamiento, se puede volver a habilitar.

Puede habilitar o deshabilitar los puntos de conexión mediante el portal de Traffic Manager, PowerShell, la CLI o la API REST.

> [!NOTE]
> Deshabilitar un punto de conexión de Azure no afectará a su estado de implementación en Azure. Un servicio de Azure (como una máquina virtual o una aplicación web) permanece ejecución y puede recibir tráfico incluso cuando esté deshabilitado en Traffic Manager. El tráfico se puede enviar directamente a la instancia del servicio en lugar de por medio del nombre DNS del perfil de Traffic Manager. Para obtener más información, consulte [Cómo funciona el Administrador de tráfico](traffic-manager-how-it-works.md).

La idoneidad actual de cada punto de conexión para recibir tráfico depende de los siguientes factores:

* El estado del perfil (habilitado/deshabilitado)
* El estado del punto de conexión (habilitado/deshabilitado)
* Los resultados de las comprobaciones de estado de ese punto de conexión

Consulte [Acerca de la supervisión del Administrador de tráfico](traffic-manager-monitoring.md#endpoint-and-profile-status)para obtener más información.

> [!NOTE]
> Debido a que el Administrador de tráfico funciona en el nivel de DNS, no es posible influir en las conexiones existentes con ningún punto de conexión. Cuando un punto de conexión no está disponible, Traffic Manager dirige las nuevas conexiones a otro que sí lo esté. Sin embargo, el host tras el punto de conexión deshabilitado o en mal estado puede continuar recibiendo tráfico a través de conexiones existentes hasta que finalizan esas sesiones. Las aplicaciones deberían limitar la duración de la sesión para permitir que se agote el tráfico procedente de conexiones existentes.

Si se deshabilitan todos los puntos de conexión de un perfil o si se deshabilita el perfil en sí, Traffic Manager envía una respuesta "NXDOMAIN" a una nueva consulta de DNS.

## <a name="faqs"></a>Preguntas más frecuentes

* [¿Puedo usar el Administrador de tráfico con puntos de conexión de varias suscripciones?](./traffic-manager-faqs.md#can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions)

* [¿Puedo usar Traffic Manager con espacios de ensayo de servicio en la nube?](./traffic-manager-faqs.md#can-i-use-traffic-manager-with-cloud-service-staging-slots)

* [¿Admite el Administrador de tráfico puntos de conexión IPv6?](./traffic-manager-faqs.md#does-traffic-manager-support-ipv6-endpoints)

* [¿Puedo usar el Administrador de tráfico con más de una aplicación web en la misma región?](./traffic-manager-faqs.md#can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region)

* [¿Cómo muevo los puntos de conexión de Azure de mi perfil de Traffic Manager a un grupo de recursos diferente?](./traffic-manager-faqs.md#how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group-or-subscription)

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Cómo funciona el Administrador de tráfico](traffic-manager-how-it-works.md).
* Obtenga información sobre la [supervisión del punto de conexión y la conmutación por error automática](traffic-manager-monitoring.md)del Administrador de tráfico.
* Conozca los [métodos de enrutamiento de tráfico](traffic-manager-routing-methods.md)del Administrador de tráfico.