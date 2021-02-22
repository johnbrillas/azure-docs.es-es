---
title: Solución de problemas de las conexiones en Azure Purview
description: En este artículo se explican los pasos para solucionar problemas con las conexiones en Azure Purview.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/23/2020
ms.openlocfilehash: db3954b0368392d63680a8d92f13d6a0423e8ef5
ms.sourcegitcommit: e3151d9b352d4b69c4438c12b3b55413b4565e2f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2021
ms.locfileid: "100526891"
---
# <a name="troubleshoot-your-connections-in-azure-purview"></a>Solución de problemas de las conexiones en Azure Purview

En este artículo se describe cómo solucionar errores de conexión durante la configuración de exámenes en orígenes de datos en Azure Purview.

## <a name="permission-the-credential-on-the-data-source"></a>Permiso de la credencial en el origen de datos

Si usa una identidad administrada o una entidad de servicio como método de autenticación para los exámenes, deberá permitir que estas identidades tengan acceso a su origen de datos.

Existen instrucciones específicas para cada tipo de origen:

- [Azure Blob Storage](register-scan-azure-blob-storage-source.md#setting-up-authentication-for-a-scan)
- [Azure Cosmos DB](register-scan-azure-cosmos-database.md#setting-up-authentication-for-a-scan)
- [Azure Data Explorer](register-scan-azure-data-explorer.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen2](register-scan-adls-gen2.md#setting-up-authentication-for-a-scan)
- [Azure SQL Database](register-scan-azure-sql-database.md)
- [Instancia administrada de Azure SQL Database](register-scan-azure-sql-database-managed-instance.md#setting-up-authentication-for-a-scan)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#setting-up-authentication-for-a-scan)
- [SQL Server](register-scan-on-premises-sql-server.md#setting-up-authentication-for-a-scan)
- [Power BI](register-scan-power-bi-tenant.md)

## <a name="storing-your-credential-in-your-key-vault-and-using-the-right-secret-name-and-version"></a>Almacenamiento de la credencial en el almacén de claves y uso del nombre y la versión del secreto correctos

También debe almacenar la credencial en la instancia de Azure Key Vault y usar el nombre y la versión del secreto correctos.

Compruebe si es así mediante los pasos siguientes:

1. Vaya a su instancia de Key Vault.
1. Seleccione **Configuración** > **Secretos**.
1. Seleccione el secreto que va a usar para autenticarse en el origen de datos para realizar exámenes.
1. Seleccione la versión que desea usar y compruebe que la contraseña o la clave de la cuenta sean correctas. Para ello, haga clic en **Mostrar el valor secreto**. 

## <a name="verify-permissions-for-the-purview-managed-identity-on-your-azure-key-vault"></a>Compruebe los permisos para la identidad administrada de Purview en la instancia de Azure Key Vault

Compruebe que se hayan configurado los permisos correctos para que la identidad administrada de Purview acceda a la instancia de Azure Key Vault.

Para ello, siga estos pasos:

1. En el almacén de claves, vaya a la sección **Directivas de acceso**.

1. Compruebe que la identidad administrada de Purview se muestra en la sección *Directivas de acceso actuales* con los permisos **Obtener** y **Enumerar** como mínimo en Secretos.

   :::image type="content" source="./media/troubleshoot-connections/verify-minimum-permissions.png" alt-text="Imagen que muestra la selección desplegable de las opciones de permiso Obtener y Enumerar":::

Si no ve la identidad administrada de Purview, siga los pasos descritos en [Creación y administración de credenciales para exámenes](manage-credentials.md) para agregarla. 

## <a name="next-steps"></a>Pasos siguientes

- [Examen del catálogo de datos de Azure Purview](how-to-browse-catalog.md)
- [Búsqueda en el catálogo de datos de Azure Purview](how-to-search-catalog.md)
