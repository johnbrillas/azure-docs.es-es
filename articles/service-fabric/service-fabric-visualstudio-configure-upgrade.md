---
title: Configuración de la actualización de una aplicación de Service Fabric
description: Obtenga información sobre cómo configurar los parámetros para la actualización de la aplicación de Service Fabric mediante Microsoft Visual Studio.
ms.topic: conceptual
ms.date: 06/29/2017
ms.openlocfilehash: 11df0387db07669ebacdba2e3d850e32903da133
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96573687"
---
# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Configuración de la actualización de una aplicación de Service Fabric en Visual Studio
Las herramientas de Visual Studio para Azure Service Fabric proporcionan compatibilidad con la actualización para la publicación el clústeres locales o remotos. Hay tres escenarios en los que le convendría actualizar la aplicación a una versión más reciente en lugar de reemplazarla durante las pruebas y la depuración:

* Los datos de la aplicación no se perderán durante la actualización.
* La disponibilidad permanece alta, por lo que no habrá ninguna interrupción del servicio durante la actualización, si no hay suficientes instancias de servicio que se extienden entre los dominios de actualización.
* Pueden ejecutarse las pruebas en una aplicación mientras se realiza la actualización.

## <a name="parameters-needed-to-upgrade"></a>Parámetros necesarios para la actualización
Puede elegir entre dos tipos de implementación: normal o actualización. Una implementación normal borra cualquier información de implementación anterior y los datos en el clúster, mientras que una implementación de actualización los conserva. Cuando actualice una aplicación de Service Fabric en Visual Studio, deberá proporcionar directivas de comprobación de estado y parámetros de actualización de la aplicación. Los parámetros de actualización de la aplicación ayudan a controlar la actualización, mientras que las directivas de comprobación de estado determinan si la actualización se realizó correctamente o no. Consulte [Actualización de la aplicación de Service Fabric: parámetros de actualización](service-fabric-application-upgrade-parameters.md) para obtener más información.

Existen tres modos de actualización: *Monitored*, *UnmonitoredAuto* y *UnmonitoredManual*.

* Una actualización Monitored automatiza la actualización y la comprobación de estado de la aplicación.
* Una actualización UnmonitoredAuto automatiza la actualización, pero omite la comprobación de estado de la aplicación.
* Cuando realice una actualización UnmonitoredManual, deberá actualizar manualmente cada dominio de actualización.

Cada modo de actualización requiere diferentes conjuntos de parámetros. Consulte [Parámetros de actualización de la aplicación](service-fabric-application-upgrade-parameters.md) para obtener más información sobre las opciones de actualización disponibles.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Actualización de la aplicación de Service Fabric en Visual Studio
Si usa las herramientas de Visual Studio Service Fabric para actualizar una aplicación de Service Fabric, puede especificar un proceso de publicación para que sea una actualización en lugar de una implementación normal si activa la casilla **Actualizar la aplicación** .

### <a name="to-configure-the-upgrade-parameters"></a>Para configurar los parámetros de actualización
1. Haga clic en el botón **Configuración** situado junto a la casilla. Aparecerá el cuadro de diálogo **Edit Upgrade Parameters** . El cuadro de diálogo **Edit Upgrade Parameters** admite los modos de actualización Monitored, UnmonitoredAuto y UnmonitoredManual.
2. Seleccione el modo de actualización que quiera usar y luego complete la cuadrícula de parámetros.

    Cada parámetro tiene valores predeterminados. El parámetro *DefaultServiceTypeHealthPolicy* opcional toma una entrada de tabla hash. Este es un ejemplo del formato de entrada de la tabla hash para *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* es otro parámetro opcional que toma una entrada de tabla hash en el formato siguiente:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Este es un ejemplo real:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. Si selecciona el modo de actualización UnmonitoredManual, tendrá que iniciar manualmente una consola de PowerShell para continuar y finalizar el proceso de actualización. Consulte [Actualización de la aplicación de Service Fabric: temas avanzados](service-fabric-application-upgrade-advanced.md) para ver cómo funciona la actualización manual.

## <a name="upgrade-an-application-by-using-powershell"></a>Actualización de una aplicación mediante PowerShell
Puede usar cmdlets de PowerShell para actualizar una aplicación de Service Fabric. Consulte [Tutorial sobre la actualización de aplicaciones de Service Fabric](service-fabric-application-upgrade-tutorial.md) y [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade) para obtener información detallada.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Especificación de una directiva de comprobación de estado en el archivo de manifiesto de aplicación
Cada servicio en una aplicación de Service Fabric puede tener sus propios parámetros de directiva de estado que reemplazan a los valores predeterminados. Puede proporcionar estos valores de parámetro en el archivo de manifiesto de aplicación.

El ejemplo siguiente muestra cómo aplicar una directiva de comprobación de estado única para cada servicio en el manifiesto de aplicación.

```xml
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Pasos siguientes
Para obtener más información acerca de cómo actualizar una aplicación, consulte el [Tutorial sobre la actualización de aplicaciones de Service Fabric con Visual Studio](service-fabric-application-upgrade-tutorial.md).
