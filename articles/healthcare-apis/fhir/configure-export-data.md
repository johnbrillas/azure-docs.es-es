---
title: Definición de la configuración de exportación en Azure API for FHIR
description: En este artículo se describe cómo definir la configuración de exportación en Azure API for FHIR
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 3/18/2021
ms.author: matjazl
ms.openlocfilehash: ee110420c697afb6ecad857ba823c61d03c6be6c
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2021
ms.locfileid: "105046998"
---
# <a name="configure-export-setting-and-set-up-the-storage-account"></a>Definición de la configuración de la exportación y de la configuración de la cuenta de almacenamiento

Azure API for FHIR admite el comando $export, que le permite exportar los datos de la cuenta de Azure API for FHIR a una cuenta de almacenamiento.

Para configurar la exportación en Azure API for FHIR es necesario realizar tres pasos:

1. Habilite la identidad administrada en la API de Azure para el servicio FHIR.
2. Creación de una cuenta de Azure Storage (si no se ha hecho antes) y asignación de permisos a la API de Azure para FHIR a la cuenta de almacenamiento.
3. Selección de la cuenta de almacenamiento en la API de Azure para FHIR como cuenta de almacenamiento de exportación.

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Habilitación de la identidad administrada en Azure API for FHIR

El primer paso para configurar Azure API for FHIR para la exportación consiste en habilitar la identidad administrada en todo el sistema en el servicio. Para obtener más información sobre las identidades administradas en Azure, consulte [acerca de las identidades administradas para los recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md).

Para ello, vaya a la API de Azure para el servicio FHIR y seleccione **Identity (identidad**). Al cambiar el estado a **activado** , se habilitará la identidad administrada en la API de Azure para el servicio FHIR.

![Habilitación de una entidad administrada](media/export-data/fhir-mi-enabled.png)

Ahora, puede pasar al siguiente paso creando una cuenta de almacenamiento y asignando permiso a nuestro servicio.

## <a name="adding-permission-to-storage-account"></a>Concesión de permisos a la cuenta de almacenamiento

El siguiente paso de la exportación es asignar el permiso para que la API de Azure para el servicio FHIR escriba en la cuenta de almacenamiento.

Después de crear una cuenta de almacenamiento, vaya a **Access Control (IAM)** en la cuenta de almacenamiento y seleccione **Agregar asignación de roles**.

![Exportación de asignaciones de rol](media/export-data/fhir-export-role-assignment.png)

Aquí está aquí para agregar el **colaborador de datos de BLOB de almacenamiento** de rol a nuestro nombre de servicio y, a continuación, seleccione **Guardar**.

![Agregar rol](media/export-data/fhir-export-role-add.png)

Ahora está listo para seleccionar la cuenta de almacenamiento en la API de Azure para FHIR como cuenta de almacenamiento predeterminada para $export.

## <a name="selecting-the-storage-account-for-export"></a>Selección de la cuenta de almacenamiento para el comando $export

El paso final consiste en asignar la cuenta de Azure Storage que la API de Azure para FHIR usará para exportar los datos a. Para ello, vaya a **integración** en la API de Azure para el servicio FHIR y seleccione la cuenta de almacenamiento.

![Almacenamiento de las exportaciones de FHIR](media/export-data/fhir-export-storage.png)

Después de completar este paso final, ya está listo para exportar los datos mediante $export comando.

> [!Note]
> Solo las cuentas de almacenamiento de la misma suscripción que para la API de Azure para FHIR se pueden registrar como destino de las operaciones de $export.

Para obtener más información sobre cómo configurar las opciones de base de datos, el control de acceso, la habilitación del registro de diagnóstico y el uso de encabezados personalizados para agregar datos a los registros de auditoría, vea:

>[!div class="nextstepaction"]
>[Configuración adicional](azure-api-for-fhir-additional-settings.md)
