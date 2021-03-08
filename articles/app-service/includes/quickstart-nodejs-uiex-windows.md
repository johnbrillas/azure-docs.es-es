---
title: 'Inicio rápido: Crear una aplicación web de Node.js en Windows'
description: Implemente su primera aplicación Hola mundo de Node.js en Azure App Service en pocos minutos para la plataforma de Windows.
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 08/01/2020
ms.custom: mvc, devcenter, seodec18
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 302e0dc79d13eedebf810df042dc31f78b173fb6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101747787"
---
<!-- advanced for windows -->

## <a name="3-deploy-to-azure-app-service-from-visual-studio-code"></a>3. Implementación en Azure App Service desde Visual Studio Code

1. Abra su carpeta de aplicaciones en Visual Studio Code.

    ```bash
    code .
    ```

1. En el explorador de **AZURE APP SERVICE**, seleccione **Iniciar sesión en Azure...** y siga las instrucciones. Una vez que haya iniciado sesión, el explorador debe mostrar el nombre de la suscripción de Azure.

    ![Inicio de sesión en Azure](../media/quickstart-nodejs/sign-in.png)

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

1. seleccione la opción **Create new Web App... Advanced** (Crear nueva aplicación web... Avanzado) para implementar aplicaciones en App Service, en Windows.

1. Escriba un nombre único global <abbr title="Los caracteres válidos para un nombre de aplicación son “a-z”, “0-9” y “-”.">name</abbr> para la aplicación web y presione **Entrar**. 
1. Seleccione **Create a new resource group** (Crear un grupo de recursos nuevo) y escriba un nombre para el grupo de recursos, como `AppServiceQS-rg`.
1. Seleccione la **versión de Node.js** (le recomendamos que use “LTS”).

    En el canal de notificación, se muestran los recursos de Azure que se estén creando para la aplicación.
1. Seleccione **Windows** como sistema operativo.
1. Seleccione **Create new App Service plan** (Crear nuevo plan de App Service) y, a continuación, escriba un nombre para el plan (por ejemplo, `AppServiceQS-plan`); después, seleccione **F1 Free** (F1 gratis) como plan de tarifa.
1. Elija **Omitir por ahora** cuando se le pregunte sobre Application Insights.
1. Elija una región cerca de usted o de los recursos a los que desea acceder.

1. Seleccione **Sí** cuando se le solicite que actualice el área de trabajo para que las implementaciones posteriores se dirijan automáticamente a la misma instancia de App Service Web App. 

    :::image type="content" source="../media/quickstart-nodejs/save-configuration.png" alt-text="Captura de pantalla del símbolo del sistema para actualizar el área de trabajo con el botón Sí seleccionado.":::

1. Haga clic con el botón derecho en el no del servicio de aplicaciones una vez más y seleccione **Implementar en aplicación web**.

1. Haga clic con el botón derecho en el nodo del servicio de aplicaciones una vez más y seleccione **Browse Website** (Examinar sitio web).

    [Notificar un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

1. Una vez finalizada la implementación, seleccione **Browse Website** (Examinar sitio web) en el mensaje para ver la aplicación web recién implementada.

<br>
<details>
<summary>Solución de problemas</summary>
* Asegúrese de que la aplicación escuche en el puerto proporcionado por la variable de entorno PORT: `process.env.PORT`.
* Si ve el error **“No tiene permiso para ver este directorio o esta página”** , puede que la aplicación no se haya iniciado correctamente. Revise la salida del registro para identificar y corregir el error. 

</details>

<br>

[Notificar un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&prepare-your-environment)


<br>
<hr/>