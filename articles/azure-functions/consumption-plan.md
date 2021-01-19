---
title: Hospedaje del plan de consumo de Azure Functions
description: Conozca como el hospedaje de planes de consumo de Azure Functions permite ejecutar el código en un entorno que se escala dinámicamente, pero solo se paga por los recursos usados durante la ejecución.
ms.date: 8/31/2020
ms.topic: conceptual
ms.openlocfilehash: c0619def4687935cd9e403563966b35b84f13c7c
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97937543"
---
# <a name="azure-functions-consumption-plan-hosting"></a>Hospedaje del plan de consumo de Azure Functions

Cuando se usa el plan de consumo, las instancias del host de Azure Functions se agregan y quitan de forma dinámica según el número de eventos entrantes. El plan de consumo es la opción de hospedaje completamente <em>sin servidor</em> de Azure Functions.

## <a name="benefits"></a>Ventajas

El plan de consumo se escala automáticamente, incluso durante períodos de carga alta. Cuando se ejecutan funciones en un plan de consumo, se le cobran los recursos de proceso solo cuando se ejecutan las funciones. En un plan de consumo, se agota el tiempo de espera de una ejecución de función tras un período de tiempo configurable.

Puede ver una comparación del plan de consumo con el otro plan y los tipos de hospedaje en [Opciones de escala y hospedaje de funciones](functions-scale.md).

## <a name="billing"></a>Facturación

La facturación se basa en el número de ejecuciones, el tiempo de ejecución y el uso de la memoria. La utilización es un agregado de todas las funciones dentro de la aplicación de función. Para más información, consulte la [página de precios de Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

Para obtener más información sobre cómo calcular los costos cuando se ejecutan en un plan de consumo, vea [Descripción de los costos en un plan de consumo](functions-consumption-costs.md).

## <a name="create-a-consumption-plan-function-app"></a>Creación de una aplicación de funciones en el plan de consumo

Al crear una aplicación de funciones en Azure Portal, el plan de consumo es el predeterminado. Cuando se usan las API para crear una aplicación de funciones, no es necesario crear primero un plan de App Service como sucede con los planes Prémium y dedicado.

Use los vínculos siguientes para aprender a crear una aplicación de funciones sin servidor en un plan de consumo, ya sea mediante programación o en Azure Portal:

+ [CLI de Azure](./scripts/functions-cli-create-serverless.md)
+ [Azure Portal](functions-create-first-azure-function.md)
+ [Plantilla de Azure Resource Manager](functions-create-first-function-resource-manager.md)

También puede crear aplicaciones de funciones en un plan de consumo al publicar un proyecto de Functions desde [Visual Studio Code](functions-create-first-function-vs-code.md#publish-the-project-to-azure) o [Visual Studio](functions-create-your-first-function-visual-studio.md#publish-the-project-to-azure).

## <a name="multiple-apps-in-the-same-plan"></a>Varias aplicaciones en el mismo plan

Las aplicaciones de funciones de la misma región se pueden asignar al mismo plan de consumo. No hay ningún inconveniente ni problema en el hecho de tener varias aplicaciones ejecutándose en el mismo plan de consumo. La asignación de varias aplicaciones al mismo plan de consumo no tiene ningún impacto en la resistencia, la escalabilidad o la confiabilidad de cada aplicación.

## <a name="next-steps"></a>Pasos siguientes

+ [Opciones de hospedaje de Azure Functions](functions-scale.md)
+ [Escalado basado en eventos en Azure Functions](event-driven-scaling.md)
