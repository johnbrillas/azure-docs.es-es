---
title: Uso de la CLI de Azure para configurar la copia de seguridad continua y la restauración a un momento dado en Azure Cosmos DB
description: Aprenda a aprovisionar una cuenta con datos de copia de seguridad continua y restauración mediante la CLI de Azure.
author: kanshiG
ms.service: cosmos-db
ms.topic: how-to
ms.date: 02/01/2021
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 9ea71dae746ac423e7b17b6235b4d5cd3e143cd7
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100377336"
---
# <a name="configure-and-manage-continuous-backup-and-point-in-time-restore-preview---using-azure-cli"></a>Configuración y administración de copia de seguridad continua y restauración a un momento dado (versión preliminar) con la CLI de Azure
[!INCLUDE[appliesto-sql-mongodb-api](includes/appliesto-sql-mongodb-api.md)]

> [!IMPORTANT]
> La característica de restauración a un momento dado (modo de copia de seguridad continua) de Azure Cosmos DB está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La característica de restauración a un momento dado (versión preliminar) de Azure Cosmos DB ayuda a recuperarse de un cambio accidental en un contenedor, a restaurar una cuenta, una base de datos o un contenedor eliminados o a realizar una restauración en cualquier región (donde existan copias de seguridad). El modo de copia de seguridad continua permite realizar la restauración a cualquier momento dado de los últimos 30 días.

En este artículo se explica cómo aprovisionar una cuenta con datos de copia de seguridad continua y restauración mediante la CLI de Azure.

## <a name="install-azure-cli"></a><a id="install"></a>Instalación de la CLI de Azure

1. Instale la versión más reciente de la CLI de Azure.

   * Instale la versión más reciente de la [CLI de Azure](/cli/azure/install-azure-cli) o una versión superior a 2.17.1.
   * Si ya tiene instalada la CLI, ejecute el comando `az upgrade` para actualizarla a la versión más reciente. Este comando funcionará solo con una versión de la CLI superior a 2.11. Si tiene una versión anterior, use el vínculo anterior para instalar la versión más reciente.

1. Instale la extensión `cosmosdb-preview` de la CLI.

   * Los comandos de restauración a un momento dado están disponibles en la extensión `cosmosdb-preview`.
   * Ejecute el comando siguiente para instalar esta extensión: `az extension add --name cosmosdb-preview`
   * Ejecute el comando siguiente para desinstalar esta extensión: `az extension remove --name cosmosdb-preview`

1. Inicie sesión y seleccione su suscripción.

   * Use el comando `az login` para iniciar sesión en su cuenta de Azure.
   * Use el comando `az account set -s <subscriptionguid>` para seleccionar la suscripción requerida.

## <a name="provision-a-sql-api-account-with-continuous-backup"></a><a id="provision-sql-api"></a>Aprovisionamiento de una cuenta de API de SQL con copia de seguridad continua

Para aprovisionar una cuenta de API de SQL con copia de seguridad continua, se debe agregar un argumento `--backup-policy-type Continuous` adicional junto con el comando de aprovisionamiento normal. El comando siguiente es un ejemplo de una cuenta de escritura de una sola región denominada `pitracct2` con una directiva de copia de seguridad continua creada en la región *Oeste de EE. UU.* en el grupo de recursos *myrg*:

```azurecli-interactive

az cosmosdb create \
  --name pitracct2 \
  --resource-group myrg \
  --backup-policy-type Continuous \
  --default-consistency-level Session \
  --locations regionName="West US"

```

## <a name="provision-an-azure-cosmos-db-api-for-mongodb-account-with-continuous-backup"></a><a id="provision-mongo-api"></a>Aprovisionamiento de una cuenta de Azure Cosmos DB API para MongoDB con copia de seguridad continua

El comando siguiente muestra un ejemplo de una cuenta de escritura de una sola región denominada `pitracct3` con una directiva de copia de seguridad continua creada en la región *Oeste de EE. UU.* en el grupo de recursos *myrg*:

```azurecli-interactive

az cosmosdb create \
  --name pitracct3 \
  --kind MongoDB \
  --resource-group myrg \
  --server-version "3.6" \
  --backup-policy-type Continuous \
  --default-consistency-level Session \
  --locations regionName="West US"

```

## <a name="trigger-a-restore-operation-with-cli"></a><a id="trigger-restore"></a>Desencadenamiento de una operación de restauración con la CLI

La manera más sencilla de desencadenar una restauración es mediante la emisión del comando de restauración con el nombre de la cuenta de destino, la cuenta de origen, la ubicación, el grupo de recursos, la marca de tiempo (en hora UTC) y, opcionalmente, los nombres de la base de datos y del contenedor. A continuación, se muestran algunos ejemplos para desencadenar la operación de restauración:

