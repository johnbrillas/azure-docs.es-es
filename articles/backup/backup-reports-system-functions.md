---
title: Funciones del sistema en registros de Azure Monitor
description: Escribir consultas personalizadas en registros de Azure Monitor mediante funciones del sistema
ms.topic: conceptual
ms.date: 03/01/2021
ms.openlocfilehash: 1d26adfd2bd1a3fc1506a334b4b661b66172192d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102510430"
---
# <a name="system-functions-on-azure-monitor-logs"></a>Funciones del sistema en registros de Azure Monitor

Azure Backup proporciona un conjunto de funciones, llamadas funciones del sistema o funciones de la solución, que están disponibles de manera predeterminada en las áreas de trabajo de Log Analytics (LA).
 
Estas funciones operan sobre los datos de las [tablas de Azure Backup sin formato](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model) en LA y devuelven datos con formato que le ayudan a recuperar fácilmente la información de todas las entidades relacionadas con la copia de seguridad mediante consultas simples. Los usuarios pueden pasar parámetros a estas funciones para filtrar los datos devueltos por estas funciones. 

Se recomienda usar funciones del sistema para consultar los datos de copia de seguridad de las áreas de trabajo de LA para la creación de informes personalizados, ya que proporcionan una serie de ventajas, como se detalla en la sección siguiente.

## <a name="benefits-of-using-system-functions"></a>Ventajas del uso de las funciones del sistema

