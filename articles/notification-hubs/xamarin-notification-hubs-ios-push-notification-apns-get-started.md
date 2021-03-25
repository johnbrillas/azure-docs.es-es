---
title: Envío de notificaciones push a Xamarin mediante Azure Notification Hubs | Microsoft Docs
description: En este tutorial, aprenderá a usar Azure Notification Hubs para enviar notificaciones de inserción a una aplicación Xamarin iOS.
services: notification-hubs
keywords: notificaciones push de ios,mensajes de inserción,notificaciones push,mensaje de notificación
documentationcenter: xamarin
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc, devx-track-csharp
ms.date: 01/12/2021
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 05/23/2019
ms.openlocfilehash: ff1e5edad05ebd7157f71ad2e099ea88905be4f3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98221143"
---
# <a name="tutorial-send-push-notifications-to-xamarinios-apps-using-azure-notification-hubs"></a>Tutorial: Envío de notificaciones push a aplicaciones de Xamarin.iOS mediante Azure Notification Hubs

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Información general

Este tutorial muestra cómo puede utilizar los Centros de notificaciones de Azure para enviar notificaciones de inserción a una aplicación iOS. Cree una aplicación Xamarin.iOS vacía que reciba notificaciones push mediante [Apple Push Notification Service (APN)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).

Cuando haya finalizado, podrá usar un Centro de notificaciones para difundir notificaciones push a todos los dispositivos que ejecutan su aplicación. El código acabado está disponible en el ejemplo de la [aplicación NotificationHubs][GitHub].

En este tutorial, se crea o actualiza código para realizar las tareas siguientes:

> [!div class="checklist"]
> * Generación del archivo de solicitud de firma de certificado
> * Registro de la aplicación para notificaciones push
> * Creación de un perfil de aprovisionamiento para la aplicación
> * Configuración de un Centro de notificaciones para notificaciones push de iOS
> * Prueba de envío de las notificaciones push

## <a name="prerequisites"></a>Prerrequisitos

* **Suscripción de Azure**. Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
* La versión más reciente de [Xcode][Install Xcode]
* Un dispositivo compatible con iOS 10 (o una versión posterior)
* [programa para desarrolladores de Apple](https://developer.apple.com/programs/)
* [Visual Studio para Mac]
  
  > [!NOTE]
  > Debido a los requisitos de configuración de las notificaciones push de iOS, debe implementar y probar la aplicación de ejemplo en un dispositivo iOS físico (iPhone o iPad) en lugar de hacerlo en un simulador.

La realización de este tutorial es un requisito previo para todos los demás tutoriales de Notification Hubs para aplicaciones Xamarin.iOS.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-app-to-the-notification-hub"></a>Conexión de la aplicación al Centro de notificaciones

### <a name="create-a-new-project"></a>Creación de un nuevo proyecto

1. En Visual Studio, cree un nuevo proyecto de iOS y seleccione la plantilla **Aplicación de una vista**. A continuación, haga clic en **Siguiente**.

     ![Visual Studio: selección del tipo de aplicación][31]

2. Escriba el nombre de la aplicación y el identificador de la organización y, a continuación, haga clic en **Siguiente** y en **Crear**.

3. En la vista Solución, haga doble clic en *Into.plist* y, en **Identity** (Identidad), asegúrese de que el identificador del conjunto coincida con el que usó al crear el perfil de aprovisionamiento. En **Firma**, asegúrese de que la cuenta de desarrollador esté seleccionada en **Equipo**, "Automatically manage signing" (Administrar automáticamente la firma) esté seleccionado, y las opciones Certificado de firma y Perfil de aprovisionamiento estén seleccionadas automáticamente.

    ![Visual Studio: configuración de aplicaciones iOS][32]

4. En la vista Solución, haga doble clic en `Entitlements.plist` y asegúrese de que **Habilitar notificaciones push** esté activado.

    ![Configuración de los derechos de Visual Studio-iOS][33]

5. Agregue el paquete de mensajería de Azure. En la vista Solución, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **Agregar paquetes NuGet**. Busque **Xamarin.Azure.NotificationHubs.iOS** y agregue el paquete al proyecto.

6. Agregue un nuevo archivo a su clase, asígnele el nombre `Constants.cs`. A continuación, agregue las siguientes variables y reemplace los marcadores de posición de literal de cadena con los valores `hubname` y `DefaultListenSharedAccessSignature` que anotó anteriormente.

    ```csharp
    // Azure app-specific connection string and hub path
    public const string ListenConnectionString = "<Azure DefaultListenSharedAccess Connection String>";
    public const string NotificationHubName = "<Azure Notification Hub Name>";
    ```

7. En `AppDelegate.cs`, agregue la siguiente instrucción using:

    ```csharp
    using WindowsAzure.Messaging.NotificationHubs;
    using UserNotifications
    ```

8. Cree una implementación de `MSNotificationHubDelegate` en `AppDelegate.cs`:

    ```csharp
    public class AzureNotificationHubListener : MSNotificationHubDelegate
    {
        public override void DidReceivePushNotification(MSNotificationHub notificationHub, MSNotificationHubMessage message)
        {

        }
    }
    ```

9. En `AppDelegate.cs`, actualice `FinishedLaunching()` para que coincida con el código siguiente:

    ```csharp
    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // Set the Message listener
        MSNotificationHub.SetDelegate(new AzureNotificationHubListener());
        
        // Start the SDK
        MSNotificationHub.Start(ListenConnectionString, NotificationHubName);

        return true;
    }
    ```

10. En `AppDelegate.cs`, implemente el método `DidReceivePushNotification` para la clase `AzureNotificationHubListener`:

    ```csharp
    public override void DidReceivePushNotification(MSNotificationHub notificationHub, MSNotificationHubMessage message)
    {
        // This sample assumes { aps: { alert: { title: "Hello", body: "World" } } }
        var alertTitle = message.Title ?? "Notification";
        var alertBody = message.Body;

        var myAlert = UIAlertController.Create(alertTitle, alertBody, UIAlertControllerStyle.Alert);
        myAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
        UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(myAlert, true, null);
    }
    ```

11. Ejecute la aplicación en el dispositivo.

## <a name="send-test-push-notifications"></a>Prueba de envío de las notificaciones push

Puede probar de recibir notificaciones en la aplicación con la opción *Envío de prueba* en [Azure Portal]. Envía una notificación push de prueba al dispositivo.

![Azure Portal: envío de prueba][30]

Las notificaciones push se envían normalmente en un servicio back-end como Mobile Apps o ASP.NET mediante una biblioteca compatible. Si no hay disponible ninguna biblioteca para su back-end, también puede usar la API de REST directamente para enviar mensajes de notificación.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha difundido notificaciones de difusión a todos los dispositivos iOS registrados con el back-end. Para aprender a enviar notificaciones push a dispositivos iOS específicos, pase al siguiente tutorial:

> [!div class="nextstepaction"]
>[Envío de notificaciones push a dispositivos específicos](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

<!-- Images. -->
[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png
[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png
[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png
[32]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-app-settings.png
[33]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-entitlements-settings.png

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: https://go.microsoft.com/fwlink/p/?LinkId=272456
[Visual Studio para Mac]: https://visualstudio.microsoft.com/vs/mac/
[Local and Push Notification Programming Guide]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[Apple Push Notification Service fwlink]: https://go.microsoft.com/fwlink/p/?LinkId=272584
[GitHub]: https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs
[Azure Portal]: https://portal.azure.com
