---
title: Use la plantilla de ARM para configurar la copia de seguridad continua y la restauración a un momento dado en Azure Cosmos DB.
description: Aprenda a aprovisionar una cuenta con datos de copia de seguridad continua y restauración mediante plantillas de Azure Resource Manager.
author: kanshiG
ms.service: cosmos-db
ms.topic: how-to
ms.date: 02/01/2021
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 4abfdd0209bd9f13fb7bd902b27a53f65156da2e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100381824"
---
# <a name="configure-and-manage-continuous-backup-and-point-in-time-restore-preview---using-azure-resource-manager-templates"></a>Configuración y administración de la copia de seguridad continua y la restauración a un momento dado (versión preliminar) mediante plantillas de Azure Resource Manager
[!INCLUDE[appliesto-sql-mongodb-api](includes/appliesto-sql-mongodb-api.md)]

> [!IMPORTANT]
> La característica de restauración a un momento dado (modo de copia de seguridad continua) de Azure Cosmos DB está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La característica de restauración a un momento dado (versión preliminar) de Azure Cosmos DB ayuda a recuperarse de un cambio accidental en un contenedor, a restaurar una cuenta, una base de datos o un contenedor eliminados o a realizar una restauración en cualquier región (donde existan copias de seguridad). El modo de copia de seguridad continua permite realizar la restauración en cualquier punto temporal de los últimos 30 días.

En este artículo se describe el aprovisionamiento de una cuenta con datos de copia de seguridad continua y restauración mediante plantillas de Azure Resource Manager.

## <a name="provision-an-account-with-continuous-backup"></a><a id="provision"></a>Aprovisionamiento de una cuenta con copia de seguridad continua

Puede usar plantillas de Azure Resource Manager para implementar una cuenta de base de datos de Azure Cosmos DB con modo continuo. Al definir la plantilla para aprovisionar una cuenta, incluya el parámetro `backupPolicy` como se muestra en el ejemplo siguiente:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "ademo-pitr1",
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "apiVersion": "2016-03-31",
      "location": "West US",
      "properties": {
        "locations": [
          {
            "locationName": "West US"
          }
        ],
        "backupPolicy": {
          "type": "Continuous"
        },
        "databaseAccountOfferType": "Standard"
      }
    }
  ]
}
```

A continuación, implemente la plantilla mediante Azure PowerShell o la CLI. En el ejemplo siguiente se muestra cómo implementar la plantilla con un comando de la CLI:

```azurecli-interactive
az group deployment create -g <ResourceGroup> --template-file <ProvisionTemplateFilePath>
```

## <a name="restore-using-the-resource-manager-template"></a><a id="restore"> </a>Restauración mediante la plantilla de Resource Manager

También puede restaurar una cuenta mediante la plantilla de Resource Manager. Al definir la plantilla, incluya los parámetros siguientes:

* Establezca el parámetro `createMode` en *Restaurar*
* Defina `restoreParameters`, tenga en cuenta que el valor de `restoreSource` se extrae de la salida del comando `az cosmosdb restorable-database-account list` de la cuenta de origen. El atributo del identificador de instancia para el nombre de cuenta se usa para realizar la restauración.
* Establezca el parámetro `restoreMode` en *PointInTime* y configure el valor `restoreTimestampInUtc`.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "vinhpitrarmrestore-kal3",
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "apiVersion": "2016-03-31",
      "location": "West US",
      "properties": {
        "locations": [
          {
            "locationName": "West US"
          }
        ],
        "databaseAccountOfferType": "Standard",
        "createMode": "Restore",
        "restoreParameters": {
            "restoreSource": "/subscriptions/2296c272-5d55-40d9-bc05-4d56dc2d7588/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts/6a18ecb8-88c2-4005-8dce-07b44b9741df",
            "restoreMode": "PointInTime",
            "restoreTimestampInUtc": "6/24/2020 4:01:48 AM"
        }
      }
    }
  ]
}
```

A continuación, implemente la plantilla mediante Azure PowerShell o la CLI. En el ejemplo siguiente se muestra cómo implementar la plantilla con un comando de la CLI:  

```azurecli-interactive
az group deployment create -g <ResourceGroup> --template-file <RestoreTemplateFilePath> 
```

## <a name="next-steps"></a>Pasos siguientes

* Configure y administre la copia de seguridad continua mediante la [CLI de Azure](continuous-backup-restore-command-line.md), [PowerShell](continuous-backup-restore-command-line.md) o [Azure Portal](continuous-backup-restore-portal.md).
* [Modelo de recursos del modo de copia de seguridad continua](continuous-backup-restore-resource-model.md)
* [Administre los permisos](continuous-backup-restore-permissions.md) necesarios para restaurar datos con el modo de copia de seguridad continua.