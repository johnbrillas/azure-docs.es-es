---
title: Tutorial para configurar Azure Active Directory B2C con Zscaler
titleSuffix: Azure AD B2C
description: Aprenda a integrar la autenticación de Azure AD B2C con Zscaler
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/09/2020
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: ff51c2a71dfcaec580733a92e265628ac816e229
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97095877"
---
# <a name="tutorial-to-configure-zscaler-private-access-with-azure-active-directory-b2c-for-secure-hybrid-access"></a>Tutorial para configurar Zscaler Private Access con Azure Active Directory B2C para el acceso híbrido seguro

En este tutorial, aprenderá a integrar la autenticación de Azure AD B2C con [Zscaler Private Access (ZPA)](https://www.zscaler.com/products/zscaler-private-access). ZPA ofrece acceso seguro basado en directivas a aplicaciones y recursos privados sin el costo, los inconvenientes o los riesgos de seguridad que presenta una red privada virtual (VPN). La oferta de acceso híbrido seguro de Zscaler, cuando se combina con Azure AD B2C, permite una superficie sin ataques en aplicaciones orientadas al consumidor.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesitará lo siguiente:

- Suscripción a Azure. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).

- [Un inquilino de Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant) vinculado a la suscripción de Azure.

- [Una suscripción a ZPA](https://azuremarketplace.microsoft.com/marketplace/apps/aad.zscalerprivateaccess?tab=Overview)

## <a name="scenario-description"></a>Descripción del escenario

La integración de ZPA incluye los siguientes componentes:

- Azure AD B2C: el proveedor de identidades (IdP) responsable de comprobar las credenciales del usuario. También es responsable de suscribir nuevos usuarios.

- ZPA: el servicio responsable de proteger la aplicación web al exigir [acceso de confianza cero](https://www.microsoft.com/security/blog/2018/12/17/zero-trust-part-1-identity-and-access-management/#:~:text=Azure%20Active%20Directory%20%28Azure%20AD%29%20provides%20the%20strong%2C,to%20express%20their%20access%20requirements%20in%20simple%20terms.).

- Aplicación web: hospeda el servicio al que el usuario intenta acceder.

En el diagrama siguiente se muestra cómo ZPA se integra con Azure AD B2C.

![Imagen que muestra el diagrama de la arquitectura de Zscaler](media/partner-zscaler/zscaler-architecture-diagram.png)

|Paso | Descripción |
|:-----| :-----------|
| 1. | El usuario llega a un portal de usuarios de ZPA o a una aplicación de acceso mediante el explorador a ZPA.
| 2. | ZPA necesita la información de contexto del usuario antes de decidir si permitirle que acceda a la aplicación web. Para autenticar al usuario, ZPA realiza una redirección SAML a la página de inicio de sesión de Azure AD B2C.  
| 3. | El usuario llega a la página de inicio de sesión de Azure AD B2C. Si es nuevo, el usuario se registra para crear una cuenta. Un usuario existente iniciaría la sesión con sus credenciales existentes. Azure AD B2C valida la identidad del usuario.
| 4. | Tras una autenticación correcta, Azure AD B2C redirige al usuario de vuelta a ZPA junto con la aserción de SAML. ZPA comprueba la aserción de SAML y establece el contexto del usuario.
| 5. | ZPA evalúa las directivas de acceso del usuario. Si al usuario se le permite el acceso a la aplicación web, se permite el paso de la conexión.

## <a name="onboard-to-zpa"></a>Incorporación a ZPA

En este tutorial se supone que ya tiene una configuración de ZPA que funciona. Si está comenzando con ZPA, consulte [Guía de configuración paso a paso de ZPA](https://help.zscaler.com/zpa/step-step-configuration-guide-zpa).

## <a name="integrate-with-azure-ad-b2c"></a>Integración con Azure AD B2C

### <a name="part-1---configure-azure-ad-b2c-as-an-idp-on-zpa"></a>Parte 1: Configuración de Azure AD B2C como IdP en ZPA

Para configurar Azure AD B2C como [proveedor de identidades de (IdP) en ZPA](https://help.zscaler.com/zpa/configuring-idp-single-sign), siga estos pasos:

1. Inicie sesión en el [portal de administración de ZPA](https://admin.private.zscaler.com).

2. Vaya a **Administration** > **IdP Configuration** (Administración > Configuración del IdP).

3. Seleccione **Add IdP Configuration** (Agregar configuración del IdP).

4. En **1 IdP Information** (1 Información de IdP), escriba lo siguiente:

   a. **Name** (Nombre): Azure AD B2C

   b. **Single Sign-On** (Inicio de sesión único): Seleccionar usuario

   c. **Domains** (Dominios): elija los dominios de autenticación que quiere asociar a este IdP y, luego, seleccione **Done** (Listo).

   ![Imagen que muestra la información del IdP](media/partner-zscaler/add-idp-configuration.png)

5. Seleccione **Siguiente**.

6. En **2 SP Metadata** (2 Metadatos del SP):

   a. Copie la dirección URL del proveedor de servicios y anótela; la necesitará más tarde.

   b. Copie el identificador de la entidad del proveedor de servicios y anótela; la necesitará más tarde.

   ![Imagen que muestra información de metadatos del SP](media/partner-zscaler/sp-metadata.png)

7. Seleccione **Pause** (Pausa).

El resto de la configuración del IdP se reanudará después de configurar Azure AD B2C.

### <a name="part-2---configure-custom-policy-in-azure-ad-b2c"></a>Parte 2: Configuración de una directiva personalizada en Azure AD B2C

>[!Note]
>Este paso solo es necesario si aún no tiene directivas personalizadas. Si ya tiene una o varias directivas personalizadas, puede omitirlo.

En el artículo [Introducción a las directivas personalizadas en Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started) se proporcionan instrucciones sobre cómo configurar directivas personalizadas en el inquilino de Azure AD B2C.

### <a name="part-3---register-zpa-as-a-saml-application-in-azure-ad-b2c"></a>Parte 3: Registro de ZPA como aplicación de SAML en Azure AD B2C

En el artículo [Registrar una aplicación SAML en Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers) se proporcionan instrucciones sobre cómo configurar una aplicación de SAML en Azure AD B2C. En el [paso 3.2](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers#32-upload-and-test-your-policy-metadata), se le proporcionará la dirección URL de metadatos de SAML del IdP que se usa en Azure AD B2C. La necesitará más tarde.

Siga las instrucciones que se indican en el [paso 4.2](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers#42-update-the-app-manifest). En el paso 4.2 de la instrucción, se le pide que actualice el manifiesto de la aplicación. Actualice las propiedades de la manera siguiente:

- **identifierUris**: use el identificador de la entidad del proveedor de servicios indicado en la **parte 1, paso 6A**.

- **samlMetadataUrl**: omita esta propiedad, ya que ZPA no hospeda una dirección URL de metadatos de SAML.

- **replyUrlsWithType**: use la dirección URL del proveedor de servicios indicada en la **parte 1, paso 6B**.

- **logoutUrl**: omita esta propiedad, ya que ZPA no admite una dirección URL de cierre de sesión.

El resto de los pasos no son de interés para este tutorial.

### <a name="part-4---extract-the-idp-saml-metadata-from-azure-ad-b2c"></a>Parte 4: Extracción de los metadatos de SAML del IdP de Azure AD B2C

En el paso anterior, debe obtener una dirección URL de metadatos de SAML con el siguiente formato:

```https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/<policy-name>/Samlp/metadata```

donde `<tenant-name>` es el nombre del inquilino de Azure AD B2C y `<policy-name>` es el nombre de la directiva de SAML personalizada que creó en el último paso.

Por ejemplo: https://safemarch.b2clogin.com/safemarch.onmicrosoft.com/B2C_1A_signup_signin_saml//Samlp/metadata

Abra un explorador web y vaya a la dirección URL de metadatos de SAML. Cuando se cargue la página, haga clic con el botón derecho en cualquier parte. Seleccione **Save Page As** (Guardar página como) y guarde el archivo en el equipo; lo usará en la parte siguiente.

### <a name="part-5---complete-idp-configuration-on-zpa"></a>Parte 5: Finalización de la configuración del IdP en ZPA

Termine de realizar la [configuración del IdP en el portal de administración de ZPA](https://help.zscaler.com/zpa/configuring-idp-single-sign) que hizo parcialmente en la parte 1:

1. Inicie sesión en el [portal de administración de ZPA](https://admin.private.zscaler.com).

2. Vaya a **Administration** > **IdP Configuration** (Administración > Configuración del IdP).

3. Seleccione el IdP que se configuró en la parte 1 y elija **Resume** (Reanudar).

4. En **3 Create IdP** (3 Crear IdP):

   a. Vaya a **IdP Metadata File** > **Select File** (Archivo de metadatos del IdP > Seleccionar archivo) y cargue el archivo de metadatos que guardó en la parte 4.

   b. Compruebe que, en **Status** (Estado), la configuración del IdP es **Enabled** (Habilitado).

   c. Seleccione **Guardar**.

      ![Imagen que muestra la información de creación del IdP](media/partner-zscaler/create-idp.png)

## <a name="test-the-solution"></a>Probar la solución

Vaya a un portal de usuarios de ZPA o a una aplicación de acceso mediante el explorador y pruebe el proceso de registro o inicio de sesión. La autenticación de SAML debería realizarse correctamente.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los artículos siguientes:

- [Introducción a las directivas personalizadas en Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started)

- [Registrar una aplicación SAML en Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers)

- [Guía de configuración paso a paso de ZPA](https://help.zscaler.com/zpa/step-step-configuration-guide-zpa)

- [Configuración de un IdP para el inicio de sesión único](https://help.zscaler.com/zpa/configuring-idp-single-sign)
