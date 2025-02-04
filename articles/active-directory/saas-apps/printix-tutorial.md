---
title: 'Tutorial: Integración de Azure Active Directory con Printix | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Printix.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: dfde9bbbeb7f6b349ecbdc4c2da605d39a0708da
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "94357885"
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a>Tutorial: Integración de Azure Active Directory con Printix

En este tutorial, obtendrá información sobre cómo integrar Printix con Azure Active Directory (Azure AD).

La integración de Printix con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Printix.
- Puede permitir que los usuarios inicien sesión automáticamente en Printix (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerrequisitos

Para configurar la integración de Azure AD con Printix, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Printix

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Printix desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-printix-from-the-gallery"></a>Adición de Printix desde la galería
Para configurar la integración de Printix en Azure AD, deberá agregar Printix desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Printix desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Captura de pantalla que muestra la opción Aplicaciones empresariales seleccionada en Azure Portal, bajo Administrar y, a su vez, aparece seleccionada la opción Todas las aplicaciones.][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Captura de pantalla que muestra seleccionada la opción Nueva aplicación.][3]

1. En el cuadro de búsqueda, escriba **Printix**.

    ![Captura de pantalla que muestra la búsqueda de Printix en el cuadro de diálogo Agregar desde la galería.](./media/printix-tutorial/tutorial_printix_search.png)

1. En el panel de resultados, seleccione **Printix** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Captura de pantalla que muestra la opción Printix seleccionada.](./media/printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con Printix con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Printix para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Printix.

Para establecer la relación de vínculo, asigne el valor de **nombre de usuario** de Azure AD como valor de **nombre de usuario** de Printix.

Para configurar y probar el inicio de sesión único de Azure AD con Printix, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba en Printix](#creating-a-printix-test-user)** : el objetivo es tener un homólogo de Britta Simon en Printix que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Printix.

**Para configurar el inicio de sesión único de Azure AD con Printix, realice los pasos siguientes:**

1. En la página de integración de la aplicación **Printix** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Captura de pantalla que muestra que se ha seleccionado Inicio de sesión único en Administrar en Azure Portal.][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![La captura de pantalla muestra el modo de inicio de sesión basado en SAML seleccionado.](./media/printix-tutorial/tutorial_printix_samlbase.png)

1. En la sección **Dominio y direcciones URL de Printix**, realice los pasos siguientes:

    ![Captura de pantalla que muestra la sección Dominio y direcciones URL de Printix, donde puede especificar una URL para el inicio de sesión.](./media/printix-tutorial/tutorial_printix_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.printix.net`.

    > [!NOTE] 
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de atención al cliente de Printix](mailto:support@printix.net) para obtener este valor. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Captura de pantalla que muestra el panel Certificado de firma de SAML, donde puede descargar un certificado.](./media/printix-tutorial/tutorial_printix_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Captura de pantalla que muestra el botón Save (Guardar).](./media/printix-tutorial/tutorial_general_400.png)

1. Inicie la sesión en el inquilino de Printix como administrador.

1. En el menú de la parte superior, haga clic en el icono situado en la esquina superior derecha y seleccione "**Authentication**" (Autenticación).
   
    ![Captura de pantalla que muestra la opción Authentication (Autenticación) seleccionada en el menú.](./media/printix-tutorial/tutorial_printix_06.png)

1. En la pestaña **Setup** (Configuración), seleccione **Enable Azure/Office 365 authentication** (Habilitar la autenticación de Azure y Office 365).
   
    ![Captura de pantalla que muestra la página Printix.net, donde puede seleccionar Enable Azure/Office 365 authentication (Habilitar la autenticación de Azure y Office 365).](./media/printix-tutorial/tutorial_printix_07.png)

1. En la pestaña **Azure**, escriba la dirección URL de metadatos de federación en el cuadro de texto de "**Federation metadata document**" (Documento de metadatos de federación). 

    Adjunte el archivo XML de metadatos que descargó de Azure AD al [equipo de soporte técnico de Printix](mailto:support@printix.net). Ellos se encargarán de cargar este archivo y le proporcionarán una dirección URL de metadatos de federación.
   
    ![Captura de pantalla que muestra la página Printix.net, donde puede especificar un documento de metadatos de federación.](./media/printix-tutorial/tutorial_printix_08.png)
   
1. Haga clic en el botón "**Test**" (Probar) y, si la prueba se ha realizado correctamente, en el botón "**OK**" (Aceptar).
   
     Después de hacer clic en el botón **Test** (Probar) se mostrará la página de Azure Active Directory. El mensaje "The test was successful" (La prueba se ha realizado correctamente) aquí significa que después de escribir las credenciales de su cuenta de prueba de Azure se mostrará el mensaje "Settings tested OK" (Configuración probada correctamente). Después, haga clic en el botón **OK** (Aceptar).
   
    ![Captura de pantalla que muestra los resultados de la prueba.](./media/printix-tutorial/tutorial_printix_09.png)

1. Haga clic en el botón **Save** (Guardar) en la página "**Authentication**" (Autenticación).


> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Captura de pantalla que muestra un nombre y un nombre de usuario que se van a crear.](./media/printix-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![La captura de pantalla muestra el icono de Azure AD en Azure Portal.](./media/printix-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Captura de pantalla que muestra los usuarios y grupos seleccionados en el menú Administrar, con la opción Todos los usuarios seleccionada.](./media/printix-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Captura de pantalla que muestra el cuadro de diálogo Usuario, donde puede especificar los valores descritos.](./media/printix-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Crear**.
 
### <a name="creating-a-printix-test-user"></a>Creación de un usuario de prueba de Printix

El objetivo de esta sección es crear un usuario llamado Britta Simon en Printix. Printix admite el aprovisionamiento just-in-time, que está habilitado de forma predeterminada.

No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a Printix, se creará un nuevo usuario, en caso de que no exista. 

> [!NOTE]
> Si necesita crear manualmente un usuario, es preciso que se ponga en contacto con el [equipo de soporte técnico de Printix](mailto:support@printix.net).
> 

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Printix.

![Captura de pantalla que muestra un usuario con acceso predeterminado.][200] 

**Para asignar a Britta Simon a Printix, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, vaya a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego, haga clic en **Todas las aplicaciones**.

    ![Captura de pantalla que muestra la opción Aplicaciones empresariales seleccionada en Administrar con la opción Todas las aplicaciones seleccionada.][201] 

1. En la lista de aplicaciones, seleccione **Printix**.

    ![Captura de pantalla en la que se muestra la lista de aplicaciones, donde se puede seleccionar Printix.](./media/printix-tutorial/tutorial_printix_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Captura de pantalla que muestra la opción Usuarios y grupos seleccionada en el menú Administrar.][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Captura de pantalla que muestra el botón Agregar y la página Agregar asignación, donde puede seleccionar Usuarios y Grupos.][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Printix en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Printix.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/printix-tutorial/tutorial_general_01.png
[2]: ./media/printix-tutorial/tutorial_general_02.png
[3]: ./media/printix-tutorial/tutorial_general_03.png
[4]: ./media/printix-tutorial/tutorial_general_04.png

[100]: ./media/printix-tutorial/tutorial_general_100.png

[200]: ./media/printix-tutorial/tutorial_general_200.png
[201]: ./media/printix-tutorial/tutorial_general_201.png
[202]: ./media/printix-tutorial/tutorial_general_202.png
[203]: ./media/printix-tutorial/tutorial_general_203.png

