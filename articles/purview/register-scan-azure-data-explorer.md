---
title: Cómo examinar Azure Data Explorer
description: En esta guía se describen los detalles de cómo analizar Azure Data Explorer.
author: nayenama
ms.author: nayenama
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/9/2020
ms.openlocfilehash: 01a1ded570d20d175b5e8eadb3e6cc8556155a85
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96551274"
---
# <a name="register-and-scan-azure-data-explorer"></a>Registro y examen de Azure Data Explorer

En este artículo se describe cómo registrar una cuenta de Azure Data Explorer en Azure Purview y cómo configurar un examen.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Azure Data Explorer admite exámenes completos e incrementales para capturar los metadatos y el esquema. Los exámenes también clasifican los datos automáticamente en función de las reglas de clasificación personalizadas y del sistema.

## <a name="prerequisites"></a>Requisitos previos

- Antes de registrar los orígenes de datos, cree una cuenta de Azure Purview. Para obtener más información sobre cómo crear una cuenta de Purview, consulte [Inicio rápido: creación de una cuenta de Azure Purview](create-catalog-portal.md).
- Tenga en cuenta que debe ser administrador de los orígenes de datos de Azure Purview.

## <a name="setting-up-authentication-for-a-scan"></a>Configuración de la autenticación para un examen

Solo hay una manera de configurar la autenticación de Azure Data Explorer:

- Entidad de servicio

### <a name="service-principal"></a>Entidad de servicio

Para usar la autenticación de entidad de servicio para realizar exámenes, puede usar una existente o crear una nuevo. 

> [!Note]
> Si tiene que crear una entidad de servicio nueva, siga estos pasos:
> 1. Acceda a [Azure Portal](https://portal.azure.com).
> 1. Seleccione **Azure Active Directory** en el menú de la izquierda.
> 1. Seleccione **App registrations** (Registros de aplicaciones).
> 1. Seleccione **+ Nuevo registro de aplicaciones**.
> 1. Escriba un nombre para la **aplicación** (el nombre de la entidad de servicio).
> 1. Seleccione **Solo las cuentas de este directorio organizativo**.
> 1. En el URI de redirección, seleccione **Web** y escriba cualquier dirección URL; no tiene que ser real ni del trabajo.
> 1. Después, seleccione **Register** (Registrar).

Es necesario obtener el id. de aplicación y el secreto de la entidad de servicio:

1. Vaya a la entidad de servicio en [Azure Portal](https://portal.azure.com).
1. Copie los valores del **id. de aplicación (cliente)** de la **Información general** y el **Secreto de cliente** que están en **Certificados y secretos**.
1. Vaya a almacén de claves.
1. Seleccione **Configuración > Secretos**.
1. Seleccione **+ Generar/Importar** y escriba el **nombre** que quiera y el **valor** como **Secreto de cliente** de la entidad de servicio.
1. Seleccione **Crear** para completar la acción.
1. Si el almacén de claves no está conectado todavía a Purview, necesitará [crear una nueva conexión del almacén de claves](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account).
1. Por último, [cree una nueva credencial](manage-credentials.md#create-a-new-credential) mediante la entidad de servicio para configurar el examen.

#### <a name="granting-the-service-principal-access-to-your-azure-data-explorer-instance"></a>Acceso de la entidad de servicio a la instancia de Azure Data Explorer

1. Acceda a Azure Portal. A continuación, vaya a la instancia de Azure Data Explorer.

1. Agregue la entidad de servicio al rol **AllDatabasesViewer** en la pestaña **Permisos**, tal como se muestra en la captura de pantalla siguiente.

    :::image type="content" source="./media/register-scan-azure-data-explorer/permissions-auth.png" alt-text="Captura de pantalla para agregar la entidad de servicio en los permisos" border="true":::

## <a name="register-an-azure-data-explorer-account"></a>Registro de una cuenta de Azure Data Explorer

Para registrar una nueva cuenta de Azure Data Explorer (Kusto) en el catálogo de datos, haga lo siguiente:

1. Vaya a la cuenta de Purview.
1. Seleccione **Orígenes** en el panel de navegación izquierdo.
1. Seleccione **Registrar**.
1. En **Registrar orígenes**, seleccione **Azure Data Explorer**.
1. Seleccione **Continuar**

:::image type="content" source="media/register-scan-azure-data-explorer/register-new-data-source.png" alt-text="registrar un nuevo origen de datos" border="true":::

En la pantalla **Registrar orígenes (Azure Data Explorer [Kusto])** , haga lo siguiente:

1. Escriba un **nombre** con el que se mostrará el origen de datos en el catálogo.
1. Elija cómo quiere apuntar a la cuenta de almacenamiento que seleccione:
   1. Seleccione **From Azure subscription** (Desde una suscripción de Azure), seleccione la suscripción adecuada en el cuadro desplegable **Suscripción de Azure** y el clúster correspondiente en el cuadro desplegable **Clúster**.
   1. O bien, puede seleccionar **Escribir manualmente** y especificar un punto de conexión de servicio (URL).
1. **Finalice** el proceso para registrar el origen de datos.

:::image type="content" source="media/register-scan-azure-data-explorer/register-sources.png" alt-text="opciones de registro de orígenes" border="true":::

[!INCLUDE [create and manage scans](includes/manage-scans.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Examinar el catálogo de datos de Azure Purview](how-to-browse-catalog.md)
- [Búsqueda en Azure Purview Data Catalog](how-to-search-catalog.md)
