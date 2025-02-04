---
title: 'Tutorial: Integración de Azure Active Directory con Keeper Password Manager & Digital Vault | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Keeper Password Manager & Digital Vault.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/13/2020
ms.author: jeedes
ms.openlocfilehash: b70c50e7c2900f884dd4d91c6650205bc626326e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96178048"
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a>Tutorial: Integración de Azure Active Directory con Keeper Password Manager & Digital Vault

En este tutorial, obtendrá información sobre cómo integrar Keeper Password Manager & Digital Vault con Azure Active Directory (Azure AD).
La integración le proporciona las siguientes ventajas:

* Desde Azure AD puede controlar quién tiene acceso a Keeper Password Manager & Digital Vault.
* Puede permitir que los usuarios inicien sesión automáticamente en Keeper Password Manager & Digital Vault (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.


## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Keeper Password Manager & Digital Vault, se necesita lo siguiente:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una evaluación gratuita de un mes](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción de Keeper Password Manager & Digital Vault, habilitada para el inicio de sesión único.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Keeper Password Manager & Digital Vault admite el inicio de sesión único iniciado por SP.

* Keeper Password Manager & Digital Vault admite el aprovisionamiento de usuarios Just-In-Time.

## <a name="add-keeper-password-manager--digital-vault-from-the-gallery"></a>Incorporar Keeper Password Manager & Digital Vault desde la galería

Para configurar la integración de Keeper Password Manager & Digital Vault en Azure AD, es preciso agregar la aplicación desde la galería a la lista de aplicaciones de software como servicio (SaaS) administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel izquierdo, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En **Agregar desde la galería**, escriba **Keeper Password Manager & Digital Vault** en el cuadro de búsqueda.
1. Seleccione **Keeper Password Manager & Digital Vault** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-keeper-password-manager--digital-vault"></a>Configuración y prueba del inicio de sesión único de Azure AD para Keeper Password Manager & Digital Vault

Configure y pruebe el inicio de sesión único de Azure AD con Keeper Password Manager & Digital Vault con un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Keeper Password Manager & Digital Vault.

Para configurar y probar el inicio de sesión único de Azure AD para Keeper Password Manager & Digital Vault:

1. [Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso), para permitir que los usuarios puedan utilizar esta característica.

    * [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con Britta Simon.
    * [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para permitir que Britta Simon use el inicio de sesión único de Azure AD.

1. [Configuración del inicio de sesión único en Keeper Password Manager & Digital Vault](#configure-keeper-password-manager--digital-vault-sso), para configurar los valores de inicio de sesión único en la aplicación.
    * [Creación de un usuario de prueba de Keeper Password Manager & Digital Vault](#create-a-keeper-password-manager--digital-vault-test-user), para tener un homólogo de Britta Simon en la aplicación que esté vinculado a la representación del usuario en Azure AD.
1. [Comprobación del inicio de sesión único](#test-sso), para verificar que la configuración funciona correctamente.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Keeper Password Manager & Digital Vault**, busque la sección **Administrar**. Seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, seleccione el icono con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Captura de pantalla de la página Configuración del inicio de sesión único con SAML, con el icono de lápiz resaltado.](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:
    * Para el inicio de sesión único en la nube: `https://keepersecurity.com/api/rest/sso/saml/sso/<CLOUD_INSTANCE_ID>`
    * Para el inicio de sesión único local: `https://<KEEPER_FQDN>/sso-connect/saml/login`

    b. En **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón:
    * Para el inicio de sesión único en la nube: `https://keepersecurity.com/api/rest/sso/saml/<CLOUD_INSTANCE_ID>`
    * Para el inicio de sesión único local: `https://<KEEPER_FQDN>/sso-connect`

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón:
    * Para el inicio de sesión único en la nube: `https://keepersecurity.com/api/rest/sso/saml/sso/<CLOUD_INSTANCE_ID>`
    * Para el inicio de sesión único local: `https://<KEEPER_FQDN>/sso-connect/saml/sso`

    > [!NOTE]
    > Estos valores no son reales. Actualícelos con la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de Keeper Password Manager & Digital Vault](https://keepersecurity.com/contact.html) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación Keeper Password Manager & Digital Vault espera las aserciones de SAML en un formato específico, lo cual requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![Captura de pantalla que muestra la sección Atributos y notificaciones de usuario.](common/default-attributes.png)

1. Además, la aplicación Keeper Password Manager & Digital Vault espera que la respuesta SAML devuelva algunos atributos más. Estos se muestran en la tabla siguiente. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Atributo de origen|
    | ------------| --------- |
    | First | user.givenname |
    | Último | user.surname |
    | Email | user.mail |

5. En **Configuración del inicio de sesión único con SAML**, en la sección **Certificado de firma SAML**, seleccione **Descargar**. Esta acción descarga el archivo **XML de metadatos de federación** a partir de las opciones configuradas, y lo guarda en el equipo.

    ![Captura de pantalla que muestra la sección Certificado de firma SAML, con la opción Descargar resaltada.](common/metadataxml.png)

6. En **Set up Keeper Password Manager & Digital Vault** (Configurar Password Manager & Digital Vault), copie las direcciones URL que necesite.

    ![Captura de pantalla de configuración del administrador de contraseñas de Password Manager & Vault digital, con las direcciones URL resaltadas.](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

En esta sección, creará un usuario de prueba en Azure Portal llamado `B.Simon`.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
1. En la parte superior de la pantalla, seleccione **Nuevo usuario**.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En **Nombre**, escriba `B.Simon`.  
   1. En **Nombre de usuario**, escriba `username@companydomain.extension`. Por ejemplo, `B.Simon@contoso.com`.
   1. Seleccione **Mostrar contraseña** y anote el valor que se muestra.
   1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección va a permitir que B.Simon acceda a Keeper Password Manager & Digital Vault mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales** > **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Keeper Password Manager & Digital Vault**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. En **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En **Usuarios y grupos**, seleccione **B.Simon** en la lista de usuarios. A continuación, elija **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, se seleccionará el rol **Acceso predeterminado**.
1. En **Agregar asignación**, seleccione **Asignar**.


## <a name="configure-keeper-password-manager--digital-vault-sso"></a>Configuración del inicio de sesión único de Keeper Password Manager & Digital Vault

Si desea configurar el inicio de sesión único para la aplicación, consulte las directrices de la [guía de soporte técnico de Keeper](https://docs.keeper.io/sso-connect-guide/).

### <a name="create-a-keeper-password-manager--digital-vault-test-user"></a>Creación de un usuario de prueba de Keeper Password Manager & Digital Vault

Para permitir que los usuarios de Azure AD inicien sesión en Keeper Password Manager & Digital Vault, es necesario aprovisionarlos. La aplicación admite el aprovisionamiento de usuarios Just-In-Time. Tras la autenticación, los usuarios se crearán automáticamente en la aplicación. Si desea configurar manualmente los usuarios, póngase en contacto con el [equipo de soporte técnico de Keeper](https://keepersecurity.com/contact.html).

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* En Azure Portal, seleccione **Probar esta aplicación**. Esta acción le redirigirá a la URL de inicio de sesión de Keeper Password Manager & Digital Vault, donde puede poner en marcha el inicio de sesión. 

* También es posible acceder directamente a la URL de inicio de sesión de la aplicación e iniciar sesión desde ahí.

* Puede usar el Panel de acceso de Microsoft. Al seleccionar el icono de **Keeper Password Manager & Digital Vault** en el panel de acceso, se le redirigirá a la dirección URL de inicio de sesión de la aplicación. Para más información acerca del panel de acceso, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Mis aplicaciones](../user-help/my-apps-portal-end-user-access.md).


## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Keeper Password Manager & Digital Vault, puede aplicar el control de sesión. Esto protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. Para más información, consulte [Cómo aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).