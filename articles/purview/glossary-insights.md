---
title: Informes de glosario sobre los datos con información detallada de Purview
description: En esta guía paso a paso se describe cómo ver y usar los informes de glosario con información detallada de Purview sobre los datos.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/20/2020
ms.openlocfilehash: f61d99a61cb50886d70489b586d948bfa751e196
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96576781"
---
# <a name="glossary-insights-on-your-data-in-azure-purview"></a>Información detallada de glosario sobre los datos de Azure Purview

En esta guía paso a paso se describe cómo acceder a los informes de información detallada de glosario de Purview en los datos, y cómo verlos y filtrarlos.

En esta guía paso a paso, aprenderá a:

> [!div class="checklist"]
> - Consultar información detallada desde la cuenta de Purview.
> - Obtener una visión general de los datos.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar a trabajar con la información detallada de Purview, asegúrese de que ha completado los pasos siguientes:

- Ha configurado los recursos de Azure y ha rellenado la cuenta con datos.

- Ha configurado y realizado un examen en el tipo de origen.

- Ha configurado un glosario y asociado recursos a los términos del glosario.

Para más información, consulte [Administración de orígenes de datos en Azure Purview (versión preliminar)](manage-data-sources.md).

## <a name="use-purview-glossary-insights"></a>Uso de información detallada de glosario de Purview

En Azure Purview, puede crear términos de glosario y asociarlos a los recursos. Más adelante, puede ver la distribución del glosario en la información detallada del glosario. Esto le indica el estado del glosario según los términos asociados a los recursos. También indica los términos por estado y la distribución de los roles por número de usuarios.

**Visualización de información detallada de glosarios:**

1. Vaya a la [pantalla de la instancia de **Azure Purview** en Azure Portal](https://aka.ms/purviewportal) y seleccione su cuenta de Purview.

1. En la página **Información general**, en la sección **Inicio**, seleccione el icono **Launch Purview account** (Iniciar cuenta de Purview).

   :::image type="content" source="./media/glossary-insights/portal-access.png" alt-text="Inicio de Purview desde Azure Portal":::

1. En la página **Home** (Inicio) de Purview, seleccione el icono **View insights** (Ver información) para acceder al área **Insights** :::image type="icon" source="media/glossary-insights/ico-insights.png" border="false"::: (Información detallada).

   :::image type="content" source="./media/glossary-insights/view-insights.png" alt-text="Visualización de información detallada en Azure Portal":::

1. En el área **Insights** (Información detallada) :::image type="icon" source="media/glossary-insights/ico-insights.png" border="false":::, seleccione **Glossary** (Glosario) para mostrar el informe **Glossary insights** (Información detallada de glosario).

La página **Glossary Insights** (Información detallada de glosario) muestra las áreas siguientes:
1. **High level KPIs** (KPI de alto nivel) para mostrar términos del glosario y usuarios del catálogo.

2. **Top glossary terms and count of assets** (Términos principales del glosario y recursos totales) muestra los cinco términos principales del glosario con los recursos asociados a ellos. Todos los demás recursos se contabilizan en la categoría "Other" (Otros) del gráfico.

3. **Glossary terms by term status** (Términos del glosario por estado) muestra la distribución de los términos del glosario por estado, como "Draft" (Borrador), "Approved" (Aprobado), "Alert" (Alerta) y "Expired" (Expirado). 

1. Mantenga el puntero o haga clic en el segmento del gráfico de un estado y anote el total de términos con ese estado.

1. **Distribution of roles by number of users** (Distribución de roles por número de usuarios) muestra la distribución de los roles por número de usuarios por rol en Purview.

   :::image type="content" source="./media/glossary-insights/glossary-insights1.png" alt-text="Visualización de información de glosarios":::

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los informes de información detallada de Azure Purview con [Información detallada de recursos](./asset-insights.md)