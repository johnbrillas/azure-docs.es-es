---
title: Planeamiento para administrar los costos de Azure Cognitive Services
description: Aprenda a planear y administrar los costos de Azure Cognitive Services mediante el análisis de costos de Azure Portal.
author: erhopf
ms.author: erhopf
ms.custom: subject-cost-optimization
ms.service: cognitive-services
ms.topic: how-to
ms.date: 12/15/2020
ms.openlocfilehash: 61eb7d06773428074940d153b01d23b13468795d
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98788831"
---
# <a name="plan-and-manage-costs-for-azure-cognitive-services"></a>Planeamiento y administración de los costos de Azure Cognitive Services

En este artículo se describe cómo puede planear y administrar los costos de Azure Cognitive Services. En primer lugar, antes de agregar recursos del servicio para calcular los costos, use la calculadora de precios de Azure para planear los costos de Cognitive Services. Después, a medida que agregue recursos de Azure, revise los costos estimados. Una vez que ha comenzado a usar los recursos de Cognitive Services (por ejemplo, Voz, Computer Vision, LUIS, Text Analytics, Translator, etc.), use las características de Cost Management para establecer presupuestos y supervisar los costos. También puede revisar los costos previstos e identificar las tendencias de gasto para identificar las áreas en las que podría querer actuar. Los costos de Cognitive Services son solo una parte de los costos mensuales de la factura de Azure. Aunque en este artículo se explica cómo planear y administrar los costos de Cognitive Services, se le facturarán todos los servicios y recursos de Azure usados para su suscripción de Azure, incluidos los servicios de terceros.

## <a name="prerequisites"></a>Requisitos previos

El análisis de costos de Cost Management admite la mayoría de los tipos de cuenta de Azure, pero no todos. Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](../cost-management-billing/costs/understand-cost-mgt-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) (Información sobre los datos de Cost Management). Para ver los datos de costos, se necesita al menos acceso de lectura en la cuenta de Azure. Para más información acerca de cómo asignar acceso a los datos de Azure Cost Management, consulte [Asignación de acceso a los datos](../cost-management-billing/costs/assign-access-acm-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

<!--Note for Azure service writer: If you have other prerequisites for your service, insert them here -->

## <a name="estimate-costs-before-using-cognitive-services"></a>Estimación de costos antes de usar Cognitive Services

Use la [calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/) para calcular los costos antes de agregar Cognitive Services.

:::image type="content" source="media/cognitive-services-pricing-calculator.png" alt-text="Calculadora de precios de Azure para Cognitive Services" border="true":::

A medida que agregue nuevos recursos al área de trabajo, vuelva a esta calculadora y agregue el mismo recurso aquí para actualizar las estimaciones de costos.

Para más información, consulte [Precios de Azure Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/).

## <a name="understand-the-full-billing-model-for-cognitive-services"></a>Modelo de facturación completo de Cognitive Services

Cognitive Services se ejecuta en la infraestructura de Azure que [genera los costos](https://azure.microsoft.com/pricing/details/cognitive-services/) al implementar el nuevo recurso. Es importante que comprenda que hay otras infraestructuras que pueden generar costos. Estos costos deben administrarse cuando se realizan cambios en los recursos implementados. 

### <a name="costs-that-typically-accrue-with-cognitive-services"></a>Costos que normalmente se generan con Cognitive Services

Normalmente, después de implementar un recurso de Azure, los costos vienen determinados por el plan de tarifa y las llamadas API que realice en el punto de conexión. Si el servicio que está usando tiene un nivel de compromiso, puede que se produzca un cargo por uso por encima del límite sobre las llamadas asignadas del nivel.

Se pueden generar costos adicionales al usar estos servicios:

#### <a name="qna-maker"></a>QnA Maker

Cuando se crean recursos para QnA Maker, también se pueden crear recursos para otros servicios de Azure. Incluyen:

- [Azure App Service (por el tiempo de ejecución)](https://azure.microsoft.com/pricing/details/app-service/)
- [Azure Cognitive Search (por los datos)](https://azure.microsoft.com/pricing/details/search/)
 
### <a name="costs-that-might-accrue-after-resource-deletion"></a>Costos que pueden generarse tras eliminar un recurso

#### <a name="qna-maker"></a>QnA Maker

Después de eliminar los recursos de QnA Maker, es posible que los siguientes recursos sigan existiendo. Estos recursos siguen generando costos hasta que se eliminan.

- [Azure App Service (por el tiempo de ejecución)](https://azure.microsoft.com/pricing/details/app-service/)
- [Azure Cognitive Search (por los datos)](https://azure.microsoft.com/pricing/details/search/)

### <a name="using-azure-prepayment-credit-with-cognitive-services"></a>Uso del crédito del pago por adelantado de Azure con Cognitive Services

Puede pagar los cargos de Cognitive Services con el crédito del pago por adelantado de Azure (antes conocido como compromiso monetario). Sin embargo, no puede usar los créditos del pago por adelantado de Azure para pagar los gastos de productos y servicios de terceros, como los de Azure Marketplace.

## <a name="create-budgets"></a>Creación de presupuestos

Puede crear [presupuestos](../cost-management-billing/costs/tutorial-acm-create-budgets.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) para administrar los costos y crear [alertas](../cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) que envíen notificaciones automáticamente a las partes interesadas sobre anomalías en los gastos y riesgos de gastos adicionales. Las alertas se basan en el gasto comparado con los umbrales de presupuesto y costo. Los presupuestos y las alertas se crean para las suscripciones y los grupos de recursos de Azure, por lo que son útiles como parte de una estrategia general de supervisión de costos. 

Los presupuestos se pueden crear con filtros para recursos o servicios específicos de Azure si quiere disponer de más granularidad en la supervisión. Los filtros ayudan a garantizar que no se crean accidentalmente recursos nuevos con un costo adicional. Para más información sobre las opciones de filtro al crear un presupuesto, consulte [Opciones de agrupación y filtrado](../cost-management-billing/costs/group-filter.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

## <a name="export-cost-data"></a>Exportación de datos de costos

También puede [exportar los datos de costos](../cost-management-billing/costs/tutorial-export-acm-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) a una cuenta de almacenamiento. Esto resulta útil cuando usted u otro usuario necesita hacer un análisis de datos adicional para los costos. Por ejemplo, los equipos de finanzas pueden analizar los datos con Excel o Power BI. Puede exportar los costos en una programación diaria, semanal o mensual y establecer un intervalo de fechas personalizado. La exportación de los datos de costos es la forma recomendada de recuperar conjuntos de datos de costos.

<!--
## Other ways to manage and reduce costs for Cognitive Services

Work with Dean to complete this section in 2021.

-->

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [optimizar su inversión en la nube con Azure Cost Management](../cost-management-billing/costs/cost-mgt-best-practices.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Obtenga más información sobre la administración de costos con los [análisis de costos](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Obtenga información sobre cómo [evitar los costos inesperados](../cost-management-billing/cost-management-billing-overview.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Haga el curso de aprendizaje guiado sobre [Cost Management](/learn/paths/control-spending-manage-bills?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).