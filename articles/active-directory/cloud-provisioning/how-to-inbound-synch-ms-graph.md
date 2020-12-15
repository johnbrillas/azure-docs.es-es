---
title: Sincronización de entrada para el aprovisionamiento en la nube mediante MS Graph API
description: En este tema se describe cómo habilitar la sincronización de entrada con solo Graph API.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/04/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f308f46fc021a1d08f4065d48558a6dd71786c7c
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96860362"
---
# <a name="inbound-synchronization-for-cloud-provisioning-using-ms-graph-api"></a>Sincronización de entrada para el aprovisionamiento en la nube mediante MS Graph API

En el documento siguiente se describe cómo replicar un perfil de sincronización desde cero usando solo instancias de MS Graph API.  
La estructura de este procedimiento consta de los siguientes pasos.  Son las siguientes:

- [Configuración básica](#basic-setup)
- [Creación de las entidades de servicio](#create-service-principals)
- [Creación del trabajo de sincronización](#create-sync-job)
- [Actualización del dominio de destino](#update-targeted-domain)
- [Inicio del trabajo de sincronización](#start-sync-job)
- [Revisión del estado](#review-status)

Use estos comandos del [Módulo Microsoft Azure Active Directory para Windows PowerShell](https://docs.microsoft.com/powershell/module/msonline/) para permitir la sincronización de un inquilino de producción, un requisito previo para poder llamar al servicio web de administración de ese inquilino.

## <a name="basic-setup"></a>Configuración básica

### <a name="enable-tenant-flags"></a>Habilitación de las marcas de inquilino

 ```PowerShell
 Connect-MsolService ('-AzureEnvironment <AzureEnvironmnet>')
 Set-MsolDirSyncEnabled -EnableDirSync $true
 ```
El primero de estos dos comandos requiere las credenciales de Azure Active Directory. Estos commandlets identifican de manera implícita el inquilino y permiten que se sincronice.

## <a name="create-service-principals"></a>Creación de entidades de servicio
A continuación, es necesario crear la [aplicación o la entidad de servicio de AD2AAD](https://docs.microsoft.com/graph/apiapplicationtemplate-instantiate?view=graph-rest-beta&tabs=http).

Debe usar el identificador de aplicación 1a4721b3-e57f-4451-ae87-ef078703ec94. El valor de displayName es la dirección URL del dominio de AD, si se usa en el portal (por ejemplo, contoso.com), pero puede tener un nombre diferente.

 ```
 POST https://graph.microsoft.com/beta/applicationTemplates/1a4721b3-e57f-4451-ae87-ef078703ec94/instantiate
 Content-type: application/json
 {
    displayName: [your app name here]
 }
 ```


## <a name="create-sync-job"></a>Creación del trabajo de sincronización
La salida del comando anterior devolverá el valor de objectId de la entidad de servicio que se creó. En este ejemplo, es 614ac0e9-a59b-481f-bd8f-79a73d167e1c.  Use Microsoft Graph para agregar un trabajo de sincronización a esa entidad de servicio.  

Puede encontrar documentación sobre la creación de un trabajo de sincronización [aquí](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-post?view=graph-rest-beta&tabs=http).

Si no registró el identificador anterior, puede encontrar la entidad de servicio mediante la ejecución de la siguiente llamada de MS Graph. Para hacer esa llamada, necesitará los permisos Directory.Read.All:
 
 `GET https://graph.microsoft.com/beta/servicePrincipals `

A continuación, busque el nombre de la aplicación en la salida.

Ejecute los dos comandos siguientes para crear dos trabajos: uno para el aprovisionamiento de usuarios o grupos y otro para la sincronización del hash de contraseñas. La solicitud es la misma dos veces, pero con distintos identificadores de plantilla.


Llame a las dos solicitudes siguientes:

 ```
 POST https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs
 Content-type: application/json
 {
 "templateId":"AD2AADProvisioning"
 } 
 ```

 ```
 POST https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs
 Content-type: application/json
 {
 "templateId":"AD2AADPasswordHash"
 }
 ```

Necesitará dos llamadas si quiere crear ambas.

Valor devuelto de ejemplo (para el aprovisionamiento):

 ```
HTTP 201/Created
{
    "@odata.context": "https://graph.microsoft.com/beta/$metadata#servicePrincipals('614ac0e9-a59b-481f-bd8f-79a73d167e1c')/synchronization/jobs/$entity",
    "id": "AD2AADProvisioning.fc96887f36da47508c935c28a0c0b6da",
    "templateId": "ADDCInPassthrough",
    "schedule": {
        "expiration": null,
        "interval": "PT40M",
        "state": "Disabled"
    },
    "status": {
        "countSuccessiveCompleteFailures": 0,
        "escrowsPruned": false,
        "code": "Paused",
        "lastExecution": null,
        "lastSuccessfulExecution": null,
        "lastSuccessfulExecutionWithExports": null,
        "quarantine": null,
        "steadyStateFirstAchievedTime": "0001-01-01T00:00:00Z",
        "steadyStateLastAchievedTime": "0001-01-01T00:00:00Z",
        "troubleshootingUrl": null,
        "progress": [],
        "synchronizedEntryCountByType": []
    }
}
```

## <a name="update-targeted-domain"></a>Actualización del dominio de destino
En este inquilino, el identificador de objeto y el identificador de aplicación de la entidad de servicio son los siguientes:

ObjectId: 8895955e-2e6c-4d79-8943-4d72ca36878f AppId: 00000014-0000-0000-c000-000000000000 DisplayName: testApp

Vamos a tener que actualizar el dominio de destino de esta configuración, por lo que habrá que actualizar los secretos de este dominio.

Asegúrese de que el nombre de dominio que usa sea la misma dirección URL que estableció para el controlador de dominio local.

 ```
 PUT https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/secrets
 Content-type: application/json
 {
  "value": [
    {
      "key": "Domain",
       {"value":[{"key":"Domain","value":"{\"domain\":\"[YOUR_DOMAIN_NAME]\"}"}]}
    }
  ]
 }
 ```

La respuesta esperada es: HTTP 204/Sin contenido.

Aquí, el valor de "dominio" resaltado es el nombre del dominio local Active Directory desde el que se van a aprovisionar las entradas para Azure Active Directory.

## <a name="start-sync-job"></a>Inicio del trabajo de sincronización
El trabajo se puede volver a recuperar con el siguiente comando:

 `GET https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs/ ` 

Puede encontrar documentación sobre cómo recuperar trabajos [aquí](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-list?view=graph-rest-beta&tabs=http). 
 
Para iniciar el trabajo, emita esta solicitud con el identificador de objeto de la entidad de servicio creada en el primer paso y el identificador de trabajo devuelto por la solicitud que creó el trabajo.

Puede encontrar documentación sobre cómo iniciar un trabajo [aquí](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-start?view=graph-rest-beta&tabs=http). 

 ```
 POST  https://graph.microsoft.com/beta/servicePrincipals/8895955e-2e6c-4d79-8943-4d72ca36878f/synchronization/jobs/AD2AADProvisioning.fc96887f36da47508c935c28a0c0b6da/start
 ```

La respuesta esperada es: HTTP 204/Sin contenido.

Otros comandos para controlar el trabajo se documentan [aquí](https://docs.microsoft.com/graph/api/resources/synchronization-synchronizationjob?view=graph-rest-beta).
 
Para reiniciar un trabajo, usaría:

 ```
 POST  https://graph.microsoft.com/beta/servicePrincipals/8895955e-2e6c-4d79-8943-4d72ca36878f/synchronization/jobs/AD2AADProvisioning.fc96887f36da47508c935c28a0c0b6da/restart
 {
   "criteria": {
       "resetScope": "Full"
   }
 }
 ```

## <a name="review-status"></a>Revisión del estado
Recupere los estados de trabajo mediante:

 ```
 GET https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs/ 
 ```

Busque los detalles pertinentes en la sección "estado" del objeto devuelto.

## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
- [Transformaciones](how-to-transformation.md)
- [API de sincronización de Azure AD](https://docs.microsoft.com/graph/api/resources/synchronization-overview?view=graph-rest-beta)
