---
title: Planeamiento de la administración de costos de Azure Synapse Analytics
description: Aprenda a planear y administrar los costos de Azure Synapse Analytics mediante el análisis de costos de Azure Portal.
author: julieMSFT
ms.author: jrasnick
ms.custom: subject-cost-optimization
ms.service: synapse-analytics
ms.topic: how-to
ms.date: 12/09/2020
ms.openlocfilehash: e62d494989c86b4c0eab9cdbd51b68fc49fffe76
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681341"
---
# <a name="plan-and-manage-costs-for-azure-synapse-analytics"></a>Planeamiento y administración de costos de Azure Synapse Analytics

En este artículo se describe cómo planear y administrar los costos de Azure Synapse Analytics. Antes de agregar recursos en Azure Synapse, use la calculadora de precios de Azure para estimar y planear los costos del servicio. Después, a medida que agregue los recursos de Azure Synapse, revise los costos estimados.

Después de comenzar a usar los recursos de Azure Synapse, utilice las características de Cost Management para establecer presupuestos y supervisar los costos. También puede revisar los costos previstos e identificar las tendencias de gasto para identificar las áreas en las que podría querer actuar. Los costos de Azure Synapse son solo una parte de los costos mensuales de la factura de Azure. Aunque en este artículo se explica cómo planear y administrar los costos de Azure Synapse, se le facturarán todos los servicios y recursos de Azure que use en su suscripción de Azure, incluidos los servicios de terceros.

## <a name="prerequisites"></a>Requisitos previos

