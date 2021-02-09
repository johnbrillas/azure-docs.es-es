---
title: Configuración de un entorno de ensayo en Azure Spring Cloud | Microsoft Docs
description: Aprenda a usar la implementación blue-green con Azure Spring Cloud
author: MikeDodaro
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: brendm
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 991a335207fc29cef7b243d7e520dd5f62ff691f
ms.sourcegitcommit: 2dd0932ba9925b6d8e3be34822cc389cade21b0d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99226122"
---
# <a name="set-up-a-staging-environment-in-azure-spring-cloud"></a>Configuración de un entorno de ensayo en Azure Spring Cloud

**Este artículo se aplica a:** ✔️ Java

En este artículo se aborda cómo configurar una implementación de ensayo con el patrón de implementación azul-verde en Azure Spring Cloud. La implementación azul/verde es un patrón de entrega continua de Azure DevOps que se basa en mantener una versión existente (azul) activa mientras se implementa una nueva (verde). En este artículo se muestra cómo llevar la implementación de ensayo a producción sin cambiar directamente la implementación de producción.

## <a name="prerequisites"></a>Requisitos previos

* Instancia de Azure Spring Cloud con el **plan de tarifa** *estándar*.
* Una aplicación en ejecución.  Consulte [Quickstart: Implementación de la primera aplicación de Azure Spring Cloud](spring-cloud-quickstart.md).
* [Extensión asc](https://docs.microsoft.com/cli/azure/azure-cli-extensions-overview) de la CLI de Azure

Si desea usar otra aplicación en este ejemplo, deberá realizar un sencillo cambio en una parte pública de la aplicación.  Con este cambio se diferencia la implementación de ensayo de la producción.

>[!TIP]
> Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar las instrucciones de este artículo.  Incluye herramientas comunes de Azure preinstaladas, entre las que se incluyen las versiones más recientes de Git, JDK, Maven y la CLI de Azure. Si ha iniciado sesión en su suscripción a Azure, inicie [Azure Cloud Shell](https://shell.azure.com).  Para obtener más información, consulte [Información general de Azure Cloud Shell](../cloud-shell/overview.md).

Para configurar un entorno de ensayo en Azure Spring Cloud, siga las instrucciones de las secciones siguientes.

## <a name="install-the-azure-cli-extension"></a>Instalación de la extensión de la CLI de Azure

Para instalar la extensión de Azure Spring Cloud para la CLI de Azure, use el siguiente comando:

```azurecli
az extension add --name spring-cloud
```
    
## <a name="view-apps-and-deployments"></a>Visualización de aplicaciones e implementaciones

Vea las aplicaciones implementadas siguiendo los procedimientos a continuación.

1. Vaya a la instancia de Azure Spring Cloud en Azure Portal.

1. En el panel de navegación izquierdo, abra **Implementaciones**.

    [ ![Implementación: en desuso](media/spring-cloud-blue-green-staging/deployments.png)](media/spring-cloud-blue-green-staging/deployments.png)

1. Abra la hoja "Aplicaciones" para ver las aplicaciones de la instancia de servicio.

    [ ![Aplicaciones: panel](media/spring-cloud-blue-green-staging/app-dashboard.png)](media/spring-cloud-blue-green-staging/app-dashboard.png)

1. Puede hacer clic en una aplicación y ver los detalles.

    [ ![Aplicaciones: información general](media/spring-cloud-blue-green-staging/app-overview.png)](media/spring-cloud-blue-green-staging/app-overview.png)

1. Abra la hoja **Implementaciones** para ver todas las implementaciones de la aplicación. La cuadrícula de implementación muestra si la implementación está en etapa de producción o de ensayo.

    [ ![Panel Implementaciones](media/spring-cloud-blue-green-staging/deployments-dashboard.png)](media/spring-cloud-blue-green-staging/deployments-dashboard.png)

1. Puede hacer clic en el nombre de la implementación para ver la información general de la implementación. En este caso, la única implementación se denomina *Predeterminado*.

    [ ![Información general de implementaciones](media/spring-cloud-blue-green-staging/deployments-overview.png)](media/spring-cloud-blue-green-staging/deployments-overview.png)
    

## <a name="create-a-staging-deployment"></a>Creación de una implementación de ensayo

1. En el entorno de desarrollo local, haga una pequeña modificación en la aplicación. Hacerlo le permitirá diferenciar fácilmente las dos implementaciones. Para compilar el paquete jar, ejecute el siguiente comando: 

    ```console
    mvn clean package -DskipTests
    ```

1. En la CLI de Azure, cree una implementación y asígnele el nombre de la implementación de ensayo "green".

    ```azurecli
    az spring-cloud app deployment create -g <resource-group-name> -s <service-instance-name> --app <appName> -n green --jar-path gateway/target/gateway.jar
    ```

1. Después de que la implementación de la CLI se complete correctamente, acceda a la página de la aplicación desde el **Panel de la aplicación** y vea todas las instancias en la pestaña **Implementaciones** a la izquierda.

   [ ![Panel implementaciones después de la implementación de green](media/spring-cloud-blue-green-staging/deployments-dashboard-2.png)](media/spring-cloud-blue-green-staging/deployments-dashboard-2.png)

  
> [!NOTE]
> El estado de la detección es *OUT_OF_SERVICE*, por lo que el tráfico no se enrutará a esta implementación antes de que se complete la verificación.

## <a name="verify-the-staging-deployment"></a>Comprobación de la implementación de ensayo

Para comprobar que el desarrollo de ensayo de green funciona:
1. Vaya a **Implementaciones** y haga clic en `green` **Implementación de almacenamiento provisional**.
1. En la página **Información general**, haga clic en **Punto de conexión de prueba**.
1. Se abrirá la compilación de ensayo que muestra los cambios.

>[!TIP]
> * Confirme que el punto de conexión de prueba termina con una barra diagonal (/), con el fin de asegurarse de que el archivo CSS se ha cargado correctamente.  
> * Si el explorador requiere que escriba las credenciales de inicio de sesión para ver la página, use [Descodificar como dirección URL ](https://www.urldecoder.org/) para descodificar el punto de conexión de prueba. La descodificación de URL devuelve una dirección URL con el formato "https://\<username>:\<password>@\<cluster-name>.test.azureapps.io/gateway/green".  Use este formulario para acceder al punto de conexión.

>[!NOTE]    
> Los valores del servidor de configuración se aplican tanto al entorno de ensayo como al de producción. Por ejemplo, si establece la ruta de acceso del contexto (`server.servlet.context-path`) para la puerta de enlace de aplicaciones en el servidor de configuración como *somepath*, la ruta de acceso a la implementación green cambia a "https://\<username>:\<password>@\<cluster-name>.test.azureapps.io/gateway/green/somepath/...".
 
 Si va a la puerta de enlace de aplicaciones pública en este momento, debería ver la página anterior sin el nuevo cambio.
    
## <a name="set-the-green-deployment-as-the-production-environment"></a>Establecimiento de la implementación green como entorno de producción

1. Una vez que haya verificado el cambio en el entorno de ensayo, puede enviarlo a producción. Vuelva a **Administración de implementación** y seleccione la aplicación que está actualmente en `Production`.

1. Haga clic en los puntos suspensivos después del **Estado de registro** y establezca la compilación de producción en `staging`.

   [ ![Implementación de ensayo de conjunto de implementaciones](media/spring-cloud-blue-green-staging/set-staging-deployment.png)](media/spring-cloud-blue-green-staging/set-staging-deployment.png)

1. Vuelva a la página **Administración de implementación**. Establezca la implementación `green` en `production`. Una vez finalizada la configuración, el estado de la implementación `green` debería mostrar *Activo*. Ahora esta es la compilación de producción en ejecución.

   [ ![Resultado de la implementación de ensayo de conjunto de implementaciones](media/spring-cloud-blue-green-staging/set-staging-deployment-result.png)](media/spring-cloud-blue-green-staging/set-staging-deployment-result.png)

1. La dirección URL de la aplicación debe mostrar los cambios.

>[!NOTE]
> Después de haber establecido la implementación green como entorno de producción, la implementación anterior se convierte en la implementación de ensayo.

## <a name="modify-the-staging-deployment"></a>Modificación de la implementación de ensayo

Si no está satisfecho con el cambio, puede modificar el código de la aplicación, compilar un nuevo paquete jar y cargarlo en la implementación green mediante la CLI de Azure.

```azurecli
az spring-cloud app deploy  -g <resource-group-name> -s <service-instance-name> -n gateway -d green --jar-path gateway.jar
```

## <a name="delete-the-staging-deployment"></a>Eliminación de la implementación de ensayo

Para eliminar una implementación de ensayo desde Azure Portal, vaya a la página de la implementación de ensayo y seleccione el botón **Eliminar**.

Como alternativa, también puede eliminarla desde la CLI de Azure con el siguiente comando:

```azurecli
az spring-cloud app deployment delete -n <staging-deployment-name> -g <resource-group-name> -s <service-instance-name> --app gateway
```

## <a name="next-steps"></a>Pasos siguientes

* [CI/CD para Azure Spring Cloud](https://review.docs.microsoft.com/azure/spring-cloud/spring-cloud-howto-cicd?branch=pr-en-us-142929&pivots=programming-language-java)
