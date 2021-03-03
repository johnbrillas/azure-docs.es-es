---
title: Configuración de un Pase de acceso temporal en Azure AD para registrar métodos de autenticación sin contraseña
description: Obtenga información sobre cómo configurar y permitir que los usuarios registren métodos de autenticación sin contraseña mediante un Pase de acceso temporal (TAP).
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: justinha
author: inbarckms
manager: daveba
ms.reviewer: inbarckms
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56d45119fa86ab47e6a625c628d8cb9763db83bd
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100520792"
---
# <a name="configure-temporary-access-pass-in-azure-ad-to-register-passwordless-authentication-methods-preview"></a>Configuración de un Pase de acceso temporal en Azure AD para registrar métodos de autenticación sin contraseña (versión preliminar)

Los métodos de autenticación sin contraseña, como FIDO2 y el inicio de sesión telefónico sin contraseña a través de la aplicación Microsoft Authenticator, permiten a los usuarios iniciar sesión de manera segura sin una contraseña. Los usuarios pueden arrancar métodos sin contraseña de una de dos maneras:

- Usando los métodos de autenticación multifactor de Azure AD existentes 
- Usando un Pase de acceso temporal (TAP) 

TAP es un código de acceso de tiempo limitado emitido por un administrador, que satisface los requisitos de autenticación sólida y se puede usar para incorporar otros métodos de autenticación, incluidos aquellos sin contraseña. Además, TAP facilita la recuperación cuando un usuario ha perdido u olvidado su factor de autenticación sólida, como una llave de seguridad FIDO2 o la aplicación Microsoft Authenticator, pero debe iniciar sesión para registrar nuevos métodos de autenticación sólida.


En este artículo se muestra cómo habilitar y usar TAP en Azure AD mediante Azure Portal. También puede realizar estas acciones con las API REST. 

>[!NOTE]
>El Pase de acceso temporal está actualmente en versión preliminar pública. Es posible que algunas características no se admitan o que tengan funcionalidades limitadas. 

## <a name="enable-the-tap-policy"></a>Habilitación de la directiva de TAP

Una directiva de TAP define la configuración, como la duración de los pases creados en el inquilino, o los usuarios y grupos que pueden usar un TAP para iniciar sesión. Antes de que un usuario pueda iniciar sesión con un TAP, debe habilitar la directiva de método de autenticación y elegir qué usuarios y grupos pueden iniciar sesión mediante un TAP.
Aunque puede crear un TAP para cualquier usuario, solo aquellos incluidos en la directiva pueden iniciar sesión con él.

Los titulares del rol Administrador global y Administrador de directiva de método de autenticación pueden actualizar la directiva de método de autenticación TAP.
Para configurar la directiva de método de autenticación TAP:

1. Inicie sesión en Azure Portal como Administrador global y haga clic en **Azure Active Directory** > **Seguridad** > **Método de autenticación** > **Pase de acceso temporal**.
1. Haga clic en **Sí** para habilitar la directiva, seleccione qué usuarios tienen aplicada la directiva y los valores de **General**.

   ![Captura de pantalla de cómo habilitar la directiva de método de autenticación TAP](./media/how-to-authentication-temporary-access-pass/policy.png)

   En la tabla siguiente se describen el valor predeterminado y el intervalo de valores permitidos.


   | Configuración          | Valores predeterminados | Valores permitidos               | Comentarios                                                                                                                                                                                                                                                                 |   |
   |------------------|----------------|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
    Duración mínima | 1 hora         | De 10 a 43 200 minutos (30 días) | Número mínimo de minutos que TAP es válido.                                                                                                                                                                                                                         |   |
   | Duración máxima | 24 horas       | De 10 a 43 200 minutos (30 días) | Número máximo de minutos que TAP es válido.                                                                                                                                                                                                                         |   |
   | Duración predeterminada | 1 hora         | De 10 a 43 200 minutos (30 días) | Los valores predeterminados se pueden invalidar mediante pases individuales, dentro de la vigencia mínima y máxima configurada por la directiva.                                                                                                                                                |   |
   | Uso único     | False          | True o False                 | Cuando la directiva se establece en False, se pueden usar los pases en el inquilino una vez o más de una vez durante su validez (vigencia máxima). Al aplicar un uso único en la directiva de TAP, todos los pases creados en el inquilino se crearán como de un solo uso. |   |
   | Length           | 8              | De 8 a 48 caracteres              | Define la longitud del código de acceso.                                                                                                                                                                                                                                      |   |

