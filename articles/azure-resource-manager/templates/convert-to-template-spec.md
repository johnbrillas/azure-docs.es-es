---
title: Conversión de plantilla de portal en especificaciones de plantilla
description: Se describe cómo convertir una plantilla existente en la galería de Azure Portal en especificaciones de plantilla.
ms.topic: conceptual
ms.date: 01/22/2021
ms.author: tomfitz
author: tfitzmac
ms.openlocfilehash: 8fe02f55348f2cdcabb43e05bb547819d4b51228
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98739064"
---
# <a name="convert-template-gallery-in-portal-to-template-specs"></a>Conversión de la galería de plantillas en el portal en especificaciones de plantilla

Azure Portal ofrece una manera de almacenar plantillas de Azure Resource Manager (plantillas de ARM) en su cuenta. **Esta característica está en desuso.** Para seguir usando las plantillas en esta galería, conviértalas en [especificaciones de plantilla](template-specs.md).

En este artículo se muestra cómo convertir las plantillas existentes en la galería de plantillas en especificaciones de plantilla.

En el portal, la característica que quedará en desuso se denomina **Plantillas (versión preliminar)** . Para ver si tiene plantillas para convertir, vea la [galería de plantillas en el portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Gallery%2Fmyareas%2Fgalleryitems). Estas plantillas tienen el tipo de recurso `Microsoft.Gallery/myareas/galleryitems`.

## <a name="deprecation-of-portal-feature"></a>Desuso de la característica de portal

La galería de plantillas en el portal quedará en desuso el 21 de enero de 2021. Puede seguir usándola hasta el 21 de febrero. A partir del 22 de febrero, no se podrán crear nuevas plantillas en la galería del portal, aunque aún podrá ver e implementar plantillas existentes.

El 22 de junio, la característica se quitará del portal y se bloquearán todas las operaciones de la API. No podrá ver ni implementar ninguna plantilla de la galería.

Antes del 22 de junio, deberá migrar las plantillas que quiera seguir usando. Puede usar uno de los métodos que se muestran en este artículo para migrar las plantillas. Una vez que se haya quitado la característica, tendrá que abrir un caso de soporte técnico para obtener las plantillas que no haya migrado.

## <a name="convert-with-powershell-script"></a>Conversión con script de PowerShell

Para simplificar la conversión de plantillas en la galería de plantillas, use un script de PowerShell del repositorio de plantillas de inicio rápido de Azure. Al ejecutar el script, podrá crear una nueva especificación de plantilla para cada plantilla o descargar una plantilla que cree la especificación de plantilla. El script no elimina la plantilla de la galería de plantillas.

1. Copie el [script de migración](https://github.com/Azure/azure-quickstart-templates/blob/master/201-templatespec-migrate-create/Migrate-GalleryItems.ps1). Guarde una copia local con el nombre *Migrate-GalleryItems.ps1*.
1. Para crear nuevas especificaciones de plantilla, proporcione valores para los parámetros `-ResourceGroupName` y `-Location`. 

   Establezca `ItemsToExport` en `MyGalleryItems` para exportar las plantillas. Establezca el valor en `AllGalleryItems` para exportar todas las plantillas a las que tiene acceso.

   En el ejemplo siguiente se crean nuevas especificaciones de plantilla para cada plantilla de un grupo de recursos denominado **migratedRG**. El script crea el grupo de recursos si no existe.

   ```azurepowershell
   .\Migrate-GalleryItems.ps1 -ResourceGroupName migratedRG -Location westus2 -ItemsToExport MyGalleryItems
   ```

1. Para descargar plantillas que puede usar para crear las especificaciones de plantilla, no proporcione valores de grupo de recursos o ubicación. En su lugar, especifique `-ExportToFile`. La plantilla no es la misma que la de la galería. En su lugar, contiene un recurso de especificación de plantilla que crea la especificación de plantilla correspondiente.

   En el siguiente ejemplo se descargan las plantillas sin crear especificaciones de plantilla.

   ```azurepowershell
   .\Migrate-GalleryItems.ps1 -ItemsToExport MyGalleryItems -ExportToFile
   ```

   Para obtener más información sobre cómo implementar la plantilla que crea la especificación de plantilla, consulte [Inicio rápido: Creación e implementación de una especificación de plantilla (versión preliminar)](quickstart-create-template-specs.md).

Para obtener más información sobre el script y sus parámetros, consulte [Creación de TemplateSpecs desde plantillas de la galería de plantillas](https://github.com/Azure/azure-quickstart-templates/tree/master/201-templatespec-migrate-create).

## <a name="manually-convert-through-portal"></a>Conversión manual a través del portal

Puede copiar manualmente las plantillas de la galería a nuevas especificaciones de plantilla.

1. Abra las [plantillas (versión preliminar)](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Gallery%2Fmyareas%2Fgalleryitems) en el portal.
1. Seleccione la plantilla que quiere migrar.
1. Seleccione **Ver plantilla**.
1. Copie el contenido de la plantilla.
1. En la barra de búsqueda del portal, busque **Especificaciones de plantilla**. Seleccione esa opción.
1. Seleccione **Create template spec** (Crear una especificación de plantilla).
1. Proporcione valores para nombre, suscripción, grupo de recursos, ubicación y versión.
1. Seleccione **Siguiente: Editar plantilla**.
1. En el caso del contenido de la plantilla, pegue a plantilla que copió de la galería de plantillas.
1. Seleccione **Revisar + crear**.
1. Una vez completada correctamente la validación, seleccione **Crear**.

Si necesita compartir la especificación de plantilla con otros usuarios de su organización, [establezca el control de acceso basado en rol](../../role-based-access-control/tutorial-role-assignments-group-powershell.md) al grupo o los usuarios que necesitan acceso.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las especificaciones de plantilla, consulte [Creación e implementación de especificaciones de plantilla](template-specs.md).
