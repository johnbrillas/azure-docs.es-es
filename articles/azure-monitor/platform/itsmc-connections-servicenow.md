---
title: Conexión de ServiceNow al Conector de Administración de servicios de TI
description: En este artículo, se proporciona información sobre cómo conectar ServiceNow al Conector de Administración de servicios de TI (ITSMC) en Azure Monitor para supervisar y administrar de forma centralizada los elementos de trabajo de ITSM.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 662c36e4f0082c376a6e250e9a0885f0cd225964
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2020
ms.locfileid: "97729563"
---
# <a name="connect-servicenow-with-it-service-management-connector"></a>Conexión de ServiceNow al Conector de Administración de servicios de TI

En este artículo se proporciona información sobre cómo configurar la conexión entre la instancia de ServiceNow y el Conector de Administración de servicios de TI (ITSMC) en Log Analytics para administrar de forma centralizada los elementos de trabajo.

En las secciones siguientes se proporcionan detalles sobre cómo conectar su producto de ServiceNow a ITSMC en Azure.

## <a name="prerequisites"></a>Prerrequisitos
Asegúrese de que se cumplen los siguientes requisitos previos:
- ITSMC instalado Más información: [Agregar la solución IT Service Management Connector](./itsmc-definition.md#add-it-service-management-connector).
- Versiones compatibles con ServiceNow: Orlando, Nueva York, Madrid, Londres, Kingston, Yakarta, Estambul, Helsinki, Ginebra.
- Actualmente, las alertas que se envían desde Azure Monitor pueden crear en ServiceNow uno de los siguientes elementos: eventos, incidentes o alertas.
> [!NOTE]
> ITSMC admite solo la oferta de SaaS oficial de ServiceNow. No se admiten implementaciones privadas de ServiceNow. 

**Los administradores de ServiceNow deben realizar lo siguiente en la instancia de ServiceNow:** :
- Generar el identificador y el secreto del cliente para el producto de ServiceNow. Para más información sobre cómo generar el identificador y el secreto de cliente, consulte la siguiente información según sea necesario:

    - [Configuración de OAuth para Orlando](https://docs.servicenow.com/bundle/orlando-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Configuración de OAuth para Nueva York](https://docs.servicenow.com/bundle/newyork-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Configuración de OAuth para Madrid](https://docs.servicenow.com/bundle/madrid-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Configuración de OAuth para Londres](https://docs.servicenow.com/bundle/london-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Configuración de OAuth para Kingston](https://docs.servicenow.com/bundle/kingston-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Configuración de OAuth para Yakarta](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Configuración de OAuth para Estambul](https://docs.servicenow.com/bundle/istanbul-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Configuración de OAuth para Helsinki](https://docs.servicenow.com/bundle/helsinki-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Configuración de OAuth para Ginebra](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/security/task/t_SettingUpOAuth.html)
> [!NOTE]
> Como parte de la definición de "Configuración de OAuth", se recomienda:
>
> 1) **Actualizar la duración del token de actualización a 90 días (7 776 000 segundos).** Como parte de la [Configuración de OAuth](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.servicenow.com%2Fbundle%2Fnewyork-platform-administration%2Fpage%2Fadminister%2Fsecurity%2Ftask%2Ft_SettingUpOAuth.html&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C2c6812e429a549e71cdd08d7d1b148d8%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637208431696739125&sdata=Q7mF6Ej8MCupKaEJpabTM56EDZ1T8vFVyihhoM594aA%3D&reserved=0) en la fase 2: [Crear un punto de conexión para que los clientes accedan a la instancia](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.servicenow.com%2Fbundle%2Fnewyork-platform-administration%2Fpage%2Fadminister%2Fsecurity%2Ftask%2Ft_CreateEndpointforExternalClients.html&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C2c6812e429a549e71cdd08d7d1b148d8%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637208431696749123&sdata=hoAJHJAFgUeszYCX1Q%2FXr4N%2FAKiFcm5WV7mwR2UqeWA%3D&reserved=0) Después de la definición del punto de conexión, en la hoja ServiceNow, busque OAuth del sistema y, a continuación, seleccione Registro de aplicaciones. Seleccione el nombre de OAuth que se definió y actualice el campo de duración del token de actualización a 7 776 000 (90 días expresados en segundos).
> Finalmente, haga clic en Actualizar.
> 2) **Se recomienda establecer un procedimiento interno para asegurarse de que la conexión permanezca activa** según la duración del token de actualización para actualizar el token. Asegúrese de realizar las siguientes operaciones antes de la fecha de expiración esperada del token de actualización (se recomienda un par de días antes de que expire la duración del token de actualización):
>
>     1. [Completar un proceso de sincronización manual para la configuración del conector de ITSM](./itsmc-resync-servicenow.md)
>     2. Revocar el token de actualización anterior, ya que no se recomienda mantener las claves antiguas por motivos de seguridad. En la hoja ServiceNow, busque System OAuth (OAuth del sistema) y seleccione Manage Tokens (Administrar tokens). Elija el token antiguo de la lista según el nombre de OAuth y la fecha de expiración.
> ![Definición de OAuth del sistema de nieve](media/itsmc-connections/snow-system-oauth.png)
>     3. Haga clic en Revoke Access (Revocar acceso) y, a continuación, en Revoke (Revocar).