## <a name="create-a-tap-in-the-azure-ad-portal"></a>Creación de un TAP en el portal de Azure AD

Después de habilitar una directiva de TAP, puede crear un TAP para un usuario en Azure AD. Estos roles pueden realizar las siguientes acciones relacionadas con TAP.

- El Administrador global puede crear, eliminar y ver el TAP en cualquier usuario (excepto en sí mismo).
- Los Administradores de autenticación con privilegios pueden crear, eliminar y ver el TAP en administradores y miembros (excepto en sí mismos).
- Los Administradores de autenticación pueden crear, eliminar y ver el TAP en miembros (excepto en sí mismos).
- El Administrador global puede ver los detalles del TAP en el usuario (sin leer el propio código).

Para crear un TAP:

1. Inicie sesión en el portal como Administrador global, Administrador de autenticación con privilegios o Administrador de autenticación. 
1. Haga clic en **Azure Active Directory**, vaya a Usuarios, seleccione un usuario, como *Chris Green*, y, a continuación, elija **Métodos de autenticación**.
1. Si es necesario, seleccione la opción para **Try the new user authentication methods experience** (Probar la nueva experiencia de métodos de autenticación de usuario).
1. Seleccione la opción para **Add authentication methods** (Agregar métodos de autenticación).
1. Debajo de **Seleccionar método**, haga clic en **Temporary Access Pass (Preview)** (Pase de acceso temporal [versión preliminar]).
1. Defina una duración o una hora de activación personalizada y haga clic en **Agregar**.

   ![Captura de pantalla sobre cómo crear un TAP](./media/how-to-authentication-temporary-access-pass/create.png)

1. Una vez que se agregue, se muestran los detalles del TAP. Tome nota del valor real del TAP. Este valor se proporcionará al usuario. No puede ver este valor después de hacer clic en **Aceptar**.
   
   ![Captura de pantalla de los detalles del TAP](./media/how-to-authentication-temporary-access-pass/details.png)

## <a name="use-a-tap"></a>Uso de un TAP

El uso más común de un TAP es para que un usuario registre los detalles de autenticación durante el primer inicio de sesión, sin necesidad de completar mensajes de seguridad adicionales. Los métodos de autenticación se registran en [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo). Los usuarios también pueden actualizar los métodos de autenticación existentes aquí.

1. Abra un explorador web en [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo).
1. Escriba el nombre principal de usuario de la cuenta para la que ha creado el TAP, como *tapuser@contoso.com* .
1. Si el usuario está incluido en la directiva de TAP, verá una pantalla para escribir su TAP.
1. Escriba el TAP que se mostró en Azure Portal.

   ![Captura de pantalla sobre cómo escribir un TAP](./media/how-to-authentication-temporary-access-pass/enter.png)

>[!NOTE]
>En el caso de los dominios federados, se prefiere un TAP frente a la federación. Un usuario con un TAP completará la autenticación en Azure AD y no se le redirigirá al proveedor de identidades federado (IdP).

El usuario ahora ha iniciado sesión y puede actualizar o registrar un método, como una llave de seguridad FIDO2. Los usuarios que actualicen sus métodos de autenticación debido a la pérdida de sus credenciales o dispositivos deben asegurarse de que quitan los métodos de autenticación antiguos.

Los usuarios también pueden usar su TAP para registrarse en el inicio de sesión telefónico sin contraseña directamente desde la aplicación Authenticator. Para obtener más información, consulte [Agregar la cuenta profesional o educativa a la aplicación Microsoft Authenticator](../user-help/user-help-auth-app-add-work-school-account.md).

