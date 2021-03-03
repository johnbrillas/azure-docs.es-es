---
title: Conexión a Common Data Service (Microsoft Dataverse)
description: Creación y administración de registros de Common Data Service (Microsoft Dataverse) mediante Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: jdaly, logicappspm
ms.topic: conceptual
ms.date: 02/11/2021
tags: connectors
ms.openlocfilehash: bec3416195358121b85eb61679ab39647e664a9e
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100382365"
---
# <a name="create-and-manage-records-in-common-data-service-microsoft-dataverse-by-using-azure-logic-apps"></a>Creación y administración de registros de Common Data Service (Microsoft Dataverse) mediante Azure Logic Apps

> [!NOTE]
> En noviembre de 2020, se cambió el nombre de Common Data Service a Microsoft Dataverse.

Con [Azure Logic Apps](../logic-apps/logic-apps-overview.md) y el [conector de Common Data Service](/connectors/commondataservice/), puede crear flujos de trabajo automatizados que administren registros en la base de datos de [Common Data Service (ahora Microsoft Dataverse)](/powerapps/maker/common-data-service/data-platform-intro). Estos flujos de trabajo pueden crear registros, actualizar registros y realizar otras operaciones. También puede obtener información de la base de datos Dataverse y hacer que la salida esté disponible para que otras acciones la usen en la aplicación lógica. Por ejemplo, cuando se actualice un registro en la base de datos Dataverse, puede enviar un correo electrónico mediante el conector de Office 365 Outlook.

En este artículo se explica cómo compilar una aplicación lógica que cree un registro de tarea cada vez que se cree un nuevo registro de cliente potencial.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Un [entorno de Common Data Service](/power-platform/admin/environments-overview), que es un espacio donde la organización almacena, administra y comparte datos empresariales y una base de datos de Common Data Service. Para obtener más información, vea estos recursos:<p>

  * [Learn: Introducción a Common Data Service](/learn/modules/get-started-with-powerapps-common-data-service/)
  * [Power Platform: Información general de entornos](/power-platform/admin/environments-overview)