1. Cree una cuenta de Azure Cosmos DB mediante la restauración de una cuenta existente.

   ```azurecli-interactive

   az cosmosdb restore \
    --target-database-account-name MyRestoredCosmosDBDatabaseAccount \
    --account-name MySourceAccount \
    --restore-timestamp 2020-07-13T16:03:41+0000 \
    --resource-group MyResourceGroup \
    --location "West US"

   ```

2. Cree una cuenta de Azure Cosmos DB mediante la restauración solo de las bases de datos y los contenedores seleccionados de una cuenta de base de datos existente.

   ```azurecli-interactive

   az cosmosdb restore \
    --resource-group MyResourceGroup \
    --target-database-account-name MyRestoredCosmosDBDatabaseAccount \
    --account-name MySourceAccount \
    --restore-timestamp 2020-07-13T16:03:41+0000 \
    --location "West US" \
    --databases-to-restore name=MyDB1 collections=collection1 collection2 \
    --databases-to-restore name=MyDB2 collections=collection3 collection4

   ```

## <a name="enumerate-restorable-resources-for-sql-api"></a><a id="enumerate-sql-api"></a>Enumeración de los recursos que se pueden restaurar de la API de SQL

Los comandos de enumeración que se describen a continuación ayudan a detectar los recursos que están disponibles para restaurar en varias marcas de tiempo. Además, también proporcionan una fuente de eventos clave en los recursos que se pueden restaurar de cuenta, base de datos y contenedor.

**Enumeración de todas las cuentas que se pueden restaurar en la suscripción actual**

Ejecute el comando de CLI siguiente para enumerar todas las cuentas que se pueden restaurar en la suscripción actual.

```azurecli-interactive
az cosmosdb restorable-database-account list --account-name "pitrbb"
```

La respuesta incluye todas las cuentas de base de datos (tanto activas como eliminadas) que se pueden restaurar y las regiones desde las que se puede hacer esta restauración:

```json
{
    "accountName": "pitrbb",
    "apiType": "Sql",
    "creationTime": "2021-01-08T23:34:11.095870+00:00",
    "deletionTime": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts/7133a59a-d1c0-4645-a699-6e296d6ac865",
    "identity": null,
    "location": "West US",
    "name": "7133a59a-d1c0-4645-a699-6e296d6ac865",
    "restorableLocations": [
      {
        "creationTime": "2021-01-08T23:34:11.095870+00:00",
        "deletionTime": null,
        "locationName": "West US",
        "regionalDatabaseAccountInstanceId": "f02df26b-c0ec-4829-8bef-3482d36e6230"
      }
    ],
    "tags": null,
    "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts"
  }
```

Igual que los elementos `CreationTime` o `DeletionTime` de la cuenta, existe también un elemento `CreationTime` o `DeletionTime` para la región. Estas horas permiten elegir la región correcta y un intervalo de tiempo válido para restaurar en esa región.

**Enumeración de todas las versiones de bases de datos de una cuenta de base de datos activa**

La enumeración de todas las versiones de bases de datos permite elegir la base de datos correcta en un escenario en el que se desconoce la hora real de existencia de la base de datos.

Ejecute el comando de CLI siguiente para enumerar todas las versiones de bases de datos. Este comando solo funciona con cuentas activas. Los parámetros `instanceId` y `location` se obtienen de las propiedades `name` y `location` en la respuesta del comando `az cosmosdb restorable-database-account list`. El atributo instanceId también es una propiedad de la cuenta de base de datos de origen que se está restaurando:

```azurecli-interactive
az cosmosdb sql restorable-database list \
  --instance-id "7133a59a-d1c0-4645-a699-6e296d6ac865" \
  --location "West US"
```

La salida de este comando ahora muestra cuándo se creó y eliminó una base de datos.

```json
[
  {
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts/7133a59a-d1c0-4645-a699-6e296d6ac865/restorableSqlDatabases/40e93dbd-2abe-4356-a31a-35567b777220",
    ..
    "name": "40e93dbd-2abe-4356-a31a-35567b777220",
    "resource": {
      "database": {
        "id": "db1"
      },
      "eventTimestamp": "2021-01-08T23:27:25Z",
      "operationType": "Create",
      "ownerId": "db1",
      "ownerResourceId": "YuZAAA=="
    },
    ..
  },
  {
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts/7133a59a-d1c0-4645-a699-6e296d6ac865/restorableSqlDatabases/243c38cb-5c41-4931-8cfb-5948881a40ea",
    ..
    "name": "243c38cb-5c41-4931-8cfb-5948881a40ea",
    "resource": {
      "database": {
        "id": "spdb1"
      },
      "eventTimestamp": "2021-01-08T23:25:25Z",
      "operationType": "Create",
      "ownerId": "spdb1",
      "ownerResourceId": "OIQ1AA=="
    },
   ..
  }
]
```

