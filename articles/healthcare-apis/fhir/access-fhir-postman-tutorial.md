---
title: 'Servidor FHIR con Postman en Azure: Azure API for FHIR'
description: En este tutorial, se le guiará por los pasos necesarios para usar Postman para acceder a un servidor de FHIR. Postman es útil para depurar aplicaciones que tienen acceso a las API.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: dseven
ms.author: matjazl
author: matjazl
ms.date: 03/16/2021
ms.openlocfilehash: e9031dc77054a2bbac8015bbbdd7b9ed2a35e84f
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2021
ms.locfileid: "105043349"
---
# <a name="access-azure-api-for-fhir-with-postman"></a>Acceso a Azure API for FHIR con Postman

Una aplicación cliente puede tener acceso a la API de Azure para FHIR a través de una [API de REST](https://www.hl7.org/fhir/http.html). Para enviar solicitudes, ver respuestas y depurar la aplicación mientras se está generando, use una herramienta de prueba de API de su elección. En este tutorial, le guiaremos a través de los pasos necesarios para acceder al servidor de FHIR mediante [Postman](https://www.getpostman.com/). 

## <a name="prerequisites"></a>Requisitos previos

- Un punto de conexión de FHIR en Azure. 

   Para implementar la API de Azure para FHIR (un servicio administrado), puede usar el [Azure portal](fhir-paas-portal-quickstart.md), [PowerShell](fhir-paas-powershell-quickstart.md)o [CLI de Azure](fhir-paas-cli-quickstart.md).
- Una [aplicación cliente confidencial](register-confidential-azure-ad-client-app.md) registrada para tener acceso al servicio FHIR.
- Ha concedido permisos a la aplicación cliente confidencial, por ejemplo, "FHIR Data Contributor", para tener acceso al servicio FHIR. Para obtener más información, consulte [configuración de RBAC de Azure para FHIR](./configure-azure-rbac.md).
- Postman instalado. 
    
    Para obtener más información acerca de Postman, consulte Introducción [a Postman](https://www.getpostman.com).

## <a name="fhir-server-and-authentication-details"></a>Detalles de autenticación y servidor de FHIR

Para usar Postman, se necesitan los siguientes parámetros de autenticación:

- La dirección URL del servidor de FHIR, por ejemplo, `https://MYACCOUNT.azurehealthcareapis.com`

- El valor `Authority` del proveedor de identidades para el servidor de FHIR, por ejemplo `https://login.microsoftonline.com/{TENANT-ID}`

- La configuración `audience` que suele ser la dirección URL del servidor de FHIR, por ejemplo, `https://<FHIR-SERVER-NAME>.azurehealthcareapis.com` o `https://azurehealthcareapis.com` .

- El `client_id` identificador de aplicación o de la [aplicación cliente confidencial](register-confidential-azure-ad-client-app.md) que se usa para obtener acceso al servicio FHIR.

- El `client_secret` secreto de la aplicación o de la aplicación cliente confidencial.

Por último, debe comprobar que `https://www.getpostman.com/oauth2/callback` es una dirección URL de respuesta registrada para la aplicación cliente.

## <a name="connect-to-fhir-server"></a>Conexión al servidor de FHIR

Abra Postman y, a continuación, seleccione **obtener** para realizar una solicitud a `https://fhir-server-url/metadata` .

![Instrucción de funcionalidad de metadatos de Postman](media/tutorial-postman/postman-metadata.png)

La dirección URL de metadatos para Azure API for FHIR es `https://MYACCOUNT.azurehealthcareapis.com/metadata`. 

En este ejemplo, la dirección URL del servidor `https://fhirdocsmsft.azurewebsites.net` de FHIR es y la instrucción de capacidad del servidor está disponible en `https://fhirdocsmsft.azurewebsites.net/metadata` . Este punto de conexión es accesible sin autenticación.

Si intenta obtener acceso a recursos restringidos, se produce una respuesta de "error de autenticación".

![Error de autenticación](media/tutorial-postman/postman-authentication-failed.png)

## <a name="obtaining-an-access-token"></a>Obtención de un token de acceso
Para obtener un token de acceso válido, seleccione **autorización** y seleccione **OAuth 2,0** en el menú desplegable **tipo** .

![Establecimiento de OAuth 2.0](media/tutorial-postman/postman-select-oauth2.png)

Seleccione **Obtener token de acceso nuevo**.

![Solicitar nuevo token de acceso](media/tutorial-postman/postman-request-token.png)

En el cuadro de diálogo **obtener nuevo token de acceso** , escriba los detalles siguientes:

| Campo                 | Valor de ejemplo                                                                                                   | Comentario                    |
|-----------------------|-----------------------------------------------------------------------------------------------------------------|----------------------------|
| Nombre del token            | MYTOKEN                                                                                                         | Un nombre que elija          |
| Tipo de concesión            | Código de autorización                                                                                              |                            |
| Dirección URL de devolución de llamadas          | `https://www.getpostman.com/oauth2/callback`                                                                      |                            |
| Dirección URL de autenticación              | `https://login.microsoftonline.com/{TENANT-ID}/oauth2/authorize?resource=<audience>` | El valor`audience` es `https://MYACCOUNT.azurehealthcareapis.com` para Azure API for FHIR |
| Dirección URL del token de acceso      | `https://login.microsoftonline.com/{TENANT ID}/oauth2/token`                                                      |                            |
| Id. de cliente             | `XXXXXXXX-XXX-XXXX-XXXX-XXXXXXXXXXXX`                                                                            | Identificador de aplicación             |
| Secreto del cliente         | `XXXXXXXX`                                                                                                        | Clave de cliente secreta          |
| Ámbito | `<Leave Blank>` |
| State                |  `1234`                                                                                                           |                            |
| Autenticación de clientes | Enviar credenciales de cliente en el cuerpo                                                                                 |                 

Seleccione el **token de solicitud** que se guiará a través del flujo de autenticación de Azure Active Directory y se devolverá un token a Postman. Si se produce un error de autenticación, consulte la consola de Postman para obtener más detalles. **Nota**: en la cinta de opciones, seleccione **Ver** y, a continuación, seleccione **Mostrar consola de Postman**. El método abreviado de teclado para la consola de Postman es **Alt-Ctrl + C**.

Desplácese hacia abajo para ver la pantalla de token devuelta y, a continuación, seleccione **usar token**.

![Usar token](media/tutorial-postman/postman-use-token.png)

Consulte el campo **token de acceso** para ver el token que se acaba de rellenar. Si selecciona **Enviar** para repetir la `Patient` búsqueda de recursos, se devuelve un **Estado** `200 OK` . Un estado devuelto `200 OK` indica una solicitud HTTP correcta.

![200 OK](media/tutorial-postman/postman-200-OK.png)

En el ejemplo de *búsqueda de pacientes* , no hay ningún paciente en la base de datos de modo que el resultado de la búsqueda esté vacío.

Puede inspeccionar el token de acceso mediante una herramienta como [JWT.ms](https://jwt.ms). A continuación se muestra un ejemplo del contenido de.

```json
{
  "aud": "https://MYACCOUNT.azurehealthcareapis.com",
  "iss": "https://sts.windows.net/{TENANT-ID}/",
  "iat": 1545343803,
  "nbf": 1545343803,
  "exp": 1545347703,
  "acr": "1",
  "aio": "AUQAu/8JXXXXXXXXXdQxcxn1eis459j70Kf9DwcUjlKY3I2G/9aOnSbw==",
  "amr": [
    "pwd"
  ],
  "appid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "oid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "appidacr": "1",

  ...// Truncated
}
```

En situaciones de solución de problemas, un buen punto de partida es validar que tiene la audiencia correcta (notificación `aud`). Si el token es del emisor correcto ( `iss` notificaciones) y tiene la audiencia correcta ( `aud` Claim), pero sigue sin poder acceder a la API FHIR, es probable que el usuario o la entidad de servicio ( `oid` Claim) no tenga acceso al plano de datos FHIR. Se recomienda usar [el control de acceso basado en roles de Azure (RBAC de Azure)](configure-azure-rbac.md) para asignar roles de plano de datos a los usuarios. Si usa un inquilino externo de Azure Active Directory secundario para su plano de datos, deberá [configurar RBAC local para las asignaciones de FHIR](configure-local-rbac.md) .

También es posible obtener un token para la [API de Azure para FHIR mediante el CLI de Azure](get-healthcare-apis-access-token-cli.md). Si usa un token obtenido con el CLI de Azure, debe usar el *token de portador* del tipo de autorización. Pegue el token directamente en.

## <a name="inserting-a-patient"></a>Inserción de un paciente

Con un token de acceso válido, ahora puede insertar un nuevo paciente. En Postman, cambie el método seleccionando **post** y, a continuación, agregue el siguiente documento JSON en el cuerpo de la solicitud.

[!code-json[](../samples/sample-patient.json)]

Seleccione **Enviar** para determinar que el paciente se ha creado correctamente.

![Captura de pantalla que muestra que el paciente se ha creado correctamente.](media/tutorial-postman/postman-patient-created.png)

Si repite la búsqueda de pacientes, ahora debería ver el registro del paciente:

![Paciente creado](media/tutorial-postman/postman-patient-found.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha tenido acceso a la API de Azure para FHIR mediante Postman. Para obtener más información acerca de las características de la API de Azure para FHIR, consulte
 
>[!div class="nextstepaction"]
>[Características compatibles](fhir-features-supported.md)
