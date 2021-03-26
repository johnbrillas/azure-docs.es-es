---
title: Conexión de Provance al Conector de Administración de servicios de TI
description: En este artículo se ofrece información sobre cómo conectar Provance al Conector de Administración de servicios de TI (ITSMC) en Azure Monitor para supervisar y administrar de forma centralizada los elementos de trabajo de ITSM.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 542b127823a73058f319e6a39c001bd563f042ae
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102045384"
---
# <a name="connect-provance-with-it-service-management-connector"></a>Conexión de Provance al Conector de Administración de servicios de TI

En este artículo se ofrece información sobre cómo configurar la conexión entre la instancia de Provance y el Conector de Administración de servicios de TI (ITSMC) en Log Analytics para administrar de forma centralizada los elementos de trabajo.

> [!NOTE]
> A partir del 1 de octubre de 2020, la integración de ITSM de Provance con alertas de Azure ya estará habilitada para los clientes nuevos. No se admitirán las nuevas conexiones de ITSM.
> Se admitirán las conexiones de ITSM existentes.

En las secciones siguientes se proporcionan detalles sobre cómo conectar su producto de Provance a ITSMC en Azure.

## <a name="prerequisites"></a>Prerrequisitos

Asegúrese de que se cumplen los siguientes requisitos previos:

- ITSMC instalado Más información: [Agregar la solución IT Service Management Connector](./itsmc-definition.md#add-it-service-management-connector).
- La aplicación Provance debe registrarse con Azure AD y estará disponible el identificador de cliente. Para una información detallada, consulte el tema sobre [cómo configurar la autenticación de Active Directory](../../app-service/configure-authentication-provider-aad.md).

- Rol de usuario:  Administrador.

## <a name="connection-procedure"></a>Procedimiento de conexión

Use el procedimiento siguiente para crear una conexión a Provance:

1. En Azure Portal, vaya a **Todos los recursos** y busque **ServiceDesk(YourWorkspaceName)** .

2. En **ORÍGENES DE DATOS DEL ÁREA DE TRABAJO**, haga clic en **Conexiones de ITSM**.
    ![Nueva conexión](media/itsmc-overview/add-new-itsm-connection.png)

3. En la parte superior del panel derecho, haga clic en **Agregar**.

4. Proporcione la información tal como se describe en la tabla siguiente y haga clic en **Aceptar** para crear la conexión.

> [!NOTE]
> Todos estos parámetros son obligatorios.

| **Campo** | **Descripción** |
| --- | --- |
| **Nombre de la conexión**   | Escriba un nombre para la instancia de Provance que quiere conectar con ITSMC.  Usará este nombre más adelante cuando configure los elementos de trabajo en el análisis de registros detallados de ITSM o vista. |
| **Tipo de asociado**   | Seleccione **Provance**. |
| **Nombre de usuario**   | Escriba el nombre de usuario que puede conectarse a ITSMC.    |
| **Contraseña**   | Escriba la contraseña asociada con este nombre de usuario. **Nota:** El nombre de usuario y la contraseña se utilizan para generar únicamente tokens de autenticación y no se almacenan en ningún lugar dentro del servicio ITSMC.|
| **Dirección URL del servidor**   | Escriba la dirección URL de la instancia de Provance que quiere conectar con ITSMC. |
| **Id. de cliente**   | Escriba el identificador de cliente para autenticar esta conexión, que genera en la instancia de Provance.  Para más información sobre el identificador de cliente, consulte el tema sobre [cómo configurar la autenticación de Active Directory](../../app-service/configure-authentication-provider-aad.md). |
| **Ámbito de sincronización de datos**   | Seleccione los elementos de trabajo de Provance que quiera sincronizar con Azure Log Analytics mediante ITSMC.  Estos elementos de trabajo se importan en Log Analytics.   **Opciones:**   incidentes, solicitudes de cambio.|
| **Sincronización de datos** | Escriba el número de días pasados de los que desea los datos. **Límite máximo**: 120 días. |
| **Creación de un elemento de configuración de solución ITSM** | Seleccione esta opción si desea crear los elementos de configuración en el producto ITSM. Cuando se selecciona, ITMSC crea los elementos de configuración afectados como elementos de configuración (en el caso de que no existan) en el sistema ITSM compatible. **Valor predeterminado**: deshabilitado.|

![Captura de pantalla que resalta las listas Connection Name (Nombre de la conexión) y Partner Type (Tipo de asociado).](media/itsmc-connections-provance/itsm-connections-provance-latest.png)

**Cuando se ha conectado y sincronizado correctamente**:

- Los elementos de trabajo seleccionados en la instancia de Provance se importan en Azure **Log Analytics**. Puede ver el resumen de estos elementos de trabajo en el icono de IT Service Management Connector.

- Puede crear incidentes a partir de alertas de Log Analytics, de entradas de registros o de alertas de Azure en esta instancia de Provance.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción al Conector de Administración de servicios de TI](itsmc-overview.md)
* [Creación de elementos de trabajo de ITSM a partir de alertas de Azure](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [Solución de problemas del Conector de Administración de servicios de TI](./itsmc-resync-servicenow.md)