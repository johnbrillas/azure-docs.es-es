---
title: Errores comunes
description: Este documento contiene información sobre los errores comunes que existen en el panel.
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/18/2021
ms.openlocfilehash: 7240c1b0f19dc49ab4130c5ee2516dcfefb2e2c2
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98602186"
---
# <a name="errors-in-the-connector-status"></a>Errores en el estado del conector

En la lista de estados del conector puede encontrar errores que puedan ayudarlo a corregir incidencias en el conector de Administración de servicios de TI.

## <a name="status-common-errors"></a>Errores comunes de estado

En esta sección puede encontrar los errores comunes que se muestran en la sección de estado del conector y cómo resolverlos.

* **Error**: "Respuesta inesperada de ServiceNow junto con el código de estado correcto. Respuesta: { "import_set": "{import_set_id}", "staging_table": "x_mioms_microsoft_oms_incident", "result": [ { "transform_map": "OMS Incident", "table": "incident", "status": "error", "error_message": "{Target record not found|Invalid table|Invalid staging table" }"

    **Causa**: un error de este tipo se devuelve desde ServiceNow cuando:
  * Un script personalizado implementado en la instancia de ServiceNow hace que se omitan los incidentes.
  * El código "OMS Integrator App" mismo se modificó en el lado de ServiceNow, por ejemplo, el script onBefore.

    **Solución:** deshabilite todos los scripts personalizados o modificaciones de código de la ruta de acceso de importación de datos.

* **Error**: "{"error":{"message":"Operation Failed","detail":"ACL Exception Update Failed due to security constraints"}"

    **Causa**: error de configuración de los permisos de ServiceNow.

    **Solución:** compruebe que todos los roles se asignaron correctamente como [especificado](itsmc-connections-servicenow.md#install-the-user-app-and-create-the-user-role).

* **Error**: "Se produjo un error al enviar la solicitud".

    **Causa**: "La instancia de ServiceNow no está disponible".

    **Solución:** revise su instancia en ServiceNow, puede que se haya eliminado o que no esté disponible.

* **Error**: "ServiceDeskHttpBadRequestException: StatusCode=429"

    **Causa**: los límites de frecuencia de ServiceNow son demasiado altos o bajos.

    **Solución:** aumente o cancele los límites de frecuencia de la instancia de ServiceNow como se explica [aquí](https://docs.servicenow.com/bundle/london-application-development/page/integrate/inbound-rest/task/investigate-rate-limit-violations.html).

* **Error**: "AccessToken y RefreshToken no son válidos. El usuario debe volver a autenticarse".

    **Causa**: el token de actualización expiró.

    **Solución:** sincronice el Conector ITSM para generar un token de actualización nuevo como se explica [aquí](./itsmc-resync-servicenow.md).

* **Error**: "No se pudo crear o actualizar el elemento de trabajo para la alerta {alertName}. El Conector ITSM {connectionIdentifier} no existe o se eliminó".

    **Causa**: se eliminó el Conector ITSM.

    **Solución:** el Conector ITSM se eliminó, pero todavía tiene grupos de acciones ITSM asociados. Hay dos opciones para solucionar esta incidencia:
  * Buscar y deshabilitar o eliminar esta acción.
  * [Volver a configurar el grupo de acciones](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts) para usar un Conector ITSM existente.
  * [Crear un conector de ITSM](./itsmc-definition.md#create-an-itsm-connection) y [volver a configurar el grupo de acciones para usarlo](itsmc-definition.md#create-itsm-work-items-from-azure-alerts).

## <a name="ui-common-errors"></a>Errores comunes de UI

* **Error**:"Se produjo un problema. No se pudieron obtener los detalles de la conexión". Este error se presenta cuando el cliente define el grupo de acciones de ITSM.

    **Causa**: el Conector ITSM recién creado aún tiene que finalizar la sincronización inicial.

    **Solución:** cuando se crea un nuevo conector de ITSMTSM, el Conector ITSM inicia la sincronización de la información del sistema ITSM, como elementos de trabajo y plantillas de elementos de trabajo. Sincronice el Conector ITSM para generar un token de actualización nuevo como se explica [aquí](./itsmc-resync-servicenow.md).
