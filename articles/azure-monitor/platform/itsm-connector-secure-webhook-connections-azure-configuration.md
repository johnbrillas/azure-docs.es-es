---
title: 'Conector de Administración de servicios de TI: exportación segura en Azure Monitor y configuraciones de Azure'
description: En este artículo se muestra cómo configurar Azure para conectar los productos o servicios de ITSM con Exportación segura de Azure Monitor para supervisar y administrar de manera centralizada elementos de trabajo de ITSM.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 01/03/2021
ms.openlocfilehash: 03a16bfbcb3a8d46a6cb4faa03aa6b6e96cf3db3
ms.sourcegitcommit: 16887168729120399e6ffb6f53a92fde17889451
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98165967"
---
# <a name="configure-azure-to-connect-itsm-tools-using-secure-export"></a>Configuración de Azure para conectar herramientas de ITSM mediante Exportación segura

En este artículo se proporciona información sobre cómo configurar Azure para usar "Exportación segura".
Para usar "Exportación segura", siga estos pasos:

1. [Registrar la aplicación con Azure AD.](./itsm-connector-secure-webhook-connections-azure-configuration.md#register-with-azure-active-directory)
1. [Definir una entidad de servicio.](./itsm-connector-secure-webhook-connections-azure-configuration.md#define-service-principal)
1. [Crear un grupo de acciones Webhook seguro.](./itsm-connector-secure-webhook-connections-azure-configuration.md#create-a-secure-webhook-action-group)
1. Configurar el entorno del asociado.
    Exportación segura admite conexiones con las siguientes herramientas ITSM:
    * [ServiceNow](./itsmc-secure-webhook-connections-servicenow.md)
    * [BMC Helix](./itsmc-secure-webhook-connections-bmc.md)

## <a name="register-with-azure-active-directory"></a>Registro con Azure Active Directory

Siga estos pasos para registrar la aplicación con Azure AD:

1. Siga los pasos que aparecen en [Registro de una aplicación en la plataforma de identidad de Microsoft](../../active-directory/develop/quickstart-register-app.md).
2. En Azure AD, seleccione **Exponer aplicación**.
3. Seleccione **Establecer** para **URI de id. de aplicación**.

   [![Captura de pantalla de la opción para establecer el identificador URI del id. de la aplicación.](media/it-service-management-connector-secure-webhook-connections/azure-ad.png)](media/it-service-management-connector-secure-webhook-connections/azure-ad-expand.png#lightbox)
4. Seleccione **Guardar**.

## <a name="define-service-principal"></a>Definición de la entidad de servicio

El servicio Grupo de acciones es una aplicación de primera entidad, por lo que tiene permiso para adquirir tokens de autenticación de la aplicación de AAD con el fin de autenticarse en el servicio ahora.
Como paso opcional, puede definir el rol de la aplicación en el manifiesto de la aplicación creada, lo que le permite restringir aún más el acceso de forma que solo determinadas aplicaciones con ese rol específico puedan enviar mensajes. Este rol debe asignarse a la entidad de servicio de Grupo de acciones (requiere privilegios de administrador de inquilinos).

Este paso se puede realizar con los mismos [comandos de PowerShell](./action-groups.md#secure-webhook-powershell-script).

## <a name="create-a-secure-webhook-action-group"></a>Creación de un grupo de acciones Webhook seguro

Una vez registrada la aplicación con Azure AD, puede crear elementos de trabajo en la herramienta de ITSM en función de las alertas de Azure mediante el uso de la acción de webhook seguro en grupos de acciones.

Los grupos de acciones proporcionan una manera modular y reutilizable de desencadenar acciones para las alertas de Azure. Puede usar los grupos de acciones con alertas de métricas, de registros de actividad y de Azure Log Analytics en Azure Portal.
Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones en Azure Portal](./action-groups.md).

Para agregar un webhook a una acción, siga estas instrucciones para Webhook seguro:

1. En [Azure Portal](https://portal.azure.com/), busque y seleccione **Monitor**. El panel **Monitor** consolida todas las opciones de configuración y todos los datos de supervisión en una vista.
2. Seleccione **Alertas** > **Administrar acciones**.
3. Seleccione [Agregar grupo de acciones](./action-groups.md#create-an-action-group-by-using-the-azure-portal) y rellene los campos.
4. Escriba un nombre en el cuadro de texto **Nombre del grupo de acciones** y especifique un nombre en el cuadro de texto **Nombre corto**. El nombre corto se utiliza en lugar del nombre completo del grupo de acciones cuando se envían notificaciones mediante este grupo.
5. Seleccione **Webhook seguro**.
6. Seleccione estos detalles:
   1. Seleccione el id. de objeto de la instancia de Azure Active Directory que registró.
   2. En el URI, pegue la dirección URL del webhook que copió del [entorno de la herramienta ITSM](#configure-the-itsm-tool-environment).
   3. Establezca **Habilitar el esquema de alerta común** en **Sí**. 

   En la imagen siguiente se muestra la configuración de una acción de webhook seguro de ejemplo:

   ![Captura de pantalla que muestra una acción de webhook segura.](media/it-service-management-connector-secure-webhook-connections/secure-webhook.png)

## <a name="configure-the-itsm-tool-environment"></a>Configuración del entorno de la herramienta ITSM

La configuración contiene dos pasos:

1. Obtención del URI de la definición de exportación segura.
2. Definiciones según el flujo de la herramienta ITSM.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de Exportación segura de ServiceNow](./itsmc-secure-webhook-connections-servicenow.md)
* [Configuración de Exportación segura de BMC](./itsmc-secure-webhook-connections-bmc.md)