* Información básica sobre [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md) y la aplicación lógica desde donde quiere acceder a los registros de la base de datos Dataverse. Para iniciar la aplicación lógica con un desencadenador de Common Data Service, necesita una aplicación lógica en blanco. Si es la primera vez que usa Azure Logic Apps, revise [Inicio rápido: Creación del primer flujo de trabajo mediante Azure Logic Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-common-data-service-trigger"></a>Adición del desencadenador de Common Data Service

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

En este ejemplo, agregue el desencadenador de Common Data Service, que se activará al crear un nuevo registro.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en blanco en el Diseñador de aplicación lógica, si aún no está abierta.

1. En el cuadro de búsqueda, escriba `common data service`. En este ejemplo, en la lista de desencadenadores, seleccione este desencadenador: **When a record is created**

   ![Selección del desencadenador "Al crear un registro"](./media/connect-common-data-service/select-when-record-created-trigger.png)

1. Si se le solicita, inicie sesión en Common Data Service.

1. En el desencadenador, proporcione información sobre el entorno en el que desea supervisar los nuevos registros de "Clientes potenciales", por ejemplo:

   ![Información del desencadenador para el entorno que se va a supervisar](./media/connect-common-data-service/when-record-created-trigger-details.png)

   | Propiedad | Obligatorio | Descripción |
   |----------|----------|-------------|
   | **Entorno** | Sí | El entorno que se va a supervisar, por ejemplo, "Fabrikam Sales Production". Para obtener más información, consulte [Power Platform: Información general sobre los entornos](/power-platform/admin/environments-overview). |
   | **Nombre de entidad** | Sí | La entidad que se va a supervisar; por ejemplo, "Clientes potenciales". |
   | **Ámbito** | Sí | El origen que creó el nuevo registro; por ejemplo, un usuario de la unidad de negocio o cualquier usuario de la organización. En este ejemplo se usa "Unidad de negocio". |
   ||||

## <a name="add-common-data-service-action"></a>Adición de la acción de Common Data Service

Ahora, agregue una acción de Common Data Service que cree un registro de tarea para un nuevo registro de "Clientes potenciales".

1. En el desencadenador **Al crear un registro**, seleccione **Nuevo paso**.

1. En el cuadro de búsqueda, escriba `common data service`. En la lista de acciones, seleccione esta acción: **Create a new record**

   ![Seleccione la acción "Crear un nuevo registro".](./media/connect-common-data-service/select-create-new-record-action.png)

1. En la acción, proporcione la información sobre el entorno en el que desea crear el nuevo registro de tarea. Si está disponible, también aparecen otras propiedades basadas en la entidad seleccionada para esta acción, como, por ejemplo:

   ![Información de la acción para el entorno en el que se va a crear el registro](./media/connect-common-data-service/create-new-record-action-details.png)

   | Propiedad | Obligatorio | Descripción |
   |----------|----------|-------------|
   | **Nombre de la organización** | Sí | Entorno donde desea crear el registro, que no tiene que ser el mismo del desencadenador; en este ejemplo, "Fabrikam Sales Production" |
   | **Nombre de entidad** | Sí | La entidad donde desea crear el registro; por ejemplo, "Tareas" |
   | **Subject** | Sí, en función de la entidad seleccionada en este ejemplo | Breve descripción del objetivo de esta tarea |
   ||||

   1. En la propiedad **Asunto**, escriba este texto con un espacio en blanco final:

      `Follow up with new lead:`

   1. Mantenga el puntero dentro del cuadro **Asunto** para que la lista de contenido dinámico permanezca visible.
   
   1. En la lista, en la sección **Al crear un registro**, seleccione las salidas del desencadenador que desea incluir en el registro de la tarea, como, por ejemplo:

      ![Seleccionar las salidas del desencadenador que se usarán en el registro de tareas](./media/connect-common-data-service/create-new-record-action-select-trigger-outputs.png)

      | Salida del desencadenador | Descripción |
      |----------------|-------------|
      | **Nombre** | Nombre del registro de clientes potenciales que se va a usar como contacto principal en el registro de tareas. |
      | **Last Name** | Apellido del registro de clientes potenciales que se usará como contacto principal en el registro de tareas. |
      | **Descripción** | Otras salidas que se incluirán en el registro de tareas, como la dirección de correo electrónico y el número de teléfono del trabajo. |
      |||

   Cuando haya terminado, la acción debe ser parecida a la de este ejemplo:

   ![Acción "Crear un nuevo registro" finalizada](./media/connect-common-data-service/finished-create-record-action-details.png)

1. Guarde la aplicación lógica. En la barra de herramientas del diseñador, seleccione **Save** (Guardar).

1. Para iniciar manualmente la aplicación lógica, en la barra de herramientas del diseñador, elija **Ejecutar**. Para probar la aplicación lógica, cree un nuevo registro de "Clientes potenciales".

## <a name="trigger-only-on-updated-attributes"></a>Desencadenamiento solo en atributos actualizados

En el caso de los desencadenadores que se ejecutan cuando se actualizan los registros, como la acción **Al actualizar un registro**, puede usar atributos de filtro para que la aplicación lógica se ejecute solo cuando se actualicen los atributos especificados. Esta funcionalidad le ayuda a evitar las ejecuciones de aplicaciones lógicas innecesarias.

1. En el desencadenador, en la lista **Agregar nuevo parámetro**, seleccione **Filtros de atributo**.

   ![Captura de pantalla que muestra la acción "Al actualizar un registro" y la lista "Agregar nuevo parámetro" abierta con la propiedad "Filtros de atributos" seleccionada.](./media/connect-common-data-service/when-record-updated-trigger-add-attribute-filters.png)

1. En cada **elemento de Filtros de atributo**, seleccione el atributo del que desea supervisar las actualizaciones, como, por ejemplo:

   ![Agregar la propiedad "Filtros de atributo"](./media/connect-common-data-service/when-record-updated-trigger-select-attribute-filter.png)

## <a name="list-records-based-on-a-filter"></a>Enumerar registros en función de un filtro

Para las acciones que devuelven registros, como la acción **Enumerar registros**, puede usar una consulta de ODATA que devuelva registros basados en el filtro especificado. Por ejemplo, la acción solo muestra los registros de las cuentas activas.

1. En la acción, abra la lista **Agregar nuevo parámetro** y seleccione la propiedad **Consulta de filtro**.

   ![Agregar la propiedad "Consulta de filtro"](./media/connect-common-data-service/list-records-action-filter-query.png)

1. En la propiedad **Consulta de filtro** que ahora aparece en la acción, escriba esta consulta de filtro de ODATA: `statuscode eq 1`.

   ![Escribir consulta de filtro de ODATA para filtrar registros](./media/connect-common-data-service/list-records-action-filter-query-value.png)

Para obtener más información sobre las opciones de consulta del sistema `$filter`, consulte [Common Data Service: Filtrar resultados](/powerapps/developer/common-data-service/webapi/query-data-web-api#filter-results).

## <a name="list-records-based-on-an-order"></a>Enumerar registros según un orden

En el caso de las acciones que devuelven registros, como la acción **Enumerar registros**, puede usar una consulta de ODATA que devuelva registros en un orden especificado, que varía en función de los registros devueltos por la acción. Por ejemplo, puede hacer que la acción enumere los registros por el nombre de la cuenta.

1. En la acción, abra la lista **Agregar nuevo parámetro** y seleccione la propiedad **Ordenar por**.

   ![Agregar la propiedad "Ordenar por"](./media/connect-common-data-service/list-records-action-order-by.png)

1. En la propiedad **Ordenar por** que ahora aparece en la acción, escriba esta consulta de filtro de ODATA: `name`.

   ![Especificar consulta de filtro de ODATA para ordenar registros](./media/connect-common-data-service/list-records-action-order-by-value.png)

Para obtener más información sobre las opciones de consulta del sistema `$orderby`, consulte [Common Data Service: Ordenar resultados](/powerapps/developer/common-data-service/webapi/query-data-web-api#order-results).

## <a name="field-data-types"></a>Tipos de datos de campo

Independientemente de si escribe manualmente un valor o lo selecciona de la lista de contenido dinámico para un campo de una acción o un desencadenador, el tipo de datos del valor debe coincidir con el tipo de datos que requiere el campo.

En esta tabla se describen algunos tipos de campos y los tipos de datos necesarios para sus valores.

| Campo | Tipo de datos | Descripción |
|-------|-----------|-------------|
| Campo de texto | Línea de texto única | Requiere una sola línea de texto o contenido dinámico que tenga el tipo de datos de texto, como, por ejemplo, estas propiedades: <p><p>- **Descripción** <br>- **Categoría** |
| Campo de entero | Número entero | Requiere un entero o contenido dinámico que tenga el tipo de datos de entero, como, por ejemplo, estas propiedades: <p><p>- **Porcentaje completado** <br>- **Duración** |
| Campo de fecha | Fecha y hora | Requiere una fecha en formato MM/DD/AAAA o contenido dinámico que tenga el tipo de datos de fecha, como, por ejemplo, estas propiedades: <p><p>- **Creado el** <br>- **Fecha de inicio** <br>- **Inicio real** <br>- **Finalización real** <br>- **Fecha de vencimiento** |
| Campo que hace referencia a otro registro de entidad | Clave principal | Requiere un identificador de registro, como un GUID, y un tipo de búsqueda, lo que significa que los valores de la lista de contenido dinámico no funcionarán. Por ejemplo, estas propiedades: <p><p>- **Propietario**: debe ser un identificador de usuario válido o un identificador de registro de equipo. <br>- **Tipo de propietario**: debe ser un tipo de búsqueda, como `systemusers` o `teams`, respectivamente. <p><p>- **Referente**: debe ser un identificador de registro válido, como un identificador de cuenta o un identificador de registro de contacto. <br>- **Tipo de referente**: debe ser un tipo de búsqueda, como `accounts` o `contacts`, respectivamente. <p><p>- **Cliente**: debe ser un identificador de registro válido, como un identificador de cuenta o un identificador de registro de contacto. <br>- **Tipo de cliente**: debe ser debe ser el tipo de búsqueda, como `accounts` o `contacts`, respectivamente. |
||||

En este ejemplo se muestra cómo la acción **Crear un nuevo registro** crea un nuevo registro "Tareas" que está asociado a otros registros de entidad, específicamente un registro de usuario y un registro de cuenta. La acción especifica los identificadores y los tipos de búsqueda para esos registros de entidad mediante valores que coinciden con los tipos de datos esperados para las propiedades pertinentes.

* En función de la propiedad **Propietario**, que especifica un identificador de usuario, y la propiedad **Tipo de propietario**, que especifica el tipo de búsqueda `systemusers`, la acción asocia el nuevo registro "Tareas" a un usuario específico.

* En función de la propiedad **Referente**, que especifica un identificador de registro, y la propiedad **Tipo de referente**, que especifica el tipo de búsqueda `accounts`, la acción asocia el nuevo registro "Tareas" con una cuenta específica.

![Crear un registro "Tareas" asociado a identificadores y tipos de búsqueda](./media/connect-common-data-service/create-new-record-task-properties.png)

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnicos basados en la descripción de Swagger del conector, como desencadenadores, acciones, límites y otros detalles, consulte la [página de referencia del conector](/connectors/commondataservice/).

## <a name="troubleshooting-problems"></a>Solución de problemas

### <a name="calls-from-multiple-environments"></a>Llamadas desde varios entornos

Ambos conectores, Common Data Service y Common Data Service (entorno actual), almacenan información sobre los flujos de trabajo de la aplicación lógica y reciben notificaciones sobre los cambios de entidad mediante la entidad `callbackregistrations` de Dataverse de Microsoft. Si copia una organización de Dataverse, también se copian los webhooks. Si copia la organización antes de deshabilitar los flujos de trabajo que están asignados a esta, los webhooks copiados también apuntarán a las mismas aplicaciones lógicas, que, a su vez, recibirán notificaciones de varias organizaciones.

Para detener las notificaciones no deseadas, siga estos pasos para eliminar el registro de devolución de llamada de la organización que envía esas notificaciones:

1. Identifique la organización de Dataverse de donde quiere quitar las notificaciones e inicie sesión en esa organización.

1. En el explorador Chrome, siga estos pasos para buscar el registro de devolución de llamada que quiere eliminar:

   1. Revise la lista genérica de todos los registros de devolución de llamada en el siguiente URI de OData para que pueda ver los datos dentro de la entidad `callbackregistrations`:

      `https://{organization-name}.crm{instance-number}.dynamics.com/api/data/v9.0/callbackregistrations`:

      > [!NOTE]
      > Si no se devuelve ningún valor, es posible que no tenga permisos para ver este tipo de entidad o que no haya iniciado sesión en la organización correcta.

   1. Filtre por el nombre lógico de la entidad desencadenadora `entityname` y el evento de notificación que coincida con el flujo de trabajo de la aplicación lógica (mensaje). Cada tipo de evento se asigna al entero del mensaje como se indica a continuación:

      | Tipo de evento | Entero del mensaje |
      |------------|-----------------|
      | Crear | 1 |
      | Eliminar | 2 |
      | Actualizar | 3 |
      | CreateOrUpdate | 4 |
      | CreateOrDelete | 5 |
      | UpdateOrDelete | 6 |
      | CreateOrUpdateOrDelete | 7 |
      |||

      En este ejemplo se muestra cómo filtrar por las notificaciones `Create` en una entidad llamada `nov_validation` mediante el siguiente URI de OData en una organización de ejemplo:

      `https://fabrikam-preprod.crm1.dynamics.com/api/data/v9.0/callbackregistrations?$filter=entityname eq 'nov_validation' and message eq 1`

      ![Captura de pantalla que muestra la ventana del explorador y el URI de OData en la barra de direcciones.](./media/connect-common-data-service/find-callback-registrations.png)

      > [!TIP]
      > Si hay varios desencadenadores para la misma entidad o evento, puede filtrar la lista usando filtros adicionales, como los atributos `createdon` y `_owninguser_value`. El nombre del usuario propietario aparece en `/api/data/v9.0/systemusers({id})`.

   1. Después de encontrar el identificador del registro de devolución de llamada que quiere eliminar, siga estos pasos:
   
      1. En el explorador Chrome, abra Herramientas de desarrollo de Chrome (teclado: F12).

      1. En la ventana, en la parte superior, seleccione la pestaña **Consola**.

      1. En el símbolo de la línea de comandos, escriba este comando, que envía una solicitud para eliminar el registro de devolución de llamada especificado:

         `fetch('http://{organization-name}.crm{instance-number}.dynamics.com/api/data/v9.0/callbackregistrations({ID-to-delete})', { method: 'DELETE'})`

         > [!IMPORTANT]
         > Asegúrese de realizar la solicitud desde una página de interfaz de cliente no unificada (UCI), por ejemplo, desde la propia página de respuesta de API o OData. De lo contrario, la lógica del archivo app.js podría interferir con esta operación.

   1. Para confirmar que el registro de devolución de llamada ya no existe, consulte la lista de registros de devolución de llamada.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre otros [conectores para Azure Logic Apps](../connectors/apis-list.md).
