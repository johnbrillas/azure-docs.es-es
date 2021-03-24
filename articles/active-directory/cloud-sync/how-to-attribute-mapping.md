---
title: Asignación de atributos de sincronización en la nube de Azure AD Connect
description: En este artículo se describe cómo usar la característica de sincronización en la nube de Azure AD Connect para asignar atributos.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/21/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdb043374cf6252da3929c8f0cda6c0a4be558b7
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102555217"
---
# <a name="attribute-mapping-in-azure-ad-connect-cloud-sync"></a>Asignación de atributos de sincronización en la nube de Azure AD Connect

Puede usar la característica de sincronización en la nube de Azure Active Directory (Azure AD) Connect para asignar atributos entre los objetos de usuario o de grupo locales y los objetos de Azure AD. Esta capacidad se ha agregado a la configuración de sincronización en la nube.

Puede personalizar (cambiar, eliminar o crear) las asignaciones de atributos predeterminadas según sus necesidades empresariales. Para obtener una lista de los atributos que se sincronizan, consulte [Atributos sincronizados con Azure Active Directory](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md).

## <a name="understand-types-of-attribute-mapping"></a>Descripción de los tipos de asignación de atributos
Con la asignación de atributos, puede controlar cómo se rellenan los atributos en Azure AD. Azure AD admite cuatro tipos de asignación:

- **Directa**: el atributo de destino se rellena con el valor de un atributo del objeto vinculado en Active Directory.
- **Constante**: el atributo de destino se rellena con una cadena específica que se ha especificado.
- **Expresión**: el atributo de destino se rellena según el resultado de una expresión similar a un script. Para más información, consulte [Escritura de expresiones para la asignación de atributos en Azure Active Directory](reference-expressions.md).
- **Ninguna**: el atributo de destino se deja sin modificar. Sin embargo, si el atributo de destino está vacío, se rellena con el valor predeterminado que especifique.

Además de estos tipos básicos, las asignaciones de atributos personalizadas admiten el concepto de una asignación de valor *predeterminada* opcional. La asignación de valor predeterminada garantiza que un atributo de destino se rellene con un valor si no hay ningún valor en Azure AD ni en el objeto de destino. La configuración más habitual consiste en dejarlo en blanco.

## <a name="understand-properties-of-attribute-mapping"></a>Descripción de las propiedades de asignación de atributos

Además de la propiedad de tipo, las asignaciones de atributos admiten los siguientes atributos:

- **Atributo de origen**: especifica el atributo de usuario del sistema de origen (por ejemplo, Active Directory).
- **Atributo de destino**: especifica el atributo de usuario del sistema de destino (por ejemplo, Azure Active Directory).
- **Valor predeterminado si es nulo (opcional)** : valor que se enviará al sistema de destino si el atributo de origen es NULL. Este valor solo se aprovisionará cuando se cree un usuario. No se aprovisionará al actualizar un usuario existente.  
- **Aplicar esta asignación**:
  - **Siempre**: esta asignación se aplica a las acciones de creación y actualización de usuarios.
  - **Solo durante la creación**: esta asignación se aplica solo a las acciones de creación de usuarios.

> [!NOTE]
> En este artículo se describe cómo usar Azure Portal para asignar atributos.  Para obtener información sobre el uso de Microsoft Graph, consulte [Transformaciones](how-to-transformation.md).

## <a name="add-an-attribute-mapping"></a>Adición de una asignación de atributos

Para usar la nueva capacidad, siga estos pasos:

1.  En Azure Portal, seleccione **Azure Active Directory**.
2.  Seleccione **Azure AD Connect**.
3.  Seleccione **Manage cloud sync** (Administrar sincronización en la nube).

    ![Captura de pantalla que muestra el vínculo para administrar la sincronización en la nube.](media/how-to-install/install-6.png)

4. En **Configuración**, seleccione su configuración.
5. Seleccione **Click to edit mappings** (Haga clic para editar las asignaciones).  Este vínculo abre la pantalla **Asignaciones de atributos**.

    ![Captura de pantalla que muestra el vínculo para agregar atributos.](media/how-to-attribute-mapping/mapping-6.png)

6.  Seleccione **Agregar atributo**.

    ![Captura de pantalla que muestra el botón para agregar un atributo, junto con listas de atributos y tipos de asignación.](media/how-to-attribute-mapping/mapping-1.png)

7. Seleccione el tipo de asignación. Para este ejemplo, usamos **Expresión**.
8. Escriba la expresión en el cuadro. Para este ejemplo, usamos `Replace([mail], "@contoso.com", , ,"", ,)`.
9. Escriba el atributo de destino. Para este ejemplo, usamos **ExtensionAttribute15**.
10. Seleccione cuándo quiere aplicar esta asignación y, a continuación, haga clic en **Aplicar**.

    ![Captura de pantalla que muestra los cuadros rellenados para crear una asignación de atributos.](media/how-to-attribute-mapping/mapping-2a.png)

11. En la pantalla **Asignación de atributos**, debería ver la nueva asignación de atributos.  
12. Seleccione **Guardar esquema**.

    ![Captura de pantalla que muestra el botón Guardar esquema.](media/how-to-attribute-mapping/mapping-3.png)

## <a name="test-your-attribute-mapping"></a>Prueba de la asignación de atributos

Para probar la asignación de atributos, puede usar el [ aprovisionamiento a petición](how-to-on-demand-provision.md): 

1. En Azure Portal, seleccione **Azure Active Directory**.
2. Seleccione **Azure AD Connect**.
3. Seleccione **Administrar aprovisionamiento**.
4. En **Configuración**, seleccione su configuración.
5. En **Validar**, seleccione el botón **Aprovisionar un usuario**. 
6. En la pantalla **Aprovisionamiento a petición**, escriba el nombre distintivo de un usuario o grupo y seleccione el botón **Aprovisionar**. 

   La pantalla muestra que el aprovisionamiento está en curso.

   ![Captura de pantalla que muestra el aprovisionamiento en curso.](media/how-to-attribute-mapping/mapping-4.png)

8. Una vez finalizado el aprovisionamiento, aparece una pantalla de éxito con cuatro marcas de verificación verdes. 

   En **Realizar una acción**, seleccione **Ver detalles**. A la derecha, debería ver el nuevo atributo sincronizado y la expresión aplicada.

   ![Captura de pantalla que muestra los detalles de éxito y exportación.](media/how-to-attribute-mapping/mapping-5.png)

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es la sincronización en la nube de Azure AD Connect?](what-is-cloud-sync.md)
- [Escritura de expresiones para la asignación de atributos](reference-expressions.md)
- [Atributos sincronizados con Azure Active Directory](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md)