![Captura de pantalla de cómo escribir un TAP con una cuenta profesional o educativa](./media/how-to-authentication-temporary-access-pass/enter-work-school.png)

## <a name="delete-a-tap"></a>Eliminación de un TAP

No se puede usar un TAP expirado. Debajo de **Métodos de autenticación** de un usuario, la columna **Detalle** muestra cuándo expiró el TAP. Puede eliminar estos TAP caducados siguiendo estos pasos:

1. En el portal de Azure AD, vaya a **Usuarios**, seleccione un usuario, por ejemplo, *Usuario de TAP* y, después, elija **Métodos de autenticación**.
1. En el lado derecho del método de autenticación **Temporary Access Pass (Preview)** (Pase de acceso temporal [vista previa]) que se muestra en la lista, seleccione **Eliminar**.

## <a name="replace-a-tap"></a>Reemplazo de un TAP 

- Un usuario solo puede tener un TAP. El código de acceso se puede usar durante las horas de inicio y finalización del TAP.
- Si el usuario requiere un nuevo TAP:
  - Si el TAP existente es válido, el administrador debe eliminar el TAP existente y crear un nuevo pase para el usuario. Si se elimina un TAP válido, se revocarán las sesiones del usuario. 
  - Si el TAP existente ha expirado, un nuevo TAP invalidará el TAP existente y no revocará las sesiones del usuario.

Para obtener más información sobre los estándares de NIST para la incorporación y la recuperación, consulte la [publicación especial de NIST 800-63A](https://pages.nist.gov/800-63-3/sp800-63a.html#sec4).

## <a name="limitations"></a>Limitaciones

Tenga en cuenta las limitaciones siguientes:

- Cuando se usa un TAP de un uso para registrar un método sin contraseña, como FIDO2 o el inicio de sesión telefónico, el usuario debe completar el registro en un plazo de 10 minutos a partir del inicio de sesión con el TAP de único uso. Esta limitación no se aplica a un TAP que se puede usar más de una vez.
- Los usuarios invitados no pueden iniciar sesión con un TAP.
- Los usuarios en el ámbito de la directiva de registro de autoservicio de restablecimiento de contraseña (SSPR) tendrán que registrar uno de los métodos de SSPR después de haber iniciado sesión con TAP. Si el usuario solo va a usar una llave FIDO2, exclúyalo de la directiva SSPR o deshabilite la directiva de registro de SSPR. 
- TAP no se puede usar con la extensión del servidor de directivas de redes (NPS) y el adaptador de Servicios de federación de Active Directory (AD FS).
- Cuando el inicio de sesión único de conexión directa está habilitado en el inquilino, se pide a los usuarios que escriban una contraseña. El vínculo **Use your Temporary Access Pass instead** (Usar el Pase de acceso temporal en su lugar) estará disponible para que el usuario inicie sesión con TAP.

![Captura de pantalla de Usar un TAP en su lugar](./media/how-to-authentication-temporary-access-pass/alternative.png)

## <a name="troubleshooting"></a>Solución de problemas    

- Si no se ofrece TAP a un usuario durante el inicio de sesión, compruebe lo siguiente:
  - El usuario está en el ámbito de la directiva del método de autenticación TAP.
  - El usuario tiene un TAP válido y, si es de un solo uso, todavía no se ha usado.
- Si aparece **Temporary Access Pass sign in was blocked due to User Credential Policy** (El inicio de sesión con el Pase de acceso temporal se bloqueó debido a la directiva de credenciales de usuario) durante el inicio de sesión con TAP, compruebe lo siguiente:
  - El usuario tiene un TAP de varios usos, mientras que la directiva de métodos de autenticación requiere un TAP de un solo uso.
  - Ya se ha usado un TAP de un solo uso.

## <a name="next-steps"></a>Pasos siguientes

- [Planeamiento de una implementación de autenticación sin contraseña en Azure Active Directory](howto-authentication-passwordless-deployment.md)

