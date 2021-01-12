---
title: Incorporación de destinatarios en Azure Data Share
description: Aprenda a agregar destinatarios a un recurso compartido de datos existente en Azure Data Share.
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 12/17/2020
ms.openlocfilehash: a8e3dac620873ab11ae24395310066037f6d2df4
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97680638"
---
# <a name="how-to-add-a-recipient-to-your-share"></a>Procedimiento para agregar un destinatario a un recurso compartido

Puede agregar destinatarios al crear un recurso compartido, o bien a uno existente. Desde la interfaz de usuario de Azure Data Share, puede agregar destinatarios mediante la dirección de correo electrónico de inicio de sesión de Azure del usuario.  Desde la API, puede usar una combinación de usuario o entidad servicio e identificador de inquilino. Cuando se especifica un identificador de inquilino, la invitación solo se puede aceptar en este inquilino. También desde la API, puede crear una invitación sin enviar un correo electrónico al destinatario. 

## <a name="add-recipient-to-an-existing-share"></a>Adición de un destinatario a un recurso compartido existente

En Azure Data Share, vaya a su recurso compartido enviado y seleccione la pestaña **Invitaciones**. Aquí se enumeran todos los destinatarios de invitaciones a este recurso compartido de datos. Para agregar uno nuevo, haga clic en **Agregar destinatario**.

![Captura de pantalla que muestra la opción para agregar destinatario seleccionada.](./media/how-to/how-to-add-recipients/add-recipient.png)

Emergerá un panel en el lado derecho de la página. Haga clic en **Agregar destinatario** y rellene el correo electrónico del nuevo destinatario en la línea en blanco. Asegúrese de usar la dirección de correo electrónico de inicio de sesión de Azure del destinatario (el alias de correo electrónico no funcionará). 

![Captura de pantalla que muestra el panel Agregar destinatario, donde puede agregar y enviar una invitación.](./media/how-to/how-to-add-recipients/add-recipient-side.png)

Haga clic en **Add and send invitation** (Agregar y enviar invitación). Los nuevos destinatarios recibirán correos electrónicos de invitación para este recurso compartido.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre cómo [eliminar una invitación a un recurso compartido](how-to-delete-invitation.md).
