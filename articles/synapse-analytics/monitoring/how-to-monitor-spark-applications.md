---
title: Supervisión de aplicaciones de Apache Spark en Synapse Studio
description: Obtenga información sobre cómo supervisar las aplicaciones de Apache Spark mediante Synapse Studio.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 11/30/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 5f9733866e85d79bdb85b8a24d1878e1169c2479
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97586047"
---
# <a name="how-to-use-synapse-studio-to-monitor-your-apache-spark-applications"></a>Uso de Synapse Studio para supervisar sus aplicaciones de Apache Spark

Con Azure Synapse Analytics, puede usar Spark para ejecutar cuadernos, trabajos y otros tipos de aplicaciones en los grupos de Spark en su área de trabajo.

En este artículo se explica cómo supervisar las aplicaciones Apache Spark, lo que le permite vigilar el estado más reciente, los problemas y el progreso.

## <a name="access-apache-spark-applications-list"></a>Acceso a la lista de aplicaciones de Apache Spark

Para ver la lista de aplicaciones de Apache Spark en el área de trabajo, primero [abra Synapse Studio](https://web.azuresynapse.net/) y seleccione el área de trabajo.

![Inicio de sesión en el área de trabajo](./media/common/login-workspace.png)

Una vez que haya abierto el área de trabajo, seleccione la sección **Supervisar** a la izquierda.

![Selección del centro Supervisar](./media/common/left-nav.png)

Seleccione **Apache Spark applications** (Aplicaciones Apache Spark) para ver la lista de aplicaciones Spark.

 ![Selección de aplicaciones Spark](./media/how-to-monitor-spark-applications/monitor-hub-nav-spark-applications.png)

## <a name="filter-your-apache-spark-applications"></a>Filtrado de las aplicaciones de Apache Spark

Puede filtrar la lista de aplicaciones Apache Spark por las que le interesan. Los filtros en la parte superior de la pantalla le permiten especificar un campo por el que desea filtrar.

Por ejemplo, puede filtrar la vista para ver solo las aplicaciones Apache Spark que contienen el nombre "sales":

![Filtro de ejemplo](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="view-details-about-a-specific-apache-spark-application"></a>Visualización de detalles sobre una aplicación de Apache Spark específica

Para ver los detalles de una de las aplicaciones Apache Spark, seleccione la aplicación y vea los detalles. Si la aplicación de Apache Spark todavía está en ejecución, puede supervisar el progreso. [Más información](apache-spark-applications.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la supervisión de ejecuciones de canalización, consulte el artículo [Supervisión de ejecuciones de canalización en Synapse Studio](how-to-monitor-pipeline-runs.md). 

Para más información sobre la depuración de la aplicación de Apache Spark, consulte el artículo sobre la [supervisión de aplicaciones de Apache Spark en Synapse Studio](apache-spark-applications.md).