---
title: 'Tutorial: Integración del inicio de sesión único de Azure Active Directory con Kendis-Scaling Agile Platform | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Kendis-Scaling Agile Platform.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/28/2021
ms.author: jeedes
ms.openlocfilehash: e02ff4926897fafc72e1a5081366faad5d2f03ba
ms.sourcegitcommit: b85ce02785edc13d7fb8eba29ea8027e614c52a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99509630"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-kendis-scaling-agile-platform"></a>Tutorial: Integración del inicio de sesión único de Azure Active Directory con Kendis-Scaling Agile Platform

En este tutorial, aprenderá a integrar Kendis-Scaling Agile Platform con Azure Active Directory (Azure AD). Al integrar Kendis-Scaling Agile Platform con Azure AD, puede realizar lo siguiente:

* Controlar en Azure AD quién tiene acceso a Kendis-Scaling Agile Platform.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Kendis-Scaling Agile Platform con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en Kendis-Scaling Agile Platform.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Kendis-Scaling Agile Platform admite el inicio de sesión único activado habilitado por **SP e IDP**.
* Kendis-Scaling Agile Platform admite el aprovisionamiento de usuarios **Just-In-Time**.


## <a name="adding-kendis-scaling-agile-platform-from-the-gallery"></a>Adición de Kendis-Scaling Agile Platform desde la galería

Para configurar la integración de Kendis-Scaling Agile Platform en Azure AD, deberá agregar Kendis-Scaling Agile Platform desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Kendis-Scaling Agile Platform** en el cuadro de búsqueda.
1. Seleccione **Kendis-Scaling Agile Platform** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-sso-for-kendis-scaling-agile-platform"></a>Configuración y prueba del inicio de sesión único de Azure AD para Kendis-Scaling Agile Platform

Configure y pruebe el inicio de sesión único de Azure AD con Kendis-Scaling Agile Platform mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Kendis-Scaling Agile Platform.

Para configurar y probar el inicio de sesión único de Azure AD con Kendis-Scaling Agile Platform, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Kendis-Scaling Agile Platform](#configure-kendis-scaling-agile-platform-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Kendis-Scaling Agile Platform](#create-kendis-scaling-agile-platform-test-user)** : para tener un homólogo de B.Simon en Kendis-Scaling Agile Platform que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Kendis-Scaling Agile Platform**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<SUBDOMAIN>.kendis.io`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.kendis.io/login/saml`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.kendis.io/login`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico al cliente de Kendis-Scaling Agile Platform](mailto:support@kendis.io) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Kendis-Scaling Agile Platform**, copie las direcciones URL adecuadas en función de sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a Kendis-Scaling Agile Platform mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Kendis-Scaling Agile Platform**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-kendis-scaling-agile-platform-sso"></a>Configuración del inicio de sesión único de Kendis-Scaling Agile Platform

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de Kendis-Scaling Agile Platform como administrador.

1. Vaya a **Configuración > Configuraciones de SAML**.

    ![Configuración en Configuraciones de SAML](./media/kendis-scaling-agile-platform-tutorial/settings.png)

1. Haga clic en el botón **Editar** en la parte inferior de la página y realice los pasos siguientes.

    ![Configuraciones de SAML](./media/kendis-scaling-agile-platform-tutorial/saml-configuration-settings.png)

    a. Copie el valor de **URL devolución de llamada**, péguelo en el cuadro de texto **Dirección URL de respuesta**, en la sección Configuración básica de SAML de Azure Portal.

    b. En el cuadro de texto **Dirección URL del inicio de sesión único del proveedor de identidades**, pegue el valor de la **Dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    c.  En el cuadro de texto **Emisor de proveedor de identidades**, pegue el valor de **Identificador de Azure AD (Identificador de entidad)** que copió de Azure Portal.

    d. Abra en el Bloc de notas el archivo del **certificado (Base64)** que descargó de Azure Portal, copie el contenido y, luego, péguelo en el cuadro de texto **Certificate X.509** (Certificado).

    e. **Seleccione el grupo predeterminado** en la lista de opciones. 

    f. Haga clic en **Save**(Guardar).

### <a name="create-kendis-scaling-agile-platform-test-user"></a>Creación del usuario de prueba de Kendis-Scaling Agile Platform

En esta sección, se creará una usuaria llamada Britta Simon en Kendis-Scaling Agile Platform. Kendis-Scaling Agile Platform admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si todavía no hay ningún usuario en Kendis-Scaling Agile Platform, se creará uno después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la URL de inicio de sesión de Kendis-Scaling Agile Platform, donde podrá poner en marcha el flujo de inicio de sesión.  

* Acceda directamente a la URL de inicio de sesión de Kendis-Scaling Agile Platform y ponga en marcha el flujo de inicio de sesión desde ahí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Debería iniciarse sesión automáticamente en la aplicación Kendis-Scaling Agile Platform para la que configuró el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Kendis-Scaling Agile Platform en Aplicaciones, si se ha configurado el modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para poner en marcha el flujo de inicio de sesión y, si ha configurado el modo IDP, se debería iniciar sesión automáticamente en la aplicación Kendis-Scaling Agile Platform para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).


## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Kendis-Scaling Agile Platform, puede aplicar el control de sesión que protege la información confidencial de la organización de la filtración y la infiltración en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


