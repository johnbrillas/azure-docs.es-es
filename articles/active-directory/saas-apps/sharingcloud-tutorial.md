---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con SharingCloud | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SharingCloud.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/09/2021
ms.author: jeedes
ms.openlocfilehash: 7ae447a9577feba8b43b5b03a757ec4095ee2cb4
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102177987"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sharingcloud"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con SharingCloud

En este tutorial, aprenderá a integrar SharingCloud con Azure Active Directory (Azure AD). Al integrar SharingCloud con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a SharingCloud.
* Permitir que los usuarios puedan iniciar sesión automáticamente en SharingCloud con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el inicio de sesión único (SSO)?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede obtener la evaluación gratuita de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en SharingCloud.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SharingCloud admite el inicio de sesión único iniciado por **SP e IDP**.
* SharingCloud admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="adding-sharingcloud-from-the-gallery"></a>Adición de SharingCloud desde la galería

Para configurar la integración de SharingCloud en Azure AD, será preciso que agregue SharingCloud desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)
    
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)
    
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.

    ![Botón Nueva aplicación](common/add-new-app.png)
    
1. En la sección **Agregar desde la galería**, escriba **SharingCloud** en el cuadro de búsqueda.

    ![SharingCloud en la lista de resultados](common/search-new-app.png)
    
1. Seleccione **SharingCloud** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sharingcloud"></a>Configuración y prueba del inicio de sesión único de Azure AD para SharingCloud

Configure y pruebe el inicio de sesión único de Azure AD con SharingCloud mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SharingCloud.

Para configurar y probar el inicio de sesión único de Azure AD con SharingCloud, complete los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[ Configuración del inicio de sesión único en SharingCloud](#configure-sharingcloud-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en SharingCloud](#create-sharingcloud-test-user)** : para tener un homólogo de B.Simon en SharingCloud que esté vinculado a su representación en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **SharingCloud**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
    
    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)
    
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, siga estos pasos:

    Cargue el archivo de metadatos con el archivo XML proporcionado por SharingCloud. Póngase en contacto con el [equipo de soporte técnico de SharingCloud](mailto:support@sharingcloud.com) para obtener el archivo.

    ![imagen](common/upload-metadata.png)
    
    Seleccione el archivo de metadatos proporcionado y haga clic en **Cargar**.

    ![imagen](common/browse-upload-metadata.png)

1. La aplicación SharingCloud espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/edit_attribute.png)

1. Además de lo anterior, la aplicación SharingCloud espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Atributo de origen|
    | ---------------| --------- |
    | urn:sharingcloud:sso:firstname | user.givenname |
    | urn:sharingcloud:sso:lastname | user.surname |
    | urn:sharingcloud:sso:email | user.mail |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el icono de **copia** para copiar la **dirección URL de metadatos de federación** de las opciones proporcionadas según sus requisitos.

    ![Dirección URL de metadatos que se va a copiar](common/copy_metadataurl.png)

## <a name="configure-sharingcloud-sso"></a>Configuración del inicio de sesión único de SharingCloud

Para configurar el inicio de sesión único en **SharingCloud**, debe enviar la **dirección URL de metadatos de federación** que ha copiado de Azure Portal al [equipo de soporte técnico de SharingCloud](mailto:support@sharingcloud.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

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

En esta sección va a permitir que B.Simon acceda a SharingCloud mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **SharingCloud**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

   ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-sharingcloud-test-user"></a>Creación de un usuario de prueba de SharingCloud

En esta sección se crea un usuario llamado Britta Simon en SharingCloud. SharingCloud admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en SharingCloud, se crea uno nuevo después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

* Vaya directamente a la dirección URL de SharingCloud e inicie el flujo de inicio de sesión desde allí.

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado SharingCloud, puede aplicar el control de sesión, que protege su organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

