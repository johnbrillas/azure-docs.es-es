---
title: Registros de diagnóstico de Azure Notification Hubs | Microsoft Docs
description: En este artículo se proporciona información general sobre todos los registros operativos y de diagnóstico que hay disponibles para Azure Notification Hubs.
author: brannon
ms.author: brjones
ms.service: notification-hubs
ms.topic: article
ms.date: 01/29/2021
ms.openlocfilehash: b98a04a70062461cec603bea83052c4f1224819e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101736243"
---
# <a name="enable-diagnostics-logs-for-notification-hubs"></a>Habilitación de registros de diagnóstico para Notification Hubs

Cuando empiece a usar el espacio de nombres de Azure Notification Hubs, puede que quiera supervisar cómo y cuándo se crea, se elimina o se accede al espacio de nombres. En este artículo encontrará información general de todos los registros operativos y de diagnóstico que hay disponibles.

Actualmente, Azure Notification Hubs admite registros operativos y de actividad, que capturan *operaciones de administración* que se realizan en el espacio de nombres de Azure Notification Hubs.

## <a name="diagnostic-logs-schema"></a>Esquema de registros de diagnósticos

Todos los registros se almacenan en el formato de notación de objetos JavaScript (JSON) en estas dos ubicaciones:

- **AzureActivity**: muestra registros de operaciones y acciones realizadas en el espacio de nombres de Azure Portal o mediante implementaciones de plantillas de Azure Resource Manager.
- **AzureDiagnostics**: muestra registros de operaciones y acciones realizadas en el espacio de nombres mediante la API o mediante clientes de administración en el SDK de lenguaje.

Las cadenas JSON de registros de diagnóstico incluyen los elementos enumerados en la tabla siguiente:

| Nombre | Descripción |
| ------- | ------- |
| time | Marca de tiempo UTC del registro |
| resourceId | Ruta de acceso relativa al recurso de Azure |
| operationName | Nombre de la operación de administración |
| category | Categoría del registro. Valores válidos: `OperationalLogs`. |
| callerIdentity | Identidad del autor de la llamada que inició la operación de administración. |
| resultType | Estado de la operación de administración. Valores válidos: `Succeeded` o `Failed` |
| resultDescription | Descripción de la operación de administración. |
| correlationId | Identificador de correlación de la operación de administración (si se especifica). |
| callerIpAddress | Dirección IP del autor de la llamada. Está vacío para las llamadas que se originaron en Azure Portal. |

Este es un ejemplo de una cadena JSON de registro operativo:

```json
{
    "operationName": "Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action",
    "resourceId": "/SUBSCRIPTIONS/2CAC2A14-BA6B-46A6-BCE8-2D9781A41BA2/RESOURCEGROUPS/SAMPLES/PROVIDERS/MICROSOFT.NOTIFICATIONHUBS/NAMESPACES/SAMPLE-NS",
    "time": "1/1/2021 5:16:32 AM +00:00",
    "category": "OperationalLogs",
    "resultType": "Succeeded",
    "resultDescription": "Gets Hub Authorization Rules",
    "correlationId": "00000000-0000-0000-0000-000000000000",
    "callerIdentity": "{ \"identityType\": \"Portal\", \"identity\": \"\" }"
}
```

El campo `callerIdentity` puede estar vacío o puede tener una cadena JSON con uno de los siguientes formatos.

En las llamadas que se originan desde Azure Portal, el campo `identity` está vacío. El registro se puede correlacionar con los registros de actividad para determinar el usuario que ha iniciado sesión.
```json
{
    "identityType": "Portal",
    "identity": ""
}
```

En el caso de las llamadas realizadas mediante Azure Resource Manager, el campo `identity` contendrá el nombre del usuario que ha iniciado sesión.
```json
{
   "identityType": "Username",
   "identity": "test@foo.com"
}
```

En el caso de las llamadas a la API REST de Notification Hubs, el campo `identity` contendrá el nombre de la directiva de acceso que se usa para generar el token SharedAccessSignature.
```json
{
   "identityType": "KeyName",
   "identity": "SharedAccessRootKey2"
}
```

