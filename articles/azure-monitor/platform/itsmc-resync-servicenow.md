---
title: Cómo corregir manualmente los problemas de sincronización de ServiceNow
description: Restablecimiento de la conexión a ServiceNow para que las alertas en Microsoft Azure puedan volver a llamar a ServiceNow
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: 01e492072bd75af9f80656b71d2cc1c473d64263
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97803806"
---
# <a name="troubleshooting-problems-in-itsm-connector"></a>Solución de problemas del Conector de Administración de servicios de TI

En este artículo se describen los problemas comunes de Conector ITSM y cómo solucionarlos.

Las alertas de Azure Monitor le informan de forma proactiva cuando se detectan condiciones importantes en los datos que se supervisan. Le permiten identificar y solucionar los problemas antes de que los usuarios del sistema puedan verlos. Para más información sobre las alertas, consulte Información general sobre las alertas en Microsoft Azure.
El cliente puede seleccionar cómo desea recibir notificaciones en la alerta, ya sea por correo, SMS, webhook, o incluso automatizar una solución. Otra opción que se va a notificar es el uso de ITSM.
ITSM le ofrece la opción de enviar las alertas a un sistema externo de vales como ServiceNow.

## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Visualizar y analizar los datos de incidentes y solicitudes de cambios

En función de las opciones de configuración de una conexión, ITSMC puede sincronizar hasta 120 días de datos referentes a incidentes y a solicitudes cambios. El esquema de registros de estos datos se proporciona en la [sección Información adicional](./itsmc-overview.md) de este artículo.

Puede visualizar los datos de incidentes y solicitudes de cambio mediante el panel de ITSMC:

![Captura de pantalla en la que se muestra el panel de ITSMC.](media/itsmc-overview/itsmc-overview-sample-log-analytics.png)

El panel también proporciona información sobre el estado del conector, que puede usar como punto de partida para analizar los problemas con las conexiones.

También puede visualizar los incidentes sincronizados con los equipos afectados en Service Map.

Mapa de servicio detecta automáticamente los componentes de la aplicación en sistemas Windows y Linux y asigna la comunicación entre servicios. Permite ver los servidores a medida que piensa en ellos: como los sistemas interconectados que ofrecen servicios críticos. Service Map muestra las conexiones entre servidores, procesos y puertos en cualquier arquitectura con conexión TCP. No se requiere ninguna configuración aparte de la instalación de un agente. Para más información, vea [Uso de la solución Service Map en Azure](../insights/service-map.md).

Si usa Service Map, puede ver los elementos de la consola de servicio creados en las soluciones de ITSM, como se muestra aquí:

![Captura de pantalla en la que se muestra la pantalla de Log Analytics.](media/itsmc-overview/itsmc-overview-integrated-solutions.png)

## <a name="troubleshoot-itsm-connections"></a>Solución de problemas de conexión de ITSM

- Si una conexión no puede conectarse al sistema ITSM y recibe un mensaje **Error al guardar la conexión**, siga estos pasos:
   - En el caso de conexiones de ServiceNow, Cherwell y Provance:  
     - Asegúrese de que ha introducido correctamente el nombre de usuario, la contraseña, el identificador de cliente y el secreto de cliente de cada una de las conexiones.  
     - Asegúrese de disponer de privilegios suficientes en el producto de ITSM correspondiente para realizar la conexión.  
   - En el caso de conexiones de Service Manager:  
     - Asegúrese de que la aplicación web se implementa correctamente y de que se crea la conexión híbrida. Para comprobar que la conexión se ha establecido correctamente con el equipo de Service Manager local, visite la dirección URL de la aplicación web como se detalla en la documentación para realizar la [conexión híbrida](./itsmc-connections-scsm.md#configure-the-hybrid-connection).  

- Si los datos de ServiceNow no se sincronizan con Log Analytics, asegúrese de que la instancia de ServiceNow no esté suspendida. En algunas ocasiones, las instancias de desarrollo de ServiceNow se suspenden si están inactivas durante mucho tiempo. Si no sucede eso, informe del problema.
- Si se generan alertas de Log Analytics, pero no se crean elementos de trabajo en el producto de ITSM, si no se crean elementos de configuración o no se vinculan a elementos de trabajo, o si necesita información adicional, vea estos recursos:
   -  ITSMC: La solución muestra un resumen de conexiones, elementos de trabajo, equipos, etc. Seleccione el icono que tiene la etiqueta **Estado del conector**. Al hacerlo, se le remitirá a **Búsqueda de registros** con la consulta pertinente. Consulte las entradas de registro con un elemento `LogType_S` de `ERROR` para obtener más información.
   - Página de **búsqueda de registros**: vea los errores y la información relacionada directamente mediante la consulta `*ServiceDeskLog_CL*`.

### <a name="troubleshoot-service-manager-web-app-deployment"></a>Solución de problemas de implementación de aplicaciones web de Service Manager

-   Si tiene problemas con la implementación de la aplicación web, asegúrese de tener los permisos para crear o implementar recursos en la suscripción.
-   Si aparece el mensaje de error **Referencia a objeto no establecida como instancia de un objeto** al ejecutar el [script](itsmc-service-manager-script.md), asegúrese de que especificó valores válidos en la sección **Configuración de usuario**.
-   Si no puede crear el espacio de nombres de retransmisión de Service Bus, asegúrese de que el proveedor de recursos necesario está registrado en la suscripción. Si no está registrado, cree el espacio de nombres de retransmisión de Service Bus manualmente desde Azure Portal. También puede crearlo mientras [crea la conexión híbrida](./itsmc-connections-scsm.md#configure-the-hybrid-connection) en Azure Portal.

### <a name="how-to-manually-fix-sync-problems"></a>Cómo corregir manualmente los problemas de sincronización

Azure Monitor puede conectarse a otros proveedores de Administración de servicios de TI (ITSM) de terceros. ServiceNow es uno de estos proveedores.

Por motivos de seguridad, puede que tenga que actualizar el token de autenticación que se usa para la conexión con ServiceNow.
Use el proceso de sincronización siguiente para volver a activar la conexión y actualizar el token:


1. Busque la solución en el banner de búsqueda superior y, luego, seleccione las soluciones apropiadas.

    ![Captura de pantalla que muestra el banner de búsqueda principal y dónde seleccionar las soluciones pertinentes.](media/itsmc-resync-servicenow/solution-search-8bit.png)

1. En la pantalla de la solución, elija "Seleccionar todo" en el filtro de suscripción y, a continuación, filtre por "ServiceDesk".

    ![Captura de pantalla que muestra dónde elegir "Seleccionar todo" y dónde filtrar según ServiceDesk.](media/itsmc-resync-servicenow/solutions-list-8bit.png)

1. Seleccione la solución de la conexión de ITSM.
1. Seleccione la conexión de ITSM en el banner izquierdo.

    ![Captura de pantalla que muestra dónde seleccionar las conexiones de ITSM.](media/itsmc-resync-servicenow/itsm-connector-8bit.png)

1. Seleccione cada conector en la lista. 
    1. Haga clic en el nombre del conector para configurarlo.
    1. Elimine los conectores que ya no se estén usando.

    1. Actualice los campos de acuerdo con [estas definiciones](./itsmc-connections.md) según el tipo de asociado.

    1. Haga clic en Sincronizar

       ![Captura de pantalla que resalta el botón Sincronizar.](media/itsmc-resync-servicenow/resync-8bit2.png)

    1. Haga clic en Guardar

        ![Nueva conexión](media/itsmc-resync-servicenow/save-8bit.png)

f.    Revise las notificaciones para ver si se inició el proceso.
