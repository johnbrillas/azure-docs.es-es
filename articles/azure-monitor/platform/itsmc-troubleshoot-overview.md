---
title: Solución de problemas del Conector de Administración de servicios de TI
description: Solución de problemas en Conector de Administración de servicios de TI
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: 2ffe7c8994d32917a08896c7d25f20d4adf09066
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98601899"
---
# <a name="troubleshooting-problems-in-itsm-connector"></a>Solución de problemas del Conector de Administración de servicios de TI

En este artículo se describen los problemas comunes de Conector ITSM y cómo solucionarlos.

Las alertas de Azure Monitor le informan de forma proactiva cuando se detectan condiciones importantes en los datos que se supervisan. Le permiten identificar y solucionar los problemas antes de que los usuarios del sistema puedan verlos. Para más información sobre las alertas, consulte Información general sobre las alertas en Microsoft Azure.
El cliente puede seleccionar cómo desea recibir notificaciones en la alerta, ya sea por correo, SMS, webhook, o incluso automatizar una solución. Otra opción que se va a notificar es el uso de ITSM.
ITSM le ofrece la opción de enviar las alertas a un sistema externo de vales como ServiceNow.

## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Visualizar y analizar los datos de incidentes y solicitudes de cambios

En función de las opciones de configuración de una conexión, ITSMC puede sincronizar hasta 120 días de datos referentes a incidentes y a solicitudes cambios. El esquema de registros de estos datos se proporciona en la [sección Información adicional](./itsmc-synced-data.md) de este artículo.

Puede visualizar los datos de incidentes y solicitudes de cambio mediante el panel de ITSMC:

![Captura de pantalla en la que se muestra el panel de ITSMC.](media/itsmc-overview/itsmc-overview-sample-log-analytics.png)

El panel también proporciona información sobre el estado del conector, que puede usar como punto de partida para analizar los problemas con las conexiones.

Para obtener más información sobre la investigación del panel, consulte [Investigación de errores mediante el panel](./itsmc-dashboard.md).

### <a name="service-map"></a>Mapa de servicio

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

- Si se generan alertas de Log Analytics, pero no se crean elementos de trabajo en el producto de ITSM, si no se crean elementos de configuración o no se vinculan a elementos de trabajo, o si necesita información adicional, vea estos recursos:
   -  ITSMC: La solución muestra un [resumen de conexiones](itsmc-dashboard.md), elementos de trabajo, equipos, etc. Seleccione el icono que tiene la etiqueta **Estado del conector**. Al hacerlo, se le remitirá a **Búsqueda de registros** con la consulta pertinente. Consulte las entradas de registro con un elemento `LogType_S` de `ERROR` para obtener más información.
   Puede ver los detalles de los mensajes de la tabla [aquí](itsmc-dashboard-errors.md).
   - Página de **búsqueda de registros**: vea los errores y la información relacionada directamente mediante la consulta `*ServiceDeskLog_CL*`.

## <a name="common-symptoms---how-it-should-be-resolved"></a>Síntomas comunes: ¿cómo se deben resolver?

En la siguiente lista se incluyen los síntomas comunes y cómo deben resolverse:

* **Síntoma**: Se crean elementos de trabajo duplicados

    **Causa**: La causa puede deberse a una de estas dos opciones:
    * Se definió más de una acción de ITSM para la alerta.
    * Se resolvió la alerta.

    **Solución:** Puede haber dos soluciones:
    * Asegúrese de tener un único grupo de acciones de ITSM por alerta.
    * El Conector ITSM no admite la actualización del estado de elementos de trabajo coincidentes cuando se ha resuelto una alerta. Se crea un nuevo elemento de trabajo resuelto.
* **Síntoma**: No se han creado elementos de trabajo.

    **Causa**: Puede haber un par de motivos para este síntoma:
    * Modificación del código desde ServiceNow.
    * Error de configuración de los permisos.
    * Los límites de frecuencia de ServiceNow son demasiado altos o bajos.
    * El token de actualización expiró.
    * Se eliminó el Conector ITSM.

    **Solución:** Puede consultar el [panel](itsmc-dashboard.md) y revisar los errores en la sección de estado del conector. Revise los [errores comunes](itsmc-dashboard-errors.md) y descubra cómo resolver el error.

* **Síntoma**: No se puede crear la acción de ITSM para el grupo de acciones

    **Causa**: El Conector ITSM recién creado aún tiene que finalizar la sincronización inicial.

    **Solución**: Puede revisar los [errores comunes de la UI](itsmc-dashboard-errors.md#ui-common-errors) y descubrir cómo resolver el error.