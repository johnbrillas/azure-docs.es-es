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
ms.date: 03/17/2021
ms.openlocfilehash: ee8a192bcfe20c77e47ee9c89dfc3286a979e358
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "104722300"
---
# <a name="creating-a-synapse-workspace"></a>Creación de un área de trabajo de Synapse

En este tutorial, aprenderá a crear un área de trabajo de Synapse, un grupo de SQL dedicado y un grupo de Apache Spark sin servidor. 

## <a name="prerequisites"></a>Prerrequisitos

Para completar los pasos de este tutorial, es preciso tener acceso a un grupo de recursos para el que tenga asignado el rol de **propietario**. Cree el área de trabajo de Synapse en este grupo de recursos.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Creación de un área de trabajo de Synapse en Azure Portal

### <a name="start-the-process"></a>Inicio del proceso
1. Abra [Azure Portal](https://portal.azure.com) y escriba **Synapse** en la barra de búsqueda sin pulsar entrar.
1. En los resultados de la búsqueda, en **Servicios**, seleccione **Azure Synapse Analytics**.
1. Seleccione **Agregar** para crear un área de trabajo.

## <a name="basics-tab--project-details"></a>Pestaña Información básica > Detalles del proyecto
1. En la pestaña **Básico**, rellene los campos siguientes en **Detalles del proyecto**:
      1. **Suscripción**: seleccione cualquier suscripción.
      2. **Grupo de recursos**: use cualquier grupo de recursos.
      3. **Grupo de recursos**: déjelo en blanco.


## <a name="basics-tab--workspace-details"></a>Pestaña Información básica > Detalles del área de trabajo
1. En la pestaña **Básico**, rellene los campos siguientes en **Detalles del área de trabajo**:
      1. **Nombre del área de trabajo**: seleccione cualquier nombre que sea globalmente único. Para este tutorial usaremos **myworkspace**.
      1. **Región**: seleccione cualquier región.
      1. **Selección de Data Lake Storage Gen2**
        1. Haga clic en el botón **De la suscripción**.
        1. Junto a **Nombre de cuenta**, haga clic en **Crear nuevo** y asigne a la nueva cuenta de almacenamiento el nombre **contosolake** o uno similar, ya que este nombre debe ser único.
        1. Junto a **Nombre del sistema de archivos**, haga clic en **Crear nuevo** y asigne el nombre **users**. Se creará un contenedor de almacenamiento llamado **users**. El área de trabajo usará esta cuenta de almacenamiento como cuenta de almacenamiento "principal" para las tablas de Spark y los registros de aplicaciones de Spark.
        1. Active la casilla "Es necesario asignarme el rol Colaborador de datos de Storage Blob en la cuenta de Data Lake Storage Gen2". 

### <a name="completing-the-process"></a>Finalización del proceso
5. Seleccione **Revisar y crear** > **Crear**. El área de trabajo estará lista en unos minutos.

> [!NOTE]
> Para habilitar las características del área de trabajo de un grupo de SQL dedicado existente (anteriormente SQL DW), consulte [Habilitación de un área de trabajo para el grupo de SQL dedicado (anteriormente SQL DW)](./sql-data-warehouse/workspace-connected-create.md).


## <a name="open-synapse-studio"></a>Abrir Synapse Studio

Una vez creada el área de trabajo de Azure Synapse, hay dos maneras de abrir Synapse Studio:

* Abra el área de trabajo de Synapse en [Azure Portal](https://portal.azure.com) y, en la sección **Información general** del área de trabajo de Synapse, seleccione **Abrir** en el cuadro Abrir Synapse Studio.
* Vaya a `https://web.azuresynapse.net` e inicie sesión en el área de trabajo.











## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis de datos con un grupo de SQL sin servidor](get-started-analyze-sql-on-demand.md)