## <a name="events-and-operations-captured-in-operational-logs"></a>Eventos y operaciones capturados en registros operativos

Los registros operativos capturan todas las operaciones de administración que se realizan en el espacio de nombres de Azure Notification Hubs. Las operaciones de datos no se capturan debido al elevado volumen de este tipo de operaciones que se realizan en Azure Notification Hubs.

Estas operaciones de administración se capturan en los registros operativos: 

| Ámbito | Nombre de la operación | Descripción de la operación |
| :-- | :-- | :-- |
| Espacio de nombres | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | Enumeración de reglas de autorización |
| Espacio de nombres | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Eliminación de reglas de autorización |
| Espacio de nombres | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | Enumera las claves |
| Espacio de nombres | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | Obtención de reglas de autorización |
| Espacio de nombres | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Regeneración de claves |
| Espacio de nombres | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Creación o actualización de reglas de autorización |
| Espacio de nombres | Microsoft.NotificationHubs/Namespaces/delete | Eliminar espacio de nombres |
| Espacio de nombres | Microsoft.NotificationHubs/Namespaces/read | Obtención del espacio de nombres |
| Espacio de nombres | Microsoft.NotificationHubs/Namespaces/write | Creación o actualización del espacio de nombres |
| Centro de notificaciones | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | Enumeración de reglas de autorización |
| Centro de notificaciones | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Eliminación de reglas de autorización |
| Centro de notificaciones | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | Enumera las claves |
| Centro de notificaciones | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | Lectura de reglas de autorización |
| Centro de notificaciones | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Regeneración de claves |
| Centro de notificaciones | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Creación o actualización de reglas de autorización |
| Centro de notificaciones | Microsoft.NotificationHubs/Namespaces/NotificationHubs/delete | Eliminación de un centro de notificaciones |
| Centro de notificaciones | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Creación, actualización y obtención de credenciales de PNS |
| Centro de notificaciones | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Obtención del centro de notificaciones |
| Centro de notificaciones | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Creación o actualización del centro de notificaciones |

## <a name="enable-operational-logs"></a>Habilitar registros operativos

Los registros operativos están deshabilitados de forma predeterminada. Para habilitar los registros, haga lo siguiente:

1. En [Azure Portal](https://portal.azure.com), vaya a su espacio de nombres de Azure Notification Hubs y, en **Supervisión**, seleccione **Configuración de diagnóstico**.

   ![Vínculo "Configuración de diagnóstico"](./media/notification-hubs-diagnostic-logs/image-1.png)

1. En el panel **Configuración de diagnóstico**, seleccione **Agregar configuración de diagnóstico**.  

   ![Vínculo "Agregar configuración de diagnóstico"](./media/notification-hubs-diagnostic-logs/image-2.png)

1. Para configurar las opciones de diagnóstico, siga estos pasos:

   a. En el cuadro **Nombre**, escriba un nombre para la configuración de diagnóstico.  

   b. Seleccione uno de estos destinos para los registros de diagnóstico:  
   - Si elige **Enviar a Log Analytics**, tiene que especificar a qué instancia de Log Analytics se enviarán los diagnósticos.  
   - Si elige **Archivar en una cuenta de almacenamiento**, tiene que configurar la cuenta de almacenamiento donde se almacenarán los registros de diagnóstico.  
   - Si selecciona **Transmitir a un centro de eventos**, tiene que configurar el centro de eventos al que quiere transmitir los registros de diagnóstico.

   c. Active la casilla **OperationalLogs**.

    ![Panel "Configuración de diagnóstico"](./media/notification-hubs-diagnostic-logs/image-3.png)

1. Seleccione **Guardar**.

La nueva configuración surte efecto en unos 10 minutos. Los registros aparecen en el destino de archivo configurado, en el panel **Registros de diagnóstico**.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la configuración de diagnóstico, consulte:
* [Información general de los registros de diagnóstico de Azure](../azure-monitor/essentials/platform-logs-overview.md).

Para más información sobre Azure Notification Hubs, consulte:
* [¿Qué es Azure Notification Hubs?](notification-hubs-push-notification-overview.md)