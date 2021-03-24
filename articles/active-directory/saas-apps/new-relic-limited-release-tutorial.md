---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con New Relic | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y New Relic.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/04/2020
ms.author: jeedes
ms.openlocfilehash: 29e19eea51b5ee55831bf1d694a9a6473a62d471
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97504056"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-new-relic"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con New Relic

En este tutorial, aprenderá a integrar New Relic con Azure Active Directory (Azure AD). Al integrar New Relic con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a New Relic.
* Permitir que los usuarios inicien sesión automáticamente en New Relic con sus cuentas de Azure AD.
* Administrar sus cuentas en una ubicación central: Azure Portal.

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesitará lo siguiente:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en New Relic.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* New Relic admite el inicio de sesión único puesto en marcha por el proveedor de servicios o por el proveedor de identidades.
* Una vez que haya configurado New Relic, puede aplicar el control de sesión, que protege su organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="add-new-relic-from-the-gallery"></a>Incorporación de New Relic desde la galería

Para configurar la integración de New Relic en Azure AD, debe agregar **New Relic (By Organization)** desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. Seleccione el servicio **Azure Active Directory**.
1. Seleccione **Aplicaciones empresariales** > **Nueva aplicación**.
1. En la página **Examinar la Galería de Azure AD**, escriba **New Relic (By Organization)** en el cuadro de búsqueda.
1. Seleccione **New Relic (By Organization)** en el panel de resultados y, a continuación, **Create** (Crear). Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-new-relic"></a>Configuración y prueba del inicio de sesión único de Azure AD para New Relic

Configure y pruebe el inicio de sesión único de Azure AD con New Relic mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de New Relic.

Para configurar y probar el inicio de sesión único de Azure AD con New Relic, siga estos pasos:

