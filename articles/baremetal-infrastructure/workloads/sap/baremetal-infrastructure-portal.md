---
title: Unidades de instancia de BareMetal en Azure
description: Aprenda a identificar e interactuar con las unidades de instancia de BareMetal mediante Azure Portal.
ms.topic: how-to
ms.date: 12/31/2020
ms.openlocfilehash: 927baa79519781ef74920b17bc9fcd858f0f6c6f
ms.sourcegitcommit: 42922af070f7edf3639a79b1a60565d90bb801c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97829077"
---
# <a name="manage-baremetal-instances-through-the-azure-portal"></a>Administración de instancias de BareMetal mediante Azure Portal
 
En este artículo se muestra cómo [Azure Portal](https://portal.azure.com/) muestra [instancias de BareMetal](baremetal-overview-architecture.md). En este artículo también se muestran las actividades que puede realizar en Azure Portal con las unidades de instancia de BareMetal implementadas. 
 
## <a name="register-the-resource-provider"></a>Registrar el proveedor de recursos
Un proveedor de recursos de Azure para instancias de BareMetal proporciona visibilidad de las instancias en Azure Portal, actualmente en versión preliminar pública. De forma predeterminada, la suscripción a Azure que se usa para las implementaciones de instancias de BareMetal registra el proveedor de recursos *BareMetalInfrastructure*. Si no ve las unidades de instancia de BareMetal implementadas, debe registrar el proveedor de recursos en la suscripción. Hay dos maneras de registrar el proveedor de recursos de instancias de BareMetal:
 
* [CLI de Azure](#azure-cli)
 
* [Azure Portal](#azure-portal)
 
### <a name="azure-cli"></a>CLI de Azure
 
Inicie sesión en la suscripción de Azure que usa para la implementación de instancias de BareMetal mediante la CLI de Azure. Puede registrar el proveedor de recursos BareMetalInfrastructure con:

```azurecli-interactive
az provider register --namespace Microsoft.BareMetalInfrastructure
```
 
Para más información, consulte el artículo [Tipos y proveedores de recursos de Azure](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell).
 
### <a name="azure-portal"></a>Azure portal
 
Puede registrar el proveedor de recursos BareMetalInfrastructure mediante Azure Portal.
 
Deberá mostrar su suscripción en Azure Portal y, luego, hacer doble clic en la suscripción que usó para implementar sus unidades de instancia de BareMetal.
 
1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el menú de Azure Portal, seleccione **Todos los servicios**.

1. En el cuadro **Todos los servicios**, escriba **suscripción** y, a continuación, seleccione **Suscripciones**.

1. Seleccione la suscripción en la lista de suscripción para verla.

1. Seleccione **Proveedores de recursos** y escriba **BareMetalInfrastructure** en la búsqueda. El proveedor de recursos debe estar **registrado**, como se muestra en la imagen.
 
>[!NOTE]
>Si no lo está, seleccione **Registrar**.
 
:::image type="content" source="media/baremetal-infrastructure-portal/register-resource-provider-azure-portal.png" alt-text="Captura de pantalla que muestra la unidad de instancia de BareMetal registrada":::
 
## <a name="baremetal-instance-units-in-the-azure-portal"></a>Unidades de instancia de BareMetal en Azure Portal
 
Al enviar una solicitud de implementación de instancias de BareMetal, especificará la suscripción de Azure que se va a conectar a estas instancias. Use la misma suscripción que para implementar la capa de aplicación que funciona con las unidades de instancia de BareMetal.
 
Durante la implementación de las instancias de BareMetal, se creará un [grupo de recursos de Azure](../../../azure-resource-manager/management/manage-resources-portal.md) en la suscripción de Azure que usó en la solicitud de implementación. Este nuevo grupo de recursos muestra todas las unidades de instancia de BareMetal que ha implementado en la suscripción específica.

1. En la suscripción de BareMetal, en Azure Portal, seleccione **Grupos de recursos**.
 
   :::image type="content" source="media/baremetal-infrastructure-portal/view-baremetal-instance-units-azure-portal.png" alt-text="Captura de pantalla que muestra una lista de grupos de recursos":::

1. En la lista, busque el nuevo grupo de recursos.
 
   :::image type="content" source="media/baremetal-infrastructure-portal/filter-resource-groups.png" alt-text="Captura de pantalla que muestra la unidad de instancia de BareMetal en una lista de grupos de recursos filtrada" lightbox="media/baremetal-infrastructure-portal/filter-resource-groups.png":::
   
   >[!TIP]
   >Puede filtrar por la suscripción que usó para implementar la instancia de BareMetal. Después de filtrar por suscripción adecuada, es posible que tenga una larga lista de grupos de recursos. Busque uno con un postfijo **-Txxx**, donde "xxx" son tres dígitos, como **-T250**.

1. Seleccione el nuevo grupo de recursos para mostrar sus detalles. La imagen muestra una unidad de instancia de BareMetal implementada.
   
   >[!NOTE]
   >Si ha implementado varios inquilinos de instancia de BareMetal con la misma suscripción de Azure, verá varios grupos de recursos de Azure.
 
## <a name="view-the-attributes-of-a-single-instance"></a>Visualización de los atributos de una sola instancia
 
Puede ver los detalles de una sola unidad. En la lista de la instancia de BareMetal, seleccione la instancia que quiera ver.
 
:::image type="content" source="media/baremetal-infrastructure-portal/view-attributes-single-baremetal-instance.png" alt-text="Captura de pantalla que muestra los atributos de la unidad de instancia de BareMetal de una sola instancia" lightbox="media/baremetal-infrastructure-portal/view-attributes-single-baremetal-instance.png":::
 
Los atributos de la imagen no son muy diferentes de los atributos de máquina virtual (VM) de Azure. A la izquierda, verá el grupo de recursos, la región de Azure y el nombre y el identificador de la suscripción. Si ha asignado etiquetas, las verá aquí también. De forma predeterminada, las unidades de instancia de BareMetal no tienen etiquetas asignadas.
 
A la derecha, verá el nombre de la unidad, el sistema operativo (SO), la dirección IP y la SKU que muestra el número de subprocesos de CPU y la memoria. También verá el estado de la energía y la versión de hardware (revisión del sello de la instancia de BareMetal). El estado de energía indica si la unidad de hardware está encendida o apagada. Sin embargo, los detalles del sistema operativo no indican si está funcionando.
 
Las revisiones de hardware posibles son:
 
* Revisión 3
 
* Revisión 4
 
* Revisión 4.2
 
>[!NOTE]
>La revisión 4.2 es la infraestructura de BareMetal más reciente renombrada que usa la arquitectura de la revisión 4. Presenta mejoras significativas en la latencia de red entre las máquinas virtuales de Azure y las unidades de instancia de BareMetal implementadas en los sellos o filas de la revisión 4. Para más información sobre las distintas revisiones, consulte [Infraestructura de BareMetal en Azure](baremetal-overview-architecture.md).
 
Además, en el lado derecho, encontrará el nombre del [grupo con ubicación por proximidad de Azure](../../../virtual-machines/linux/co-location.md), que se crea automáticamente para cada unidad de instancia implementada de BareMetal. Cuando implemente las máquinas virtuales de Azure que hospedan la capa de aplicación, haga referencia al grupo con ubicación por proximidad. Cuando use el grupo con ubicación por proximidad asociado a la unidad de instancia de BareMetal, asegúrese de que las máquinas virtuales de Azure se implementan cerca de la unidad de instancia de BareMetal.
 
>[!TIP]
>Para encontrar la capa de aplicación en el mismo centro de recursos de Azure que la revisión 4.x, consulte [Grupos de selección de ubicación de proximidad de Azure para una latencia de red óptima](../../../virtual-machines/workloads/sap/sap-proximity-placement-scenarios.md).
 
## <a name="check-activities-of-a-single-instance"></a>Comprobación de las actividades de una sola instancia
 
Puede comprobar las actividades de una sola unidad. Una de las actividades principales que se registra son los reinicios de la unidad. Los datos que se muestran incluyen el estado de la actividad, la marca de tiempo de la actividad desencadenada, el identificador de la suscripción y el usuario de Azure que desencadenó la actividad.
 
:::image type="content" source="media/baremetal-infrastructure-portal/check-activities-single-baremetal-instance.png" alt-text="Captura de pantalla que muestra las actividades de la unidad de instancia de BareMetal" lightbox="media/baremetal-infrastructure-portal/check-activities-single-baremetal-instance.png":::
 
Los cambios en los metadatos de la unidad en Azure también se anotan en el registro de actividad. Además del reinicio iniciado, puede ver la actividad de **escritura de BareMetallnstances**. Esta actividad no realiza ningún cambio en la propia unidad de instancia de BareMetal, sino que documenta los cambios en los metadatos de la unidad en Azure.
 
Otra actividad que se registra es cuando se agrega o elimina una [etiqueta](../../../azure-resource-manager/management/tag-resources.md) en una instancia.
 
## <a name="add-and-delete-an-azure-tag-to-an-instance"></a>Adición y eliminación de una etiqueta de Azure en una instancia
 
Puede agregar etiquetas de Azure a una unidad de instancia de BareMetal o eliminarlas. La forma en que se asignan las etiquetas es igual que en las máquinas virtuales. Al igual que sucede con las máquinas virtuales, las etiquetas existen en los metadatos de Azure y, en el caso de las instancias de BareMetal, tienen las mismas restricciones que las etiquetas de las máquinas virtuales.
 
La eliminación de las etiquetas funciona del mismo modo que con las máquinas virtuales. La aplicación y la eliminación de una etiqueta se indican en el registro de actividad de la unidad de instancia de BareMetal.
 
## <a name="check-properties-of-an-instance"></a>Comprobación de las propiedades de una instancia
 
Al adquirir las instancias, puede ir a la sección de propiedades para ver los datos recopilados sobre ellas. Los datos recopilados incluyen la conectividad de Azure, el back-end de almacenamiento, el identificador de circuito ExpressRoute, el identificador de recurso único y el identificador de suscripción. Esta información se usará en las solicitudes de soporte técnico o al configurar las instantáneas de almacenamiento.
 
Otra parte crítica de la información que verá es la dirección IP de NFS de almacenamiento. Esta dirección aísla el almacenamiento en el **inquilino** de la pila de instancias de BareMetal. Usará esta dirección IP al editar el [archivo de configuración de las copias de seguridad de instantáneas de almacenamiento](../../../virtual-machines/workloads/sap/hana-backup-restore.md#set-up-storage-snapshots).
 
:::image type="content" source="media/baremetal-infrastructure-portal/baremetal-instance-properties.png" alt-text="Captura de pantalla que muestra la configuración de propiedades de instancias de BareMetal" lightbox="media/baremetal-infrastructure-portal/baremetal-instance-properties.png":::
 
## <a name="restart-a-unit-through-the-azure-portal"></a>Reinicio de una unidad mediante Azure Portal
 
Hay varias situaciones en las que el sistema operativo no finalizará un reinicio, lo que requerirá apagar y volver a encender la unidad de instancia de BareMetal. Esto lo puede hacer directamente desde Azure Portal:
 
Seleccione **Reiniciar** y, después, **Sí** para confirmar el reinicio de la unidad.
 
:::image type="content" source="media/baremetal-infrastructure-portal/baremetal-instance-restart.png" alt-text="Captura de pantalla que muestra cómo reiniciar la unidad de instancia de BareMetal":::
 
Cuando reinicie una unidad de instancia de BareMetal, experimentará un retraso. Durante este retraso, el estado de energía se mueve de **Iniciando** a **Iniciado**, lo que significa que el sistema operativo se ha iniciado por completo. Como resultado, después de un reinicio, no puede iniciar sesión en la unidad tan pronto como el estado cambie a **Iniciado**.
 
>[!IMPORTANT]
>Según la cantidad de memoria en la unidad de instancia de BareMetal, reiniciar y volver a arrancar el hardware y el sistema operativo pueden tardar hasta una hora.
 
## <a name="open-a-support-request-for-baremetal-instances"></a>Apertura de una solicitud de soporte técnico para las instancias de BareMetal
 
Puede enviar solicitudes de soporte técnico específicamente para una unidad de instancia de BareMetal.
1. En Azure Portal, en **Ayuda y soporte técnico**, cree una **[solicitud de soporte técnico](https://rc.portal.azure.com/#create/Microsoft.Support)** y proporcione la siguiente información para el vale:
 
   - **Tipo de problema**: Selección de un tipo de problema
 
   - **Subscription** (Suscripción): Seleccione su suscripción.
 
   - **Servicio:** infraestructura de BareMetal.
 
   - **Recurso:** proporcione el nombre de la instancia.
 
   - **Resumen:** proporcione un resumen de la solicitud.
 
   - **Tipo de problema**: seleccione un tipo de problema.
 
   - **Subtipo de problema:** seleccione un subtipo del problema.

1. Seleccione la pestaña **Soluciones** para encontrar una solución a su problema. Si no encuentra una solución, vaya al paso siguiente.

1. Seleccione la pestaña **Detalles** y elija si el problema tiene que ver con las máquinas virtuales o con las unidades de instancia de BareMetal. Esta información le ayudará a dirigir la solicitud de soporte técnico a los especialistas adecuados.

1. Indique cuándo comenzó el problema y seleccione la región de la instancia.

1. Proporcione más detalles sobre la solicitud y cargue un archivo si es necesario.

1. Seleccione **Revisar + crear** para enviar la solicitud.
 
Un representante de soporte técnico tardará hasta cinco días laborables en confirmar su solicitud.

## <a name="next-steps"></a>Pasos siguientes

Si quiere más información sobre las cargas de trabajo, consulte [Tipos de carga de trabajo de BareMetal](../../../virtual-machines/workloads/sap/get-started.md).