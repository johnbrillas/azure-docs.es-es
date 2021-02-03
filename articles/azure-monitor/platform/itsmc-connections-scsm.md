---
title: Conexión de SCSM al Conector de Administración de servicios de TI
description: En este artículo, se proporciona información sobre cómo conectar SCSM al Conector de Administración de servicios de TI (ITSMC) en Azure Monitor para supervisar y administrar de forma centralizada los elementos de trabajo de ITSM.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 907a78b15cca4718308f79bc6be2e6258bc04d19
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99492678"
---
# <a name="connect-system-center-service-manager-with-it-service-management-connector"></a>Conexión de System Center Service Manager al Conector de Administración de servicios de TI

En este artículo se proporciona información sobre cómo configurar la conexión entre la instancia de System Center Service Manager y el Conector de Administración de servicios de TI (ITSMC) en Log Analytics para administrar de forma centralizada los elementos de trabajo.

En las secciones siguientes se proporcionan detalles sobre cómo conectar su producto de System Center Service Manager con ITSMC en Azure.

## <a name="prerequisites"></a>Prerrequisitos

Asegúrese de que se cumplen los siguientes requisitos previos:

- ITSMC instalado Más información: [Agregar la solución IT Service Management Connector](./itsmc-definition.md).
- Se implementa y se configura la aplicación web de Service Manager (aplicación web). [Aquí](#create-and-deploy-service-manager-web-app-service) se puede obtener información sobre la aplicación web.
- Conexión híbrida creada y configurada Más información: [Configuración de la conexión híbrida](#configure-the-hybrid-connection).
- Versiones admitidas de Service Manager:  2012 R2 o 2016.
- Rol de usuario:  [operador avanzado](/previous-versions/system-center/service-manager-2010-sp1/ff461054(v=technet.10)).
- Actualmente, las alertas que se envían desde Azure Monitor se pueden crear en incidentes de System Center Service Manager.

> [!NOTE]
> - El conector de ITSM solo se puede conectar a instancias de ServiceNow basadas en la nube. Actualmente no se admiten instancias locales de ServiceNow.
> - Para usar [plantillas](./itsmc-definition.md#define-a-template) personalizadas como parte de las acciones, se debe asignar el parámetro "ProjectionType" de la plantilla SCSM a "IncidentManagement!System.WorkItem.Incident.ProjectionType".

## <a name="connection-procedure"></a>Procedimiento de conexión

Use el siguiente procedimiento para conectar la instancia de System Center Service Manager con ITSMC:

1. En Azure Portal, vaya a **Todos los recursos** y busque **ServiceDesk(YourWorkspaceName)** .

2. En **ORÍGENES DE DATOS DEL ÁREA DE TRABAJO**, haga clic en **Conexiones de ITSM**.

    ![Nueva conexión](media/itsmc-connections/add-new-itsm-connection.png)

3. En la parte superior del panel derecho, haga clic en **Agregar**.

4. Proporcione la información tal como se describe en la tabla siguiente y haga clic en **Aceptar** para crear la conexión.

> [!NOTE]
> Todos estos parámetros son obligatorios.

| **Campo** | **Descripción** |
| --- | --- |
| **Nombre de la conexión**   | Escriba un nombre para la instancia de System Center Service Manager que quiere conectar con ITSMC.  Usará este nombre más adelante cuando configure los elementos de trabajo en el análisis de registros detallados de la instancia o vista. |
| **Tipo de asociado**   | Seleccione **System Center Service Manager**. |
| **Dirección URL del servidor**   | Escriba la dirección URL de la aplicación web de Service Manager. [Aquí](#create-and-deploy-service-manager-web-app-service) podrá obtener más información sobre la aplicación web de Service Manager.
| **Id. de cliente**   | Escriba el identificador de cliente que ha generado (mediante el script automático) para autenticar la aplicación web. [Aquí](./itsmc-service-manager-script.md) podrá obtener más información sobre el script automatizado.|
| **Secreto de cliente**   | Escriba el secreto de cliente generado para este identificador.   |
| **Sincronización de datos**   | Seleccione los elementos de trabajo de Service Manager que quiere sincronizar mediante ITSMC.  Estos elementos de trabajo se importan en Log Analytics. **Opciones:**  incidentes, solicitudes de cambio.|
| **Ámbito de sincronización de datos** | Escriba el número de días pasados de los que desea los datos. **Límite máximo**: 120 días. |
| **Creación de un elemento de configuración de solución ITSM** | Seleccione esta opción si desea crear los elementos de configuración en el producto ITSM. Al seleccionarla, Log Analytics crea los elementos de configuración afectados como elementos de configuración (si no existen) en el sistema ITSM compatible. **Valor predeterminado**: deshabilitado. |

![Conexión de Service Manager](media/itsmc-connections/service-manager-connection.png)

**Cuando se ha conectado y sincronizado correctamente**:

- Los elementos de trabajo seleccionados de Service Manager se importan en Azure **Log Analytics**. Puede ver el resumen de estos elementos de trabajo en el icono de IT Service Management Connector.

- Puede crear incidentes a partir de alertas de Log Analytics, de entradas de registros o de alertas de Azure en esta instancia de Service Manager.

Más información: [Creación de elementos de trabajo de ITSM a partir de alertas de Azure](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts).

## <a name="create-and-deploy-service-manager-web-app-service"></a>Creación e implementación del servicio de aplicaciones web de Service Manager

Para conectar la instancia de Service Manager local con ITSMC en Azure, Microsoft ha creado una aplicación web de Service Manager en GitHub.

Para configurar la aplicación web de ITSM para Service Manager, haga lo siguiente:

- **Implemente la aplicación web**: implemente la aplicación web, establezca las propiedades y autentíquese con Azure AD. Puede implementar la aplicación web mediante el [script automatizado ](./itsmc-service-manager-script.md) que Microsoft ha proporcionado.
- **Configure la conexión híbrida** - [Configure esta conexión](#configure-the-hybrid-connection), manualmente.

### <a name="deploy-the-web-app"></a>Implementación de la aplicación web
Use el [script](./itsmc-service-manager-script.md) automatizado para implementar la aplicación web, establecer las propiedades y autenticarse con Azure AD.

Ejecute el script proporcionando los siguientes detalles necesarios:

- Detalles de la suscripción de Azure
- Definición de un nombre de grupo de recursos
- Location
- Detalles del servidor de Service Manager (nombre del servidor, dominio, nombre de usuario y contraseña)
- Prefijo de nombre de sitio de la aplicación web
- Espacio de nombres de ServiceBus.

El script crea la aplicación web con el nombre que especificó (junto con algunas cadenas adicionales para hacerlo único). Genera la **dirección URL de la aplicación web**, el **id. de cliente** y el **secreto de cliente**.

Guarde los valores, ya que se usan al crear una conexión con ITSMC.

**Comprobación de la instalación de la aplicación web**

1. Vaya a **Azure Portal** > **Recursos**.
2. Seleccione la aplicación web y haga clic en **Configuración** > **Configuración de la aplicación**.
3. Confirme la información sobre la instancia de Service Manager que ha proporcionado en el momento de la implementación de la aplicación a través del script.

## <a name="configure-the-hybrid-connection"></a>Configuración de la conexión híbrida

Use el procedimiento siguiente para configurar la conexión híbrida que conecta la instancia de Service Manager con ITSMC en Azure.

1. Busque la aplicación web de Service Manager, **Recursos de Azure**.
2. Haga clic en **Configuración** > **Redes**.
3. En **Conexiones híbridas**, haga clic en **Configure los puntos de conexión de la conexión híbrida**.

    ![Redes de conexión híbrida](media/itsmc-connections/itsmc-hybrid-connection-networking-and-end-points.png)
4. En la hoja **Conexiones híbridas**, haga clic en **Agregar conexión híbrida**.

    ![Adición de conexión híbrida](media/itsmc-connections/itsmc-new-hybrid-connection-add.png)

5. En la hoja **Agregar conexiones híbridas**, haga clic en **Crear conexión híbrida nueva**.

    ![Conexión híbrida nueva](media/itsmc-connections/itsmc-create-new-hybrid-connection.png)

6. Escriba los valores siguientes:

   - **Nombre del punto de conexión**: especifique un nombre para la conexión híbrida nueva.
   - **Host del punto de conexión**: nombre de dominio completo del servidor de administración de Service Manager.
   - **Puerto del punto de conexión**: tipo 5724
   - **Espacio de nombres de Service Bus**: use un espacio de nombres de Service Bus existente o cree uno.
   - **Ubicación**: seleccione la ubicación.
   - **Name**: especifique un nombre para la instancia de Service Bus si lo está creando.

     ![Valores de la conexión híbrida](media/itsmc-connections/itsmc-new-hybrid-connection-values.png)
6. Haga clic en **Aceptar** para cerrar la hoja **Crear conexión híbrida** y empiece a crearla.

    Una vez creada, se muestra debajo de la hoja.

7. Cuando se crea la conexión híbrida, seleccione la conexión y haga clic en **Agregar conexión híbrida seleccionada**.

    ![Conexión híbrida nueva](media/itsmc-connections/itsmc-new-hybrid-connection-added.png)

### <a name="configure-the-listener-setup"></a>Configuración del programa de instalación del agente de escucha

Utilice el procedimiento siguiente para configurar el programa de instalación del agente de escucha para la conexión híbrida.

1. En la hoja **Conexiones híbridas**, haga clic en **Descargar administrador de conexión** e instálelo en el equipo donde se ejecuta la instancia de System Center Service Manager.

    Una vez finalizada la instalación, la opción **Hybrid Connection Manager UI** (IU de administración de conexión híbrida) está disponible en el menú **Iniciar**.

2. Haga clic en **Hybrid Connection Manager UI** (IU de administración de conexión híbrida), se le pedirán las credenciales de Azure.

3. Inicie sesión con sus credenciales de Azure y seleccione la suscripción en la que se creó la conexión híbrida.

4. Haga clic en **Save**(Guardar).

La conexión híbrida se ha conectado correctamente.

![conexión híbrida correcta](media/itsmc-connections/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]
> 
> Después de crear la conexión híbrida, compruebe y pruebe la conexión visitando la aplicación web de la instancia de Service Manager implementado. Asegúrese de que la conexión es correcta antes de intentar conectarse con ITSMC en Azure.

En la siguiente imagen de ejemplo se muestran los detalles de una conexión correcta:

![Prueba de conexión híbrida](media/itsmc-connections/itsmc-hybrid-connection-test.png)

## <a name="next-steps"></a>Pasos siguientes

* [Introducción al Conector de Administración de servicios de TI](itsmc-overview.md)
* [Creación de elementos de trabajo de ITSM a partir de alertas de Azure](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [Solución de problemas del Conector de Administración de servicios de TI](./itsmc-resync-servicenow.md)