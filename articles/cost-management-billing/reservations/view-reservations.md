---
title: Visualización de las reservas de los recursos de Azure
description: Aprenda a ver las reservas de Azure Reservations en Azure Portal. Consulte la información sobre reservas y uso con las API, PowerShell, la CLI y Power BI.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 02/24/2021
ms.author: banders
ms.openlocfilehash: 477dff9db28672f8231710af34786ac387f43b71
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102050348"
---
# <a name="view-azure-reservations"></a>Visualización de reservas de Azure

En este artículo se explica cómo ver las reservas de Azure en Azure Portal. Las reservas compradas se pueden ver y administrar en Azure Portal.

## <a name="who-can-manage-a-reservation-by-default"></a>Quiénes pueden administrar una reservas de manera predeterminada

De forma predeterminada, estos son los usuarios que pueden ver y administrar reservas:

- Tanto la persona que compra una reserva como el administrador de cuenta de la suscripción de facturación que se ha usado para comprar la reserva se agregan al pedido de reserva.
- Administradores de facturación del Contrato Enterprise y del Contrato de cliente de Microsoft.

Para que otras personas puedan administrar las reservas, tiene dos opciones:

- Delegar la administración de acceso en un pedido de reserva individual:
    1. Inicie sesión en [Azure Portal](https://portal.azure.com).
    1. Seleccione **Todos los servicios** > **Reservations** para enumerar las reservas a las que tiene acceso.
    1. Seleccione la reserva cuyo acceso quiere delegar a otros usuarios.
    1. En Detalles de reserva, seleccione el pedido de reserva.
    1. Seleccione **Access Control (IAM)** .
    1. Seleccione **Agregar asignación de roles** > **Rol** > **Propietario**. Si desea otorgar un acceso limitado, seleccione otro rol.
    1. Escriba la dirección de correo electrónico del usuario al que desea agregar como propietario.
    1. Seleccione el usuario y, después, **Guardar**.

- Agregue un usuario como administrador de facturación a un Contrato Enterprise o un Contrato de cliente de Microsoft:
    - En el caso de un Contrato Enterprise, agregue usuarios con el rol de _Administrador de empresa_ para ver y administrar todos los pedidos de reserva que se aplican al Contrato Enterprise. Los usuarios con el rol de _Administrador de empresa (solo lectura)_ solo pueden ver la reserva. Ni los administradores de departamento ni los propietarios de cuentas no pueden ver reservas, _salvo que_ se les agreguen mediante el Control de acceso (IAM). Para más información, consulte [Administración de roles de Azure Enterprise](../manage/understand-ea-roles.md).

        _Los administradores de empresa pueden tomar posesión de un pedido de reserva y pueden agregar otros usuarios a una reserva mediante el Control de acceso (IAM)._
    - En el caso de un Contrato de cliente de Microsoft, los usuarios con el rol Propietario de perfil de facturación o el rol Colaborador de perfil de facturación pueden administrar todas las compras de reserva realizadas mediante el perfil de facturación. Los lectores del perfil de facturación y los administradores de facturas pueden ver todas las reservas que se pagan con el perfil de facturación. Sin embargo, no pueden realizar cambios en las reservas.
    Para más información, consulte [Tareas y roles del perfil de facturación](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="how-billing-administrators-view-or-manage-reservations"></a>De qué forma ven o administran las reservas los administradores de facturación

Si tiene acceso a las reservas o a los pedidos de reserva con el acceso de Azure RBAC, es posible que vea solo un subconjunto de transacciones de reserva, o incluso ninguna, al ir a Reservas. Utilice los pasos siguientes para ver y administrar todas las reservas y transacciones de reserva.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Cost Management + Billing**.
    - Los administradores de EA pueden seleccionar **Ámbitos de facturación** en el menú de la izquierda y, después, seleccionar uno de los ámbitos de la lista.
    - Los propietarios de un perfil de facturación de Contrato de cliente de Microsoft, deben seleccionar **Perfiles de facturación** en el menú de la izquierda. En la lista de perfiles de facturación, seleccione uno.
1. En el menú de la izquierda, seleccione **Transacciones de reserva**. Se muestra la lista de transacciones de reserva.
1. En la parte superior de la página aparece el siguiente banner: *Now billing administrators can manage reservations. Click here to manage reservations.* (Ahora los administradores de facturación pueden administrar reservas. Haga clic aquí para administrar las reservas) Seleccione el banner.
1. Se muestra la lista completa de reservas de su perfil de facturación o inscripción de EA.
1. Si desea tomar posesión de una reserva, selecciónela. Luego, en la página de configuración de permisos, seleccione **Conceder acceso**. Se le concede acceso de propietario tanto a la reserva como al pedido de reserva.

## <a name="view-reservation-and-utilization-in-the-azure-portal"></a>Visualización de la reserva y el uso en Azure Portal

Para ver una reserva como propietario o lector

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Vaya a [Reservas](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
3. La lista muestra todas las reservas donde tenga el rol de propietario o lector. Cada reserva muestra el último porcentaje de utilización conocido.
4. Seleccione el porcentaje de uso para ver el historial y los detalles de uso. Consulte los detalles en el vídeo siguiente.
   > [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4sYwk] 

## <a name="get-reservations-and-utilization-using-apis-powershell-and-cli"></a>Obtención de reservas y uso mediante API, PowerShell y la CLI

Obtenga una lista de todas las reservas mediante los siguientes recursos:

- [API: Pedido de reserva: lista](/rest/api/reserved-vm-instances/reservationorder/list)
- [PowerShell: Pedido de reserva: lista](/powershell/module/azurerm.reservations/get-azurermreservationorder)
- [CLI: Pedido de reserva: lista](/cli/azure/reservations/reservation-order#az-reservations-reservation-order-list)

También puede obtener el [uso de la reserva](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) mediante la API de uso de la instancia reservada. 

## <a name="see-reservations-and-utilization-in-power-bi"></a>Visualización de reservas y uso en Power BI

Los usuarios de Power BI tienen dos opciones:
- Paquete de contenido: En el [paquete de contenido de Power BI de Consumption Insights](/power-bi/desktop-connect-azure-cost-management) están disponibles la fecha de compra y los datos de utilización de la reserva. Cree los informes que desee mediante el paquete de contenido. 
- Aplicación Cost Management: use la [aplicación Cost Management](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp) para obtener informes creados previamente que puede personalizar.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- [Administración de reservas de Azure](manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](understand-reserved-instance-usage.md).
- [Información sobre el uso de reservas para la inscripción Enterprise](understand-reserved-instance-usage-ea.md).
- [Información sobre el uso de reservas para suscripciones de CSP](/partner-center/azure-reservations).

