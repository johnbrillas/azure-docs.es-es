---
title: Adición de personalización de marca a la página de inicio de sesión de la organización
titleSuffix: Azure AD B2C
description: Aprenda a agregar la personalización de marca de la organización a las páginas de Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/10/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: bee81876b066b9fc1ea69c418ee4d0839db27b3c
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "97111077"
---
# <a name="add-branding-to-your-organizations-azure-active-directory-b2c-pages"></a>Adición de la personalización de marca a las páginas de Azure Active Directory B2C de la organización

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

Puede personalizar las páginas de Azure AD B2C con un logotipo de banner, una imagen de fondo y un color de fondo mediante [Personalización de marca de empresa](../active-directory/fundamentals/customize-branding.md) de Azure Active Directory. La personalización de marca de una empresa incluye las páginas de registro, inicio de sesión, edición de perfiles y restablecimiento de contraseñas. 

> [!TIP]
> Para personalizar otros aspectos de las páginas del flujo de usuario más allá del logotipo del banner, la imagen de fondo o el color de fondo, consulte [Personalización de la interfaz de usuario con una plantilla HTML](customize-ui-with-html.md).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]


Antes de personalizar las páginas del flujo de usuario, debe configurar la personalización de marca de empresa en Azure Active Directory y, a continuación, habilitarla en los diseños de página de los flujos de usuario en Azure AD B2C.

## <a name="configure-company-branding"></a>Configuración de la personalización de marca de la compañía

Empiece por establecer el logotipo del banner, la imagen de fondo y el color de fondo en **Personalización de marca de empresa**.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. En **Administrar**, seleccione **Personalización de marca de empresa**.
1. Siga los pasos de [Incorporación de la personalización de marca en la página de inicio de sesión de Azure Active Directory de la organización](../active-directory/fundamentals/customize-branding.md).

Tenga en cuenta las siguientes cuestiones a la hora de configurar la personalización de marca de empresa en Azure AD B2C:

* La personalización de marca de empresa en Azure AD B2C está actualmente limitada a la personalización de la **imagen de fondo**, el **logotipo del banner** y el **color de fondo**. Las demás propiedades del panel de personalización de marca de empresa como, por ejemplo, las incluidas en **Configuración avanzada**, *no son compatibles*.
* En las páginas del flujo de usuario se muestra el color de fondo antes de cargar la imagen de fondo. Se recomienda elegir un color de fondo que coincida en la medida de lo posible con los colores de la imagen de fondo, para conseguir una experiencia de carga más fluida.
* El logotipo del banner aparece en los mensajes de correo electrónico de verificación que se envían a los usuarios cuando inician un flujo de usuario de inicio de sesión.

::: zone pivot="b2c-user-flow"

## <a name="enable-branding-in-user-flow-pages"></a>Habilitación de la personalización de marca en las páginas del flujo de usuario

Después de configurar la personalización de marca de la empresa, habilítela en los flujos de usuario.

1. En el menú izquierdo de Azure Portal, seleccione **Azure AD B2C**.
1. En **Directivas**, seleccione **Flujos de usuario (directivas)** .
1. Seleccione el flujo de usuario para el que desea habilitar la personalización de marca de empresa. La personalización de marca de la empresa **no es compatible** con los tipos de flujo de usuario *Inicio de sesión* estándar y *Edición de perfiles* estándar.
1. En **Personalizar**, seleccione **Diseños de página** y, a continuación, seleccione el diseño en el que desea personalizar la marca. Por ejemplo, seleccione **Página unificada de inicio de sesión o de registro**.
1. En **Versión de Diseño de página (versión preliminar)** , elija la versión **1.2.0** o posterior.
1. Seleccione **Guardar**.

Si desea personalizar la marca de todas las páginas del flujo de usuario, establezca la versión de cada diseño de página incluido en el flujo de usuario.

![Selección del diseño de página de Azure AD B2C en Azure Portal](media/company-branding/portal-02-page-layout-select.png)

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="select-a-page-layout"></a>Selección de un diseño de página

Para habilitar la personalización de marca, tendrá que [definir una versión de diseño de página](contentdefinitions.md#migrating-to-page-layout) con la versión `contract` de página para *todas* las definiciones de contenido de la directiva personalizada. El formato del valor debe contener la palabra `contract`: _urn:com:microsoft:aad:b2c:elements:**contract**:page-name:version_. Para especificar un diseño de página en las directivas personalizadas que utilizan un valor de **DataUri** anterior.

Aprenda a [migrar al diseño de página](contentdefinitions.md#migrating-to-page-layout) con la versión de la página.

En el ejemplo siguiente se muestran los identificadores de definición de contenido y el elemento **DataUri** correspondiente con el contrato de página: 

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.2.0</DataUri>
  </ContentDefinition>
</ContentDefinitions>
```

::: zone-end

En el siguiente ejemplo se muestra un logotipo de banner y una imagen de fondo personalizados en una página del flujo de usuario de *Registro e inicio de sesión* que usa la plantilla Azul océano:

![Página de registro o inicio de sesión con personalización de marca ofrecida por Azure AD B2C](media/company-branding/template-ocean-blue-branded.png)

## <a name="use-company-branding-assets-in-custom-html"></a>Uso de recursos de personalización de marca de empresa en HTML personalizado

Para usar los recursos de personalización de marca de la empresa en [HTML personalizado](customize-ui-with-html.md), agregue las siguientes etiquetas fuera de la etiqueta `<div id="api">`:

```HTML
<img data-tenant-branding-background="true" />
<img data-tenant-branding-logo="true" alt="Company Logo" />
```

El origen de la imagen se reemplaza con el de la imagen de fondo y el logotipo del banner.

## <a name="next-steps"></a>Pasos siguientes

Encuentre más información sobre cómo personalizar la interfaz de usuario de las aplicaciones en [Personalización de la interfaz de usuario de la aplicación en Azure Active Directory B2C](customize-ui-with-html.md).