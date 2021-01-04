---
title: Examen de Azure Cosmos Database (SQL API)
description: En esta guía se describen los detalles de cómo examinar Azure Cosmos Database (SQL API).
author: djpmsft
ms.author: daperlov
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/9/2020
ms.openlocfilehash: 1aaeed1973ebd15af312b722ab61938aa4271947
ms.sourcegitcommit: b6267bc931ef1a4bd33d67ba76895e14b9d0c661
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696243"
---
# <a name="register-and-scan-azure-cosmos-database-sql-api"></a>Registro y examen de Azure Cosmos Database (SQL API)

En este artículo se describe cómo registrar una cuenta de Azure Cosmos Database (SQL API) en Azure Purview y cómo configurar un examen.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Azure Cosmos Database (SQL API) admite exámenes completos e incrementales para capturar los metadatos y el esquema. Los exámenes también clasifican los datos automáticamente en función de las reglas de clasificación personalizadas y del sistema.

## <a name="prerequisites"></a>Requisitos previos

- Antes de registrar los orígenes de datos, cree una cuenta de Azure Purview. Para más información sobre cómo crear una cuenta de Purview, consulte [Inicio rápido: Creación de una cuenta de Azure Purview en Azure Portal](create-catalog-portal.md).
- Tenga en cuenta que debe ser Administrador de orígenes de datos de Azure Purview.

## <a name="setting-up-authentication-for-a-scan"></a>Configuración de la autenticación para un examen

Solo hay una manera de configurar la autenticación de Azure Cosmos Database (SQL API):

- Clave de cuenta
 
### <a name="account-key"></a>Clave de cuenta

Cuando el método de autenticación seleccionado es **Clave de cuenta**, debe obtener la clave de acceso y almacenarla en el almacén de claves:

1. Vaya a la cuenta de Cosmos DB en Azure Portal. 
1. Seleccione **Configuración** > **Claves**. 
1. Copie la *clave* y guárdela para usarla en los pasos siguientes.
1. Vaya a almacén de claves.
1. Seleccione **Configuración > Secretos**.
1. Seleccione **+ Generar/Importar** y escriba el **nombre** y el **valor** como la *clave* de la cuenta de almacenamiento.
1. Seleccione **Crear** para completar la acción.
1. Si el almacén de claves no está conectado todavía a Purview, necesitará [crear una conexión del almacén de claves](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account).
1. Por último, [cree una nueva credencial](manage-credentials.md#create-a-new-credential) mediante la clave para configurar el examen.

## <a name="register-an-azure-cosmos-database-sql-api-account"></a>Registro de una cuenta de Azure Cosmos Database (SQL API)

Para registrar una nueva cuenta de Azure Cosmos Database (SQL API) en el catálogo de datos, haga lo siguiente:

1. Vaya a la cuenta de Purview.
1. Seleccione **Sources** (Orígenes) en el panel de navegación izquierdo.
1. Seleccione **Registrar**.
1. En **Register sources** (Registrar orígenes), seleccione **Azure Cosmos DB (SQL API)** .
1. Seleccione **Continuar**

:::image type="content" source="media/register-scan-azure-cosmos-database/register-new-data-source.png" alt-text="Registro de un origen de datos nuevo" border="true":::

En la pantalla **Register sources (Azure Cosmos DB (SQL API))** (Registrar orígenes [Azure Cosmos DB (SQL API)]), haga lo siguiente:

1. Escriba un **nombre** con el que se mostrará el origen de datos en el catálogo.
1. Elija cómo quiere apuntar a la cuenta de almacenamiento elegida:
   1. Seleccione **From Azure subscription** (Desde la suscripción de Azure), seleccione la suscripción adecuada en el cuadro desplegable **Azure subscription** (Suscripción de Azure) y la cuenta de Cosmos DB correspondiente en el cuadro desplegable **Cosmos DB account name** (Nombre de la cuenta de Cosmos DB).
   1. O bien, puede seleccionar **Enter manually** (Escribir manualmente) y especificar un punto de conexión de servicio (dirección URL).
1. Seleccione **Finish** (Finalizar) para registrar el origen de datos.

:::image type="content" source="media/register-scan-azure-cosmos-database/register-sources.png" alt-text="Opciones de registro de orígenes" border="true":::


[!INCLUDE [create and manage scans](includes/manage-scans.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Examen del catálogo de datos de Azure Purview](how-to-browse-catalog.md)
- [Búsqueda en el catálogo de datos de Azure Purview](how-to-search-catalog.md)
