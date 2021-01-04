---
title: Habilitación de Snapshot Debugger para aplicaciones de .NET en Azure App Service | Microsoft Docs
description: Habilitación de Snapshot Debugger para aplicaciones de .NET en Azure App Service
ms.topic: conceptual
author: brahmnes
ms.author: bfung
ms.date: 03/26/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 35653840c5ddd6f5ae2d5dc078513f0fa35ab34a
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97560940"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Habilitación de Snapshot Debugger para aplicaciones de .NET en Azure App Service

Actualmente, Snapshot Debugger puede usarse con aplicaciones ASP.NET y ASP.NET Core que se ejecutan en Azure App Service en planes de servicio de Windows. Se recomienda ejecutar la aplicación en el nivel de servicio básico o superior al usar el depurador de instantáneas. Para la mayoría de las aplicaciones, los niveles de servicio gratis y compartidos no tienen suficiente memoria para guardar las instantáneas.

## <a name="enable-snapshot-debugger"></a><a id="installation"></a> Habilitación de Snapshot Debugger
Para habilitar Snapshot Debugger en una aplicación, siga las instrucciones que se indican a continuación.

Si está ejecutando otro tipo de servicio de Azure, aquí encontrará instrucciones para habilitar Snapshot Debugger en otras plataformas compatibles:
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Servicios de Azure Service Fabric](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines y Virtual Machine Scale Sets](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuales o físicas locales](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

> [!NOTE]
> Si usa una versión preliminar de .NET Core o su aplicación hace referencia a un SDK de Application Insights, ya sea directamente o indirectamente a través de un ensamblado dependiente, siga primero las instrucciones que se indican en [Habilitar Snapshot Debugger para otros entornos](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) para incluir el paquete NuGet [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) con la aplicación y, luego, complete el resto de las instrucciones siguientes. 

Snapshot Debugger se instaló previamente como parte del entorno de ejecución de App Services, pero es necesario activarlo si quiere obtener instantáneas para la aplicación de App Service.

Una vez que haya implementado una aplicación, siga los pasos que se indican a continuación para habilitar Snapshot Debugger:

1. Vaya al panel de control de Azure y busque la instancia de App Service.
2. Vaya a la página **Configuración > Application Insights**.

   ![Habilitación de App Insights en el portal de App Services](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Siga las instrucciones que aparecen en la página para crear un nuevo recurso o seleccione un recurso de App Insights existente para supervisar la aplicación. También asegúrese de que los dos modificadores de Snapshot Debugger están **activados**.

   ![Adición de la extensión de sitio de App Insights][Enablement UI]

4. Snapshot Debugger está habilitado con una configuración de aplicación de App Services.

    ![Configuración de aplicación para Snapshot Debugger][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>Deshabilitación de Snapshot Debugger

Siga los mismos pasos que para **Habilitación de Snapshot Debugger**, pero cambie los dos modificadores de Snapshot Debugger al modo **desactivado**.

Recomendamos que tenga habilitado Snapshot Debugger en todas las aplicaciones para facilitar el diagnóstico de las excepciones de la aplicación.

## <a name="azure-resource-manager-template"></a>Plantilla del Administrador de recursos de Azure

En el caso de una instancia de Azure App Service, puede establecer la configuración de la aplicación en la plantilla de Azure Resource Manager para habilitar Snapshot Debugger y Profiler. Para ello, consulte el fragmento de código de plantilla siguiente:

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('webSiteName')]",
  "type": "Microsoft.Web/sites",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  "tags": { 
    "[concat('hidden-related:', resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName')))]": "empty",
    "displayName": "Website"
  },
  "properties": {
    "name": "[parameters('webSiteName')]",
    "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "name": "appsettings",
      "type": "config",
      "dependsOn": [
        "[parameters('webSiteName')]",
        "[concat('AppInsights', parameters('webSiteName'))]"
      ],
      "properties": {
        "APPINSIGHTS_INSTRUMENTATIONKEY": "[reference(resourceId('Microsoft.Insights/components', concat('AppInsights', parameters('webSiteName'))), '2014-04-01').InstrumentationKey]",
        "APPINSIGHTS_PROFILERFEATURE_VERSION": "1.0.0",
        "APPINSIGHTS_SNAPSHOTFEATURE_VERSION": "1.0.0",
        "DiagnosticServices_EXTENSION_VERSION": "~3",
        "ApplicationInsightsAgent_EXTENSION_VERSION": "~2"
      }
    }
  ]
},
```

## <a name="next-steps"></a>Pasos siguientes

- Genere tráfico para la aplicación que pueda desencadenar una excepción. A continuación, espere de 10 a 15 minutos para que se empiecen a enviar instantáneas a la instancia de Application Insights.
- Vea las [instantáneas](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) en Azure Portal.
- Por ayuda para solucionar problemas de Snapshot Debugger, consulte la sección [Solución de problemas de Snapshot Debugger](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

