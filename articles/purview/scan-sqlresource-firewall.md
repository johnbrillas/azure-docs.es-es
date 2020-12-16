---
title: Examen de una base de datos SQL de Azure detrás de un firewall
description: Aprenda a examinar recursos detrás de un firewall mediante el portal de Azure Purview.
author: sudhandkumar
ms.author: kumarsud
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/16/2020
ms.openlocfilehash: 4e1e76aaae2951e6d65fd1ebaaa5798ff417daf9
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96551101"
---
# <a name="scan-azure-sql-db-behind-a-firewall-in-azure-purview"></a>Examen de una base de datos SQL de Azure detrás de un firewall en Azure Purview

En este artículo, aprenderá a usar Azure Purview para examinar un recurso detrás de un firewall.

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Su propio [inquilino de Azure Active Directory](../active-directory/fundamentals/active-directory-access-create-new-tenant.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com) con su cuenta de Azure.

## <a name="set-up-sql-storage-behind-a-firewall"></a>Configuración del almacenamiento de SQL detrás de un firewall

El primer paso consiste en agregar la identidad administrada del catálogo a una instancia de Azure SQL Database mediante las instrucciones que se describen en [Registro y examen de una instancia de Azure SQL Database](register-scan-azure-sql-database.md).

## <a name="scan-sql-db-from-purview"></a>Examen de una base de datos SQL desde Purview

1. Vaya a la página principal de Purview.

1. En el panel de navegación izquierdo, seleccione **Management Center** (Centro de administración).

1. En **Sources and Scanning** (Orígenes y examen), seleccione **Data Sources** (Orígenes de datos).

1. Seleccione **+ New** (+ Nuevo) para agregar el origen de datos.

1. Seleccione **Azure SQL Database**.

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-source.png" alt-text="Captura de selección de SQL Server.":::

1. Proporcione un nombre para el nuevo origen de datos, seleccione **From Azure subscription** (Desde la suscripción de Azure) y elija la suscripción y el nombre del servidor en la lista desplegable.

   Seleccione **Finish** (Finalizar) para completar el registro. 

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-register.png" alt-text="Captura de pantalla de finalización de la selección":::

1. Seleccione **Set Up Scan** (Configurar examen) para el almacenamiento que está detrás del firewall y pruebe la conexión. La conexión indica que se ha realizado correctamente. 

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-setscan.png" alt-text="Captura de pantalla de selección de la configuración del examen t0.":::

1. Configure la frecuencia de detección y seleccione **Continue** (Continuar).

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-once.png" alt-text="Captura de pantalla de selección del inicio del examen de SQL.":::

1.  El examen finaliza y el estado se actualiza en la lista de orígenes de datos.

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-success.png" alt-text="Captura de pantalla del mensaje de examen finalizado":::
   
## <a name="next-steps"></a>Pasos siguientes

A continuación, puede configurar un conjunto de reglas de examen en [Creación de un conjunto de reglas de examen](create-a-scan-rule-set.md) para agrupar los exámenes.
