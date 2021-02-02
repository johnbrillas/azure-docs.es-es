---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 01/18/2021
ms.author: mimart
ms.openlocfilehash: 4f6ba0892438d49dcc982a01a6b30dfa36ed43b5
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98674244"
---
## <a name="add-a-user-journey"></a>Adición de un recorrido del usuario 

En este momento, el proveedor de identidades ya se ha configurado, pero no está disponible en ninguna de las pantallas de inicio de sesión. Si no tiene su propio recorrido del usuario personalizado, cree un duplicado de un recorrido del usuario de la plantilla existente; de lo contrario, continúe con el paso siguiente. 

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
1. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
1. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
1. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
1. Cambie el identificador del recorrido del usuario. Por ejemplo, `Id="CustomSignUpSignIn"`.

## <a name="add-the-identity-provider-to-a-user-journey"></a>Adición del proveedor de identidades a un recorrido del usuario 

Ahora que tiene un recorrido del usuario, agregue el nuevo proveedor de identidades al recorrido del usuario. En primer lugar, agregue un botón de inicio de sesión y, después, vincule el botón a una acción. La acción es el perfil técnico que creó anteriormente.

1. Busque el elemento del paso de orquestación que incluye `Type="CombinedSignInAndSignUp"` o `Type="ClaimsProviderSelection"`en el recorrido del usuario. Normalmente es el primer paso de orquestación. El elemento **ClaimsProviderSelections** contiene una lista de proveedores de identidades con los que un usuario puede iniciar sesión. El orden de los elementos controla el orden de los botones de inicio de sesión que se presentan al usuario. Agregue un elemento XML **ClaimsProviderSelection**. Establezca el valor de **TargetClaimsExchangeId** en un nombre descriptivo.

1. En el paso de orquestación siguiente, agregue un elemento **ClaimsExchange**. Establezca el **Id** en el valor del identificador de intercambio de notificaciones de destino. Actualice el valor de **TechnicalProfileReferenceId** al del identificador del perfil técnico que creó anteriormente.

En el siguiente código XML se muestran los dos primeros pasos de orquestación de un recorrido del usuario con el proveedor de identidades: