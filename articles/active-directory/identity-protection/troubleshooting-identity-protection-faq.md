---
title: Preguntas más frecuentes sobre Identity Protection en Azure Active Directory
description: Preguntas más frecuentes sobre Azure AD Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: troubleshooting
ms.date: 01/07/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6117b1ac78faf84d73f5a78202709aec7a1f84d9
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99492593"
---
# <a name="frequently-asked-questions-identity-protection-in-azure-active-directory"></a>Preguntas más frecuentes sobre Identity Protection en Azure Active Directory

## <a name="dismiss-user-risk-known-issues"></a>Problemas conocidos al descartar el riesgo del usuario

**Descartar el riesgo del usuario** en la versión clásica de Identity Protection establece el actor en el historial de riesgo del usuario de Identity Protection en **Azure AD**.

**Descartar el riesgo del usuario** en Identity Protection establece el actor en el historial de riesgo del usuario de Identity Protection en **\<Admin’s name with a hyperlink pointing to user’s blade\>** .

Actualmente hay un problema conocido que provoca latencia en el flujo de descarte de riesgo de usuario. Si tiene una "directiva de riesgo de usuario", esta directiva dejará de aplicarse a los usuarios descartados a los pocos minutos de hacer clic en "Descartar el riesgo de usuario". Sin embargo, se han constatado retrasos en la experiencia de usuario al actualizar el "Estado de riesgo" de los usuarios descartados. Como alternativa, actualice la página a nivel del explorador para ver el "Estado de riesgo" más reciente del usuario.


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="why-is-a-user-at-risk"></a>¿Por qué un usuario está en riesgo?

