---
title: Creación de un controlador de datos en Azure Data Studio
description: Creación de un controlador de datos en Azure Data Studio
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 12/09/2020
ms.topic: how-to
ms.openlocfilehash: 22ad2d65710a3fc149f5a83fb511244ac3be2203
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98733247"
---
# <a name="create-data-controller-in-azure-data-studio"></a>Creación de un controlador de datos en Azure Data Studio

Puede crear un controlador de datos mediante Azure Data Studio mediante el Asistente para la implementación y los notebooks.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Requisitos previos

- Necesita acceso a un clúster de Kubernetes y tener configurado el archivo kubeconfig para que apunte al clúster de Kubernetes en el que quiere realizar la implementación.
- Debe [instalar las herramientas de cliente](install-client-tools.md), incluidas **Azure Data Studio**, las extensiones de Azure Data Studio denominadas **Azure Arc** y **[!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]** .
- Debe iniciar sesión en Azure en Azure Data Studio.  Para ello, pulse CTRL/Comando + Mayús + P para abrir la ventana de texto del comando y escriba **Azure**.  Elija **Azure: Iniciar sesión**.   En el panel que aparece, haga clic en el icono + de la parte superior derecha para agregar una cuenta de Azure.

## <a name="use-the-deployment-wizard-to-create-azure-arc-data-controller"></a>Uso del Asistente para la implementación para crear el controlador de datos de Azure Arc

Siga estos pasos para crear un controlador de datos de Azure Arc mediante el Asistente para la implementación.

1. En Azure Data Studio, haga clic en la pestaña Conexiones en el panel de navegación izquierdo.
2. Haga clic en el botón **...** en la parte superior del panel Conexiones y elija **Nueva implementación...**
3. En el nuevo Asistente para implementación, elija **Controlador de datos de Azure Arc** y, después, haga clic en el botón **Seleccionar** en la parte inferior.
4. Asegúrese de que las herramientas de los requisitos previos están disponibles y cumplen con las versiones necesarias. Haga clic en **Siguiente**.
5. Use el archivo kubeconfig predeterminado o seleccione otro.  Haga clic en **Siguiente**.
6. Seleccione el contexto del clúster de Kubernetes. Haga clic en **Siguiente**.
7. Elija un perfil de configuración de implementación en función de su clúster de Kubernetes de destino. Haga clic en **Siguiente**.
8. Si está usando Red Hat OpenShift en Azure o Red Hat OpenShift Container Platform, aplique las restricciones del contexto de seguridad. Siga las instrucciones de [Aplicación de una restricción de contexto de seguridad para los servicios de datos habilitados para Azure Arc en OpenShift](how-to-apply-security-context-constraint.md).

   >[!IMPORTANT]
   >En Red Hat OpenShift en Azure o Red Hat OpenShift Container Platform, debe aplicar la restricción de contexto de seguridad antes de crear el controlador de datos.

1. Elija la suscripción y el grupo de recursos que quiera.
1. Seleccione una ubicación de Azure.
   
   La ubicación de Azure seleccionada aquí es la ubicación en Azure donde se almacenarán los *metadatos* sobre el controlador de datos y las instancias de base de datos que administra. El controlador de datos y las instancias de base de datos se crearán realmente en el clúster de Kubernetes siempre que sea posible.

10. Seleccione el modo de conectividad adecuado. Más información sobre los [modos de conectividad](https://docs.microsoft.com/azure/azure-arc/data/connectivity). Haga clic en **Siguiente**.

    Si selecciona el modo de conectividad directa, las credenciales de la entidad de servicio son obligatorias, como se describe en [Creación de una entidad de servicio](upload-metrics-and-logs-to-azure-monitor.md#create-service-principal).

11. Escriba un nombre para el controlador de datos y para el espacio de nombres en el que se creará el controlador de datos.

    El controlador de datos y el nombre del espacio de nombres se usarán para crear un recurso personalizado en el clúster de Kubernetes, por lo que deben cumplir las [convenciones de nomenclatura de Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#names).
    
    Si el espacio de nombres ya existe, se usará si aún no contiene otros objetos de Kubernetes, como, por ejemplo, pods.  Si el espacio de nombres no existe, se hará un intento de crear el espacio de nombres.  La creación de un espacio de nombres en un clúster de Kubernetes requiere privilegios de administrador de clústeres de Kubernetes.  Si no tiene privilegios de administrador de clústeres de Kubernetes, pida a su administrador de clústeres de Kubernetes que haga los primeros pasos del artículo [Creación de un controlador de datos mediante herramientas nativas de Kubernetes](./create-data-controller-using-kubernetes-native-tools.md), que debe seguir un administrador de Kubernetes antes de completar este asistente.


12. Seleccione la clase de almacenamiento en la que se implementará el controlador de datos. 
13.  Escriba un nombre de usuario y una contraseña y confirme la contraseña de la cuenta de usuario del administrador del controlador de datos. Haga clic en **Next**.

14. Revise la configuración de la implementación.
15. Haga clic en **Implementar** para implementar la configuración deseada, o en **Script a Notebook** para revisar las instrucciones de implementación o realizar los cambios necesarios, como los nombres de clase de almacenamiento o los tipos de servicio. Haga clic en **Ejecutar todas** en la parte superior del notebook.

## <a name="monitoring-the-creation-status"></a>Supervisión del estado de creación

La creación del controlador tardará unos minutos en completarse. Puede supervisar el progreso en otra ventana de terminal con los siguientes comandos:

> [!NOTE]
>  En los siguientes comandos de ejemplo se supone que ha creado un controlador de datos y un espacio de nombres de Kubernetes con el nombre "arc".  Si ha usado otro nombre de controlador de datos o de espacio de nombres, puede reemplazar "arc" por su nombre.

```console
kubectl get datacontroller/arc --namespace arc
```

```console
kubectl get pods --namespace arc
```

También puede comprobar el estado de creación de un pod determinado ejecutando un comando como el siguiente.  Esto es especialmente útil para solucionar problemas.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/control-2g7bl --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Solución de problemas de creación

Si tiene problemas con la creación, consulte la [guía de solución de problemas](troubleshoot-guide.md).