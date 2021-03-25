---
title: Preguntas más frecuentes sobre la característica de restauración a un momento dado de Azure Cosmos DB
description: En este artículo se enumeran las preguntas más frecuentes sobre la característica de restauración a un momento dado de Azure Cosmos DB que se consigue mediante el modo de copia de seguridad continua.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/01/2021
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 1cf94964f420f7a7d4fc0f6ba0b77813b3e75787
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100393231"
---
# <a name="frequently-asked-questions-on-the-azure-cosmos-db-point-in-time-restore-feature-preview"></a>Preguntas más frecuentes sobre la característica de restauración a un momento dado de Azure Cosmos DB (versión preliminar)
[!INCLUDE[appliesto-sql-mongodb-api](includes/appliesto-sql-mongodb-api.md)]

> [!IMPORTANT]
> La característica de restauración a un momento dado (modo de copia de seguridad continua) de Azure Cosmos DB está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se enumeran las preguntas más frecuentes sobre la funcionalidad de restauración a un momento dado de Azure Cosmos DB (versión preliminar) que se consigue mediante el modo de copia de seguridad continua.

## <a name="how-much-time-does-it-takes-to-restore"></a>¿Cuánto se tarda en realizar una restauración?
La duración de la restauración depende del tamaño de los datos.

### <a name="can-i-submit-the-restore-time-in-local-time"></a>¿Puedo enviar la hora de restauración en la hora local?
Es posible que la restauración no se lleve a cabo en función de si los recursos clave, como las bases de datos o los contenedores, existían en ese momento. Para comprobarlo, especifique la hora y examine la base de datos o el contenedor seleccionados durante un tiempo determinado. Si no ve ningún recurso que se pueda restaurar, el proceso de restauración no funcionará.

### <a name="how-can-i-track-if-an-account-is-being-restored"></a>¿Cómo puedo hacer un seguimiento de si se está restaurando una cuenta?
Después de enviar el comando de restauración y esperar en la misma página, una vez que se completa la operación, la barra de estado muestra el mensaje que indica que la cuenta se restauró correctamente. También puede buscar la cuenta restaurada y [hacer un seguimiento del estado de la cuenta que se está restaurando](continuous-backup-restore-portal.md#track-restore-status). Mientras la restauración esté en curso, el estado de la cuenta será *En creación* y, cuando la operación de restauración se complete, cambiará a *En línea*.

De manera similar a lo que ocurre con PowerShell y la CLI, puede hacer un seguimiento del progreso de la operación de restauración mediante la ejecución del comando `az cosmosdb show` como se indica a continuación:

```azurecli-interactive
az cosmosdb show --name "accountName" --resource-group "resourceGroup"
```

El valor provisioningState muestra *Correcto* cuando la cuenta está en línea.

```json
{
"virtualNetworkRules": [],
"writeLocations" : [
{
    "documentEndpoint": "https://<accountname>.documents.azure.com:443/", 
    "failoverpriority": 0,
    "id": "accountName" ,
    "isZoneRedundant" : false, 
    "locationName": "West US 2", 
    "provisioningState": "Succeeded"
}
]
}
```

### <a name="how-can-i-find-out-whether-an-account-was-restored-from-another-account"></a>¿Cómo puedo averiguar si una cuenta se restauró desde otra cuenta?
Ejecute el comando `az cosmosdb show`; en la salida, verá el valor de la propiedad `createMode`. Si el valor está establecido en **Restore**, indica que la cuenta se restauró desde otra cuenta. La propiedad `restoreParameters` tiene más detalles, como `restoreSource`, que tiene el identificador de la cuenta de origen. El último GUID del parámetro `restoreSource` es el instanceId de la cuenta de origen.

Por ejemplo, en la salida siguiente, el identificador de instancia de la cuenta de origen es *7b4bb-f6a0-430e-ade1-638d781830cc*.

```json
"restoreParameters": {
   "databasesToRestore" : [],
   "restoreMode": "PointInTime",
   "restoreSource": "/subscriptions/2a5b-f6a0-430e-ade1-638d781830dd/providers/Microsoft.DocumentDB/locations/westus/restorableDatabaseAccounts/7b4bb-f6a0-430e-ade1-638d781830cc",
   "restoreTimestampInUtc": "2020-06-11T22:05:09Z"
}
```

### <a name="what-happens-when-i-restore-a-shared-throughput-database-or-a-container-within-a-shared-throughput-database"></a>¿Qué pasa cuando se restaura una base de datos de rendimiento compartido o un contenedor dentro de una base de datos de rendimiento compartido?
Se restaura toda la base de datos de rendimiento compartido. No se puede elegir un subconjunto de contenedores en una base de datos de rendimiento compartido para la restauración.

### <a name="what-is-the-use-of-instanceid-in-the-account-definition"></a>¿Cómo se usa InstanceID en la definición de la cuenta?
En cualquier momento determinado, la propiedad `accountName` de la cuenta de Azure Cosmos DB es única globalmente mientras está activa. Sin embargo, una vez que se elimina la cuenta, es posible crear otra cuenta con el mismo nombre y, por lo tanto, "accountName" ya no es suficiente para identificar una instancia de una cuenta. 

El identificador o `instanceId` es una propiedad de una instancia de una cuenta y se usa para eliminar la ambigüedad entre varias cuentas (activas y eliminadas) si tienen el mismo nombre para la restauración. Puede obtener el identificador de instancia mediante la ejecución de los comandos `Get-AzCosmosDBRestorableDatabaseAccount` o `az cosmosdb restorable-database-account`. El valor del atributo de nombre denota el "InstanceID".

## <a name="next-steps"></a>Pasos siguientes

* ¿Qué es el modo de [copia de seguridad continua](continuous-backup-restore-introduction.md)?
* Configure y administre la copia de seguridad continua mediante [Azure Portal](continuous-backup-restore-portal.md), [PowerShell](continuous-backup-restore-powershell.md), la [CLI](continuous-backup-restore-command-line.md) o [Azure Resource Manager](continuous-backup-restore-template.md).
* [Administre los permisos](continuous-backup-restore-permissions.md) necesarios para restaurar datos con el modo de copia de seguridad continua.
* [Modelo de recursos del modo de copia de seguridad continua](continuous-backup-restore-resource-model.md)