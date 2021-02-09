---
title: Importación de contraseñas en la aplicación Microsoft Authenticator - Azure AD
description: Cómo importar contraseñas en la aplicación de autenticación de Microsoft desde administradores de contraseñas populares.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 01/28/2021
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: b0d7eeeb840a3efc560c20310b38bee93a038795
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99056953"
---
# <a name="import-passwords-into-the-microsoft-authenticator-app"></a>Importación de contraseñas en la aplicación Microsoft Authenticator

Microsoft Authenticator admite la importación de contraseñas de Google Chrome, Firefox, LastPass, Bitwarden y RoboForm. Si Microsoft no admite actualmente su administrador de contraseñas existente, puede [escribir manualmente las credenciales de inicio de sesión en nuestra plantilla CSV](https://go.microsoft.com/fwlink/?linkid=2134938). Para importar las contraseñas existentes y administrarlas en la aplicación Authenticator, solo exporte las contraseñas del administrador de contraseñas existente en el formato de valores separados por comas (CSV). A continuación, importe el archivo CSV exportado a Authenticator con nuestra extensión del explorador Chrome o directamente a la aplicación Authenticator (iOS y Android).

## <a name="import-from-google-chrome-or-android-smart-lock"></a>Importación desde Google Chrome o Android Smart Lock

Puede importar sus contraseñas de Google Chrome o Android Smart Lock a Authenticator en su smartphone o dispositivo de escritorio. Puede:

- [Importar desde Chrome en Android e iOS](#import-from-chrome-on-android-and-ios)
- [Importar desde el explorador para escritorio Chrome](#import-from-chrome-desktop-browser)

### <a name="import-from-chrome-on-android-and-ios"></a>Importación desde Chrome en Android e iOS

Los usuarios de Google Chrome en móviles Android y Apple pueden importar sus contraseñas directamente desde su teléfono con unos sencillos pasos.

1. Instale la aplicación Authenticator en el teléfono y abra la pestaña **Contraseñas**.

1. Inicie sesión en Google Chrome en el teléfono.

1. Pulse el ![menú de puntos suspensivos de Google Chrome](./media/user-help-authenticator-app-import-passwords/ellipsis-chrome.png) en la parte superior derecha de los teléfonos Android o en la parte inferior derecha de los dispositivos iOS y, a continuación, pulse en **Configuración**.

   &nbsp; | &nbsp;
   ---------- | --------
   Android | ![Ubicación del menú Configuración de Google Chrome](./media/user-help-authenticator-app-import-passwords/android-settings-menu.png)
   iOS | ![Icono del menú Configuración de Google Chrome](./media/user-help-authenticator-app-import-passwords/apple-settings-menu.png)

1. En **Configuración**, abra **Contraseñas**.

   &nbsp; | &nbsp;
   ---------- | --------
   Android | ![Ubicación del comando Contraseñas en Chrome para Android](./media/user-help-authenticator-app-import-passwords/android-passwords-location.png)
   iOS | ![Ubicación del comando Contraseñas en Chrome para Apple](./media/user-help-authenticator-app-import-passwords/apple-passwords-location.png)

1. En los dispositivos Android, pulse el ![menú de puntos suspensivos de Google Chrome](./media/user-help-authenticator-app-import-passwords/ellipsis-chrome.png) en la parte superior derecha, o pulse en la parte inferior derecha de los dispositivos iOS y, a continuación, pulse en **Exportar contraseñas**.

   &nbsp; | &nbsp;
   ---------- | --------
   Android | ![Ubicación de Exportar contraseñas en Chrome para Android](./media/user-help-authenticator-app-import-passwords/android-export-passwords-location.png)
   iOS | ![Ubicación de Exportar contraseñas en Chrome para Apple](./media/user-help-authenticator-app-import-passwords/apple-export-passwords-location.png)

   Debe proporcionar un PIN, huella digital o reconocimiento facial. Confirme su identidad y pulse **Exportar contraseñas** de nuevo para iniciar la exportación.

1. Una vez exportadas las contraseñas, Chrome le pedirá que elija la aplicación en la que las importará. Seleccione **Authenticator** para iniciar la importación de contraseñas. Se le informará sobre el estado de la importación cuando haya finalizado.

   &nbsp; | &nbsp;
   ---------- | --------
   Android | ![Ubicación de Importar contraseñas en Chrome para Android](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
   iOS | ![Ubicación de Importar contraseñas en Chrome para Apple](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

### <a name="import-from-chrome-desktop-browser"></a>Importación desde el explorador para escritorio Chrome

Antes de comenzar, debe instalar e iniciar sesión en la [extensión Autorrellenado de Microsoft](https://chrome.google.com/webstore/detail/microsoft-autofill/fiedbfgcleddlbcmgdigjgdfcggjcion) en el explorador Chrome.

1. Abra el [Gestor de contraseñas de Google](https://passwords.google.com) en cualquier explorador. Inicie sesión en su cuenta de Google si aún no lo ha hecho.

1. Seleccione el icono de engranaje. ![Icono de engranaje del gestor de contraseñas de escritorio](./media/user-help-authenticator-app-import-passwords/desktop-password-manager-gear.png) para abrir la página Configuración de contraseñas.

1. Seleccione **Exportar** y, en la página siguiente, seleccione **Exportar** de nuevo para empezar a exportar sus contraseñas. Indique su contraseña de Google cuando se le pida para confirmar su identidad. Se le informará sobre el estado de la importación cuando haya finalizado.

   ![Ubicación del comando Exportar contraseñas del explorador Chrome para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-chrome-export-passwords-location.png)

1. Abra la extensión Autorrellenado para Chrome y seleccione **Configuración**.

   ![Ubicación de Configuración en la extensión Autorrellenado del explorador Chrome para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-chrome-autofill-settings.png)

1. Seleccione **Importar datos** para abrir un cuadro de diálogo. A continuación, seleccione **Elegir archivo** para buscar e importar el archivo CSV.

   ![Ubicación de Importar datos en CSV en el explorador Chrome para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-chrome-import-csv.png)

## <a name="import-from-firefox"></a>Importación desde Firefox

Firefox permite la exportación de contraseñas solo desde el explorador de escritorio, por lo que debe asegurarse de que tiene acceso al explorador Firefox para escritorio antes de importar las contraseñas desde dicha plataforma.

1. Inicie sesión en la versión más reciente de Firefox para escritorio y seleccione el ![Menú "hamburguesa" de Firefox](./media/user-help-authenticator-app-import-passwords/desktop-firefox-ellipsis-icon.png) menú en la parte superior derecha de la pantalla.

1. Seleccione **Inicios de sesión y contraseñas**.

   ![Ubicación de Inicios de sesión y contraseñas del explorador Firefox para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-firefox-passwords-location.png)

1. En la página de Firefox Lockwise, seleccione el ![menú de puntos suspensivos de Firefox](./media/user-help-authenticator-app-import-passwords/desktop-firefox-ellipsis-icon.png), seleccione **Exportar inicios de sesión** y confirme su intención al seleccionar **Exportar**. Se le pedirá que se identifique escribiendo su PIN, contraseña del dispositivo o escaneando sus huellas digitales. Una vez correctamente identificado, Firefox exportará las contraseñas en formato CSV a la ubicación seleccionada.

   ![Ubicación de Exportar contraseñas del explorador Firefox para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-firefox-export-passwords-location.png)

1. Puede importar sus contraseñas en Authenticator desde un explorador para escritorio o en teléfonos iOS o Android. Para importar a la aplicación Authenticator en el teléfono, haga lo siguiente:

      1. Transfiera el archivo CSV que exportó al teléfono Android o iOS mediante un método seguro de su preferencia y, a continuación, descárguelo. A continuación, comparta el archivo CSV con la aplicación Authenticator para iniciar la importación.

         &nbsp; | &nbsp;
         ---------- | --------
         Android | ![Ubicación de Importar contraseñas en Chrome para Android](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
         iOS | ![Ubicación de Importar contraseñas en Chrome para Apple](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

      1. Después de importar correctamente la contraseña a Authenticator, elimine el archivo CSV de su escritorio o teléfono móvil.

## <a name="import-from-lastpass"></a>Importación desde LastPass

LastPass solo admite la exportación de contraseñas desde un explorador de escritorio, por lo que debe asegurarse de que tiene acceso a un explorador para escritorio antes de empezar a importar las contraseñas.

1. Inicie sesión en el [sitio web de LastPass](https://lastpass.com), seleccione **Opciones avanzadas** y, a continuación, seleccione **Exportar**.

   ![Ubicación de Exportar contraseñas en LastPass para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-lastpass-export-passwords-location.png)

1. Identifíquese cuando se le pida que proporcione la contraseña maestra. Después, verá las contraseñas exportadas en la página web.

1. Copie el contenido de la página web.

1. Abra el Bloc de notas (o su editor de texto favorito) y pegue el contenido copiado.

1. Guarde este archivo del bloc de notas al seleccionar **Archivo** &gt; **Guardar como**. Proporcione un nombre que termine en ".csv" (por ejemplo, LastPass.csv) en una ubicación segura del escritorio.

   ![Guardar archivo CSV en LastPass para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-lastpass-save-import-file.png)

1. Puede importar sus contraseñas en Authenticator en un explorador de escritorio o en teléfonos iOS o Android. Para importar a la aplicación Authenticator en el teléfono, haga lo siguiente:

      1. Transfiera el archivo CSV que exportó al smartphone mediante un método seguro de su preferencia y, a continuación, descárguelo. Después, comparta el archivo CSV con la aplicación Authenticator para iniciar la importación.

         &nbsp; | &nbsp;
         ---------- | --------
         Android | ![Ubicación de Importar contraseñas en LastPass para Android](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
         iOS | ![Ubicación de Importar contraseñas en LastPass para Apple](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

      1. Después de importar correctamente la contraseña a Authenticator, elimine el archivo CSV de su escritorio o teléfono móvil.

## <a name="import-from-bitwarden"></a>Importación desde Bitwarden

Bitwarden solo admite la exportación de contraseñas desde un explorador de escritorio, por lo que debe asegurarse de que tiene acceso a un explorador para escritorio antes de empezar a importar las contraseñas.

1. Inicie sesión en https://vault.bitwarden.com/ y seleccione **Tools** &gt; **Export vault** (Herramientas > Exportar almacén). Elija el formato de archivo como CSV, proporcione su contraseña maestra y, a continuación, seleccione **Export vault** (Exportar almacén) para iniciar la exportación.

   ![Ubicación de Export vault en Bitwarden](./media/user-help-authenticator-app-import-passwords/desktop-bitwarden-export-command-location.png)

1. Puede importar sus contraseñas en Authenticator en un explorador para escritorio o en teléfonos iOS o Android. Para importar a la aplicación Authenticator en el teléfono, haga lo siguiente:

      1. Transfiera el archivo CSV que exportó al smartphone mediante un método seguro de su preferencia y, a continuación, descárguelo. Después, comparta el archivo CSV con la aplicación Authenticator para iniciar la importación.

         &nbsp; | &nbsp;
         ---------- | --------
         Android | ![Ubicación de Importar contraseñas en Bitwarden para Android](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
         iOS | ![Ubicación de Importar contraseñas en Bitwarden para Apple](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

      1. Después de importar correctamente la contraseña a Authenticator, elimine el archivo CSV de su escritorio o teléfono móvil.

## <a name="import-from-roboform"></a>Importación desde RoboForm

RoboForm permite la exportación de contraseñas solo desde su aplicación de escritorio, por lo que debe asegurarse de que tiene acceso a la aplicación RoboForm en su escritorio antes de iniciar la importación.

1. Inicie RoboForm desde el cliente de escritorio e inicie sesión en su cuenta.

1. Seleccione **Options** (Opciones) en el menú **RoboForm**.

   ![Menú Options de RoboForm para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-roboform-options.png)

1. Seleccione **Account & Data** &gt; **Export** (Cuenta y datos > Exportar).

   ![Ubicación del comando Export de RoboForm para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-roboform-accounts-data.png)

1. Elija una ubicación segura para guardar el archivo exportado. Seleccione **Logins** (Inicios de sesión) como el tipo de **Data** (Datos), seleccione archivo CSV como formato y, a continuación, seleccione **Export** (Exportar).

   ![Cuadro de diálogo Export de RoboForm para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-roboform-export-dialog.png)

1. Confirme su intención y el archivo CSV se exportará a la ubicación seleccionada.

   ![Cuadro de diálogo de confirmación Export de RoboForm para escritorio](./media/user-help-authenticator-app-import-passwords/desktop-roboform-confirmation.png)

1. Puede importar sus contraseñas en Authenticator en un explorador para escritorio o en teléfonos iOS o Android. Para importar a la aplicación Authenticator en el teléfono, haga lo siguiente:

      1. Transfiera el archivo CSV que exportó al smartphone mediante un método seguro de su preferencia y, a continuación, descárguelo. Después, comparta el archivo CSV con la aplicación Authenticator para iniciar la importación.

         &nbsp; | &nbsp;
         ---------- | --------
         Android | ![Ubicación de Import passwords en RoboForm para Android](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
         iOS | ![Ubicación de Import passwords en RoboForm para Apple](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

      1. Después de importar correctamente la contraseña a Authenticator, elimine el archivo CSV de su escritorio o teléfono móvil.

## <a name="import-by-creating-a-csv"></a>Importación mediante la creación de un archivo CSV

Si los pasos para importar contraseñas de su gestor de contraseñas no se muestran en este artículo, puede crear un archivo CSV que puede usar para importar sus contraseñas en Authenticator. Microsoft recomienda que siga estos pasos en un dispositivo de escritorio para facilitar el formateo.

1. En el escritorio, [descargue y abra la plantilla de importación](https://go.microsoft.com/fwlink/?linkid=2134938). Si es un usuario de iPhone, Safari y llaveros de Apple, ahora puede ir al paso 4.

1. Exporte las contraseñas del gestor de contraseñas existente en un archivo CSV sin cifrar.

1. Copie las columnas pertinentes del archivo CSV que exportó en el CSV de la plantilla y, a continuación, guarde el archivo.

1. Si no tiene un archivo CSV exportado, puede copiar cada inicio de sesión del gestor de contraseñas existente en el CSV de la plantilla. No quite ni cambie la fila de encabezado. Cuando termine, compruebe la integridad de los datos antes de comenzar el paso siguiente.

1. Puede importar sus contraseñas en Authenticator en un explorador para escritorio o en teléfonos iOS o Android. Para importar a la aplicación Authenticator en el teléfono, haga lo siguiente:

      1. Transfiera el archivo CSV que exportó al smartphone mediante un método seguro de su preferencia y, a continuación, descárguelo. Después, comparta el archivo CSV con la aplicación Authenticator para iniciar la importación.

         &nbsp; | &nbsp;
         ---------- | --------
         Android | ![Ubicación de Importar contraseñas desde CSV para Android](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
         iOS | ![Ubicación de Importar contraseñas desde CSV para Apple](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

      1. Después de importar correctamente la contraseña a Authenticator, elimine el archivo CSV de su escritorio o teléfono móvil.

## <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

La causa más común de las importaciones con errores es un formato incorrecto en el archivo CSV. Puede probar los siguientes pasos para solucionar la incidencia.

- Consulte este artículo para ver si ya se admite la importación de contraseñas desde su gestor de contraseñas actual. De ser así, se recomienda que intente de nuevo la importación siguiendo los pasos mencionados en el proveedor correspondiente.

- Si actualmente no se admite la importación del formato de su gestor de contraseñas, puede volver a intentarlo al [crear el archivo CSV manualmente](#import-by-creating-a-csv).

- Puede comprobar la integridad de los datos CSV con las siguientes sugerencias:

  - La primera fila debe contener un encabezado con tres columnas: **url**, **username** (nombre de usuario) y **password** (contraseña).

  - Cada fila debe contener un valor en las columnas **url** y **password**.

- Para recrear el CSV, pegue el contenido en el [archivo de plantilla CSV](https://go.microsoft.com/fwlink/?linkid=2134938).

- Si ningún otro método funciona, notifique el problema con el vínculo **Enviar comentarios** desde la configuración de la aplicación Authenticator.
