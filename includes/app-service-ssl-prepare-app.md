---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 10/15/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 8b3c1992a1cff18390f9d1332103e0650af418e2
ms.sourcegitcommit: fa807e40d729bf066b9b81c76a0e8c5b1c03b536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "97347734"
---
## <a name="prepare-your-web-app"></a>Preparar la aplicación web

Para crear enlaces de TLS o SSL personalizados o habilitar certificados de cliente para la aplicación de App Service, el [plan de App Service](https://azure.microsoft.com/pricing/details/app-service/) debe ser de nivel **Básico**, **Estándar**, **Premium** o **Aislado**. En este paso, asegúrese de que la aplicación web se encuentra en el plan de tarifa compatible.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Abra [Azure Portal](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Navegar a la aplicación web

Busque y seleccione **App Services**.

![Selección de App Services](./media/app-service-ssl-prepare-app/app-services.png)

En la página **App Services**, seleccione el nombre de la aplicación web.

![Captura de pantalla de la página App Services de Azure Portal, en la que se muestra una lista de todas las aplicaciones web en ejecución en la que la primera está resaltada.](./media/app-service-ssl-prepare-app/select-app.png)

Ha llegado a la página de administración de la aplicación web.  

### <a name="check-the-pricing-tier"></a>Comprobar el plan de tarifa

En el panel de navegación izquierdo de la página de la aplicación web, desplácese a la sección **Configuración** y seleccione **Escalar verticalmente (plan de App Service)** .

![Menú Escalar verticalmente](./media/app-service-ssl-prepare-app/scale-up-menu.png)

Asegúrese de que la aplicación web no está en el nivel **F1** ni **D1**. El nivel actual de la aplicación web aparece resaltado con un cuadro azul oscuro.

![Comprobar plan de tarifa](./media/app-service-ssl-prepare-app/check-pricing-tier.png)

El SSL personalizado no es compatible con los niveles **F1** y **D1**. Si tiene que escalar verticalmente, siga los pasos de la sección siguiente. De lo contrario, cierre la página **Escalar verticalmente** y omita la sección [Scale up your App Service plan](#scale-up-your-app-service-plan) (Escalado vertical del plan de App Service).

### <a name="scale-up-your-app-service-plan"></a>Escalar verticalmente el plan de App Service

Seleccione cualquiera de los niveles no gratuitos (**B1**, **B2**, **B3**, o cualquier nivel de la categoría **Producción**). Para ver opciones adicionales, haga clic en **Ver opciones adicionales**.

Haga clic en **Aplicar**.

![Elegir plan de tarifa](./media/app-service-ssl-prepare-app/choose-pricing-tier.png)

Cuando vea la siguiente notificación, significará que la operación de escalado se habrá completado.

![Notificación de escalado vertical](./media/app-service-ssl-prepare-app/scale-notification.png)

