---
title: Errores de estado del conector en el panel de ITSMC
description: Obtenga información sobre los errores comunes que existen en el panel de Conector de Administración de servicios de TI.
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/18/2021
ms.openlocfilehash: 727e744c59d0a8d90cf320e1ee2e2a17e10ff847
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103471532"
---
# <a name="connector-status-errors-in-the-itsmc-dashboard"></a>Errores de estado del conector en el panel de ITSMC

El panel de Conector de Administración de servicios de TI (ITSMC) presenta errores que pueden ayudarle a solucionar problemas en el conector.

En las secciones siguientes se describen los errores comunes que aparecen en la sección de estado del conector del panel y cómo pueden resolverse.

## <a name="unexpected-response"></a>Respuesta inesperada

**Error**: "Respuesta inesperada de ServiceNow junto con el código de estado correcto. Respuesta: { "import_set": "{import_set_id}", "staging_table": "x_mioms_microsoft_oms_incident", "result": [ { "transform_map": "OMS Incident", "table": "incident", "status": "error", "error_message": "{Target record not found|Invalid table|Invalid staging table" }"

**Causa**: ServiceNow devuelve este error cuando:

* Un script personalizado implementado en la instancia de ServiceNow hace que se omitan los incidentes.
* El código "Aplicación del integrador de OMS" se modificó en el lado de ServiceNow (por ejemplo, mediante el script `onBefore`).

**Solución:** deshabilite todos los scripts personalizados o modificaciones de código.

## <a name="exception-update-failure"></a>Error al actualizar la excepción

**Error**: "{"error":{"message":"Operation Failed","detail":"ACL Exception Update Failed due to security constraints"}"

**Causa**: Los permisos de ServiceNow están mal configurados.

**Solución:** Compruebe que todos los roles se asignaron correctamente como [especificado](itsmc-connections-servicenow.md#install-the-user-app-and-create-the-user-role).

## <a name="problem-sending-a-request"></a>Problema al enviar una solicitud

**Error**: "Se produjo un error al enviar la solicitud".

**Causa**: La instancia de ServiceNow no está disponible.

**Solución:** Compruebe la instancia en ServiceNow. Podría haberse eliminado o no estar disponible.

## <a name="servicenow-rate-problem"></a>Problema de frecuencia de ServiceNow

**Error**: "ServiceDeskHttpBadRequestException: StatusCode=429"

**Causa**: Los límites de frecuencia de ServiceNow son demasiado altos o bajos.

**Solución:** Aumente o cancele los límites de frecuencia de la instancia de ServiceNow como se explica en la [documentación de ServiceNow](https://docs.servicenow.com/bundle/london-application-development/page/integrate/inbound-rest/task/investigate-rate-limit-violations.html).

## <a name="invalid-refresh-token"></a>Token de actualización no válido

**Error**: 
  * "AccessToken y RefreshToken no son válidos. El usuario debe volver a autenticarse".
  * "No se pudo sincronizar la configuración de plantillas para evento,alerta,incidente. Consulte el mensaje de excepción para obtener más información."

**Causa**: El token de actualización expiró.

**Solución:** Sincronice ITSMC para generar un nuevo token de actualización tal como se explica en [Cómo corregir manualmente los problemas de sincronización](./itsmc-resync-servicenow.md).

## <a name="missing-connector"></a>Falta el conector

**Error**: "No se pudo crear o actualizar el elemento de trabajo para la alerta {alertName}. El Conector ITSM {connectionIdentifier} no existe o se eliminó".

**Causa**: Se ha eliminado ITSMC.

**Solución:** ITSMC se eliminó, pero aún tiene asociados los grupos de acciones de Administración de servicios de TI (ITSM). Existen tres opciones para solucionar este problema:

* Buscar y deshabilitar o eliminar estos grupos de acciones.
* [Volver a configurar el grupo de acciones](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts) para usar una instancia de ITSMC existente.
* [Crear una instancia de ITSMC](./itsmc-definition.md#create-an-itsm-connection) y [volver a configurar el grupo de acciones para usarlo](itsmc-definition.md#create-itsm-work-items-from-azure-alerts).

## <a name="lack-of-connection-details"></a>Falta de detalles de conexión

**Error**:"Se produjo un problema. No se pudieron obtener los detalles de la conexión". Este error aparece cuando se define un grupo de acciones de ITSM.

**Causa**: Este error aparece en cualquiera de estas situaciones:

* Una instancia de Conector ITSM recién creada aún tiene que finalizar la sincronización inicial.
* El conector no se definió correctamente.

**Solución:** 

* Cuando se crea una nueva instancia de ITSMC, inicia la sincronización de la información del sistema ITSM, como elementos de trabajo y plantillas de elementos de trabajo. [Sincronice ITSMC para generar un nuevo token de actualización](./itsmc-resync-servicenow.md).
* [Revise los detalles de conexión en ITSMC](./itsmc-connections-servicenow.md#create-a-connection) y compruebe que ITSMC puede realizar la [sincronización](./itsmc-resync-servicenow.md) correctamente.


## <a name="ip-restrictions"></a>Restricciones de IP
**Error**: "No se pudo agregar la conexión de ITSM denominada "XXX"debido a una solicitud incorrecta. Error: Solicitud incorrecta. Parámetros no válidos proporcionados para la conexión. Excepción HTTP: Código de estado prohibido".

**Causa**: La dirección IP de la aplicación ITSM no permite conexiones de ITSM desde las herramientas ITSM de los asociados.

**Resolución**: Para obtener una lista de las direcciones IP de ITSM con el fin de permitir las conexiones de ITSM desde las herramientas ITSM de los asociados, se recomienda que muestre el intervalo IP pública completo de la región de Azure a la que pertenece el área de trabajo de Log Analytics. [Detalles aquí](https://www.microsoft.com/download/details.aspx?id=56519) En las regiones EUS/WEU/EUS2/WUS2/Centro y Sur de EE. UU., el cliente solo puede enumerar la etiqueta de red ActionGroup.
