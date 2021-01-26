---
title: 'Inicio rápido: Introducción a la creación de un área de trabajo de Synapse'
description: En este tutorial, aprenderá a crear un área de trabajo de Synapse, un grupo de SQL dedicado y un grupo de Apache Spark sin servidor.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: 3a2636ec73d20f3011d8413c794e68ef41b1829c
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2021
ms.locfileid: "98209192"
---
# <a name="creating-a-synapse-workspace"></a>Creación de un área de trabajo de Synapse

En este tutorial, aprenderá a crear un área de trabajo de Synapse, un grupo de SQL dedicado y un grupo de Apache Spark sin servidor. 

## <a name="prerequisites"></a>Prerrequisitos

Para completar los pasos de este tutorial, es preciso tener acceso a un grupo de recursos para el que tenga asignado el rol de **propietario**. Cree el área de trabajo de Synapse en este grupo de recursos.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Creación de un área de trabajo de Synapse en Azure Portal

1. Abra [Azure Portal](https://portal.azure.com) y, en la parte superior, busque **Synapse**.
1. En los resultados de la búsqueda, en **Servicios**, seleccione **Azure Synapse Analytics**.
1. Seleccione **Agregar** para crear un área de trabajo.
1. En la pestaña **Aspectos básicos**, en Detalles del proyecto, escriba los valores de **Suscripción**, **Grupo de recursos** y **Región** que prefiera y elija un nombre para el área de trabajo. Para este tutorial usaremos **myworkspace**.
1. Junto a **Select Data Lake Storage Gen 2** (Seleccionar Data Lake Storage Gen 2), haga clic en el botón de **From subscription** (De la suscripción).
1. Junto a **Nombre de cuenta**, haga clic en **Crear nuevo** y asigne a la nueva cuenta de almacenamiento el nombre **contosolake** o uno similar, ya que este nombre debe ser único.
1. Junto a **Nombre del sistema de archivos**, haga clic en **Crear nuevo** y asigne el nombre **users**. Se creará un contenedor de almacenamiento denominado **users**
1. El área de trabajo usará esta cuenta de almacenamiento como cuenta de almacenamiento "principal" para las tablas de Spark y los registros de aplicaciones de Spark.
1. Active la casilla "Es necesario asignarme el rol Colaborador de datos de Storage Blob en la cuenta de Data Lake Storage Gen2". 
1. Seleccione **Revisar y crear** > **Crear**. El área de trabajo estará lista en unos minutos.

> [!NOTE]
> Para habilitar las características del área de trabajo de un grupo de SQL dedicado existente (anteriormente SQL DW), consulte [Habilitación de un área de trabajo para el grupo de SQL dedicado (anteriormente SQL DW)](./sql-data-warehouse/workspace-connected-create.md).


## <a name="open-synapse-studio"></a>Abrir Synapse Studio

Una vez creada el área de trabajo de Azure Synapse, hay dos maneras de abrir Synapse Studio:

* Abra el área de trabajo de Synapse en [Azure Portal](https://portal.azure.com) y, en la sección **Información general** del área de trabajo de Synapse, seleccione **Abrir** en el cuadro Abrir Synapse Studio.
* Vaya a `https://web.azuresynapse.net` e inicie sesión en el área de trabajo.


## <a name="the-built-in-serverless-sql-pool"></a>El grupo de SQL sin servidor integrado

Todas las áreas de trabajo incluyen un grupo de SQL sin servidor prediseñado llamado **Built-in**. Este grupo no se puede eliminar. Los grupos de SQL sin servidor permiten usar SQL sin necesidad de reservar capacidad con grupos de SQL dedicados. A diferencia de los grupos de SQL dedicados, la facturación de los grupos de SQL sin servidor se basa en la cantidad de datos que se examinan para ejecutar la consulta (no en la capacidad asignada al grupo).


## <a name="create-a-dedicated-sql-pool"></a>Creación de un grupo de SQL dedicado

1. En Synapse Studio, en el panel izquierdo, seleccione **Administrar** > **Grupos de SQL**.
1. Seleccione **Nuevo paso**.
1. En **Nombre del grupo de SQL** seleccione **SQLPOOL1**
1. En **Nivel de rendimiento** seleccione **DW100C**
1. Seleccione **Revisar y crear** > **Crear**. El grupo de SQL dedicado estará listo en unos minutos. El grupo de SQL dedicado se asocia con una base de datos del grupo de SQL dedicado, que también se denomina **SQLPOOL1**.

Los grupos de SQL dedicados consumen recursos facturables mientras están activos. Puede pausar los grupos más adelante para reducir los costos.

> [!NOTE] 
> Al crear un nuevo grupo de SQL dedicado (anteriormente SQL DW) en el área de trabajo, se abrirá la página de aprovisionamiento del grupo de SQL dedicado. El aprovisionamiento se llevará a cabo en el servidor SQL lógico.


## <a name="create-a-serverless-apache-spark-pool"></a>Crear un grupo de Apache Spark sin servidor

1. En Synapse Studio, en el lado izquierdo, seleccione **Administrar** > **Grupos de Apache Spark**.
1. Seleccione **Nuevo paso**. 
1. En **Nombre del grupo de Apache Spark** escriba **Spark1**.
1. En **Tamaño del nodo** escriba **Pequeño**.
1. En **Número de nodos**, establezca el valor mínimo en 3 y el máximo en 3.
1. Seleccione **Revisar y crear** > **Crear**. El grupo de Apache Spark estará listo en unos segundos.

El grupo de Spark indica a Azure Synapse el número de recursos de Spark que se van a usar. Solo paga por los recursos que utiliza. Al dejar de usar el grupo de forma activa, los recursos agotarán el tiempo de espera y se reciclarán automáticamente.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis mediante un grupo de SQL dedicado](get-started-analyze-sql-pool.md)
