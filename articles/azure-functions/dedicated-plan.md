---
title: Hospedaje dedicado de Azure Functions
description: Conozca las ventajas de ejecutar Azure Functions en un plan de hospedaje de App Service dedicado.
ms.topic: conceptual
ms.date: 10/29/2020
ms.openlocfilehash: 0ebf83aa919d91f161b247539ae20873242a8ed8
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97937529"
---
# <a name="dedicated-hosting-plans-for-azure-functions"></a>Planes de hospedaje dedicados en Azure Functions

En este artículo se explica cómo hospedar la aplicación de funciones en un plan de App Service, como, por ejemplo, en una instancia de App Service Environment (ASE). Para ver otras opciones de hospedaje, consulte [este artículo](functions-scale.md).

Un plan de App Service define un conjunto de recursos de proceso para que una aplicación se ejecute. Estos recursos de proceso son análogos a la [_granja de servidores_](https://wikipedia.org/wiki/Server_farm) de un hospedaje convencional. Una o varias aplicaciones de funciones se pueden configurar para que se ejecuten en los mismos recursos informáticos (plan de App Service) que otras aplicaciones de App Service, como aplicaciones web. Estos planes incluyen los planes de tarifa Básico, Estándar, Prémium y Aislado. Para más información acerca del funcionamiento del plan de App Service, consulte [Introducción detallada sobre los planes de Azure App Service](../app-service/overview-hosting-plans.md).

Considere el plan de App Service en las situaciones siguientes:

* Tiene máquinas virtuales infrautilizadas que ya ejecutan otras instancias de App Service.
* Quiere proporcionar una imagen personalizada en la que ejecutar sus funciones.

## <a name="billing"></a>Facturación

Paga lo mismo por las aplicaciones de funciones de un plan de App Service que por otros recursos de App Service. Esto es diferente en el [plan de consumo](consumption-plan.md) o el [plan Premium](functions-premium-plan.md) de Azure Functions, que tienen componentes de costo basados en el consumo. Solo se le factura el plan, con independencia del número de aplicaciones de funciones o de aplicaciones web que se ejecuten en él. Para más información, consulte la [página de precios de App Service](https://azure.microsoft.com/pricing/details/app-service/windows/). 

## <a name="always-on"></a><a name="always-on"></a> Always On

Si se ejecuta en un plan de App Service, debe habilitar la configuración **Always On** para que la aplicación de función se ejecute correctamente. En un plan de App Service, el tiempo de ejecución de las funciones queda inactivo después de unos minutos de inactividad, por lo que solo los desencadenadores HTTP podrán "reactivar" las funciones. La configuración **AlwaysOn** solo está disponible en un plan de App Service. En un plan de consumo, la plataforma activa automáticamente las aplicaciones de función.

Incluso con Always On habilitado, el tiempo de espera de ejecución para las funciones individuales se controla mediante la opción `functionTimeout` en el archivo de proyecto [host.json](functions-host-json.md#functiontimeout).

## <a name="scaling"></a>Ampliación

Con un plan de App Service, para escalar horizontalmente de forma manual, puede agregar más instancias de máquina virtual. También puede habilitar el escalado automático, aunque este sea más lento que la escala elástica del plan Premium. Para obtener más información, consulte [Escalación del recuento de instancias de forma manual o automática](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). También puede escalar verticalmente eligiendo un plan de App Service diferente. Vea [Escalado vertical de aplicaciones en Azure](../app-service/manage-scale-up.md) para obtener más información. 

> [!NOTE] 
> Al ejecutar funciones de JavaScript (Node.js) en un plan de App Service, debe elegir un plan con menos CPU virtuales. Para obtener más información, consulte [Elección de los planes de App Service de un solo núcleo](functions-reference-node.md#choose-single-vcpu-app-service-plans). 
<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

## <a name="app-service-environments"></a>Entornos de App Service

La ejecución en una instancia de [App Service Environment (ASE)](../app-service/environment/intro.md) permite aislar completamente las funciones y aprovechar las ventajas de tener un mayor número de instancias que el plan de App Service. Para empezar, vea .

Si solo quiere ejecutar la aplicación de funciones en una red virtual, puede hacerlo mediante el [plan Prémium](functions-premium-plan.md). Para más información, consulte [Establecimiento del acceso al sitio privado de Azure Functions](functions-create-private-site-access.md). 

## <a name="next-steps"></a>Pasos siguientes

+ [Opciones de hospedaje de Azure Functions](functions-scale.md)
+ [Introducción a los planes de Azure App Service](../app-service/overview-hosting-plans.md)