El análisis de costos de Cost Management admite la mayoría de los tipos de cuenta de Azure, pero no todos. Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) (Información sobre los datos de Cost Management). Para ver los datos de costos, se necesita al menos acceso de lectura en la cuenta de Azure. Para más información acerca de cómo asignar acceso a los datos de Azure Cost Management, consulte [Asignación de acceso a los datos](https://docs.microsoft.com/azure/cost-management/assign-access-acm-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

## <a name="estimate-costs-before-using-azure-synapse-analytics"></a>Cálculo de costos antes de usar Azure Synapse Analytics

Use la [calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/) para calcular los costos antes de agregar Azure Synapse Analytics.

Azure Synapse incluye varios recursos que tienen distintos cargos, tal como se muestra en la estimación de costos que aparece a continuación. 

![Ejemplo que muestra el costo estimado en la calculadora de precios de Azure](./media/plan-manage-costs/cost-estimate.png)

## <a name="understand-the-full-billing-model-for-azure-synapse-analytics"></a>Modelo de facturación completo de Azure Synapse Analytics

Azure Synapse se ejecuta en una infraestructura de Azure que genera otros costos, además de los de Azure Synapse, cuando se implementa el nuevo recurso. Es importante que comprenda que hay otras infraestructuras que pueden generar costos. Estos costos deben administrarse cuando se realizan cambios en los recursos implementados. 

### <a name="costs-that-typically-accrue-with-azure-synapse-analytics"></a>Costos que normalmente se generan con Azure Synapse Analytics

Cuando se crean recursos para Azure Synapse, también se crean recursos para otros servicios de Azure. Incluyen:

- Data Lake Storage Gen2

 ### <a name="costs-might-accrue-after-resource-deletion"></a>Costos que pueden generarse tras eliminar un recurso

Después de eliminar los recursos de Azure Synapse, es posible que los siguientes recursos sigan existiendo. Estos recursos siguen generando costos hasta que se eliminan.

- Data Lake Storage Gen2

### <a name="using-monetary-credit-with-azure-synapse"></a>Uso de crédito monetario con Azure Synapse 

Puede pagar los cargos de Azure Synapse con el crédito de compromiso monetario de EA. Sin embargo, no puede usar los créditos de compromiso monetario de EA para pagar los gastos de productos y servicios de terceros, como los de Azure Marketplace.

## <a name="review-estimated-costs-in-the-azure-portal"></a>Revisión de los costos estimados en Azure Portal

A medida que cree recursos para Azure Synapse Analytics, verá los costos estimados. Con un área de trabajo se crea un grupo de SQL sin servidor. Este grupo de SQL sin servidor no incurrirá en cargos hasta que ejecute consultas. Además, deberán crearse otros recursos, como los grupos de SQL dedicados y los grupos de Apache Spark sin servidor, en el área de trabajo.

Para crear un valor de <ResourceName> y ver el precio estimado:

1. Navegue hasta el servicio en Azure Portal.
2. Cree el recurso.
3. Revise el precio estimado que se muestra en el resumen.
4. Termine de crear el recurso.

![Ejemplo que muestra los costos estimados durante la creación de un recurso](./media/plan-manage-costs/create-workspace-cost.png)


Si la suscripción de Azure tiene un límite de gasto, Azure le impide gastar por encima del importe del crédito. A medida que crea y usa recursos de Azure, se usan los créditos. Cuando alcanza el límite de crédito, los recursos que ha implementado se deshabilitan para el resto de ese período de facturación. No se puede cambiar el límite de crédito, pero sí puede quitarlo. Para más información sobre los límites de gasto, consulte [Límite de gasto de Azure](https://docs.microsoft.com/azure/billing/billing-spending-limit).

## <a name="monitor-costs"></a>Supervisión de costos

A medida que se usan recursos de Azure Synapse, se incurre en costos. Los costos de unidad de uso de recursos de Azure varían según el intervalo de tiempo (segundos, minutos, horas y días) o el uso de unidades (bytes, megabytes, etc.). En cuanto empiece a usar los recursos en Azure Synapse, incurrirá en costos, que podrá ver en el [análisis de costos](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

Al usar el análisis de costos, puede ver los costos de Azure Synapse Analytics de diferentes intervalos de tiempo en forma de gráficos y tablas. Algunos ejemplos son: por día, mes actual y anterior y año. También puede ver los costos comparados con los presupuestos y los costos previstos. Con el tiempo, cambiar a vistas más largas puede ayudarle a identificar las tendencias de gasto y comprobar dónde este se ha sobrepasado. Si ha creado presupuestos, también podrá ver fácilmente dónde se han excedido.

Para ver los costos de Azure Synapse en el análisis de costos:

1. Inicie sesión en Azure Portal.
2. Abra el ámbito, ya sea la suscripción o el grupo de recursos, en Azure Portal y seleccione **Análisis de costos** en el menú. Por ejemplo, vaya a **Suscripciones**, seleccione una suscripción de la lista y, a continuación, seleccione **Análisis de costos** en el menú. Seleccione **Ámbito** para cambiar a otro ámbito del análisis de costos.
3. De forma predeterminada, el costo de los servicios se muestra en el primer gráfico de anillos. Seleccione el área del gráfico con la etiqueta "Azure Synapse".

Los costos mensuales reales se muestran cuando se abre inicialmente el análisis de costos. Este es un ejemplo con todos los costos mensuales de uso.

![Ejemplo que muestra los costos acumulados de una suscripción](./media/plan-manage-costs/actual-monthly-costs.png)

- Para limitar la información a los costos de un único servicio, como Azure Synapse, seleccione **Agregar filtro** y, luego, elija **Nombre del servicio**. Después, seleccione **Azure Synapse Analytics**.

Este es un ejemplo que muestra solo los costos de Azure Synapse.

![Ejemplo que muestra los costos acumulados del servicio](./media/plan-manage-costs/filtered-monthly-costs.png)

En el ejemplo anterior, hemos visto el costo actual del servicio. También se muestran los costos por regiones de Azure (ubicaciones) y los costos de Azure Synapse por grupo de recursos. A partir de aquí, puede explorar los costos por su cuenta.

## <a name="create-budgets"></a>Creación de presupuestos

Puede crear [presupuestos](https://docs.microsoft.com/azure/cost-management/tutorial-acm-create-budgets?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) para administrar los costos y crear [alertas](https://docs.microsoft.com/azure/cost-management/cost-mgt-alerts-monitor-usage-spending?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) que envíen notificaciones automáticamente a las partes interesadas sobre anomalías en los gastos y riesgos de gastos adicionales. Las alertas se basan en el gasto comparado con los umbrales de presupuesto y costo. Los presupuestos y las alertas se crean para las suscripciones y los grupos de recursos de Azure, por lo que son útiles como parte de una estrategia general de supervisión de costos. 

Los presupuestos se pueden crear con filtros para recursos o servicios específicos de Azure si quiere disponer de más granularidad en la supervisión. Los filtros ayudan a garantizar que no se crean accidentalmente recursos nuevos con un costo adicional. Para más información sobre las opciones de filtro al crear un presupuesto, consulte [Opciones de agrupación y filtrado](https://docs.microsoft.com/azure/cost-management-billing/costs/group-filter?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

## <a name="export-cost-data"></a>Exportación de datos de costos

También puede [exportar los datos de costos](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) a una cuenta de almacenamiento. Esto resulta útil cuando usted u otro usuario necesita hacer un análisis de datos adicional para los costos. Por ejemplo, un equipo de finanzas puede analizar los datos con Excel o Power BI. Puede exportar los costos en una programación diaria, semanal o mensual y establecer un intervalo de fechas personalizado. La exportación de los datos de costos es la forma recomendada de recuperar conjuntos de datos de costos.


## <a name="other-ways-to-manage-and-reduce-costs-for-azure-synapse"></a>Otras formas de administrar y reducir los costos de Azure Synapse 

### <a name="serverless-sql-pool"></a>Grupo de SQL sin servidor

Para más información sobre los costos del grupo SQL sin servidor, consulte [Administración de costos del grupo de SQL sin servidor en Azure Synapse Analytics](./sql/data-processed.md).

### <a name="dedicated-sql-pool"></a>Grupo de SQL dedicado

Para controlar los costos de un grupo de SQL dedicado, puede pausar el recurso cuando no se use. Por ejemplo, si no va a usar la base de datos durante la noche y los fines de semana, puede pausarla durante esas horas y reanudarla durante el día. Para obtener más información, consulte [Pausa y reanudación del proceso en un grupo de SQL dedicado mediante Azure Portal](./sql-data-warehouse/pause-and-resume-compute-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

<!-- ### Serverless Apache Spark pool -->

### <a name="data-integration---pipelines-and-data-flows"></a>Integración de datos: canalizaciones y flujos de datos 

Para obtener más información sobre el costo de la integración de datos, consulte [Planeamiento y administración de los costos de Azure Data Factory](../data-factory/plan-manage-costs.md)

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [optimizar su inversión en la nube con Azure Cost Management](https://docs.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Obtenga más información sobre la administración de costos con los [análisis de costos](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Obtenga información sobre cómo [evitar los costos inesperados](https://docs.microsoft.com/azure/cost-management-billing/manage/getting-started?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Haga el curso de aprendizaje guiado sobre [Cost Management](https://docs.microsoft.com/learn/paths/control-spending-manage-bills?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Obtenga información sobre cómo planear y administrar los costos de [Azure Machine Learning](../machine-learning/concept-plan-manage-cost.md).