**Enumeración de todas las versiones de contenedores SQL de una base de datos de una cuenta de base de datos activa**

Use el comando siguiente para enumerar todas las versiones de contenedores SQL. Este comando solo funciona con cuentas activas. El parámetro `databaseRid` es el elemento `ResourceId` de la base de datos que se quiere restaurar. Es el valor del atributo `ownerResourceid` que se encuentra en la respuesta del comando `az cosmosdb sql restorable-database list`.

```azurecli-interactive
az cosmosdb sql restorable-container list \
    --instance-id "7133a59a-d1c0-4645-a699-6e296d6ac865" \
    --database-rid "OIQ1AA==" \
    --location "West US"
```

La respuesta de este comando ahora incluye la lista de las operaciones realizadas en todos los contenedores de esta base de datos:

```json
[
  {
    ...
    
      "eventTimestamp": "2021-01-08T23:25:29Z",
      "operationType": "Replace",
      "ownerId": "procol3",
      "ownerResourceId": "OIQ1APZ7U18="
...
  },
  {
    ...
      "eventTimestamp": "2021-01-08T23:25:26Z",
      "operationType": "Create",
      "ownerId": "procol3",
      "ownerResourceId": "OIQ1APZ7U18="
  },
]
```

**Búsqueda de bases de datos o contenedores que se pueden restaurar en cualquier marca de tiempo**

Use el comando siguiente para obtener la lista de bases de datos o contenedores que se pueden restaurar en una marca de tiempo determinada. Este comando solo funciona con cuentas activas.

```azurecli-interactive

az cosmosdb sql restorable-resource list \
  --instance-id "7133a59a-d1c0-4645-a699-6e296d6ac865" \
  --location "West US" \
  --restore-location "West US" \  
  --restore-timestamp "2021-01-10 T01:00:00+0000"

```

```json
[
  {
    "collectionNames": [
      "procol1",
      "procol2"
    ],
    "databaseName": "db1"
  },
  {
    "collectionNames": [
      "procol3",
       "spcol1"
    ],
    "databaseName": "spdb1"
  }
]
```

## <a name="enumerate-restorable-resources-for-mongodb-api-account"></a><a id="enumerate-mongodb-api"></a>Enumeración de los recursos que se pueden restaurar para una cuenta de la API de MongoDB

Los comandos de enumeración que se describen a continuación ayudan a detectar los recursos que están disponibles para restaurar en varias marcas de tiempo. Además, también proporcionan una fuente de eventos clave en los recursos que se pueden restaurar de cuenta, base de datos y contenedor. Al igual que con la API de SQL, puede usar el comando `az cosmosdb` pero con `mongodb` como parámetro en lugar de `sql`. Estos comandos solo funcionan con cuentas activas.

**Enumeración de todas las versiones de bases de datos MongoDB de una cuenta de base de datos activa**

```azurecli-interactive
az cosmosdb mongodb restorable-database list \
    --instance-id "7133a59a-d1c0-4645-a699-6e296d6ac865" \
    --location "West US"
```

**Enumeración de todas las versiones de colecciones MongoDB de una base de datos de una cuenta de base de datos activa**

```azurecli-interactive
az cosmosdb mongodb restorable-collection list \
    --instance-id "<InstanceID>" \
    --database-rid "AoQ13r==" \
    --location "West US"
```

**Enumeración de todos los recursos de una cuenta de base de datos MongoDB que están disponibles para restaurar en una determinada marca de tiempo y región**

```azurecli-interactive
az cosmosdb mongodb restorable-resource list \
    --instance-id "7133a59a-d1c0-4645-a699-6e296d6ac865" \
    --location "West US" \
    --restore-location "West US" \
    --restore-timestamp "2020-07-20T16:09:53+0000"
```

## <a name="next-steps"></a>Pasos siguientes

* Configure y administre la copia de seguridad continua mediante [Azure Portal](continuous-backup-restore-portal.md), [PowerShell](continuous-backup-restore-powershell.md) o [Azure Resource Manager](continuous-backup-restore-template.md).
* [Modelo de recursos del modo de copia de seguridad continua](continuous-backup-restore-resource-model.md)
* [Administre los permisos](continuous-backup-restore-permissions.md) necesarios para restaurar datos con el modo de copia de seguridad continua.
