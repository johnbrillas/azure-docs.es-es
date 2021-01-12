---
title: Actualización de la directiva de copia de seguridad de máquinas virtuales existente mediante la CLI
description: Aprenda a actualizar la directiva de copia de seguridad de máquinas virtuales existente mediante la CLI de Azure.
ms.topic: conceptual
ms.date: 12/31/2020
ms.openlocfilehash: ba4ded4c82a1eaea7c20ea94da580a8702467b85
ms.sourcegitcommit: 89c0482c16bfec316a79caa3667c256ee40b163f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2021
ms.locfileid: "97858823"
---
# <a name="update-the-existing-vm-backup-policy-using-cli"></a>Actualización de la directiva de copia de seguridad de máquinas virtuales existente mediante la CLI

Puede usar la CLI de Azure para actualizar una directiva de copia de seguridad de máquinas virtuales existente. En este artículo se explica cómo exportar la directiva existente a un archivo JSON, modificar el archivo y luego usar la CLI de Azure para actualizar la directiva con la directiva modificada.

## <a name="modify-an-existing-policy"></a>Modificación de una directiva existente

Para modificar una directiva de copia de seguridad de máquinas virtuales existente, siga estos pasos:

1. Ejecute el comando [az backup policy show](https://docs.microsoft.com/cli/azure/backup/policy#az_backup_policy_show) para recuperar los detalles de la directiva que quiere actualizar.

    Ejemplo:

    ```azurecli
    az backup policy show --name testing123 --resource-group rg1234 --vault-name testvault
    ```

    En el ejemplo anterior se muestran los detalles de una directiva de máquinas virtuales denominada *testing123*.

    Salida:

    ```json
    {
    "eTag": null,
    "id": "/Subscriptions/efgsf-123-test-subscription/resourceGroups/rg1234/providers/Microsoft.RecoveryServices/vaults/testvault/backupPolicies/testing123",
    "location": null,
    "name": "testing123",
    "properties": {
        "backupManagementType": "AzureIaasVM",
        "instantRpDetails": {
        "azureBackupRgNamePrefix": null,
        "azureBackupRgNameSuffix": null
        },
        "instantRpRetentionRangeInDays": 2,
        "protectedItemsCount": 0,
        "retentionPolicy": {
        "dailySchedule": {
            "retentionDuration": {
            "count": 180,
            "durationType": "Days"
            },
            "retentionTimes": [
            "2020-08-03T04:30:00+00:00"
            ]
        },
        "monthlySchedule": null,
        "retentionPolicyType": "LongTermRetentionPolicy",
        "weeklySchedule": {
            "daysOfTheWeek": [
            "Sunday"
            ],
            "retentionDuration": {
            "count": 30,
            "durationType": "Weeks"
            },
            "retentionTimes": [
            "2020-08-03T04:30:00+00:00"
            ]
        },
        "yearlySchedule": null
        },
        "schedulePolicy": {
        "schedulePolicyType": "SimpleSchedulePolicy",
        "scheduleRunDays": null,
        "scheduleRunFrequency": "Daily",
        "scheduleRunTimes": [
            "2020-08-03T04:30:00+00:00"
        ],
        "scheduleWeeklyFrequency": 0
        },
        "timeZone": "UTC"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupPolicies"
    }
    ```

1. Guarde la salida anterior en un archivo .json. Por ejemplo, guárdela como *Policy.json*.
1. Actualice el archivo JSON en función de sus requisitos y guarde los cambios.

    Ejemplo:  Para actualizar la retención semanal a 60 días, actualice la siguiente sección del archivo JSON cambiando el recuento a 60.

    ```json
            "retentionDuration": {
          "count": 60,
          "durationType": "Weeks"
        }

    ```

1. Guarde los cambios.
1. Ejecute el comando [az backup policy set](https://docs.microsoft.com/cli/azure/backup/policy#az_backup_policy_set) y pase la ruta de acceso completa del archivo JSON actualizado como valor del parámetro **- - policy**.

    ```azurecli
    az backup policy set --resource-group rg1234 --vault-name testvault --policy C:\temp2\Policy.json --name testing123
    ```

>[!NOTE]
>También puede recuperar la directiva JSON de ejemplo ejecutando el comando [az backup policy get-default-for-vm](https://docs.microsoft.com/cli/azure/backup/policy#az_backup_policy_get_default_for_vm).

## <a name="next-steps"></a>Pasos siguientes

- [Administración de las copias de seguridad de máquina virtual de Azure con el servicio Azure Backup](backup-azure-manage-vms.md)