Si es un cliente de Azure AD Identity Protection, vaya a la vista [de usuarios de riesgo](howto-identity-protection-investigate-risk.md#risky-users) y haga clic en un usuario en riesgo. En el cajón de la parte inferior, la pestaña "Historial de riesgos" mostrará todos los eventos que provocaron un cambio de riesgo del usuario. Para ver todos los inicios de sesión de riesgo para el usuario, haga clic en "Inicios de sesión de riesgo del usuario". Para ver todas las detecciones de riesgo del usuario, haga clic en "Detecciones de riesgos del usuario".

### <a name="why-was-my-sign-in-blocked-but-identity-protection-didnt-generate-a-risk-detection"></a>¿Por qué se bloqueó mi inicio de sesión, pero Identity Protection no generó una detección de riesgos?
Los inicios de sesión se pueden bloquear por varios motivos. Debe tener en cuenta que Identity Protection solo genera detecciones de riesgo cuando se usan las credenciales correctas en la solicitud de autenticación. Si un usuario usa credenciales incorrectas, no se marcará con Identity Protection, ya que no hay riesgo de que las credenciales se pongan en peligro a menos que un infiltrado use las credenciales correctas. Algunos de los motivos por los que se podría bloquear a un usuario sin generar una detección de Identity Protection incluyen:
* La **IP se puede bloquear** debido a actividad malintencionada de la dirección IP. El mensaje IP bloqueada no distingue si las credenciales son correctas o no. Si la dirección IP está bloqueada y no se usan credenciales correctas, no se generará una detección de Identity Protection.
* El **[bloqueo inteligente](../authentication/howto-password-smart-lockout.md)** puede impedir que la cuenta inicie sesión después de varios intentos erróneos.
* Se puede aplicar una **directiva de acceso condicional** que use condiciones distintas del nivel de riesgo para bloquear una solicitud de autenticación.

### <a name="how-can-i-get-a-report-of-detections-of-a-specific-type"></a>¿Cómo puedo obtener un informe de las detecciones de un tipo específico?

Vaya a la vista de detecciones de riesgos y filtre por "Tipo de detección". Después, puede descargar este informe en .CSV o formato .JSON mediante el botón **Descargar** de la parte superior. Para más información, consulte el artículo [Procedimientos: investigar los riesgos](howto-identity-protection-investigate-risk.md#risk-detections).

### <a name="why-cant-i-set-my-own-risk-levels-for-each-risk-detection"></a>¿Por qué no puedo establecer mis propios niveles de riesgo para cada detección de riesgo?

Los niveles de riesgo de Identity Protection se basan en la precisión de la detección y en la tecnología de aprendizaje automático supervisado. Para personalizar qué usuarios de la experiencia se presentan, el administrador puede incluir o excluir ciertos usuarios o grupos de las directivas Riesgo de usuario y Riesgo de inicio de sesión.

### <a name="why-does-the-location-of-a-sign-in-not-match-where-the-user-truly-signed-in-from"></a>¿Por qué la ubicación de inicio de sesión no coincide con el lugar desde donde el usuario inició sesión realmente?

La asignación de geolocalización de direcciones IP es un desafío que afecta a todo el sector. Si cree que la ubicación indicada en el informe de inicios de sesión no coincide con la ubicación real, póngase en contacto con soporte técnico de Microsoft. 

### <a name="how-can-i-close-specific-risk-detections-like-i-did-in-the-old-ui"></a>¿Cómo puedo cerrar detecciones de riesgo específicas como hice en la interfaz de usuario anterior?

Puede enviar comentarios sobre las detecciones de riesgo confirmando el inicio de sesión vinculado como en peligro o seguro. Los comentarios que se dan en el inicio de sesión engañan a todas las detecciones realizadas en ese inicio de sesión. Si desea cerrar las detecciones que no están vinculadas a un inicio de sesión, puede proporcionar esos comentarios en el nivel de usuario. Para más información, consulte el artículo [Control Enviar comentarios sobre riesgo en Azure AD Identity Protection](howto-identity-protection-risk-feedback.md).

### <a name="how-far-can-i-go-back-in-time-to-understand-whats-going-on-with-my-user"></a>¿Hasta qué punto puedo volver atrás para comprender lo que está ocurriendo con el usuario?

- La vista de [usuarios de riesgo](howto-identity-protection-investigate-risk.md#risky-users) muestra el riesgo de un usuario en función de todos los inicios de sesión anteriores. 
- La vista de [inicios de sesión de riesgo](howto-identity-protection-investigate-risk.md#risky-sign-ins) muestra los signos de riesgo los últimos 30 días. 
- La vista de [detecciones de riesgo](howto-identity-protection-investigate-risk.md#risk-detections) muestra las detecciones de riesgo realizadas en los últimos 90 días.

### <a name="how-can-i-learn-more-about-a-specific-detection"></a>¿Cómo puedo obtener más información sobre una detección específica?

Todas las detecciones de riesgo se documentan en el artículo [¿Qué es el riesgo?](concept-identity-protection-risks.md#risk-types-and-detection). Puede mantener el puntero sobre el símbolo (i) situado junto a la detección en Azure Portal para obtener más información sobre una detección.

### <a name="how-do-the-feedback-mechanisms-in-identity-protection-work"></a>¿Cómo funcionan los mecanismos de comentarios en Identity Protection?

**Confirmado (en peligro)** (en un inicio de sesión): informa a Azure AD Identity Protection de que el inicio de sesión no lo realizó el propietario de la identidad e indica un peligro.

- Al recibir estos comentarios, pasamos el estado de riesgo del inicio de sesión y del usuario a **Confirmado (en peligro)** y nivel de riesgo **Alto**.

- Además, se proporcionará la información a los sistemas de aprendizaje automático para futuras mejoras en la evaluación de riesgos.

    > [!NOTE]
    > Si el usuario ya se ha corregido, no haga clic en **Confirmar que se encuentra en peligro**, ya que pasará el estado de riesgo del inicio de sesión y del usuario a **Confirmado (en peligro)** y el nivel de riesgo a **Alto**.

**Confirmar que es seguro** (en un inicio de sesión): informa a Azure AD Identity Protection de que el inicio de sesión lo realizó el propietario de la identidad y no indica un peligro.

- Al recibir estos comentarios, pasamos el estado de riesgo del inicio de sesión (y no al usuario) a **Confirmado (en peligro)** y el nivel de riesgo a **-** .

- Además, se proporcionará la información a los sistemas de aprendizaje automático para futuras mejoras en la evaluación de riesgos. 

    > [!NOTE]
    >En la actualidad, si se selecciona Confirmar que es seguro al iniciar una sesión, esto no evita que inicios de sesión posteriores con las mismas propiedades se identifiquen como de riesgo. La mejor manera de entrenar el sistema para aprender las propiedades del usuario es usar la directiva de inicio de sesión de riesgo con autenticación multifactor (MFA). Cuando se solicita MFA para los inicios de sesión de riesgo y el usuario responde correctamente a la solicitud, el inicio de sesión puede efectuarse correctamente y contribuir a entrenar el sistema sobre el comportamiento del usuario legítimo.
    >
    > Si cree que el usuario no está en peligro, use **Descartar el riesgo del usuario** en el nivel de usuario en lugar de usar **Confirmado (seguro)** en el nivel de inicio de sesión. **Descartar el riesgo del usuario** en el nivel de usuario cierra el riesgo del usuario, así como todas las detecciones de riesgo y los inicios de sesión pasados.

### <a name="why-am-i-seeing-a-user-with-a-low-or-above-risk-score-even-if-no-risky-sign-ins-or-risk-detections-are-shown-in-identity-protection"></a>¿Por qué veo un usuario con una puntuación de riesgo baja (o superior) a pesar de que no hay inicios de sesión ni detecciones de riesgo en Identity Protection?

Dado que el riesgo del usuario es de naturaleza acumulativa y no expira, un usuario puede tener un riesgo de usuario bajo o superior, incluso si no hay detecciones de riesgo o inicios de sesión de riesgo que se muestren en Identity Protection. Esta situación podría darse si la actividad malintencionada en un usuario ha tenido lugar fuera del período de tiempo durante el cual almacenamos los detalles de los inicios de sesión y de las detecciones de riesgo. No permitimos que el riesgo del usuario expire porque se sabe que los malos actores permanecen en el entorno de los clientes más de 140 días detrás de una identidad comprometida antes de impulsar su ataque. Los clientes pueden revisar la escala de tiempo de riesgo del usuario, para entender porqué un usuario está en peligro, si se dirigen a: `Azure Portal > Azure Active Directory > Risky users’ report > Click on an at-risk user > Details’ drawer > Risk history tab`

### <a name="why-does-a-sign-in-have-a-sign-in-risk-aggregate-score-of-high-when-the-detections-associated-with-it-are-of-low-or-medium-risk"></a>¿Por qué un inicio de sesión tiene una puntuación alta en "sign-in risk (aggregate)" [Riesgo de inicio de sesión (agregado)] cuando las detecciones asociadas con él son de riesgo medio o bajo?

La puntuación agregada de riesgo alto podría basarse en otras características del inicio de sesión, o en el hecho de que se desencadenó más de una detección para ese inicio de sesión. Y a la inversa, un inicio de sesión puede tener un riesgo de inicio de sesión (agregado) de nivel Medio, incluso si las detecciones asociadas con el inicio de sesión son de riesgo alto.

### <a name="what-is-the-difference-between-the-activity-from-anonymous-ip-address-and-anonymous-ip-address-detections"></a>¿Cuál es la diferencia entre las detecciones de "Actividad desde una dirección IP anónima" y "Dirección IP anónima"?

El origen de la detección de "Dirección IP anónima" es Azure AD Identity Protection, mientras que la detección de"Actividad desde una dirección IP anónima" se integra desde MCAS (Microsoft Cloud App Security). Aunque tienen nombres muy similares y es posible que pueda ver la superposición en estas señales, tienen detecciones de back-end distintas.
