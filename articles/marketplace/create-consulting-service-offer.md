---
title: Creación de una oferta de servicios de consultoría en el marketplace comercial de Microsoft
description: Obtenga información sobre cómo crear una nueva oferta de servicios de consultoría para Microsoft AppSource o Azure Marketplace con el programa de marketplace comercial en el Centro de partners de Microsoft.
author: Microsoft-BradleyWright
ms.author: brwrigh
ms.reviewer: anbene
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 10/27/2020
ms.openlocfilehash: e9a0b2fe883fa46010fda74c58908128d05919e6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92754270"
---
# <a name="how-to-create-a-consulting-service-offer-in-the-commercial-marketplace"></a>Creación de una oferta de servicios de consultoría en el marketplace comercial

En este artículo se explica cómo crear una oferta de servicios de consultoría para el marketplace comercial de Microsoft mediante el Centro de partners. 

## <a name="before-you-begin"></a>Antes de empezar

Para publicar una oferta de servicios de consultoría, debe cumplir ciertos requisitos de elegibilidad para demostrar su experiencia en su campo. Si aún no lo ha hecho, lea la sección [Planeamiento de una oferta de servicios de consultoría para el marketplace comercial](./plan-consulting-service-offer.md). Describe los requisitos previos para un servicio de consultoría y los recursos que necesitará para crear la oferta con el Centro de partners.

## <a name="create-a-new-consulting-service-offer"></a>Creación de una oferta de servicios de consultoría

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard/home).
2.  En el menú de navegación izquierdo, seleccione **Marketplace comercial** > **Información general**.
3.  En la pestaña Información general, seleccione **+ Nueva oferta** > **Servicio de consultoría**.

    ![Muestra el menú de navegación izquierdo.](./media/new-offer-consulting-service.png)

4. En el cuadro de diálogo **Nueva oferta**, escriba un valor para **Id. de oferta**. Este identificador se puede ver en la dirección URL de la oferta en el marketplace comercial. Por ejemplo, si escribe test-offer-1 en este cuadro, la dirección web de la oferta será `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.

    * Cada oferta de su cuenta debe tener un identificador único.
    * Use solo letras minúsculas y números. El id. de la oferta puede incluir guiones y caracteres de subrayado, pero no espacios, y está limitado a 50 caracteres.
    * El identificador de oferta no se puede cambiar después de seleccionar **Crear**.

5. Escriba un **Alias de la oferta**. Este es el nombre que se usa para la oferta en el Centro de partners. No se puede ver en las tiendas en línea y es diferente del nombre de oferta que se muestra a los clientes.
6. Para generar la oferta y continuar, seleccione **Crear**.

## <a name="configure-lead-management"></a>Configuración de la administración de clientes potenciales

Conecte el sistema de administración de relaciones con clientes (CRM) con la oferta del marketplace comercial para que pueda recibir la información de contacto de un cliente cuando este muestre interés en el servicio de consultorías. Esta conexión se puede modificar en cualquier momento durante la creación de la oferta o con posterioridad a esta. Para obtener instrucciones detalladas, vea [Clientes potenciales a partir de la oferta en el marketplace comercial](./partner-center-portal/commercial-marketplace-get-customer-leads.md).

Para configurar la administración de clientes potenciales en el Centro de partners:

1.  En el Centro de partners, vaya a la pestaña **Configuración de la oferta**.
2.  En **Clientes potenciales**, seleccione el vínculo **Conectar**.
3.  En el cuadro de diálogo **Detalles de conexión**, seleccione un destino de cliente potencial de la lista.
4.  Rellene los campos que aparecen. Para más información, consulte los siguientes artículos:

    * [Configuración de la oferta para enviar clientes potenciales a la tabla de Azure](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md#configure-your-offer-to-send-leads-to-the-azure-table)
    * [Configuración de la oferta para enviar clientes potenciales a Dynamics 365 Customer Engagement](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md#configure-your-offer-to-send-leads-to-dynamics-365-customer-engagement) (antes Dynamics CRM Online)
    * [Configuración de la oferta para enviar clientes potenciales al punto de conexión HTTPS](./partner-center-portal/commercial-marketplace-lead-management-instructions-https.md#configure-your-offer-to-send-leads-to-the-https-endpoint)
    * [Configuración de la oferta para enviar clientes potenciales al Marketo](./partner-center-portal/commercial-marketplace-lead-management-instructions-marketo.md#configure-your-offer-to-send-leads-to-marketo)
    * [Configurar la oferta para enviar clientes potenciales a Salesforce](./partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce.md#configure-your-offer-to-send-leads-to-salesforce)

5.  Para validar la configuración proporcionada, seleccione la opción **Validate link** (Validar vínculo).
6.  Cuando haya configurado los detalles de conexión, seleccione **Conectar**.
7.  Seleccione **Guardar borrador**.

Después de enviar su oferta para publicarla en el Centro de partners, se validará la conexión y se le enviará un cliente potencial de prueba. Mientras obtiene una vista previa de la oferta antes de publicarla, pruebe la conexión de los clientes potenciales cuando tratan de adquirir la oferta en el entorno de versión preliminar.

> [!TIP]
> Asegúrese de que la conexión con el destino de clientes potenciales permanece actualizada para no perder ningún cliente potencial.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de las propiedades de su oferta de servicios de consultoría](./create-consulting-service-offer-properties.md)