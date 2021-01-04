---
title: Visualización de las reservas de los recursos de Azure
description: Aprenda a ver las reservas de Azure Reservations en Azure Portal. Consulte la información sobre reservas y uso con las API, PowerShell, la CLI y Power BI.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 12/15/2020
ms.author: banders
ms.openlocfilehash: 8c69f477f363654b8bd707949f0a5b4c46a4e8df
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97561111"
---
# <a name="view-azure-reservations"></a>Visualización de reservas de Azure

En este artículo se explica cómo ver las reservas de Azure en Azure Portal. Las reservas compradas se pueden ver y administrar en Azure Portal.

## <a name="who-can-manage-a-reservation-by-default"></a>Quiénes pueden administrar una reservas de manera predeterminada

De forma predeterminada, los siguientes usuarios son los que pueden ver y administrar reservas:

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

1. Vaya a **Azure Cost Management + Billing** y, después, en el lado izquierdo de la página, seleccione **Transacciones de reserva**.
2. Si tiene los permisos de facturación necesarios, puede ver y administrar las reservas. Si no ve ninguna reserva, asegúrese de que ha iniciado sesión mediante el inquilino de Azure AD en el que se crearon las reservas.

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

