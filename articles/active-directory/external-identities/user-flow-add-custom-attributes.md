---
title: Adición de atributos personalizados a flujos de registro de autoservicio - Azure AD
description: Obtenga información acerca de cómo personalizar los atributos de los flujos de usuario de registro de autoservicio.
services: active-directory
author: msmimart
manager: celestedg
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 03/02/2021
ms.author: mimart
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46b498f8b8512d0202f47dd31ba25cc851ca71e6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "101644109"
---
# <a name="define-custom-attributes-for-user-flows"></a>Definición de atributos personalizados para flujos de usuario

Para cada aplicación, puede tener requisitos diferentes para la información que quiere recopilar durante el registro. El inquilino de Azure AD incluye un conjunto integrado de información almacenada en atributos, como el nombre propio, los apellidos, la ciudad y el código postal. Con Azure AD, puede extender el conjunto de atributos almacenados en una cuenta de invitado cuando el usuario externo se registre a través de un flujo de usuario.

Puede crear atributos personalizados en Azure Portal y usarlos en los flujos de usuario de registro de autoservicio. También puede leer y escribir estos atributos mediante [Microsoft Graph API](../../active-directory-b2c/microsoft-graph-operations.md). Microsoft Graph API admite la creación y actualización de un usuario con atributos de extensión. Los atributos de extensión en Graph API se denominan mediante la convención `extension_<extensions-app-id>_attributename`. Por ejemplo:

```JSON
"extension_831374b3bd5041bfaa54263ec9e050fc_loyaltyNumber": "212342"
```

`<extensions-app-id>` es específico del inquilino. Para hallar este identificador, vaya a Azure Active Directory > Registros de aplicaciones > Ver todas las aplicaciones. Busque la aplicación que comienza por "aad-extensions-app" y selecciónela. En la página Información general de la aplicación, anote el valor que aparece en Id. de aplicación (cliente).

## <a name="create-a-custom-attribute"></a>Creación de un atributo personalizado

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de Azure AD.
2. En **Servicios de Azure**, seleccione **Azure Active Directory**.
3. En el menú de la izquierda, seleccione **External Identities**.
4. Seleccione **Atributos de usuario personalizados**. Se muestran los atributos de usuario disponibles.

   ![Selección de atributos de usuario para el registro](media/user-flow-add-custom-attributes/user-attributes.png)

5. Para agregar un atributo, seleccione **Agregar**.
6. En el panel **Agregar un atributo**, escriba los valores siguientes:

   - **Nombre**: proporcione un nombre para el atributo personalizado (por ejemplo, "ShoeSize").
   - **Tipo de datos**: elija un tipo de datos (**Cadena**, **Booleano** o **Int**).
   - **Descripción**: también puede escribir una descripción del atributo personalizado para uso interno. Esta descripción no es visible para el usuario.

   ![Agregar un atributo](media/user-flow-add-custom-attributes/add-an-attribute.png)

7. Seleccione **Crear**.

El atributo personalizado ya está disponible en la lista de atributos de usuario y puede usarlo en los flujos de usuario. Solo se crea la primera vez que se utiliza en cualquier flujo de usuario y no cuando se agrega a la lista de atributos de usuario.

Después de crear un usuario mediante un flujo de usuario que usa el atributo personalizado recién creado, el objeto se puede consultar en el [Explorador de Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer). Ahora debe ver **ShoeSize** en la lista de atributos que se recopilan durante el viaje de suscripción en el objeto de usuario. Puede llamar a Graph API desde la aplicación para obtener los datos de este atributo una vez que se agregue al objeto de usuario.

## <a name="next-steps"></a>Pasos siguientes

[Incorporación de un flujo de usuario de registro de autoservicio a una aplicación](self-service-sign-up-user-flow.md)