- Instalar la aplicación de usuario para la integración de Microsoft Log Analytics (aplicación de ServiceNow). [Más información](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1 ).
> [!NOTE]
> ITSMC solo es compatible con la aplicación de usuario oficial para la integración de Microsoft Log Analytics que se descarga del almacén de ServiceNow. ITSMC no admiten la ingesta de código en el lado de ServiceNow ni la aplicación que no forma parte de la solución oficial de ServiceNow. 
- Crear el rol de usuario de integración para la aplicación de usuario instalada. [Aquí](#create-integration-user-role-in-servicenow-app) puede encontrar más información sobre cómo crear el rol de usuario de integración.

## <a name="connection-procedure"></a>**Procedimiento de conexión**
Use el procedimiento siguiente para crear una nueva conexión a ServiceNow.


1. En Azure Portal, vaya a **Todos los recursos** y busque **ServiceDesk(YourWorkspaceName)** .

2.  En **ORÍGENES DE DATOS DEL ÁREA DE TRABAJO**, haga clic en **Conexiones de ITSM**.
    ![Nueva conexión](media/itsmc-connections/add-new-itsm-connection.png)

3. En la parte superior del panel derecho, haga clic en **Agregar**.

4. Proporcione la información tal como se describe en la tabla siguiente y haga clic en **Aceptar** para crear la conexión.


> [!NOTE]
> Todos estos parámetros son obligatorios.

| **Campo** | **Descripción** |
| --- | --- |
| **Nombre de la conexión**   | Escriba un nombre para la instancia de ServiceNow que quiere conectar con ITSMC.  Usará este nombre más adelante en Log Analytics cuando configure los elementos de trabajo en el análisis de registros detallados de ITSM o vista. |
| **Tipo de asociado**   | Seleccione **ServiceNow**. |
| **Nombre de usuario**   | Escriba el nombre de usuario de integración que ha creado en la aplicación de ServiceNow para que admita la conexión con ITSMC. Más información: [Creación de un rol de usuario de aplicación de ServiceNow](#create-integration-user-role-in-servicenow-app).|
| **Contraseña**   | Escriba la contraseña asociada con este nombre de usuario. **Nota**: El nombre de usuario y la contraseña se utilizan para generar únicamente tokens de autenticación y no se almacenan en ningún lugar dentro del servicio ITSMC.  |
| **Dirección URL del servidor**   | Escriba la dirección URL de la instancia de ServiceNow que quiere conectar con ITSMC. La dirección URL debe apuntar a una versión de SaaS compatible con el sufijo ".servicenow.com".|
| **Id. de cliente**   | Escriba el identificador de cliente que desea utilizar para la autenticación de OAuth2 y que ha generado anteriormente.  Para más información acerca de cómo generar el identificador y el secreto del cliente:   [Configuración de OAuth](https://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Secreto de cliente**   | Escriba el secreto de cliente generado para este identificador.   |
| **Ámbito de sincronización de datos**   | Seleccione los elementos de trabajo de ServiceNow que quiere sincronizar con Azure Log Analytics mediante ITSMC.  Los valores seleccionados se importan en Log Analytics.   **Opciones:**  incidentes y solicitudes de cambio.|
| **Sincronización de datos** | Escriba el número de días pasados de los que desea los datos. **Límite máximo**: 120 días. |
| **Creación de un elemento de configuración de solución ITSM** | Seleccione esta opción si desea crear los elementos de configuración en el producto ITSM. Cuando se selecciona, ITMSC crea los elementos de configuración afectados como elementos de configuración (en el caso de que no existan) en el sistema ITSM compatible. **Valor predeterminado**: deshabilitado. |

![Conexión de ServiceNow](media/itsmc-connections/itsm-connection-servicenow-connection-latest.png)

**Cuando se ha conectado y sincronizado correctamente**:

- Los elementos de trabajo seleccionados en la instancia de ServiceNow se importan en Azure **Log Analytics**. Puede ver el resumen de estos elementos de trabajo en el icono de IT Service Management Connector.

- Puede crear incidentes a partir de alertas de Log Analytics, de entradas de registros o de alertas de Azure en esta instancia de ServiceNow.

> [!NOTE]
> ServiceNow tiene un límite de frecuencia para las solicitudes por hora. Para configurar dicho límite, defina el valor de "Limitación de velocidad de la API REST de entrada" en la instancia de ServiceNow.

## <a name="create-integration-user-role-in-servicenow-app"></a>Creación de un rol de usuario de integración de aplicación de ServiceNow

Utilice el siguiente procedimiento:

1. Visite la [tienda de ServiceNow](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1) e instale la **aplicación de usuario para la integración de OMS de Microsoft y ServiceNow** en la instancia de ServiceNow.
   
   >[!NOTE]
   >Como parte de la transición continuada de Microsoft Operations Management Suite (OMS) a Azure Monitor, OMS ahora se conoce como Log Analytics.     
2. Después de la instalación, visite la barra de navegación izquierda de la instancia de ServiceNow, y busque y seleccione el integrador de OMS de Microsoft.  
3. Haga clic en **Installation Checklist** (Lista de comprobación de instalación).

   El estado se muestra como **Not complete** (Sin completar) si aún no se creó el rol de usuario.

4. En los cuadros de texto, junto a **Create integration user** (Crear usuario de integración), escriba el nombre de usuario para el usuario que puede conectarse a ITSMC en Azure.
5. Escriba la contraseña para este usuario y haga clic en **Aceptar**.  

> [!NOTE]
> Utilice estas credenciales para realizar la conexión de ServiceNow en Azure.

El usuario recién creado se muestra con los roles predeterminados asignados.

**Roles predeterminados**:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.User
-   itil
-   template_editor
-   view_changer

Cuando el usuario se crea correctamente, el estado de **Check Installation Checklist** (Comprobar lista de comprobación de instalación) pasa a Completed (Completado) y se muestran los detalles del rol de usuario creado para la aplicación.

> [!NOTE]
> El Conector ITSM puede enviar incidentes a ServiceNow sin otros módulos instalados en la instancia de ServiceNow. Si usa el módulo EventManagement en su instancia de ServiceNow y quiere crear eventos o alertas en ServiceNow mediante el conector, agregue los siguientes roles para el usuario de integración:
> 
>    - evt_mgmt_integration
>    - evt_mgmt_operator  


## <a name="next-steps"></a>Pasos siguientes

* [Introducción al Conector de Administración de servicios de TI](itsmc-overview.md)
* [Creación de elementos de trabajo de ITSM a partir de alertas de Azure](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [Solución de problemas del Conector de Administración de servicios de TI](./itsmc-resync-servicenow.md)