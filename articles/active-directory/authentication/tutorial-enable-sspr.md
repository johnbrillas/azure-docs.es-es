---
title: Habilitación del autoservicio de restablecimiento de contraseña de Azure Active Directory
description: En este tutorial aprenderá a habilitar el autoservicio de restablecimiento de contraseña de Azure Active Directory para un grupo de usuarios y a probar el proceso de restablecimiento de contraseña.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 03/23/2021
ms.author: justinha
author: justinha
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 253aa080b9c160141a274c57e0895291c78d2048
ms.sourcegitcommit: a67b972d655a5a2d5e909faa2ea0911912f6a828
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104887774"
---
# <a name="tutorial-enable-users-to-unlock-their-account-or-reset-passwords-using-azure-active-directory-self-service-password-reset"></a>Tutorial: Habilitación del autoservicio de restablecimiento de contraseña de Azure Active Directory para que los usuarios puedan desbloquear su cuenta o restablecer contraseñas

El autoservicio de restablecimiento de contraseña (SSPR) de Azure Active Directory (Azure AD) ofrece a los usuarios la posibilidad de cambiar o restablecer su contraseña sin necesidad de que intervenga el administrador o el departamento de soporte técnico. Si la cuenta de un usuario está bloqueada o se ha olvidado su contraseña, puede seguir las indicaciones para desbloquearse y volver al trabajo. Esta capacidad reduce las llamadas al departamento de soporte técnico y la pérdida de productividad cuando un usuario no puede iniciar sesión en su dispositivo o en una aplicación. Consulte este vídeo sobre [Configuración y habilitación del autoservicio de restablecimiento de contraseña en el inquilino](https://www.youtube.com/watch?v=rA8TvhNcCvQ) (**recomendado**). También disponemos de un vídeo para los administradores de TI en [Resolución de los seis mensajes de error de usuario final más comunes con SSPR](https://www.youtube.com/watch?v=9RPrNVLzT8I).

> [!IMPORTANT]
> Este tutorial muestra al administrador cómo habilitar SSPR. Los usuarios finales registrados para el restablecimiento de contraseña de autoservicio que necesiten volver a su cuenta deben ir a https://aka.ms/sspr.
>
> Si el equipo de TI no ha habilitado la capacidad para restablecer su propia contraseña, póngase en contacto con el departamento de soporte técnico para obtener ayuda adicional.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Habilitar el autoservicio de restablecimiento de contraseña para un grupo de usuarios de Azure AD
> * Configurar métodos de autenticación y opciones de registro
> * Probar el proceso SSPR como usuario

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará los siguientes recursos y privilegios:

* Un inquilino de Azure AD activo con al menos una licencia de Azure AD Free o de prueba habilitada. En el nivel Gratis, SSPR solo funciona para los usuarios de la nube en Azure AD.
    * En los tutoriales posteriores de esta serie, se necesita una licencia de Azure AD Premium P1 o de prueba para la escritura diferida de contraseñas local.
    * Si es preciso, [cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una cuenta con privilegios de *Administrador global*.
* Un usuario que no sea administrador con una contraseña que conozca, como *usuarioDePrueba*. Utilizará esta cuenta para probar la experiencia de SSPR por parte del usuario final en este tutorial.
    * Si necesita crear un usuario, consulte [Inicio rápido: Incorporación de nuevos usuarios a Azure Active Directory](../fundamentals/add-users-azure-active-directory.md).
* Un grupo del que sea miembro el usuario que no es administrador, como *Grupo-Prueba-SSPR*. En este tutorial, habilitará el autoservicio de restablecimiento de contraseña para este grupo.
    * Si necesita crear un grupo, consulte [Creación de un grupo y adición de miembros en Azure Active Directory](../fundamentals/active-directory-groups-create-azure-portal.md).

## <a name="enable-self-service-password-reset"></a>Habilitar el autoservicio de restablecimiento de contraseña

Azure AD permite habilitar el autoservicio de restablecimiento de contraseña para *Ninguno*, *Seleccionados* o *Todos* los usuarios. Esta capacidad para pormenorizar permite elegir un subconjunto de usuarios para probar el proceso de registro y el flujo de trabajo de SSPR. Cuando esté familiarizado con el proceso y pueda comunicar los requisitos con un conjunto más amplio de usuarios, podrá seleccionar un grupo de usuarios para habilitarlos en el autoservicio de restablecimiento de contraseña. O bien, podrá habilitar SSPR para todos los usuarios del inquilino de Azure AD.

> [!NOTE]
>
> Actualmente solo se puede habilitar un grupo de Azure AD para SSPR mediante Azure Portal. Como parte de una implementación más amplia del autoservicio de restablecimiento de contraseña, se admiten los grupos anidados. Asegúrese de que los usuarios de los grupos que elija tienen asignadas las licencias correspondientes. Actualmente no hay ningún proceso de validación de estos requisitos de licencia.

En este tutorial, configurará SSPR para un conjunto de usuarios de un grupo de prueba. En el ejemplo siguiente, se usa el grupo *Grupo-Prueba-SSPR*. Proporcione su propio grupo de Azure AD según sea necesario:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) mediante una cuenta con permisos de *administrador global*.
1. Busque y seleccione **Azure Active Directory** y, a continuación, elija **Restablecer la contraseña** en el menú del lado izquierdo.
1. En la página **Propiedades**, bajo la opción *Se habilitó el restablecimiento de contraseña del autoservicio*, elija **Seleccionar grupo**.
1. Busque y seleccione el grupo de Azure AD, como *Grupo-Prueba-SSPR* y, a continuación, elija *Seleccionar*.

    [ ![Selección de un grupo en Azure Portal para habilitar el autoservicio de restablecimiento de contraseña](media/tutorial-enable-sspr/enable-sspr-for-group-cropped.png) ](media/tutorial-enable-sspr/enable-sspr-for-group.png#lightbox)

1. Para habilitar SSPR para los usuarios seleccionados, seleccione **Guardar**.

## <a name="select-authentication-methods-and-registration-options"></a>Selección de métodos de autenticación y opciones de registro

Cuando los usuarios necesitan desbloquear su cuenta o restablecer su contraseña, se les pide un método de confirmación adicional. Este factor de autenticación adicional garantiza que solo se completan los eventos SSPR aprobados. Puede elegir los métodos de autenticación que se permitirán, en función de la información de registro que proporciona el usuario.

1. En la página **Métodos de autenticación** del menú del lado izquierdo, establezca el valor de **Número de métodos requeridos para el restablecimiento** en *1*.

    Si desea mejorar la seguridad, puede aumentar el número de métodos de autenticación necesarios para el autoservicio de restablecimiento de contraseña.

1. En **Métodos disponibles para los usuarios**, elija qué métodos desea permitir en la organización. En este tutorial, active las casillas para habilitar los siguientes métodos:

    * *Notificación en aplicación móvil*
    * *Código de aplicación móvil*
    * *Correo electrónico*
    * *Teléfono móvil*

    Es posible habilitar métodos de autenticación adicionales, como el *teléfono del trabajo* o *preguntas de seguridad* de acuerdo con las necesidades de su empresa.

1. Para aplicar los métodos de autenticación, seleccione **Guardar**.

Para que los usuarios puedan desbloquear su cuenta o restablecer una contraseña, deben registrar su información de contacto. Esta información de contacto se utiliza para los distintos métodos de autenticación configurados en los pasos anteriores.

Un administrador puede proporcionar manualmente esta información de contacto, o bien los usuarios pueden ir a un portal de registro para proporcionarla ellos mismos. En este tutorial, configure los usuarios para que se les solicite el registro la próxima vez que inicien sesión.

1. En la página **Registro** del menú del lado izquierdo, seleccione *Sí* en la opción **¿Desea exigir a los usuarios que se registren al iniciar sesión?**
1. Es importante que la información de contacto se mantenga actualizada. Si no está actualizada cuando se inicia un evento de SSPR, es posible que el usuario no pueda desbloquear su cuenta ni restablecer su contraseña.

    Establezca el valor de **Número de días que pasan hasta que se pide a los usuarios que vuelvan a confirmar su información de autenticación** en *180*.
1. Para aplicar la configuración de registro, seleccione **Guardar**.

## <a name="configure-notifications-and-customizations"></a>Configuración de notificaciones y personalizaciones

Para mantener informados a los usuarios sobre la actividad de la cuenta, puede configurar el envío de notificaciones por correo electrónico cuando se produzca un evento SSPR. Estas notificaciones pueden abarcar tanto las cuentas de usuario normales como las cuentas de administrador. En el caso de las cuentas de administrador, esta notificación proporciona un nivel adicional de reconocimiento cuando se restablece la contraseña de una cuenta de administrador con privilegios mediante SSPR. Todos los administradores globales recibirán una notificación cuando se utilice SSPR en una cuenta de administrador.

1. En la página **Notificaciones** del menú del lado izquierdo, configure las siguientes opciones:

   * Establezca la opción **Notificar a los usuarios el restablecimiento de contraseña** en *Sí*.
   * Establezca **Notificar a todos los administradores cuando otros administradores restablezcan su contraseña** en *Sí*.

1. Para aplicar las preferencias de notificación, seleccione **Guardar**.

Si los usuarios necesitan ayuda adicional con el proceso de autoservicio de restablecimiento de contraseña, puede personalizar el vínculo de "Póngase en contacto con el administrador". Este vínculo se usa en el proceso de registro de SSPR y cuando un usuario desbloquea su cuenta o restablece su contraseña. Para asegurarse de que los usuarios obtengan el soporte técnico necesario, se recomienda encarecidamente que se indique una dirección URL o un correo electrónico de soporte técnico personalizados.

1. En la página **Personalización** del menú del lado izquierdo, establezca *Personalizar el vínculo del departamento de soporte técnico* en **Sí**.
1. En el campo **Dirección URL o correo electrónico del departamento de soporte técnico personalizados**, indique una dirección de correo electrónico o una dirección URL de la página web donde los usuarios puedan obtener ayuda adicional de su organización, como *`https://support.contoso.com/`* .
1. Para aplicar el vínculo personalizado, seleccione **Guardar**.

## <a name="test-self-service-password-reset"></a>Autoservicio de restablecimiento de contraseña de prueba

Con el autoservicio de restablecimiento de contraseña habilitado y configurado, pruebe el proceso SSPR con un usuario que forme parte del grupo que seleccionó en la sección anterior, como *Grupo-Prueba-SSPR*. En el ejemplo siguiente, se usa la cuenta *usuarioDePrueba*. Indique su propia cuenta de usuario que forma parte del grupo que ha habilitado para SSPR en la primera sección de este tutorial.

> [!NOTE]
> Para probar el autoservicio de restablecimiento de contraseña, use una cuenta que no sea de administrador. De manera predeterminada, los administradores están habilitados para el autoservicio de restablecimiento de contraseña y deben utilizar dos métodos de autenticación para restablecer su contraseña. Para más información, consulte [Diferencias entre directivas de restablecimiento de administrador](concept-sspr-policy.md#administrator-reset-policy-differences).

1. Para ver el proceso de registro manual, abra una nueva ventana del explorador en modo InPrivate o incógnito y vaya a [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup). Se debe dirigir a los usuarios a este portal de registro la siguiente vez que inicien sesión.
1. Inicie sesión con un usuario de prueba que no sea administrador, como *usuarioDePrueba* y registre la información de contacto de los métodos de autenticación.
1. Una vez que haya terminado, seleccione el botón marcado como **Parece correcto** y cierre la ventana del explorador.
1. Abra una nueva ventana del explorador en modo de incógnito o InPrivate, y vaya a [https://aka.ms/sspr](https://aka.ms/sspr).
1. Escriba la información de cuenta del usuario de prueba sin privilegios de administrador, como *usuarioDePrueba*, y los caracteres del CAPTCHA; después, seleccione **Siguiente**.

    ![Introducción de la información de la cuenta de usuario para restablecer la contraseña](media/tutorial-enable-sspr/password-reset-page.png)

1. Siga los pasos de comprobación para restablecer la contraseña. Cuando haya terminado, recibirá una notificación por correo electrónico que le comunicará que se ha restablecido la contraseña.

## <a name="clean-up-resources"></a>Limpieza de recursos

En el siguiente tutorial de esta serie configurará la escritura diferida de contraseñas. Esta característica escribe los cambios de contraseña del autoservicio de restablecimiento de contraseña de Azure AD de nuevo en un entorno de AD local. Si desea continuar con esta serie de tutoriales para configurar la escritura diferida de contraseñas, no deshabilite SSPR por ahora.

Si ya no desea usar la funcionalidad de SSPR que ha configurado como parte de este tutorial, establezca el estado de SSPR en **Ninguno** mediante los pasos siguientes:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Busque y seleccione **Azure Active Directory** y, a continuación, elija **Restablecer la contraseña** en el menú del lado izquierdo.
1. En la página **Propiedades**, bajo la opción *Se habilitó el restablecimiento de contraseña del autoservicio*, elija **Ninguno**.
1. Para aplicar el cambio de SSPR, seleccione **Guardar**.

## <a name="faqs"></a>Preguntas más frecuentes

En esta sección se explican las preguntas comunes de los administradores y los usuarios finales que prueba el autoservicio de restablecimiento de contraseña (SSPR):

- ¿Por qué los usuarios federados esperan hasta 2 minutos después de ver el mensaje **Se ha restablecido su contraseña** y antes de usar contraseñas que se sincronizan desde el entorno local?

  En el caso de los usuarios federados cuyas contraseñas están sincronizadas, el origen de autoridad de las contraseñas es el en el entorno local. Como consecuencia, SSPR solo actualiza las contraseñas locales. La sincronización de hash de contraseñas en Azure AD está programada para producirse cada 2 minutos.

- Cuando un usuario recién creado cuyos datos de SSPR se rellenan previamente (por ejemplo, el teléfono y el correo electrónico) visita la página de registro de SSPR, el mensaje **No perder el acceso a su cuenta** aparece como el título de la página. ¿Por qué otros usuarios cuyos datos de SSPR se rellenan previamente no ven el mensaje?

  Si un usuario ve el mensaje **No perder el acceso a su cuenta**, pertenece a los grupos de registro combinado/SSPR configurados para el inquilino. Los usuarios que no ven el mensaje **No perder el acceso a su cuenta** no pertenecían a los grupos de registro combinado/SSPR.

- Cuando algunos usuarios pasan por el proceso de SSPR y restablecen la contraseña, ¿por qué no ven el indicador del nivel de seguridad de la contraseña?

  Los usuarios que no ven si el nivel de contraseña es débil o fuerte tienen habilitada la escritura diferida de contraseñas. Dado que SSPR no puede establecer la directiva de contraseñas utilizada en el entorno local del cliente, no podrá confirmar si la contraseña es fuerte o débil. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial habilitó el autoservicio de restablecimiento de contraseña en Azure AD para un grupo seleccionado de usuarios. Ha aprendido a:

> [!div class="checklist"]
> * Habilitar el autoservicio de restablecimiento de contraseña para un grupo de usuarios de Azure AD
> * Configurar métodos de autenticación y opciones de registro
> * Probar el proceso SSPR como usuario

> [!div class="nextstepaction"]
> [Habilitar Azure AD Multi-Factor Authentication](./tutorial-enable-azure-mfa.md)
