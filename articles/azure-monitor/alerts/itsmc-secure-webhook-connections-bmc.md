---
title: 'Conector de Administración de servicios de TI: exportación segura en Azure Monitor y configuración con BMC'
description: En este artículo se muestra cómo conectar los productos o servicios de ITSM con BMC en Exportación segura de Azure Monitor.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/31/2020
ms.openlocfilehash: 7c50d533f95e1131e26ddd808c77023ae4591a26
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102041712"
---
# <a name="connect-bmc-helix-to-azure-monitor"></a>Conexión de BMC Helix a Azure Monitor

En las secciones siguientes se proporcionan detalles sobre cómo conectar el producto de BMC Helix y Exportación segura en Azure.

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que se cumplen los requisitos previos siguientes:

* Azure AD está registrado.
* Tiene la versión admitida de BMC Helix Multi-Cloud Service Management (19.08 o posterior).

## <a name="configure-the-bmc-helix-connection"></a>Configuración de la conexión de BMC Helix

1. Siga el procedimiento indicado a continuación en el entorno de BMC Helix a fin de obtener el URI para la exportación segura:

   1. Inicie sesión en Integration Studio.
   1. Busque el flujo de **Create Incident from Azure Alerts** (Creación de incidente a partir de alertas de Azure).
   1. Copie la dirección URL del webhook.
   
   ![Captura de pantalla de la dirección URL del webhook en Integration Studio.](media/itsmc-secure-webhook-connections-bmc/bmc-url.png)
   
2. Siga las instrucciones correspondientes a la versión:
   * [Habilitación de la integración precompilada con Azure Monitor para la versión 20.02](https://docs.bmc.com/docs/multicloud/enabling-prebuilt-integration-with-azure-monitor-879728195.html).
   * [Habilitación de la integración precompilada con Azure Monitor para la versión 19.11](https://docs.bmc.com/docs/multicloudprevious/enabling-prebuilt-integration-with-azure-monitor-904157623.html).

3. Como parte de la configuración de la conexión en BMC Helix, vaya a la instancia de BMC de integración y siga estas instrucciones:

   1. Seleccione **Catálogo**.
   2. Seleccione **Alertas de Azure**.
   3. Seleccione **Conectores**.
   4. Seleccione **Configuración**.
   5. Seleccione la configuración **Agregar nueva conexión**.
   6. Rellene la información de la sección de configuración:
      - **Name**: cree uno propio.
      - **Tipo de autorización**: **NONE**
      - **Descripción**: cree uno propio.
      - **Sitio**: **Cloud**
      - **Número de instancias**: **2**, el valor predeterminado.
      - **Comprobación**: seleccionado de manera predeterminada para habilitar el uso.
      - El id. de aplicación de Azure y el id. de inquilino de Azure se toman de la aplicación que definió anteriormente.

![Captura de pantalla que muestra la configuración de BMC.](media/itsmc-secure-webhook-connections-bmc/bmc-configuration.png)
