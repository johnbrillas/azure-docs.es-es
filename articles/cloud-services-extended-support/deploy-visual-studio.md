---
title: Uso de Cloud Services (soporte extendido, versión preliminar)
description: Aprenda a crear e implementar una instancia de Azure Cloud Services mediante Azure Resource Manager con Visual Studio.
author: ghogen
ms.service: cloud-services-extended-support
manager: jillfra
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: tutorial
ms.date: 10/5/2020
ms.author: ghogen
ms.openlocfilehash: b12f8beeee054e547a6b58750d8482d32ee14453
ms.sourcegitcommit: 3c8964a946e3b2343eaf8aba54dee41b89acc123
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98747263"
---
# <a name="create-and-deploy-a-azure-cloud-service-extended-support-using-visual-studio"></a>Creación e implementación de una instancia de Azure Cloud Services (soporte extendido) mediante Visual Studio

A partir de [Visual Studio 2019 versión 16.9](https://visualstudio.microsoft.com/vs/preview/) (actualmente en versión preliminar), puede trabajar con los servicios en la nube con Azure Resource Manager (ARM), lo que simplifica considerablemente y moderniza el mantenimiento y la administración de los recursos de Azure. Esto lo habilita un nuevo servicio de Azure denominado Cloud Services (soporte extendido). Puede publicar un servicio en la nube existente en Cloud Services (soporte extendido). Para obtener información sobre este servicio de Azure, consulte la [documentación de Cloud Services (soporte extendido)](overview.md).

> [!IMPORTANT]
> Cloud Services (soporte extendido) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-feature-for-your-subscription"></a>Registro de la característica de la suscripción
Cloud Services (soporte extendido) se encuentra actualmente en versión preliminar. Registre de la característica de la suscripción tal como se muestra a continuación:

```powershell
Register-AzProviderFeature -FeatureName CloudServices -ProviderNamespace Microsoft.Compute
```
Para más información, consulte [Requisitos previos para implementar Cloud Services (soporte extendido)](deploy-prerequisite.md).

## <a name="create-a-project"></a>Crear un proyecto

Visual Studio proporciona una plantilla de proyecto que le permite crear un servicio en la nube de Azure con soporte extendido, denominado **Azure Cloud Services (soporte extendido)** . Una instancia de Cloud Services es un servicio de Azure de uso general sencillo. Una vez creado el proyecto, Visual Studio le permite configurar, depurar e implementar la instancia de Cloud Services en Azure.

### <a name="to-create-an-azure-cloud-service-extended-support-project-in-visual-studio"></a>Para crear un proyecto de Azure Cloud Services (soporte extendido) en Visual Studio

En esta sección se le enseñará cómo crear un proyecto de Azure Cloud Services en Visual Studio con uno o más roles web.

1. En la ventana de inicio, elija **Crear un proyecto**.

1. En el cuadro de búsqueda, escriba *Cloud* y, a continuación, elija **Azure Cloud Services (soporte extendido)** .

   ![Nueva instancia de Azure Cloud Services con soporte extendido](./media/choose-project-template.png)

1. Asigne un nombre al proyecto y elija **Crear**.

   ![Asignación de un nombre al proyecto](./media/configure-new-project.png)

1. En el cuadro de diálogo **Nuevo servicio en la nube de Microsoft Azure**, seleccione los roles que desea agregar y elija el botón de flecha a la derecha para agregarlos a la solución.

    ![Selección de nuevos roles de Azure Cloud Services](./media/choose-roles.png)

1. Para cambiar el nombre de un rol que haya agregado, mantenga el puntero sobre el rol en el cuadro de diálogo **Nuevo servicio en la nube de Microsoft Azure** y, en el menú contextual, seleccione **Cambiar nombre**. También puede cambiar el nombre de un rol dentro de la solución (en el **Explorador de soluciones**) una vez que lo haya agregado.

    ![Cambio de nombre de rol de Azure Cloud Services](./media/new-cloud-service-rename.png)

El proyecto de Azure de Visual Studio tiene asociaciones a los proyectos de rol de la solución. El proyecto incluye también el *archivo de definición de servicio* y el *archivo de configuración de servicio*:

- **Archivo de definición de servicio**: define la configuración del entorno de tiempo de ejecución de la aplicación, incluidos los roles que se requieren, los puntos de conexión y el tamaño de máquina virtual.
- **Archivo de configuración de servicio**: configura el número de instancias de un rol que se ejecutan y los valores de la configuración definida para un rol.

Para más información sobre estos archivos, consulte [Configuración de los roles para un servicio de Azure con Visual Studio](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-configure-roles-for-cloud-service).

## <a name="publish-a-cloud-service"></a>Publicación de una instancia de Cloud Services

1. Cree o abra un proyecto de Azure Cloud Services en Visual Studio.

1. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y, en el menú contextual, seleccione **Publicar**.

   ![Página de inicio de sesión](./media/publish-step-1.png)

1. **Cuenta**: seleccione una cuenta o seleccione **Agregar una cuenta** en la lista desplegable de cuentas.

1. **Elija una suscripción**: elija la suscripción que desea usar para la implementación. La suscripción que se usa para implementar instancias de Cloud Services (soporte extendido) necesita tener roles de propietario o colaborador asignados mediante el control de acceso basado en rol (RBAC). Si su suscripción no tiene ninguno de estos roles, consulte los [pasos para agregar una asignación de roles](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-steps) para agregarlos antes de continuar.

1. Elija **Siguiente** para ir a la página **Configuración**.

   ![Configuración común](./media/publish-settings.png)

1. **Servicio en la nube**: en la lista desplegable, seleccione una instancia de Cloud Services existente o seleccione **Crear nuevo** para crear una instancia nueva. El centro de datos aparece entre paréntesis para cada instancia de Cloud Services. Se recomienda que la ubicación del centro de datos de la instancia de Cloud Services sea la misma ubicación del centro de datos de la cuenta de almacenamiento.

   Si decide crear una nueva instancia de Cloud Services, verá el cuadro de diálogo **Create Cloud Service (extended support)** (Crear un servicio en la nube [soporte extendido]). Especifique la ubicación y el grupo de recursos que desea usar para la instancia de Cloud Services.

   ![Creación de una instancia de Cloud Services con soporte extendido](./media/extended-support-dialog.png)

1. **Configuración de compilación**: seleccione **Depurar** o **Liberar**.

1. **Configuración de servicio**: seleccione **Nube** o **Local**.

1. **Cuenta de almacenamiento**: seleccione la cuenta de almacenamiento que desea usar para esta implementación o **Crear nuevo** para crear una nueva. La región aparece entre paréntesis para cada cuenta de almacenamiento. Se recomienda que la ubicación del centro de datos de la cuenta de almacenamiento sea la misma ubicación del centro de datos de la instancia de Cloud Services (configuración común).

   La cuenta de almacenamiento de Azure almacena el paquete para la implementación de la aplicación. Una vez implementada la aplicación, el paquete se quita de la cuenta de almacenamiento.

1. **Key Vault**: especifique la instancia de Key Vault que contiene los secretos para esta instancia de Cloud Services. Esta opción está habilitada si el escritorio remoto está habilitado o se agregan certificados a la configuración.

1. **Habilitar Escritorio remoto para todos los roles**: seleccione esta opción si desea poder conectarse de forma remota al servicio. Se le pedirá que especifique las credenciales.

   ![Configuración de escritorio remoto](./media/remote-desktop-configuration.png)

1. Elija **Siguiente** para ir a la página **Configuración de diagnóstico**.

   ![Configuración de diagnóstico](./media/diagnostics-settings.png)

   Los diagnósticos le permiten solucionar los problemas de un servicio en una instancia de Azure Cloud Services (o en una máquina virtual de Azure). Para más información sobre el diagnóstico, consulte [Configuración de Diagnósticos en Azure Cloud Services y Virtual Machines](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines). Para más información sobre Application Insights, consulte [¿Qué es Application Insights?](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview).

1. Haga clic en **Siguiente** para avanzar a la página de **resumen**.

   ![Resumen](./media/publish-summary.png)

1. **Perfil de destino**: puede optar por crear un perfil de publicación a partir de la configuración que eligió. Por ejemplo, puede crear un perfil para un entorno de pruebas y otro para producción. Para guardar este perfil, elija el icono **Guardar**. El asistente creará el perfil y lo guardará en el proyecto de Visual Studio. Para modificar el nombre del perfil, abra la lista **Perfil de destino** y, a continuación, elija **Administrar…**.

   > [!Note]
   > El perfil de publicación aparecerá en el Explorador de soluciones en Visual Studio y su configuración se escribirá en un archivo con la extensión .azurePubxml. La configuración se guarda como atributos de etiquetas XML.

1. Una vez que ajuste toda la configuración de la implementación del proyecto, seleccione **Publicar** en la parte inferior del cuadro de diálogo. Puede supervisar el estado del proceso en la ventana de salida **Registro de actividad de Azure** de Visual Studio.

Enhorabuena. Ha publicado el proyecto de la instancia de Cloud Services con soporte extendido en Azure. Para volver a publicar con la misma configuración, puede volver a usar el perfil de publicación o repetir estos pasos para crear uno nuevo.

## <a name="clean-up-azure-resources"></a>Limpieza de los recursos de Azure

Para limpiar los recursos de Azure que creó siguiendo este tutorial, vaya al [Azure Portal](https://portal.azure.com), elija **Grupos de recursos**, busque y abra el grupo de recursos que usó para crear el servicio y elija **Eliminar el grupo de recursos**.

## <a name="next-steps"></a>Pasos siguientes

Configure la integración continua (CI) mediante el botón **Configurar** de la pantalla **Publicar**. Para más información, consulte la [documentación de Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines).
