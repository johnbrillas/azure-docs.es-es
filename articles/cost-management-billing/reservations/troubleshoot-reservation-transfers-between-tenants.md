---
title: Solución de problemas de transferencias de reservas de Azure entre inquilinos
description: Este artículo ayuda a los propietarios de reservas a transferir pedidos de reservas de un inquilino (directorio) de Azure Active Directory a otro.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 02/24/2021
ms.openlocfilehash: 79473d57cc7504e7e6ef4ef68ba0cee74203f62b
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055809"
---
# <a name="troubleshoot-reservation-transfers-between-tenants"></a>Solución de problemas de transferencias de reservas entre inquilinos

Este artículo ayuda a los propietarios de reservas a transferir pedidos de reservas de un inquilino (directorio) de Azure Active Directory a otro. Al cambiar el directorio de un pedido de reserva, se elimina todo acceso de Azure RBAC al pedido de reserva y a las reservas dependientes. Solo tendrá acceso después del cambio. Al cambiar el directorio no se cambia la propiedad de facturación del pedido de reserva. Se cambia el directorio del pedido de reserva primario y las reservas dependientes.

No se necesita un cambio de reserva y una cancelación para realizar una transferencia entre inquilinos.

Después de transferir una reserva a otro inquilino, también se pueden agregar más propietarios a la reserva. Para más información, consulte la sección [Quiénes pueden administrar reservas de manera predeterminada](view-reservations.md#who-can-manage-a-reservation-by-default).

Cuando se transfiere un pedido de reserva, se transfieren también todas las reservas del mismo.

## <a name="transfer-a-reservation"></a>Transferencia de una reserva

Siga estos pasos para transferir un pedido de reserva y sus reservas dependientes a otro inquilino.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).
1. Si no es un administrador de facturación pero es propietario de una reserva, vaya a **Reservas** y vaya al paso 6.
1. Vaya a **Administración de costos + facturación**.
    - Los administradores de EA pueden seleccionar **Ámbitos de facturación** en el menú de la izquierda y, después, seleccionar uno de los ámbitos de la lista.
    - Los propietarios de un perfil de facturación de Contrato de cliente de Microsoft, deben seleccionar **Perfiles de facturación** en el menú de la izquierda. En la lista de perfiles de facturación, seleccione uno.
1. En el menú de la izquierda, seleccione **Transacciones de reserva**. Se muestra la lista de transacciones de reserva.
1. En la parte superior de la página aparece el siguiente banner: *Now billing administrators can manage reservations. Click here to manage reservations.* (Ahora los administradores de facturación pueden administrar reservas. Haga clic aquí para administrar las reservas) Seleccione el banner.
1. Se muestra la lista completa de reservas de su perfil de facturación o inscripción de EA.
1. Seleccione la reserva que quiere transferir.
1. En los detalles de la reserva, seleccione el identificador del pedido de reserva.
1. En el pedido de reserva, seleccione **Cambiar directorio**.
1. En el panel Cambiar directorio, seleccione el directorio de Azure AD al que desea transferir la reserva y, después, seleccione **Confirmar**.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las reservas, consulte [¿Qué es Azure Reservations?](save-compute-costs-reservations.md).