---
title: Uso de filtros de características para habilitar las marcas de características condicionales
titleSuffix: Azure App Configuration
description: Obtenga información sobre cómo usar los filtros de características para habilitar las marcas de características condicionales.
ms.service: azure-app-configuration
ms.custom: devx-track-csharp
author: AlexandraKemperMS
ms.author: alkemper
ms.topic: conceptual
ms.date: 3/9/2020
ms.openlocfilehash: 39455c4bc193cce036bd169c702b5c020d53d2f6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98602247"
---
# <a name="use-feature-filters-to-enable-conditional-feature-flags"></a>Uso de filtros de características para habilitar las marcas de características condicionales

Las marcas de característica permiten activar o desactivar la funcionalidad de la aplicación. Una marca de característica simple está activada o desactivada. La aplicación siempre se comporta de la misma manera. Por ejemplo, podría implementar una nueva característica detrás de una marca de característica. Cuando la marca de característica está habilitada, todos los usuarios ven la nueva característica. Al deshabilitar la marca de característica, se oculta la nueva característica.

Por el contrario, una _marca de característica condicional_ permite que la marca de característica se habilite o deshabilite de forma dinámica. La aplicación puede comportarse de forma diferente, dependiendo de los criterios de la marca de característica. Supongamos que desea mostrar la nueva característica a un pequeño subconjunto de usuarios en primer lugar. Una marca de característica condicional permite habilitar la marca de característica para algunos usuarios al deshabilitarla para otras personas. _Los filtros de características_ determinan el estado de la marca de característica cada vez que se evalúa.

La biblioteca `Microsoft.FeatureManagement` incluye tres filtros de características:

- `PercentageFilter` habilita la marca de característica en función de un porcentaje.
- `TimeWindowFilter` habilita la marca de característica durante un período de tiempo especificado.
- `TargetingFilter` habilita la marca de características para usuarios y grupos especificados.

También puede crear su propio filtro de características que implementa la [interfaz Microsoft.FeatureManagement.IFeatureFilter](/dotnet/api/microsoft.featuremanagement.ifeaturefilter).

## <a name="registering-a-feature-filter"></a>Registro de un filtro de características

Para registrar un filtro de características, llame al método `AddFeatureFilter` y especifique el nombre del tipo del filtro que desee. Por ejemplo, el código siguiente registra `PercentageFilter`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddFeatureManagement().AddFeatureFilter<PercentageFilter>();
}
```

## <a name="configuring-a-feature-filter-in-azure-app-configuration"></a>Configuración de un filtro de características en Azure App Configuration

Algunos filtros de características tienen opciones de configuración adicionales. Por ejemplo, `PercentageFilter` activa una característica basada en un porcentaje. Tiene una configuración que define el porcentaje que se va a usar.

Puede configurar estas opciones para las marcas de característica definidas en Azure App Configuration. Por ejemplo, siga estos pasos para usar `PercentageFilter` y habilitar la marca de característica para el 50 % de las solicitudes a una aplicación web:

1. Siga las instrucciones del artículo [Guía de inicio rápido: Agregue marcas de característica a una aplicación ASP.NET Core](./quickstart-feature-flag-aspnet-core.md) para crear una aplicación web con una marca de característica.

1. En Azure Portal, vaya al almacén de configuración y haga clic en **Administrador de características**.

1. Haga clic en el menú contextual de la marca de característica *Beta* que creó en la guía de inicio rápido. Haga clic en **Editar**.

    > [!div class="mx-imgBorder"]
    > ![Editar marca de característica Beta](./media/edit-beta-feature-flag.png)

1. En la pantalla **Edit** (Editar), active la casilla **Enable feature flag** (Habilitar la marca de característica) si aún no está seleccionada. A continuación, active la casilla **Use feature filter** (Usar filtro de características) y seleccione **Custom** (Personalizado). 

1. En el campo **Name** (Nombre), seleccione *Microsoft.Percentage*.

    > [!div class="mx-imgBorder"]
    > ![Agregar filtro de características](./media/feature-flag-add-filter.png)

1. Haga clic en el menú contextual situado junto al nombre del filtro de características. Haga clic en **Edit filter parameters** (Editar parámetros de filtro).

    > [!div class="mx-imgBorder"]
    > ![Edición de parámetros de filtro de características](./media/feature-flags-edit-filter-parameters.png)

1. Escriba un **Nombre** de *Valor* y **Valor** de 50. El campo **Valor** indica el porcentaje de solicitudes para las que habilitar el filtro de características.

    > [!div class="mx-imgBorder"]
    > ![Establecimiento de parámetros de filtro de características](./media/feature-flag-set-filter-parameters.png)

1. Haga clic en **Aplicar** para volver a la pantalla **Editar marca de característica**. A continuación, haga clic en **Aplicar** de nuevo para guardar la configuración de la marca de característica.

1. En la página **Feature manager** (Administrador de características), la marca de características tiene ahora el valor un valor *Custom* (Personalizado) en **Feature filter** (Filtro de características). 

    > [!div class="mx-imgBorder"]
    > ![Marca de característica en la lista con un valor de filtro de características "Custom" (Personalizado)](./media/feature-flag-filter-custom.png)

## <a name="feature-filters-in-action"></a>Filtros de características en acción

Para ver los efectos de esta marca de característica, inicie la aplicación y presione el botón **Actualizar** en el explorador varias veces. Verá que el elemento *Beta* aparece en la barra de herramientas alrededor del 50 % del tiempo. Está oculto el resto del tiempo, porque el `PercentageFilter` desactiva la característica *Beta* para un subconjunto de solicitudes. En el vídeo siguiente se muestra este comportamiento en acción.

> [!div class="mx-imgBorder"]
> ![TargetingFilter en acción](./media/feature-flags-percentagefilter.gif)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Habilitar el lanzamiento preconfigurado de características para audiencias de destino](./howto-targetingfilter-aspnet-core.md)