* **Consultas más simples**: el uso de funciones le ayuda a reducir el número de combinaciones necesarias en las consultas. De manera predeterminada, las funciones devuelven esquemas "planos" que incorporan toda la información relativa a la entidad (instancia de copia de seguridad, trabajo, almacén, etc.) que se consulta. Por ejemplo, si necesita obtener una lista de trabajos de copia de seguridad correctos por nombre de elemento de copia de seguridad y su contenedor asociado, una llamada simple a la función **_AzureBackup_getJobs()** le proporcionará toda esta información para cada trabajo. Por otro lado, la consulta directa de las tablas sin formato requerirá la realización de varias combinaciones entre las tablas [AddonAzureBackupJobs](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model#addonazurebackupjobs) y [CoreAzureBackup](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model#coreazurebackup).

* **Transición más fluida desde el evento de diagnóstico heredado**: el uso de funciones del sistema le ayuda a realizar la transición sin problemas desde el [evento de diagnóstico heredado](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#legacy-event) (AzureBackupReport en modo AzureDiagnostics) a los [eventos específicos del recurso](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#diagnostics-events-available-for-azure-backup-users). Todas las funciones del sistema proporcionadas por Azure Backup permiten especificar un parámetro que le permite elegir si la función debe consultar los datos solo de las tablas específicas del recurso, o bien consultar los datos de la tabla heredada y de las tablas específicas del recurso (con desduplicación de registros).
    * Si ha migrado correctamente a las tablas específicas del recurso, puede optar por excluir la tabla heredada de la consulta de la función.
    * Si actualmente se encuentra en el proceso de migración y tiene algunos datos en las tablas heredadas que necesita para el análisis, puede incluir la tabla heredada. Una vez completada la transición y que ya no necesite los datos de la tabla heredada, puede actualizar el valor del parámetro pasado a la función en las consultas para excluir la tabla heredada.
    * Si todavía usa solo la tabla heredada, las funciones seguirán funcionando si decide incluir la tabla heredada a través del mismo parámetro. Sin embargo, se recomienda [cambiar a las tablas específicas del recurso](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#steps-to-move-to-new-diagnostics-settings-for-a-log-analytics-workspace) lo antes posible.

* **Reduce la posibilidad de que se interrumpan las consultas personalizadas**: si Azure Backup introduce mejoras en el esquema de las tablas de LA subyacentes para adaptarse a futuros escenarios de informes, la definición de las funciones también se actualizará para tener en cuenta los cambios del esquema. Por lo tanto, si usa funciones del sistema para crear consultas personalizadas, las consultas no se interrumpirán, aunque haya cambios en el esquema subyacente de las tablas.

> [!NOTE]
> Microsoft mantiene las funciones del sistema y los usuarios no pueden editar sus definiciones. Si necesita funciones editables, puede crear [funciones guardadas](https://docs.microsoft.com/azure/azure-monitor/logs/functions) en LA.

## <a name="types-of-system-functions-offered-by-azure-backup"></a>Tipos de funciones del sistema que Azure Backup ofrece

* **Funciones principales**: son funciones que le ayudan a consultar las entidades clave de Azure Backup, como instancias de copia de seguridad, almacenes, directivas, trabajos y entidades de facturación. Por ejemplo, la función **_AzureBackup_getBackupInstances** devuelve una lista de todas las instancias de copia de seguridad que existen en su entorno a partir del último día completado (en UTC). Los parámetros y el esquema devuelto para cada una de estas funciones principales se resumen a continuación en este artículo.

* **Funciones de tendencia**: son funciones que devuelven los registros históricos de las entidades relacionadas con la copia de seguridad (por ejemplo, instancias de copia de seguridad o grupos de facturación) y permiten obtener información de las tendencias diarias, semanales y mensuales sobre las métricas clave (por ejemplo, el recuento o el almacenamiento consumido) que pertenecen a estas entidades. Los parámetros y el esquema devuelto para cada una de estas funciones de tendencia se resumen a continuación en este artículo.

> [!NOTE]
> Actualmente, las funciones del sistema devuelven datos hasta el último día completado (en UTC). Los datos del día parcial actual no aparecen en los informes. Por lo tanto, si desea recuperar los registros del día actual, deberá usar las tablas de LA sin formato.


## <a name="list-of-system-functions"></a>Lista de funciones del sistema

### <a name="core-functions"></a>Funciones principales

#### <a name="_azurebackup_getvaults"></a>_AzureBackup_GetVaults()

Esta función devuelve la lista de todos los almacenes de Recovery Services en el entorno de Azure que están asociados con el área de trabajo de LA.

**Parámetros**

| **Nombre de parámetro** | **Descripción** | **¿Necesario?** | **Valor de ejemplo** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Use este parámetro junto con el parámetro RangeEnd solo si necesita capturar todos los registros relacionados con el almacén en el período de tiempo que va desde RangeStart hasta RangeEnd. De manera predeterminada, los valores de RangeStart y RangeEnd son NULL, lo que hará que la función recupere solo el registro más reciente de cada almacén. | N | "2021-03-03 00:00:00" |
| RangeEnd     | Use este parámetro junto con el parámetro RangeStart solo si necesita capturar todos los registros relacionados con los almacenes en el período de tiempo que va desde RangeStart hasta RangeEnd. De manera predeterminada, los valores de RangeStart y RangeEnd son NULL, lo que hará que la función recupere solo el registro más reciente de cada almacén. | N |"2021-03-10 00:00:00"|
| VaultSubscriptionList   | Use este parámetro para filtrar la salida de la función para un determinado conjunto de suscripciones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de los id. de suscripción como un parámetro de esta función le ayuda a recuperar solo los almacenes que se encuentran en las suscripciones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las suscripciones. | N | "00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111"|
| VaultLocationList     | Use este parámetro para filtrar la salida de la función para un determinado conjunto de regiones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de las regiones como un parámetro de esta función le ayuda a recuperar solo los almacenes que se encuentran en las regiones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las regiones. | N | "eastus,westus"|
| VaultList    |Use este parámetro para filtrar la salida de la función para un determinado conjunto de almacenes. La especificación de una lista separada por comas de los nombres de almacén como un parámetro de esta función le ayuda a recuperar los registros que pertenecen únicamente a los almacenes especificados. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todos los almacenes. | N |"vault1,vault2,vault3"|
| VaultTypeList     | Use este parámetro para filtrar la salida de la función en los registros que pertenecen a un tipo de almacén determinado. Actualmente, el único tipo de almacén compatible es "Microsoft.RecoveryServices/vaults", que es el valor predeterminado de este parámetro. | N | "Microsoft.RecoveryServices/vaults"|
| ExcludeLegacyEvent  | Use este parámetro para elegir si se van a consultar los datos de la tabla heredada AzureDiagnostics o no. Si el valor de este parámetro es false, la función consulta los datos de la tabla AzureDiagnostics y de las tablas específicas del recurso. Si el valor de este parámetro es true, la función consulta los datos solo de las tablas específicas del recurso. El valor predeterminado es true. | N | true |

**Campos devueltos**

| **Nombre del campo** | **Descripción** |
| -------------- | --------------- |
| UniqueId | Clave principal que indica el id. único del almacén |
| Identificador | Id. de Azure Resource Manager (ARM) del almacén |
| Nombre | Nombre del almacén |
| SubscriptionId | Id. de la suscripción en la que se existe el recurso |
| Ubicación | Ubicación en la que se existe el almacén |
| VaultStore_StorageReplicationType | Tipo de replicación de almacenamiento asociado al almacén |
| Etiquetas | Etiquetas del almacén |
| TimeGenerated | Marca de tiempo del registro. |
| Tipo |  Tipo de almacén, que es "Microsoft.RecoveryServices/vaults".|

#### <a name="_azurebackup_getpolicies"></a>_AzureBackup_GetPolicies()

Esta función devuelve la lista de directivas de copia de seguridad que se usan en el entorno de Azure junto con la información detallada sobre cada directiva, como el tipo de origen de datos, el tipo de replicación de almacenamiento, etc.

**Parámetros**

| **Nombre de parámetro** | **Descripción** | **¿Necesario?** | **Valor de ejemplo** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Use este parámetro junto con el parámetro RangeStart solo si necesita capturar todos los registros relacionados con las directivas en el período de tiempo que va desde RangeStart hasta RangeEnd. De manera predeterminada, los valores de RangeStart y RangeEnd son NULL, lo que hará que la función recupere solo el registro más reciente de cada directiva. | N | "2021-03-03 00:00:00" |
| RangeEnd     | Use este parámetro junto con el parámetro RangeStart solo si necesita capturar todos los registros relacionados con las directivas en el período de tiempo que va desde RangeStart hasta RangeEnd. De manera predeterminada, los valores de RangeStart y RangeEnd son NULL, lo que hará que la función recupere solo el registro más reciente de cada directiva. | N |"2021-03-10 00:00:00"|
| VaultSubscriptionList   | Use este parámetro para filtrar la salida de la función para un determinado conjunto de suscripciones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de los id. de suscripción como un parámetro de esta función le ayuda a recuperar solo las directivas que se encuentran en las suscripciones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las suscripciones. | N | "00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111"|
| VaultLocationList     | Use este parámetro para filtrar la salida de la función para un determinado conjunto de regiones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de las regiones como un parámetro de esta función le ayuda a recuperar solo las directivas que se encuentran en las regiones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las regiones. | N | "eastus,westus"|
| VaultList    |Use este parámetro para filtrar la salida de la función para un determinado conjunto de almacenes. La especificación de una lista separada por comas de los nombres de almacén como un parámetro de esta función le ayuda a recuperar los registros de directivas que pertenecen únicamente a los almacenes especificados. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros de directivas en todos los almacenes. | N |"vault1,vault2,vault3"|
| VaultTypeList     | Use este parámetro para filtrar la salida de la función en los registros que pertenecen a un tipo de almacén determinado. Actualmente, el único tipo de almacén compatible es "Microsoft.RecoveryServices/vaults", que es el valor predeterminado de este parámetro. | N | "Microsoft.RecoveryServices/vaults"|
| ExcludeLegacyEvent  | Use este parámetro para elegir si se van a consultar los datos de la tabla heredada AzureDiagnostics o no. Si el valor de este parámetro es false, la función consulta los datos de la tabla AzureDiagnostics y de las tablas específicas del recurso. Si el valor de este parámetro es true, la función consulta los datos solo de las tablas específicas del recurso. El valor predeterminado es true. | N | true |
| BackupSolutionList | Use este parámetro para filtrar la salida de la función para un determinado conjunto de soluciones de copia de seguridad usadas en el entorno de Azure. Por ejemplo, si especifica "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM" como el valor de este parámetro, la función solo devuelve los registros relacionados con los elementos de los que se ha realizado una copia de seguridad mediante la copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de VM de Azure o DPM en Azure Backup. De manera predeterminada, el valor de este parámetro es "*", que hace que la función devuelva los registros que pertenecen a todas las soluciones de copia de seguridad admitidas por los Informes de Backup (los valores admitidos son "Copia de seguridad de máquina virtual de Azure", "SQL en copia de seguridad de máquina virtual de Azure", "SAP HANA en copia de seguridad de máquina virtual de Azure", "Copia de seguridad de Azure Storage (Azure Files)", "Agente de Azure Backup", "DPM", "Azure Backup Server" o una combinación separada por comas de cualquiera de estos valores). | N | "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM, Agente de Azure Backup" |

**Campos devueltos**

| **Nombre del campo** | **Descripción** |
| -------------- | --------------- |
| UniqueId | Clave principal que indica el id. único de la directiva. |
| Identificador | Id. de Azure Resource Manager (ARM) de la directiva. |
| Nombre | Nombre de la directiva. |
| Solución de copia de seguridad. | Solución de copia de seguridad a la que está asociada la directiva. Por ejemplo, Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, etc. |
| TimeGenerated | Marca de tiempo del registro. |
| VaultUniqueId | Clave externa que hace referencia al almacén asociado a la directiva. |
| VaultResourceId | Id. de Azure Resource Manager (ARM) del almacén asociado a la directiva. |
| VaultName | Nombre del almacén asociado a la directiva. |
| VaultTags | Etiquetas del almacén asociado a la directiva. |
| VaultLocation | Ubicación del almacén asociado a la directiva. |
| VaultSubscriptionId | Id. de la suscripción del almacén asociado a la directiva. |
| VaultStore_StorageReplicationType | Tipo de replicación de almacenamiento del almacén asociado a la directiva. |
| VaultType | Tipo de almacén, que es "Microsoft.RecoveryServices/vaults". |
| ExtendedProperties | Propiedades adicionales de la directiva. |

#### <a name="_azurebackup_getjobs"></a>_AzureBackup_GetJobs()

Esta función devuelve una lista de todos los trabajos relacionados con la copia de seguridad y restauración que se desencadenaron en un intervalo de tiempo especificado, junto con información detallada sobre cada trabajo, como el estado del trabajo, la duración del trabajo, los datos transferidos, etc.

**Parámetros**

| **Nombre de parámetro** | **Descripción** | **¿Necesario?** | **Valor de ejemplo** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Use este parámetro junto con el parámetro RangeEnd para recuperar la lista de todos los trabajos que se iniciaron en el período de tiempo que va desde RangeStart a RangeEnd. | Y | "2021-03-03 00:00:00" |
| RangeEnd     | Use este parámetro junto con el parámetro RangeStart para recuperar la lista de todos los trabajos que se iniciaron en el período de tiempo que va desde RangeStart a RangeEnd. | Y |"2021-03-10 00:00:00"|
| VaultSubscriptionList   | Use este parámetro para filtrar la salida de la función para un determinado conjunto de suscripciones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de los id. de suscripción como un parámetro de esta función le ayuda a recuperar solo los trabajos asociados a almacenes que se encuentran en las suscripciones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las suscripciones. | N | "00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111"|
| VaultLocationList     | Use este parámetro para filtrar la salida de la función para un determinado conjunto de regiones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de las regiones como un parámetro de esta función le ayuda a recuperar solo los trabajos asociados a almacenes que se encuentran en las regiones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las regiones. | N | "eastus,westus"|
| VaultList    | Use este parámetro para filtrar la salida de la función para un determinado conjunto de almacenes. La especificación de una lista separada por comas de los nombres de almacén como un parámetro de esta función le ayuda a recuperar los trabajos que pertenecen únicamente a los almacenes especificados. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque trabajos en todos los almacenes. | N |"vault1,vault2,vault3"|
| VaultTypeList     | Use este parámetro para filtrar la salida de la función en los registros que pertenecen a un tipo de almacén determinado. Actualmente, el único tipo de almacén compatible es "Microsoft.RecoveryServices/vaults", que es el valor predeterminado de este parámetro. | N | "Microsoft.RecoveryServices/vaults"|
| ExcludeLegacyEvent  | Use este parámetro para elegir si se van a consultar los datos de la tabla heredada AzureDiagnostics o no. Si el valor de este parámetro es false, la función consulta los datos de la tabla AzureDiagnostics y de las tablas específicas del recurso. Si el valor de este parámetro es true, la función consulta los datos solo de las tablas específicas del recurso. El valor predeterminado es true. | N | true |
| BackupSolutionList | Use este parámetro para filtrar la salida de la función para un determinado conjunto de soluciones de copia de seguridad usadas en el entorno de Azure. Por ejemplo, si especifica "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM" como el valor de este parámetro, la función solo devuelve los registros relacionados con los elementos de los que se ha realizado una copia de seguridad mediante la copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de VM de Azure o DPM en Azure Backup. De manera predeterminada, el valor de este parámetro es "*", que hace que la función devuelva los registros que pertenecen a todas las soluciones de copia de seguridad admitidas por los Informes de Backup (los valores admitidos son "Copia de seguridad de máquina virtual de Azure", "SQL en copia de seguridad de máquina virtual de Azure", "SAP HANA en copia de seguridad de máquina virtual de Azure", "Copia de seguridad de Azure Storage (Azure Files)", "Agente de Azure Backup", "DPM", "Azure Backup Server" o una combinación separada por comas de cualquiera de estos valores). | N | "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM, Agente de Azure Backup" |
| JobOperationList | Use este parámetro para filtrar la salida de la función para un determinado tipo de trabajo. Por ejemplo, Backup o Restore. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque trabajos de copia de seguridad y de restauración. | N | "Backup" | 
| JobStatusList | Use este parámetro para filtrar la salida de la función para un determinado estado de trabajo. Por ejemplo, Completed, Failed, etc. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque todos los trabajos independientemente del estado. | N | "Failed,CompletedWithWarnings" |
| JobFailureCodeList | Use este parámetro para filtrar la salida de la función para un determinado código de error. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque todos los trabajos independientemente del código de error. | N | "Success"
| DatasourceSetName | Use este parámetro para filtrar la salida de la función para un recurso primario en particular. Por ejemplo, para devolver instancias de SQL en copia de seguridad de máquina virtual de Azure que pertenecen a la máquina virtual "testvm", especifique _testvm_ como el valor de este parámetro. De manera predeterminada, el valor es "*", que hace que la función busque registros en todas las instancias de copia de seguridad. | N | "testvm" |
| BackupInstanceName | Use este parámetro para buscar trabajos en una instancia de copia de seguridad determinada por nombre. De manera predeterminada, el valor es "*", que hace que la función busque registros en todas las instancias de copia de seguridad. | N | "testvm" |
| ExcludeLog | Use este parámetro para excluir que la función devuelva los trabajos de registro (ayuda en el rendimiento de las consultas). De forma predeterminada, el valor de este parámetro es true, lo que hace que la función excluya los trabajos de registro. | N | true


**Campos devueltos**

| **Nombre del campo** | **Descripción** |
| -------------- | --------------- |
| UniqueId | Clave principal que indica el id. del trabajo. |
| OperationCategory | Categoría de la operación que se realiza. Por ejemplo, Backup o Restore. |
| Operación | Detalles de la operación que se realiza. Por ejemplo, Log (para la copia de seguridad de registros).|
| Estado | Estado del trabajo. Por ejemplo, Completed, Failed, CompletedWithWarnings |
| ErrorTitle | Código de error del trabajo. |
| StartTime | Fecha y hora en que se inició el trabajo. |
| DurationInSecs | Duración del trabajo en segundos. |
| DataTransferredInMBs | Datos transferidos por el trabajo en MB. |
| RestoreJobRPDateTime | Fecha y hora en que se creó el punto de recuperación que se recupera. |
| RestoreJobRPLocation | Ubicación donde se almacenó el punto de recuperación que se va a recuperar. |
| BackupInstanceUniqueId | Clave externa que hace referencia a la instancia de copia de seguridad asociada al trabajo. |
| BackupInstanceId | Id. de Azure Resource Manager (ARM) de la instancia de copia de seguridad asociada con el trabajo. |
| BackupInstanceFriendlyName | Nombre de la instancia de copia de seguridad asociada al trabajo. |
| DatasourceResourceId | Id. de Azure Resource Manager (ARM) del origen de datos subyacente asociado al trabajo. Por ejemplo, el id. de Azure Resource Manager (ARM) de la máquina virtual. |
| DatasourceFriendlyName | Nombre descriptivo del origen de datos subyacente asociado al trabajo. |
| DatasourceType | Tipo del origen de datos asociado al trabajo. Por ejemplo "Microsoft.Compute/virtualMachines". |
| BackupSolution | Solución de copia de seguridad a la que está asociada el trabajo. Por ejemplo, Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, etc. |
| DatasourceSetResourceId | Id. de Azure Resource Manager (ARM) del recurso primario del origen de datos (siempre que sea aplicable). Por ejemplo, para un origen de datos de SQL en máquina virtual de Azure, este campo contendrá el id. de Azure Resource Manager (ARM) de la máquina virtual en la que se encuentra SQL Database. |
| DatasourceSetType | Tipo del recurso primario del origen de datos (siempre que sea aplicable). Por ejemplo, para un origen de datos SAP HANA en máquina virtual de Azure, este campo será Microsoft.Compute/virtualMachines, ya que el recurso primario es una máquina virtual de Azure. |
| VaultResourceId | Id. de Azure Resource Manager (ARM) del almacén asociado al trabajo. |
| VaultUniqueId | Clave externa que hace referencia al almacén asociado al trabajo. |
| VaultName | Nombre del almacén asociado al trabajo. |
| VaultTags | Etiquetas del almacén asociado al trabajo. |
| VaultSubscriptionId | Id. de la suscripción del almacén asociado al trabajo. |
| VaultLocation | Ubicación del almacén asociado al trabajo. |
| VaultStore_StorageReplicationType | Tipo de replicación de almacenamiento del almacén asociado al trabajo. |
| VaultType | Tipo de almacén, que es "Microsoft.RecoveryServices/vaults". |
| TimeGenerated | Marca de tiempo del registro. |

#### <a name="_azurebackup_getbackupinstances"></a>_AzureBackup_GetBackupInstances()

Esta función devuelve la lista de instancias de copia de seguridad asociadas a los almacenes de Recovery Services, junto con información detallada sobre cada instancia de copia de seguridad, como el consumo de almacenamiento en la nube, la directiva asociada, etc.

**Parámetros**

| **Nombre de parámetro** | **Descripción** | **¿Necesario?** | **Valor de ejemplo** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Use este parámetro junto con el parámetro RangeEnd solo si necesita capturar todos los registros relacionados con la instancia de copia de seguridad en el período de tiempo que va desde RangeStart hasta RangeEnd. De manera predeterminada, los valores de RangeStart y RangeEnd son NULL, lo que hará que la función recupere solo el registro más reciente de cada instancia de copia de seguridad. | N | "2021-03-03 00:00:00" |
| RangeEnd     | Use este parámetro junto con el parámetro RangeStart solo si necesita capturar todos los registros relacionados con la instancia de copia de seguridad en el período de tiempo que va desde RangeStart hasta RangeEnd. De manera predeterminada, los valores de RangeStart y RangeEnd son NULL, lo que hará que la función recupere solo el registro más reciente de cada instancia de copia de seguridad. | N |"2021-03-10 00:00:00"|
| VaultSubscriptionList   | Use este parámetro para filtrar la salida de la función para un determinado conjunto de suscripciones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de los id. de suscripción como un parámetro de esta función le ayuda a recuperar solo las instancias de copia de seguridad que se encuentran en las suscripciones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las suscripciones. | N | "00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111"|
| VaultLocationList     | Use este parámetro para filtrar la salida de la función para un determinado conjunto de regiones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de las regiones como un parámetro de esta función le ayuda a recuperar solo las instancias de copia de seguridad que se encuentran en las regiones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las regiones. | N | "eastus,westus"|
| VaultList    |Use este parámetro para filtrar la salida de la función para un determinado conjunto de almacenes. La especificación de una lista separada por comas de los nombres de almacén como un parámetro de esta función le ayuda a recuperar los registros de las instancias de copia de seguridad que pertenecen únicamente a los almacenes especificados. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros de instancias de copia de seguridad en todos los almacenes. | N |"vault1,vault2,vault3"|
| VaultTypeList     | Use este parámetro para filtrar la salida de la función en los registros que pertenecen a un tipo de almacén determinado. Actualmente, el único tipo de almacén compatible es "Microsoft.RecoveryServices/vaults", que es el valor predeterminado de este parámetro. | N | "Microsoft.RecoveryServices/vaults"|
| ExcludeLegacyEvent  | Use este parámetro para elegir si se van a consultar los datos de la tabla heredada AzureDiagnostics o no. Si el valor de este parámetro es false, la función consulta los datos de la tabla AzureDiagnostics y de las tablas específicas del recurso. Si el valor de este parámetro es true, la función consulta los datos solo de las tablas específicas del recurso. El valor predeterminado es true. | N | true |
| BackupSolutionList | Use este parámetro para filtrar la salida de la función para un determinado conjunto de soluciones de copia de seguridad usadas en el entorno de Azure. Por ejemplo, si especifica "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM" como el valor de este parámetro, la función solo devuelve los registros relacionados con los elementos de los que se ha realizado una copia de seguridad mediante la copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de VM de Azure o DPM en Azure Backup. De manera predeterminada, el valor de este parámetro es "*", que hace que la función devuelva los registros que pertenecen a todas las soluciones de copia de seguridad admitidas por los Informes de Backup (los valores admitidos son "Copia de seguridad de máquina virtual de Azure", "SQL en copia de seguridad de máquina virtual de Azure", "SAP HANA en copia de seguridad de máquina virtual de Azure", "Copia de seguridad de Azure Storage (Azure Files)", "Agente de Azure Backup", "DPM", "Azure Backup Server" o una combinación separada por comas de cualquiera de estos valores). | N | "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM, Agente de Azure Backup" |
| ProtectionInfoList | Use este parámetro para elegir entre incluir solo las instancias de copia de seguridad que están protegidas activamente o incluir también las instancias para las que se ha detenido la protección y las instancias para las que está pendiente la copia de seguridad inicial. Los valores admitidos son "Protected", "ProtectionStopped", "InitialBackupPending" o una combinación de cualquiera de estos valores separados por comas. De manera predeterminada, el valor es "*", que hace que la función busque todas las instancias de copia de seguridad con independencia de los detalles de protección. | N | "Protected" |
| DatasourceSetName | Use este parámetro para filtrar la salida de la función para un recurso primario en particular. Por ejemplo, para devolver instancias de SQL en copia de seguridad de máquina virtual de Azure que pertenecen a la máquina virtual "testvm", especifique _testvm_ como el valor de este parámetro. De manera predeterminada, el valor es "*", que hace que la función busque registros en todas las instancias de copia de seguridad. | N | "testvm" |
| BackupInstanceName | Use este parámetro para buscar una instancia de copia de seguridad determinada por nombre. De manera predeterminada, el valor es "*", que hace que la función busque todas las instancias de copia de seguridad. | N | "testvm" |
| DisplayAllFields | Use este parámetro para elegir si se va a recuperar solo un subconjunto de los campos devueltos por la función. Si el valor de este parámetro es false, la función elimina la información relacionada con el almacenamiento y el punto de retención de la salida de la función. Esto resulta útil si usa esta función como un paso intermedio en una consulta más grande y necesita optimizar el rendimiento de la consulta eliminando las columnas que no necesita para el análisis. De forma predeterminada, el valor de este parámetro es true, lo que hace que la función devuelva todos los campos que pertenecen a la instancia de copia de seguridad. | N | true |

**Campos devueltos**

| **Nombre del campo** | **Descripción** |
| -------------- | --------------- |
| UniqueId | Clave principal que indica el id. único de la instancia de copia de seguridad. |
| Identificador | Id. de Azure Resource Manager (ARM) de la instancia de copia de seguridad. |
| FriendlyName | Nombre descriptivo de la instancia de copia de seguridad. |
| ProtectionInfo | Información sobre la configuración de la protección de la instancia de copia de seguridad. Por ejemplo, protección configurada, protección detenida, copia de seguridad inicial pendiente. |
| LatestRecoveryPoint | Fecha y hora del punto de recuperación más reciente asociado a la instancia de copia de seguridad. |
| OldestRecoveryPoint | Fecha y hora del punto de recuperación más antiguo asociado a la instancia de copia de seguridad. |
| SourceSizeInMBs | Tamaño de front-end de la instancia de copia de seguridad en MB. |
| VaultStore_StorageConsumptionInMBs | Almacenamiento en la nube total consumido por la instancia de copia de seguridad en el nivel estándar de almacén. |
| DataSourceFriendlyName | Nombre descriptivo del origen de datos correspondiente a la instancia de copia de seguridad. |
| BackupSolution | Solución de copia de seguridad a la que está asociada la instancia de copia de seguridad. Por ejemplo, Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, etc. |
| DatasourceType | Tipo del origen de datos correspondiente a la instancia de copia de seguridad. Por ejemplo "Microsoft.Compute/virtualMachines". |
| DatasourceResourceId | Id. de Azure Resource Manager (ARM) del origen de datos subyacente correspondiente a la instancia de copia de seguridad. Por ejemplo, el id. de Azure Resource Manager (ARM) de la máquina virtual. |
| DatasourceSetFriendlyName | Nombre descriptivo del recurso primario del origen de datos (siempre que sea aplicable). Por ejemplo, para un origen de datos de SQL en máquina virtual de Azure, este campo contendrá el nombre de la máquina virtual en la que se encuentra SQL Database. |
| DatasourceSetResourceId | Id. de Azure Resource Manager (ARM) del recurso primario del origen de datos (siempre que sea aplicable). Por ejemplo, para un origen de datos de SQL en máquina virtual de Azure, este campo contendrá el id. de Azure Resource Manager (ARM) de la máquina virtual en la que se encuentra SQL Database. |
| DatasourceSetType | Tipo del recurso primario del origen de datos (siempre que sea aplicable). Por ejemplo, para un origen de datos SAP HANA en máquina virtual de Azure, este campo será Microsoft.Compute/virtualMachines, ya que el recurso primario es una máquina virtual de Azure. |
| PolicyName | Nombre de la directiva asociada a la instancia de copia de seguridad. |
| PolicyUniqueId | Clave externa que hace referencia a la directiva asociada a la instancia de copia de seguridad.  |
| PolicyId | Id. de Azure Resource Manager (ARM) de la directiva asociada a la instancia de copia de seguridad. |
| VaultResourceId | Id. de Azure Resource Manager (ARM) del almacén asociado a la instancia de copia de seguridad. |
| VaultUniqueId | Clave externa que hace referencia al almacén asociado a la instancia de copia de seguridad. |
| VaultName | Nombre del almacén asociado a la instancia de copia de seguridad. |
| VaultTags | Etiqueta del almacén asociado a la instancia de copia de seguridad. |
| VaultSubscriptionId | Id. de suscripción del almacén asociado a la instancia de copia de seguridad. |
| VaultLocation | Ubicación del almacén asociado a la instancia de copia de seguridad. |
| VaultStore_StorageReplicationType | Tipo de replicación de almacenamiento del almacén asociado a la instancia de copia de seguridad. |
| VaultType | Tipo de almacén, que es "Microsoft.RecoveryServices/vaults". |
| TimeGenerated | Marca de tiempo del registro. |

#### <a name="_azurebackup_getbillinggroups"></a>_AzureBackup_GetBillingGroups()

Esta función devuelve una lista de todas las entidades de facturación relacionadas con la copia de seguridad (grupos de facturación) junto con información sobre los componentes clave de la facturación, como el tamaño de front-end y el almacenamiento en la nube total.

**Parámetros**

| **Nombre de parámetro** | **Descripción** | **¿Necesario?** | **Valor de ejemplo** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Use este parámetro junto con el parámetro RangeEnd solo si necesita capturar todos los registros relacionados con el grupo de facturación en el período de tiempo que va desde RangeStart hasta RangeEnd. De manera predeterminada, los valores de RangeStart y RangeEnd son NULL, lo que hará que la función recupere solo el registro más reciente de cada grupo de facturación. | N | "2021-03-03 00:00:00" |
| RangeEnd     | Use este parámetro junto con el parámetro RangeStart solo si necesita capturar todos los registros relacionados con el grupo de facturación en el período de tiempo que va desde RangeStart hasta RangeEnd. De manera predeterminada, los valores de RangeStart y RangeEnd son NULL, lo que hará que la función recupere solo el registro más reciente de cada grupo de facturación. | N |"2021-03-10 00:00:00"|
| VaultSubscriptionList   | Use este parámetro para filtrar la salida de la función para un determinado conjunto de suscripciones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de los id. de suscripción como un parámetro de esta función le ayuda a recuperar solo los grupos de facturación que se encuentran en las suscripciones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las suscripciones. | N | "00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111"|
| VaultLocationList     | Use este parámetro para filtrar la salida de la función para un determinado conjunto de regiones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de las regiones como un parámetro de esta función le ayuda a recuperar solo los grupos de facturación que se encuentran en las regiones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las regiones. | N | "eastus,westus"|
| VaultList    |Use este parámetro para filtrar la salida de la función para un determinado conjunto de almacenes. La especificación de una lista separada por comas de los nombres de almacén como un parámetro de esta función le ayuda a recuperar los registros de las instancias de copia de seguridad que pertenecen únicamente a los almacenes especificados. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros de grupos de facturación en todos los almacenes. | N |"vault1,vault2,vault3"|
| VaultTypeList     | Use este parámetro para filtrar la salida de la función en los registros que pertenecen a un tipo de almacén determinado. Actualmente, el único tipo de almacén compatible es "Microsoft.RecoveryServices/vaults", que es el valor predeterminado de este parámetro. | N | "Microsoft.RecoveryServices/vaults"|
| ExcludeLegacyEvent  | Use este parámetro para elegir si se van a consultar los datos de la tabla heredada AzureDiagnostics o no. Si el valor de este parámetro es false, la función consulta los datos de la tabla AzureDiagnostics y de las tablas específicas del recurso. Si el valor de este parámetro es true, la función consulta los datos solo de las tablas específicas del recurso. El valor predeterminado es true. | N | true |
| BackupSolutionList | Use este parámetro para filtrar la salida de la función para un determinado conjunto de soluciones de copia de seguridad usadas en el entorno de Azure. Por ejemplo, si especifica "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM" como el valor de este parámetro, la función solo devuelve los registros relacionados con los elementos de los que se ha realizado una copia de seguridad mediante la copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de VM de Azure o DPM en Azure Backup. De manera predeterminada, el valor de este parámetro es "*", que hace que la función devuelva los registros que pertenecen a todas las soluciones de copia de seguridad admitidas por los Informes de Backup (los valores admitidos son "Copia de seguridad de máquina virtual de Azure", "SQL en copia de seguridad de máquina virtual de Azure", "SAP HANA en copia de seguridad de máquina virtual de Azure", "Copia de seguridad de Azure Storage (Azure Files)", "Agente de Azure Backup", "DPM", "Azure Backup Server" o una combinación separada por comas de cualquiera de estos valores). | N | "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM, Agente de Azure Backup" |
| BillingGroupName | Use este parámetro para buscar un grupo de facturación determinado por nombre. De manera predeterminada, el valor es "*", que hace que la función busque todos los grupos de facturación. | N | "testvm" |

**Campos devueltos**

| **Nombre del campo** | **Descripción** |
| -------------- | --------------- |
| UniqueId | Clave principal que indica el id. del grupo de facturación. |
| FriendlyName | Nombre descriptivo del grupo de facturación. |
| Nombre | Nombre del grupo de facturación. |
| Tipo | Tipo del grupo de facturación. Por ejemplo, ProtectedContainer o BackupItem. |
| SourceSizeInMBs | Tamaño de front-end del grupo de facturación en MB. |
| VaultStore_StorageConsumptionInMBs | Almacenamiento en la nube total consumido por el grupo de facturación en el nivel estándar de almacén. |
| BackupSolution | Solución de copia de seguridad a la que está asociado el grupo de facturación. Por ejemplo, Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, etc. |
| VaultResourceId | Id. de Azure Resource Manager (ARM) del almacén asociado al grupo de facturación. |
| VaultUniqueId | Clave externa que hace referencia al almacén asociado al grupo de facturación. |
| VaultName | Nombre del almacén asociado al grupo de facturación. |
| VaultTags | Etiquetas del almacén asociado al grupo de facturación. |
| VaultSubscriptionId | Id. de la suscripción del almacén asociado al grupo de facturación. |
| VaultLocation | Ubicación del almacén asociado al grupo de facturación. |
| VaultStore_StorageReplicationType | Tipo de replicación de almacenamiento del almacén asociado al grupo de facturación. |
| VaultType | Tipo de almacén, que es "Microsoft.RecoveryServices/vaults". |
| TimeGenerated | Marca de tiempo del registro. |
| ExtendedProperties | Propiedades adicionales del grupo de facturación. |

### <a name="trend-functions"></a>Funciones de tendencia

#### <a name="_azurebackup_getbackupinstancestrends"></a>_AzureBackup_GetBackupInstancesTrends()

Esta función devuelve registros históricos para cada instancia de copia de seguridad, lo que le permite ver las tendencias diarias, semanales y mensuales relacionadas con el recuento de instancias de copia de seguridad y el consumo de almacenamiento, en varios niveles de granularidad.

**Parámetros**

| **Nombre de parámetro** | **Descripción** | **¿Necesario?** | **Valor de ejemplo** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Use este parámetro junto con el parámetro RangeEnd para recuperar todos los registros relacionados con la instancia de copia de seguridad en el período de tiempo que va desde RangeStart hasta RangeEnd. | Y | "2021-03-03 00:00:00" |
| RangeEnd     | Use este parámetro junto con el parámetro RangeStart para recuperar todos los registros relacionados con la instancia de copia de seguridad en el período de tiempo que va desde RangeStart hasta RangeEnd. | Y |"2021-03-10 00:00:00"|
| VaultSubscriptionList   | Use este parámetro para filtrar la salida de la función para un determinado conjunto de suscripciones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de los id. de suscripción como un parámetro de esta función le ayuda a recuperar solo las instancias de copia de seguridad que se encuentran en las suscripciones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las suscripciones. | N | "00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111"|
| VaultLocationList     | Use este parámetro para filtrar la salida de la función para un determinado conjunto de regiones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de las regiones como un parámetro de esta función le ayuda a recuperar solo las instancias de copia de seguridad que se encuentran en las regiones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las regiones. | N | "eastus,westus"|
| VaultList    |Use este parámetro para filtrar la salida de la función para un determinado conjunto de almacenes. La especificación de una lista separada por comas de los nombres de almacén como un parámetro de esta función le ayuda a recuperar los registros de las instancias de copia de seguridad que pertenecen únicamente a los almacenes especificados. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros de instancias de copia de seguridad en todos los almacenes. | N |"vault1,vault2,vault3"|
| VaultTypeList     | Use este parámetro para filtrar la salida de la función en los registros que pertenecen a un tipo de almacén determinado. Actualmente, el único tipo de almacén compatible es "Microsoft.RecoveryServices/vaults", que es el valor predeterminado de este parámetro. | N | "Microsoft.RecoveryServices/vaults"|
| ExcludeLegacyEvent  | Use este parámetro para elegir si se van a consultar los datos de la tabla heredada AzureDiagnostics o no. Si el valor de este parámetro es false, la función consulta los datos de la tabla AzureDiagnostics y de las tablas específicas del recurso. Si el valor de este parámetro es true, la función consulta los datos solo de las tablas específicas del recurso. El valor predeterminado es true. | N | true |
| BackupSolutionList | Use este parámetro para filtrar la salida de la función para un determinado conjunto de soluciones de copia de seguridad usadas en el entorno de Azure. Por ejemplo, si especifica "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM" como el valor de este parámetro, la función solo devuelve los registros relacionados con los elementos de los que se ha realizado una copia de seguridad mediante la copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de VM de Azure o DPM en Azure Backup. De manera predeterminada, el valor de este parámetro es "*", que hace que la función devuelva los registros que pertenecen a todas las soluciones de copia de seguridad admitidas por los Informes de Backup (los valores admitidos son "Copia de seguridad de máquina virtual de Azure", "SQL en copia de seguridad de máquina virtual de Azure", "SAP HANA en copia de seguridad de máquina virtual de Azure", "Copia de seguridad de Azure Storage (Azure Files)", "Agente de Azure Backup", "DPM", "Azure Backup Server" o una combinación separada por comas de cualquiera de estos valores). | N | "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM, Agente de Azure Backup" |
| ProtectionInfoList | Use este parámetro para elegir entre incluir solo las instancias de copia de seguridad que están protegidas activamente o incluir también las instancias para las que se ha detenido la protección y las instancias para las que está pendiente la copia de seguridad inicial. Los valores admitidos son "Protected", "ProtectionStopped", "InitialBackupPending" o una combinación de cualquiera de estos valores separados por comas. De manera predeterminada, el valor es "*", que hace que la función busque todas las instancias de copia de seguridad con independencia de los detalles de protección. | N | "Protected" |
| DatasourceSetName | Use este parámetro para filtrar la salida de la función para un recurso primario en particular. Por ejemplo, para devolver instancias de SQL en copia de seguridad de máquina virtual de Azure que pertenecen a la máquina virtual "testvm", especifique _testvm_ como el valor de este parámetro. De manera predeterminada, el valor es "*", que hace que la función busque registros en todas las instancias de copia de seguridad. | N | "testvm" |
| BackupInstanceName | Use este parámetro para buscar una instancia de copia de seguridad determinada por nombre. De manera predeterminada, el valor es "*", que hace que la función busque todas las instancias de copia de seguridad. | N | "testvm" |
| DisplayAllFields | Use este parámetro para elegir si se va a recuperar solo un subconjunto de los campos devueltos por la función. Si el valor de este parámetro es false, la función elimina la información relacionada con el almacenamiento y el punto de retención de la salida de la función. Esto resulta útil si usa esta función como un paso intermedio en una consulta más grande y necesita optimizar el rendimiento de la consulta eliminando las columnas que no necesita para el análisis. De forma predeterminada, el valor de este parámetro es true, lo que hace que la función devuelva todos los campos que pertenecen a la instancia de copia de seguridad. | N | true |
| AggregationType | Use este parámetro para especificar la granularidad de tiempo con la que se deben recuperar los datos. Si el valor de este parámetro es "Daily", la función devuelve un registro por cada instancia de copia de seguridad por día, lo que le permite analizar las tendencias diarias del consumo de almacenamiento y el recuento de instancias de copia de seguridad. Si el valor de este parámetro es "Weekly", la función devuelve un registro por cada instancia de copia de seguridad por semana, lo que le permite analizar las tendencias semanales. De manera similar, puede especificar "Monthly" para analizar las tendencias mensuales. El valor predeterminado es "Daily". Si consulta datos para intervalos de tiempo mayores, se recomienda usar "Weekly" o "Monthly" para mejorar el rendimiento de las consultas y facilitar el análisis de tendencias. | N | "Weekly" |

**Campos devueltos**

| **Nombre del campo** | **Descripción** |
| -------------- | --------------- |
| UniqueId | Clave principal que indica el id. único de la instancia de copia de seguridad. |
| Identificador | Id. de Azure Resource Manager (ARM) de la instancia de copia de seguridad. |
| FriendlyName | Nombre descriptivo de la instancia de copia de seguridad. |
| ProtectionInfo | Información sobre la configuración de la protección de la instancia de copia de seguridad. Por ejemplo, protección configurada, protección detenida, copia de seguridad inicial pendiente. |
| LatestRecoveryPoint | Fecha y hora del punto de recuperación más reciente asociado a la instancia de copia de seguridad. |
| OldestRecoveryPoint | Fecha y hora del punto de recuperación más antiguo asociado a la instancia de copia de seguridad. |
| SourceSizeInMBs | Tamaño de front-end de la instancia de copia de seguridad en MB. |
| VaultStore_StorageConsumptionInMBs | Almacenamiento en la nube total consumido por la instancia de copia de seguridad en el nivel estándar de almacén. |
| DataSourceFriendlyName | Nombre descriptivo del origen de datos correspondiente a la instancia de copia de seguridad. |
| BackupSolution | Solución de copia de seguridad a la que está asociada la instancia de copia de seguridad. Por ejemplo, Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, etc. |
| DatasourceType | Tipo del origen de datos correspondiente a la instancia de copia de seguridad. Por ejemplo "Microsoft.Compute/virtualMachines". |
| DatasourceResourceId | Id. de Azure Resource Manager (ARM) del origen de datos subyacente correspondiente a la instancia de copia de seguridad. Por ejemplo, el id. de Azure Resource Manager (ARM) de la máquina virtual. |
| DatasourceSetFriendlyName | Nombre descriptivo del recurso primario del origen de datos (siempre que sea aplicable). Por ejemplo, para un origen de datos de SQL en máquina virtual de Azure, este campo contendrá el nombre de la máquina virtual en la que se encuentra SQL Database. |
| DatasourceSetResourceId | Id. de Azure Resource Manager (ARM) del recurso primario del origen de datos (siempre que sea aplicable). Por ejemplo, para un origen de datos de SQL en máquina virtual de Azure, este campo contendrá el id. de Azure Resource Manager (ARM) de la máquina virtual en la que se encuentra SQL Database. |
| DatasourceSetType | Tipo del recurso primario del origen de datos (siempre que sea aplicable). Por ejemplo, para un origen de datos SAP HANA en máquina virtual de Azure, este campo será Microsoft.Compute/virtualMachines, ya que el recurso primario es una máquina virtual de Azure. |
| PolicyName | Nombre de la directiva asociada a la instancia de copia de seguridad. |
| PolicyUniqueId | Clave externa que hace referencia a la directiva asociada a la instancia de copia de seguridad.  |
| PolicyId | Id. de Azure Resource Manager (ARM) de la directiva asociada a la instancia de copia de seguridad. |
| VaultResourceId | Id. de Azure Resource Manager (ARM) del almacén asociado a la instancia de copia de seguridad. |
| VaultUniqueId | Clave externa que hace referencia al almacén asociado a la instancia de copia de seguridad. |
| VaultName | Nombre del almacén asociado a la instancia de copia de seguridad. |
| VaultTags | Etiqueta del almacén asociado a la instancia de copia de seguridad. |
| VaultSubscriptionId | Id. de suscripción del almacén asociado a la instancia de copia de seguridad. |
| VaultLocation | Ubicación del almacén asociado a la instancia de copia de seguridad. |
| VaultStore_StorageReplicationType | Tipo de replicación de almacenamiento del almacén asociado a la instancia de copia de seguridad. |
| VaultType | Tipo de almacén, que es "Microsoft.RecoveryServices/vaults". |
| TimeGenerated | Marca de tiempo del registro. |

#### <a name="_azurebackup_getbillinggroupstrends"></a>_AzureBackup_GetBillingGroupsTrends()

Esta función devuelve registros históricos para cada entidad de facturación, lo que le permite ver las tendencias diarias, semanales y mensuales relacionadas con el tamaño de front-end y el consumo de almacenamiento, en varios niveles de granularidad.

**Parámetros**

| **Nombre de parámetro** | **Descripción** | **¿Necesario?** | **Valor de ejemplo** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Use este parámetro junto con el parámetro RangeEnd para recuperar todos los registros relacionados con el grupo de facturación en el período de tiempo que va desde RangeStart hasta RangeEnd. | Y | "2021-03-03 00:00:00" |
| RangeEnd     | Use este parámetro junto con el parámetro RangeStart para recuperar todos los registros relacionados con el grupo de facturación en el período de tiempo que va desde RangeStart hasta RangeEnd. | Y |"2021-03-10 00:00:00"|
| VaultSubscriptionList  | Use este parámetro para filtrar la salida de la función para un determinado conjunto de suscripciones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de los id. de suscripción como un parámetro de esta función le ayuda a recuperar solo los grupos de facturación que se encuentran en las suscripciones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las suscripciones. | N | "00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111"|
| VaultLocationList     | Use este parámetro para filtrar la salida de la función para un determinado conjunto de regiones en las que existen datos de copia de seguridad. La especificación de una lista separada por comas de las regiones como un parámetro de esta función le ayuda a recuperar solo los grupos de facturación que se encuentran en las regiones especificadas. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros en todas las regiones. | N | "eastus,westus"|
| VaultList    |Use este parámetro para filtrar la salida de la función para un determinado conjunto de almacenes. La especificación de una lista separada por comas de los nombres de almacén como un parámetro de esta función le ayuda a recuperar los registros de las instancias de copia de seguridad que pertenecen únicamente a los almacenes especificados. De manera predeterminada, el valor de este parámetro es "*", que hace que la función busque registros de grupos de facturación en todos los almacenes. | N |"vault1,vault2,vault3"|
| VaultTypeList     | Use este parámetro para filtrar la salida de la función en los registros que pertenecen a un tipo de almacén determinado. Actualmente, el único tipo de almacén compatible es "Microsoft.RecoveryServices/vaults", que es el valor predeterminado de este parámetro. | N | "Microsoft.RecoveryServices/vaults"|
| ExcludeLegacyEvent  | Use este parámetro para elegir si se van a consultar los datos de la tabla heredada AzureDiagnostics o no. Si el valor de este parámetro es false, la función consulta los datos de la tabla AzureDiagnostics y de las tablas específicas del recurso. Si el valor de este parámetro es true, la función consulta los datos solo de las tablas específicas del recurso. El valor predeterminado es true. | N | true |
| BackupSolutionList | Use este parámetro para filtrar la salida de la función para un determinado conjunto de soluciones de copia de seguridad usadas en el entorno de Azure. Por ejemplo, si especifica "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM" como el valor de este parámetro, la función solo devuelve los registros relacionados con los elementos de los que se ha realizado una copia de seguridad mediante la copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de VM de Azure o DPM en Azure Backup. De manera predeterminada, el valor de este parámetro es "*", que hace que la función devuelva los registros que pertenecen a todas las soluciones de copia de seguridad admitidas por los Informes de Backup (los valores admitidos son "Copia de seguridad de máquina virtual de Azure", "SQL en copia de seguridad de máquina virtual de Azure", "SAP HANA en copia de seguridad de máquina virtual de Azure", "Copia de seguridad de Azure Storage (Azure Files)", "Agente de Azure Backup", "DPM", "Azure Backup Server" o una combinación separada por comas de cualquiera de estos valores). | N | "Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, DPM, Agente de Azure Backup" |
| BillingGroupName | Use este parámetro para buscar un grupo de facturación determinado por nombre. De manera predeterminada, el valor es "*", que hace que la función busque todos los grupos de facturación. | N | "testvm" |
| AggregationType | Use este parámetro para especificar la granularidad de tiempo con la que se deben recuperar los datos. Si el valor de este parámetro es "Daily", la función devuelve un registro por cada grupo de facturación por día, lo que le permite analizar las tendencias diarias del consumo de almacenamiento y el tamaño de front-end. Si el valor de este parámetro es "Weekly", la función devuelve un registro por cada instancia de copia de seguridad por semana, lo que le permite analizar las tendencias semanales. De manera similar, puede especificar "Monthly" para analizar las tendencias mensuales. El valor predeterminado es "Daily". Si consulta datos para intervalos de tiempo mayores, se recomienda usar "Weekly" o "Monthly" para mejorar el rendimiento de las consultas y facilitar el análisis de tendencias. | N | "Weekly" |

**Campos devueltos**

| **Nombre del campo** | **Descripción** |
| -------------- | --------------- |
| UniqueId | Clave principal que indica el id. del grupo de facturación. |
| FriendlyName | Nombre descriptivo del grupo de facturación. |
| Nombre | Nombre del grupo de facturación. |
| Tipo | Tipo del grupo de facturación. Por ejemplo, ProtectedContainer o BackupItem. |
| SourceSizeInMBs | Tamaño de front-end del grupo de facturación en MB. |
| VaultStore_StorageConsumptionInMBs | Almacenamiento en la nube total consumido por el grupo de facturación en el nivel estándar de almacén. |
| BackupSolution | Solución de copia de seguridad a la que está asociado el grupo de facturación. Por ejemplo, Copia de seguridad de máquina virtual de Azure, SQL en copia de seguridad de máquina virtual de Azure, etc. |
| VaultResourceId | Id. de Azure Resource Manager (ARM) del almacén asociado al grupo de facturación. |
| VaultUniqueId | Clave externa que hace referencia al almacén asociado al grupo de facturación. |
| VaultName | Nombre del almacén asociado al grupo de facturación. |
| VaultTags | Etiquetas del almacén asociado al grupo de facturación. |
| VaultSubscriptionId | Id. de la suscripción del almacén asociado al grupo de facturación. |
| VaultLocation | Ubicación del almacén asociado al grupo de facturación. |
| VaultStore_StorageReplicationType | Tipo de replicación de almacenamiento del almacén asociado al grupo de facturación. |
| VaultType | Tipo de almacén, que es "Microsoft.RecoveryServices/vaults". |
| TimeGenerated | Marca de tiempo del registro. |
| ExtendedProperties | Propiedades adicionales del grupo de facturación. |

## <a name="sample-queries"></a>Consultas de ejemplo

A continuación, encontrará algunas consultas de ejemplo que le ayudarán a empezar a usar las funciones del sistema.

- Todos los trabajos de copia de seguridad de máquina virtual de Azure con errores en un intervalo de tiempo determinado

    ````Kusto
    _AzureBackup_GetJobs("2021-03-05", "2021-03-06") //call function with RangeStart and RangeEnd parameters set, and other parameters with default value
    | where BackupSolution=="Azure Virtual Machine Backup" and Status=="Failed"
    | project BackupInstanceFriendlyName, BackupInstanceId, OperationCategory, Status,  JobStartDateTime=StartTime, JobDuration=DurationInSecs/3600, ErrorTitle, DataTransferred=DataTransferredInMBs
    ````

- Todos los trabajos de copia de seguridad de registros de SQL en un intervalo de tiempo determinado

    ````Kusto
    _AzureBackup_GetJobs("2021-03-05", "2021-03-06","*","*","*","*",true,"*","*","*","*","*","*",false) //call function with RangeStart and RangeEnd parameters set, ExcludeLog parameter as false, and other parameters with default value
    | where BackupSolution=="SQL in Azure VM Backup" and Operation=="Log"
    | project BackupInstanceFriendlyName, BackupInstanceId, OperationCategory, Status,  JobStartDateTime=StartTime, JobDuration=DurationInSecs/3600, ErrorTitle, DataTransferred=DataTransferredInMBs
    ````

- Tendencia semanal del almacenamiento de copia de seguridad consumido para la máquina virtual "testvm"

    ````Kusto
    _AzureBackup_GetBackupInstancesTrends("2021-01-01", "2021-03-06","*","*","*","*",false,"*","*","*","*",true, "Weekly") //call function with RangeStart and RangeEnd parameters set, AggregationType parameter as Weekly, and other parameters with default value
    | where BackupSolution == "Azure Virtual Machine Backup"
    | where FriendlyName == "testvm"
    | project TimeGenerated, VaultStore_StorageConsumptionInMBs
    | render timechart 
    ````

## <a name="next-steps"></a>Pasos siguientes
[Más información sobre los Informes de Backup](https://docs.microsoft.com/azure/backup/configure-reports)
