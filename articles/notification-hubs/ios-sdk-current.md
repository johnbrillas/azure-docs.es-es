---
title: Envío de notificaciones push a iOS mediante Azure Notification Hubs y el SDK de iOS versión 3.0.0, versión preliminar 1
description: En este tutorial, aprenderá a usar Azure Notification Hubs y Apple Push Notification Service para enviar notificaciones push a dispositivos iOS (versión 3.0.0, versión preliminar 1).
author: sethmanheim
ms.author: sethm
ms.date: 06/19/2020
ms.topic: tutorial
ms.service: notification-hubs
ms.reviewer: thsomasu
ms.lastreviewed: 06/01/2020
ms.openlocfilehash: f905bfa830bfc78caa6ebb02ae49d4839168367b
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100598245"
---
# <a name="tutorial-send-push-notifications-to-ios-apps-using-azure-notification-hubs-sdk-for-apple"></a>Tutorial: Envío de notificaciones push a aplicaciones iOS mediante el SDK de Azure Notification Hubs para Apple

En este tutorial se muestra cómo usar Azure Notification Hubs para enviar notificaciones push a una aplicación iOS mediante el SDK de Azure Notification Hubs para Apple.

En este tutorial se describen los pasos siguientes:

- Cree una aplicación iOS de ejemplo.
- Conecte la aplicación iOS a Azure Notification Hubs.
- Pruebe el envío de las notificaciones push.
- Compruebe que la aplicación recibe notificaciones.