1. [Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso), para permitir que los usuarios puedan utilizar esta característica.
   1. [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con B. Simon.
   1. [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para que B. Simon pueda usar el inicio de sesión único de Azure AD.
1. [Configuración del inicio de sesión único de New Relic](#configure-new-relic-sso), para configurar los valores de inicio de sesión único en la aplicación.
   1. [Creación de un usuario de prueba de New Relic](#create-a-new-relic-test-user), para tener un homólogo de B.Simon en New Relic vinculado a la representación del usuario en Azure AD.
1. [Comprobación del inicio de sesión único](#test-sso), para verificar que la configuración funciona correctamente.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **New Relic by Organization**, busque la sección **Administrar**. A continuación, seleccione **Inicio de sesión único**.

1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.

1. En la página **Configurar el inicio de sesión único con SAML**, seleccione el icono con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Captura de pantalla de la página Configuración del inicio de sesión único con SAML, con el icono de lápiz resaltado.](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, proporcione los valores de **identificador** y **URL de respuesta**.

   * Para recuperar estos valores, use la aplicación New Relic **My Organization**. Para usar esta aplicación:
      1. [Inicie sesión](https://login.newrelic.com/) en New Relic.
      1. En el menú superior, seleccione **Apps** (Aplicaciones).
      1. En la sección **Sus aplicaciones**, seleccione **Mi organización** > **Dominios de autenticación**.
      1. Elija el dominio de autenticación al que desea que se conecte el inicio de sesión único de Azure AD (si tiene más de un dominio de autenticación). La mayoría de las empresas solo tienen un dominio de autenticación llamado **Default** (Predeterminada). Si solo hay un dominio de autenticación, no es necesario que seleccione nada.
      1. En la sección **Authentication** (Autenticación), el campo **Assertion consumer URL** (URL del consumidor de aserciones) contiene el valor que se va a usar en **URL de respuesta**.
      1. En la sección **Authentication** (Autenticación), el campo **Our entity ID** (Nuestro Id. de entidad) contiene el valor que se va a usar para **Identificador**.

1. En la sección **Atributos y notificaciones de usuario**, asegúrese de que **Identificador de usuario único** se haya asignado a un campo que contenga la dirección de correo electrónico que se usa en New Relic.

   * El campo predeterminado **user.userprincipalname** funcionará si sus valores coinciden con las direcciones de correo electrónico de New Relic.
   * El campo **user.mail** puede funcionar mejor si **user.userprincipalname** no es la dirección de correo electrónico de New Relic.

1. En la sección **Certificado de firma de SAML**, copie el valor de **Dirección URL de metadatos de federación de aplicación** y guárdelo para su uso posterior.

1. En la sección **Configurar New Relic by Organization**, copie el valor de **URL de inicio de sesión** y guárdelo para usarlo posteriormente.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En Azure Portal, seleccione **Azure Active Directory**.
1. Seleccione **Usuarios** > **Nuevo usuario**.
1. En la página **Nuevo usuario**:
   1. En el campo **Nombre de usuario**, escriba `username@companydomain.extension`. Por ejemplo, `b.simon@contoso.com`. Debe coincidir con la dirección de correo electrónico que utilizará en New Relic.
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. Seleccione **Mostrar contraseña** y guarde el valor mostrado.
   1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección va a permitir que B.Simon acceda a New Relic by Organization mediante el inicio de sesión único de Azure AD.

1. En Azure Portal, seleccione **Azure Active Directory**.
1. Seleccione **Aplicaciones empresariales** > **New Relic by Organization**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Captura de pantalla de la sección Administrar, con la opción Usuarios y grupos resaltada.](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. En **Agregar asignación**, seleccione **Usuarios y grupos** (o **usuarios** en función del nivel del plan).

   ![Captura de pantalla del botón Agregar usuario.](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos** (o **Usuarios**), seleccione **B.Simon** en la lista **Usuarios** y, a continuación, **Seleccionar** en la parte inferior de la pantalla.
1. En **Agregar asignación**, seleccione **Asignar**.

## <a name="configure-new-relic-sso"></a>Configuración del inicio de sesión único de New Relic

Siga estos pasos para configurar el inicio de sesión único en New Relic.

1. [Inicie sesión](https://login.newrelic.com/) en New Relic.

1. En el menú superior, seleccione **Apps** (Aplicaciones).

1. En la sección **Sus aplicaciones**, seleccione **Mi organización** > **Dominios de autenticación**.

1. Elija el dominio de autenticación al que desea que se conecte el inicio de sesión único de Azure AD (si tiene más de un dominio de autenticación). La mayoría de las empresas solo tienen un dominio de autenticación llamado **Default** (Predeterminada). Si solo hay un dominio de autenticación, no es necesario que seleccione nada.

1. En la sección **Authentication** (Autenticación), seleccione **Configurar** (Configurar).

   1. En el campo **Source of SAML metadata** (Origen de metadatos de SAML), escriba el valor que guardó anteriormente en el campo **Dirección URL de metadatos de federación de aplicación** en Azure AD.

   1. En el campo **SSO target URL** (Dirección URL de destino de SSO), escriba el valor que guardó anteriormente en el campo **URL de inicio de sesión** en Azure AD.

   1. Después de comprobar que la configuración es correcta en Azure AD y New Relic, seleccione **Guardar**. Si no es correcta en ambos sitios, los usuarios no podrán iniciar sesión en New Relic.

### <a name="create-a-new-relic-test-user"></a>Creación de un usuario de prueba de New Relic

En esta sección, creará un usuario llamado B.Simon en New Relic.

1. [Inicie sesión](https://login.newrelic.com/) en New Relic.

1. En el menú superior, seleccione **Apps** (Aplicaciones).

1. En la sección **Your apps** (Sus aplicaciones), seleccione **User Management** (Administración de usuarios).

1. Seleccione **Agregar usuario**.

   1. En el campo **Nombre**, escriba **B.Simon**.
   
   1. En el campo **Correo electrónico**, escriba el valor que enviará el inicio de sesión único de Azure AD.
   
   1. Elija un **tipo** y un **grupo** de usuario. Para un usuario de prueba, el tipo **Usuario básico** y el grupo **Usuario** son opciones apropiadas.
   
   1. Para guardar el usuario, seleccione **Agregar usuario**.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección va a probar la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al seleccionar el icono de **New Relic by Organization** en el panel de acceso, se debería iniciar sesión automáticamente en New Relic. Para más información acerca del Panel de acceso, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Mis aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Tutoriales para integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Prueba de New Relic con Azure AD](https://aad.portal.azure.com/)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)
