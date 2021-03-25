---
title: Habilitación de Snapshot Debugger para aplicaciones .NET y .NET Core en Azure Functions | Microsoft Docs
description: Habilitación de Snapshot Debugger para aplicaciones .NET y .NET Core en Azure Functions
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 12/18/2020
ms.openlocfilehash: ac25962cac36a149807b67a44b3b88a4f40c954a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102211947"
---
# <a name="enable-snapshot-debugger-for-net-and-net-core-apps-in-azure-functions"></a>Habilitación de Snapshot Debugger para aplicaciones .NET y .NET Core en Azure Functions

Actualmente, Snapshot Debugger puede usarse con aplicaciones ASP.NET y ASP.NET Core que se ejecutan en Azure Functions en planes de servicio de Windows.

Se recomienda ejecutar la aplicación en el nivel de servicio básico o superior al usar Snapshot Debugger.

Para la mayoría de las aplicaciones, los niveles de servicio Gratis y Compartido no tienen suficiente memoria ni espacio en disco para guardar las instantáneas.

## <a name="prerequisites"></a>Requisitos previos

* [Habilitar la supervisión de Application Insights en la aplicación de funciones](../../azure-functions/configure-monitoring.md#add-to-an-existing-function-app)

## <a name="enable-snapshot-debugger"></a> Habilitación de Snapshot Debugger

Si está ejecutando otro tipo de servicio de Azure, aquí encontrará instrucciones para habilitar Snapshot Debugger en otras plataformas compatibles:
* [Azure App Service](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Servicios de Azure Service Fabric](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines y Virtual Machine Scale Sets](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuales o físicas locales](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Para habilitar Snapshot Debugger en la aplicación de funciones, debe actualizar el archivo `host.json` agregando la propiedad `snapshotConfiguration` tal y como se define a continuación y volver a implementar la función.

```json
{
  "version": "2.0",
  "logging": {
    "applicationInsights": {
      "snapshotConfiguration": {
        "isEnabled": true
      }
    }
  }
}
```

Snapshot Debugger está preinstalado como parte del runtime de Azure Functions, que, de forma predeterminada, está deshabilitado.

Como Snapshot Debugger está incluido en el runtime de Azure Functions, no es necesario agregar paquetes NuGet adicionales ni configuraciones de la aplicación.

Como referencia, para una aplicación de función simple (.NET Core), a continuación se muestra el aspecto que tendrá `.csproj`, `{Your}Function.cs` y `host.json` después de que se haya habilitado Snapshot Debugger en él.

Proyecto csproj

```xml
<Project Sdk="Microsoft.NET.Sdk">
<PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
    <AzureFunctionsVersion>v2</AzureFunctionsVersion>
</PropertyGroup>
<ItemGroup>
    <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.31" />
</ItemGroup>
<ItemGroup>
    <None Update="host.json">
    <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </None>
    <None Update="local.settings.json">
    <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    <CopyToPublishDirectory>Never</CopyToPublishDirectory>
    </None>
</ItemGroup>
</Project>
```

Clase Function

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;

namespace SnapshotCollectorAzureFunction
{
    public static class ExceptionFunction
    {
        [FunctionName("ExceptionFunction")]
        public static Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = null)] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            throw new NotImplementedException("Dummy");
        }
    }
}
```

Archivo de host

```json
{
  "version": "2.0",
  "logging": {
    "applicationInsights": {
      "samplingExcludedTypes": "Request",
      "samplingSettings": {
        "isEnabled": true
      },
      "snapshotConfiguration": {
        "isEnabled": true
      }
    }
  }
}
```

## <a name="enable-snapshot-debugger-for-other-clouds"></a>Habilitación de Snapshot Debugger para otras nubes

Actualmente, las únicas regiones que requieren modificaciones de punto de conexión son [Azure Government](https://docs.microsoft.com/azure/azure-government/compare-azure-government-global-azure#application-insights) y [Azure China](https://docs.microsoft.com/azure/china/resources-developer-guide).

A continuación se muestra un ejemplo de `host.json` actualizado con el punto de conexión del agente en la nube del Gobierno de EE. UU.:
```json
{
  "version": "2.0",
  "logging": {
    "applicationInsights": {
      "samplingExcludedTypes": "Request",
      "samplingSettings": {
        "isEnabled": true
      },
      "snapshotConfiguration": {
        "isEnabled": true,
        "agentEndpoint": "https://snapshot.monitor.azure.us"
      }
    }
  }
}
```

A continuación se muestran las invalidaciones admitidas del punto de conexión del agente Snapshot Debugger:

|Propiedad    | Nube del Gobierno de EE. UU. | Nube de China |   
|---------------|---------------------|-------------|
|AgentEndpoint         | `https://snapshot.monitor.azure.us`    | `https://snapshot.monitor.azure.cn` |

## <a name="disable-snapshot-debugger"></a>Deshabilitación de Snapshot Debugger

Para deshabilitar Snapshot Debugger en la aplicación de funciones, solo tiene que actualizar el archivo `host.json` estableciendo en `false` la propiedad `snapshotConfiguration.isEnabled`.

```json
{
  "version": "2.0",
  "logging": {
    "applicationInsights": {
      "snapshotConfiguration": {
        "isEnabled": false
      }
    }
  }
}
```

Recomendamos que tenga habilitado Snapshot Debugger en todas las aplicaciones para facilitar el diagnóstico de las excepciones de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- Genere tráfico para la aplicación que pueda desencadenar una excepción. A continuación, espere de 10 a 15 minutos para que se empiecen a enviar instantáneas a la instancia de Application Insights.
- [Vea las instantáneas](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) en Azure Portal.
- Personalice la configuración de Snapshot Debugger según su caso de uso en la aplicación de funciones. Para obtener más información, vea la [configuración de instantáneas en host.json](../../azure-functions/functions-host-json.md#applicationinsightssnapshotconfiguration).
- Por ayuda para solucionar problemas de Snapshot Debugger, consulte la sección [Solución de problemas de Snapshot Debugger](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).