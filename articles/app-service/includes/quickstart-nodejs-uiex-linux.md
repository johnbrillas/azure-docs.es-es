---
title: 'Inicio rápido: Crear una aplicación web de Node.js en Linux'
description: Implementación de su primera aplicación Hola mundo de Node.js en Azure App Service en cuestión de minutos.
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 08/01/2020
ms.custom: mvc, devcenter, seodec18
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 59a2c96987762e6b56cc7b453877cebe3124e443
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102109142"
---
<!-- default for linux -->

## <a name="3-deploy-to-azure-app-service-from-visual-studio-code"></a>3. Implementación en Azure App Service desde Visual Studio Code

1. Abra su carpeta de aplicaciones en Visual Studio Code.

    ```bash
    code .
    ```

1. En el explorador de **AZURE APP SERVICE**, seleccione **Iniciar sesión en Azure...** y siga las instrucciones. Una vez que haya iniciado sesión, el explorador debe mostrar el nombre de la suscripción de Azure.

    ![Iniciar sesión en Azure](../media/quickstart-nodejs/sign-in.png)
    <br>
    <details>
    <summary>Solución de problemas de inicio de sesión en Azure</summary>
    
    Si ve el error **"Cannot find subscription with name [subscription ID]"** ("No se encuentra la suscripción con el nombre [identificador de suscripción]"), puede que sea debido a que esté detrás de un proxy y no se pueda establecer la conexión con la API de Azure. Configure las variables de entorno `HTTP_PROXY` y `HTTPS_PROXY` con su información de proxy en el terminal mediante `export`.
    
    ```bash
    export HTTPS_PROXY=https://username:password@proxy:8080
    export HTTP_PROXY=http://username:password@proxy:8080
    ```

    [Notificar un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)


1. En el explorador de **AZURE APP SERVICE**, seleccione el icono de flecha arriba azul para implementar la aplicación en Azure. 

    :::image type="content" source="../media/quickstart-nodejs/deploy.png" alt-text="Captura de pantalla del servicio Aplicación de Azure en VS Code que muestra el icono de flecha azul seleccionado.":::

1. Seleccione el directorio que tenga abierto actualmente, `nodejs-docs-hello-world`.

1. Elija **Create new Web App** (Crear aplicación web), que se implementa en App Service en Linux de forma predeterminada.

1. Escriba un nombre único global <abbr title="Los caracteres válidos para un nombre de aplicación son “a-z”, “0-9” y “-”.">name</abbr> para la aplicación web y presione **Entrar**. 

1. Seleccione la **versión de Node.js** (le recomendamos que use “LTS”).

    En el canal de notificación, se muestran los recursos de Azure que se estén creando para la aplicación.

1. Seleccione **Sí** cuando se le pida que actualice la configuración para ejecutar `npm install` en el servidor de destino. Después, se implementará la aplicación.

    :::image type="content" source="../media/quickstart-nodejs/server-build.png" alt-text="Captura de pantalla del símbolo del sistema para actualizar la configuración en el servidor de destino con el botón Sí seleccionado.":::

1. Seleccione **Sí** cuando se le solicite que actualice el área de trabajo para que las implementaciones posteriores se dirijan automáticamente a la misma instancia de App Service Web App. 

    :::image type="content" source="../media/quickstart-nodejs/save-configuration.png" alt-text="Captura de pantalla del símbolo del sistema para actualizar el área de trabajo con el botón Sí seleccionado.":::




1. Una vez finalizada la implementación, seleccione **Browse Website** (Examinar sitio web) en el mensaje para ver la aplicación web recién implementada.

<br/>
<details>
<summary><strong>Solución de problemas</strong></summary>

Si no pudo completar estos pasos, compruebe lo siguiente:

* Asegúrese de que la aplicación escuche en el puerto proporcionado por la variable de entorno PORT: `process.env.PORT`.

* Si ve el error **“No tiene permiso para ver este directorio o esta página”** , puede que la aplicación no se haya iniciado correctamente. Revise la salida del registro para identificar y corregir el error. 

</details>

<br>

[Notificar un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&prepare-your-environment)


<br/>
<hr/>


