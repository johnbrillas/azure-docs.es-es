---
title: Creación y administración de credenciales para exámenes
description: En este artículo se explican los pasos para crear y administrar credenciales en Azure Purview.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/23/2020
ms.openlocfilehash: 30004306d9ff44df04a26640a2bd7a09256fce25
ms.sourcegitcommit: 66479d7e55449b78ee587df14babb6321f7d1757
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2020
ms.locfileid: "97516690"
---
# <a name="credentials-for-source-authentication-in-azure-purview"></a>Credenciales para la autenticación de origen en Azure Purview

En este artículo se describe cómo se pueden crear credenciales en Azure Purview para reutilizar y aplicar rápidamente la información de autenticación guardada a los exámenes de orígenes de datos.

## <a name="prerequisites"></a>Requisitos previos

* Un almacén de claves de Azure. Para aprender a crear una, consulte el [Inicio rápido: Creación de un almacén de claves mediante Azure Portal](../key-vault/general/quick-create-portal.md).

## <a name="introduction"></a>Introducción
Una credencial es información de autenticación que Azure Purview usa para la autenticación en los orígenes de datos registrados. Se puede crear un objeto de credencial para varios tipos de escenarios de autenticación (como la autenticación básica con nombre de usuario y contraseña). Capturará la información específica necesaria según el tipo de método de autenticación elegido. Las credenciales usan los secretos existentes de Azure Key Vault para recuperar información confidencial de autenticación durante el proceso de creación de la credencial.

## <a name="using-purview-managed-identity-to-set-up-scans"></a>Uso de la identidad administrada de Purview para configurar exámenes
Si usa la identidad administrada de Purview para configurar exámenes, no tendrá que crear explícitamente una credencial y vincular el almacén de claves a Purview para almacenarlos. Para instrucciones detalladas sobre cómo agregar la identidad administrada de Purview para acceder y examinar orígenes de datos, consulte las secciones de autenticación específicas para los orígenes de datos siguientes:

- [Azure Blob Storage](register-scan-azure-blob-storage-source.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen2](register-scan-adls-gen2.md#setting-up-authentication-for-a-scan)
- [Azure SQL Database](register-scan-azure-sql-database.md)
- [Instancia administrada de Azure SQL Database](register-scan-azure-sql-database-managed-instance.md#setting-up-authentication-for-a-scan)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#setting-up-authentication-for-a-scan)

## <a name="create-azure-key-vaults-connections-in-your-azure-purview-account"></a>Creación de conexiones de Azure Key Vault en la cuenta de Azure Purview

Para crear una credencial, primero debe asociar una o varias de las instancias de Azure Key Vault existentes a su cuenta de Azure Purview.

1. En el menú de navegación de Azure Purview, vaya al centro de administración y, después, a las credenciales.

2. En la barra de comandos de Credentials (Credenciales), seleccione Manage Key Vault connections (Administrar conexiones de Key Vault)

    :::image type="content" source="media/manage-credentials/manage-kv-connections.png" alt-text="Administración de las conexiones de AKV.":::

3. Seleccione + New (+ Nuevo) en el panel Manage Key Vault connections (Administrar conexiones de Key Vault). 

4. Rellene la información necesaria y seleccione Create (Crear).

5. Confirme que el almacén de claves se ha asociado correctamente a la cuenta de Azure Purview.

    :::image type="content" source="media/manage-credentials/view-kv-connections.png" alt-text="Visualización de las conexiones de AKV.":::

## <a name="grant-the-purview-managed-identity-access-to-your-azure-key-vault"></a>Concesión de acceso a la identidad administrada de Purview a la instancia de Azure Key Vault

Vaya al almacén de claves -> Directivas de acceso -> Agregar directiva de acceso. Conceda el permiso Obtener de la lista desplegable Permisos de los secretos y establezca Seleccionar la entidad de seguridad en Purview MSI. 

:::image type="content" source="media/manage-credentials/add-msi-to-akv.png" alt-text="Incorporación de Purview MSI a AKV.":::


:::image type="content" source="media/manage-credentials/add-access-policy.png" alt-text="Agregar directiva de acceso":::


:::image type="content" source="media/manage-credentials/save-access-policy.png" alt-text="Guardado de la directiva de acceso.":::

## <a name="create-a-new-credential"></a>Creación de una credencial

Tipos de credenciales que admite Purview a día de hoy:
* Autenticación básica: agregará la **contraseña** como secreto al almacén de claves.
* Entidad de servicio: agregará la **clave de entidad de servicio** como secreto al almacén de claves. 
* Autenticación de SQL: agregará la **contraseña** como secreto al almacén de claves.
* Clave de cuenta: agregará la **clave de cuenta** como secreto al almacén de claves.

Aquí encontrará más información sobre cómo agregar secretos a un almacén de claves: (insertar artículo sobre el almacén de claves).

Después de almacenar los secretos en el almacén de claves, ya puede crear la nueva credencial; para ello, seleccione + New (+ Nuevo) en la barra de comandos de Credentials (Credenciales). Proporcione la información necesaria, incluida la selección del método de autenticación y la instancia de Key Vault de donde seleccionar el secreto. Una vez rellenos todos los detalles, haga clic en Create (Crear).

:::image type="content" source="media/manage-credentials/new-credential.png" alt-text="Nueva credencial.":::

Compruebe que la nueva credencial aparece en la vista de la lista de credenciales y está lista para usarse.

:::image type="content" source="media/manage-credentials/view-credentials.png" alt-text="Visualización de la credencial.":::

## <a name="manage-your-key-vault-connections"></a>Administración de las conexiones del almacén de claves

1. Busque las conexiones del almacén de claves por el nombre.

    :::image type="content" source="media/manage-credentials/search-kv.png" alt-text="Búsqueda del almacén de claves":::

1. Elimine una o más conexiones de almacén de claves.
 
    :::image type="content" source="media/manage-credentials/delete-kv.png" alt-text="Eliminar el almacén de claves":::

## <a name="manage-your-credentials"></a>Administración de las credenciales

1. Busque las credenciales por el nombre.
  
2. Seleccione y actualice una credencial existente.

3. Elimine una o varias credenciales.