Puede descargar el código completo para este tutorial en [GitHub](https://github.com/Azure/azure-notificationhubs-ios/tree/main/SampleNHAppObjC).

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Mac con [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532), junto con un certificado de desarrollador válido instalado en el Llavero.
- Un iPhone o iPad con iOS versión 10 o posterior.
- El dispositivo físico registrado en el [portal de Apple](https://developer.apple.com/) y asociado con el certificado.

Antes de continuar, asegúrese de seguir el tutorial anterior sobre cómo empezar a usar [Azure Notification Hubs para aplicaciones iOS](ios-sdk-get-started.md), para instalar y configurar las credenciales push en el centro de notificaciones. Aunque no tenga ninguna experiencia previa con el desarrollo de iOS, debería poder seguir estos pasos.

> [!NOTE]
> Debido a los requisitos de configuración de las notificaciones push, debe implementar y probar estas en un dispositivo iOS físico (iPhone o iPad), en lugar de hacerlo en el emulador de iOS.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Conexión de la aplicación iOS a Notification Hubs

1. En XCode, cree un nuevo proyecto iOS y seleccione la plantilla **Single View Application** (Aplicación de vista sencilla).

   :::image type="content" source="media/ios-sdk/image1.png" alt-text="Seleccionar plantilla":::

2. Al configurar las opciones para su nuevo proyecto, asegúrese de usar el **nombre de producto** y el **identificador de organización** que usó al establecer el identificador de conjunto en el portal de desarrollo de Apple.

3. En Project Navigator (Explorador de proyectos), seleccione el nombre del proyecto en **Targets** (Destinos) y, a continuación, seleccione la pestaña **Signing & Capabilities** (Firma y funcionalidades). Asegúrese de seleccionar el **equipo** adecuado para la cuenta de desarrollador de Apple. XCode debe desplegar automáticamente el perfil de aprovisionamiento que creó anteriormente según en el identificador del conjunto.

   Si no ve el nuevo perfil de aprovisionamiento que creó en Xcode, intente actualizar los perfiles de la identidad de firma. Haga clic en **Xcode** en la barra de menús, en **Preferences** (Preferencias), en la pestaña **Account** (Cuenta), en el botón **View Details** (Ver detalles), en la identidad de firma y, por último, en el botón Refresh (Actualizar) en la esquina inferior derecha.

   :::image type="content" source="media/ios-sdk/image2.png" alt-text="Ver detalles":::

4. En la pestaña **Signing & Capabilities** (Firma y funcionalidades), seleccione **+ Capability** (Funcionalidad). Haga doble clic en **Push Notifications** (Notificaciones push) para habilitarla.

   :::image type="content" source="media/ios-sdk/image3.png" alt-text="Funcionalidad":::

5. Agregue los módulos de SDK de Azure Notification Hubs.

   Puede integrar el SDK de Azure Notification Hubs en la aplicación mediante [Cocoapods](https://cocoapods.org/) o mediante la adición manual de los archivos binarios al proyecto.

   - Integración a través de Cocoapods: agregue las siguientes dependencias al podfile para incluir el SDK de Azure Notification Hubs en la aplicación:

      ```ruby
      pod 'AzureNotificationHubs-iOS'
      ```

      - Ejecute pod install para instalar el pod recién definido y abra el archivo .xcworkspace.

         Si ve un error como **Unable to find a specification for AzureNotificationHubs-iOS** (No se puede encontrar una especificación para AzureNotificationHubs-iOS) mientras se ejecuta pod install, ejecute `pod repo update` para obtener los pods más recientes del repositorio de Cocoapods y, a continuación, ejecute pod install.

   - Integración a través de Carthage: agregue las siguientes dependencias al archivo Cartfile para incluir el SDK de Azure Notification Hubs en la aplicación:

      ```ruby
      github "Azure/azure-notificationhubs-ios"
      ```

      - A continuación, actualice las dependencias de compilación:

      ```shell
      $ carthage update
      ```

      Para obtener más información sobre el uso de Carthage, consulte el [repositorio de GitHub de Carthage](https://github.com/Carthage/Carthage).

   - Integración mediante la copia de los archivos binarios en el proyecto:

      Puede realizar la integración mediante la copia de los archivos binarios en el proyecto, como se indica a continuación:

        - Descargue el marco [SDK de Azure Notification Hubs](https://github.com/Azure/azure-notificationhubs-iOS/releases/) proporcionado como archivo ZIP y descomprímalo.

        - En XCode, haga clic con el botón derecho en el proyecto y haga clic en la opción **Add Files to** (Agregar archivos a) para agregar la carpeta **WindowsAzureMessaging.framework** al proyecto de XCode. Seleccione **Options** (Opciones) y asegúrese de que **Copy items if needed** (Copiar elementos si es necesario) esté seleccionado. A continuación, haga clic en **Add** (Agregar).

          :::image type="content" source="media/ios-sdk/image4.png" alt-text="Agregar marco":::

6. Agregue o edite un archivo llamado **DevSettings.plist** que contiene dos propiedades, `CONNECTION_STRING` para la cadena de conexión a la instancia de Azure Notification Hubs y `HUB_NAME` para el nombre de dicha instancia.

7. Agregue la información para conectarse a Azure Notification Hubs en la sección `<string></string>` correspondiente.  Reemplace los marcadores de posición de literal de cadena `--HUB-NAME--` y `--CONNECTION-STRING--` por el nombre del centro de conectividad y por **DefaultListenSharedAccessSignature** respectivamente, tal como se obtuvo anteriormente en el portal:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>HUB_NAME</key>
        <string>--HUB-NAME--</string>
        <key>CONNECTION_STRING</key>
        <string>--CONNECTION-STRING--</string>
    </dict>
    </plist>
    ```

8. En el mismo archivo **AppDelegate.m**, reemplace todo el código después de `didFinishLaunchingWithOptions` por el código siguiente:

    ```objc
    #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
    #import <UserNotifications/UserNotifications.h>

    // Extend the AppDelegate to listen for messages using MSNotificationHubDelegate and User Notification Center
    @interface AppDelegate () <MSNotificationHubDelegate>

    @end

    @implementation AppDelegate
    
    @synthesize notificationPresentationCompletionHandler;
    @synthesize notificationResponseCompletionHandler;

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

        NSString *path = [[NSBundle mainBundle] pathForResource:@"DevSettings" ofType:@"plist"];
        NSDictionary *configValues = [NSDictionary dictionaryWithContentsOfFile:path];
        
        NSString *connectionString = [configValues objectForKey:@"CONNECTION_STRING"];
        NSString *hubName = [configValues objectForKey:@"HUB_NAME"];

        if([connectionString length] != 0 && [hubName length] != 0) {
            [[UNUserNotificationCenter currentNotificationCenter] setDelegate:self];
            [MSNotificationHub setDelegate:self];
            [MSNotificationHub initWithConnectionString:connectionString withHubName:hubName];
    
            return YES;
        }

        NSLog(@"Please setup CONNECTION_STRING and HUB_NAME in DevSettings.plist and restart application");

        exit(-1);
    }

    - (void)notificationHub:(MSNotificationHub *)notificationHub didReceivePushNotification:(MSNotificationHubMessage *)message {
        // Send message using NSNotificationCenter with the message
        NSDictionary *userInfo = [NSDictionary dictionaryWithObject:message forKey:@"message"];
        [[NSNotificationCenter defaultCenter] postNotificationName:@"MessageReceived" object:nil userInfo:userInfo];
    }

    @end
    ```

    Este código se conecta al centro de notificaciones usando la información de conexión que especificó en **DevSettings.plist**. Luego, proporciona el token del dispositivo al centro de notificaciones para que este pueda enviar notificaciones.

### <a name="create-notificationdetailviewcontroller-header-file"></a>Crear el archivo de encabezado NotificationDetailViewController

1. Al igual que en las instrucciones anteriores, agregue otro archivo de encabezado denominado **NotificationDetailViewController.h**. Reemplace el contenido del nuevo archivo de encabezado por el código siguiente:

   ```objc
   #import <UIKit/UIKit.h>

   NS_ASSUME_NONNULL_BEGIN

   @interface SetupViewController : UIViewController

   @end

   NS_ASSUME_NONNULL_END
   ```

2. Agregue el archivo de implementación **SetupViewController.m**. Reemplace el contenido del archivo por el código siguiente, que implementa los métodos UIViewController:

   ```objc
   #import "SetupViewController.h"

    static NSString *const kNHMessageReceived = @"MessageReceived";
    
    @interface SetupViewController ()
    
    @end

    @implementation SetupViewController

    - (void)viewDidLoad {
        [super viewDidLoad];
        
        // Listen for messages using NSNotificationCenter
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(didReceivePushNotification:) name:kNHMessageReceived object:nil];
    }

    - (void)dealloc {
        // Clean up subscription to NSNotificationCenter
        [[NSNotificationCenter defaultCenter] removeObserver:self name:kNHMessageReceived object:nil];
    }

    - (void)didReceivePushNotification:(NSNotification *)notification {
        MSNotificationHubMessage *message = [notification.userInfo objectForKey:@"message"];

        // Create UI Alert controller with message title and body
        UIAlertController *alertController = [UIAlertController alertControllerWithTitle:message.title
                             message:message.body
                      preferredStyle:UIAlertControllerStyleAlert];
        [alertController addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleCancel handler:nil]];
        [self presentViewController:alertController animated:YES completion:nil];
        
        // Dismiss after 2 seconds
        dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
            [alertController dismissViewControllerAnimated:YES completion: nil];
        });

    }

    @end
   ```

3. Para asegurarse de que no haya errores, compile y ejecute la aplicación en el dispositivo.

## <a name="send-test-push-notifications"></a>Prueba de envío de las notificaciones push

Puede probar de recibir notificaciones en la aplicación con la opción **Envío de prueba** en [Azure Portal](https://portal.azure.com/). Envía una notificación push de prueba al dispositivo.

:::image type="content" source="media/ios-sdk/image6.png" alt-text="Enviar prueba":::

Las notificaciones push se envían normalmente en un servicio back-end como Mobile Apps o ASP.NET mediante una biblioteca compatible. Si no hay disponible ninguna biblioteca para su back-end, también puede usar la API REST directamente para enviar mensajes de notificación.

A continuación, presentamos una lista de algunos otros tutoriales que podría interesarle revisar para enviar notificaciones:

- Azure Mobile Apps: Para ver un ejemplo de cómo enviar notificaciones desde un back-end de Mobile Apps con Notification Hubs, consulte [Incorporación de notificaciones push a la aplicación iOS](/previous-versions/azure/app-service-mobile/app-service-mobile-ios-get-started-push).
- ASP.NET: [Uso de Notification Hubs para enviar notificaciones push a los usuarios](notification-hubs-aspnet-backend-ios-apple-apns-notification.md).
- SDK de Java para Azure Notification Hubs: consulte [Uso de Notification Hubs desde Java](notification-hubs-java-push-notification-tutorial.md) para enviar notificaciones desde Java. Esto se probó en Eclipse para el desarrollo de Android.
- PHP: [Uso de Notification Hubs desde PHP](notification-hubs-php-push-notification-tutorial.md).

## <a name="verify-that-your-app-receives-push-notifications"></a>Compruebe que la aplicación recibe notificaciones push

Para probar las notificaciones push en iOS, debe implementar la aplicación en un dispositivo iOS físico. No puede enviar notificaciones push de Apple con el simulador de iOS.

1. Ejecute la aplicación y compruebe que el registro se realiza correctamente, luego presione **OK**(Aceptar).

   :::image type="content" source="media/ios-sdk/image7.png" alt-text="Registro":::

2. Después, envíe una notificación push de prueba desde [Azure Portal](https://portal.azure.com/), como se ha descrito en la sección anterior.

3. La notificación push se envía a todos los dispositivos registrados para recibir las notificaciones del centro de notificaciones indicado.

   :::image type="content" source="media/ios-sdk/image8.png" alt-text="Envío de prueba":::

## <a name="next-steps"></a>Pasos siguientes

En este sencillo ejemplo, se difunden notificaciones push a todos los dispositivos iOS registrados. Para aprender a enviar notificaciones push a dispositivos iOS específicos, pase al siguiente tutorial:

[Tutorial: Envío de notificaciones push a dispositivos específicos](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

Para más información, consulte los siguientes artículos.

- [Introducción a Azure Notification Hubs](notification-hubs-push-notification-overview.md)
- [API de REST de Notification Hubs](/rest/api/notificationhubs/)
- [SDK de Notification Hubs para operaciones de back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [SDK de Notification Hubs en GitHub](https://github.com/Azure/azure-notificationhubs)
- [Registro en el back-end de aplicación](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Administración de registros](notification-hubs-push-notification-registration-management.md)
- [Trabajo con etiquetas](notification-hubs-tags-segment-push-message.md)
- [Trabajo con plantillas personalizadas](notification-hubs-templates-cross-platform-push-messages.md)
- [Control de acceso de Service Bus con Firmas de acceso compartido](../service-bus-messaging/service-bus-sas.md)
- [Generar tokens de SAS mediante programación](/rest/api/eventhub/generate-sas-token)
- [Seguridad de Apple: cifrado común](https://developer.apple.com/security/)
- [Tiempo UNIX](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)