---
title: Cómo examinar Azure Files
description: En esta guía se describen los detalles de cómo examinar Azure Files.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/01/2020
ms.openlocfilehash: a0bd7a4cd8afafc16f05b4a37cd5723304ad931e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96551261"
---
# <a name="register-and-scan-azure-files"></a>Registro y examen de Azure Files

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Azure Files admite exámenes completos e incrementales para capturar los metadatos y aplicar clasificaciones en los mismos, en función de las clasificaciones del sistema y del cliente.

## <a name="prerequisites"></a>Requisitos previos

- Antes de registrar los orígenes de datos, cree una cuenta de Azure Purview. Para obtener más información sobre cómo crear una cuenta de Purview, consulte [Inicio rápido: creación de una cuenta de Azure Purview](create-catalog-portal.md).
- Tenga en cuenta que debe ser el administrador de los orígenes de datos para poder configurar y programar exámenes. Consulte los [Permisos del catálogo](catalog-permissions.md) para obtener más información.

## <a name="register-an-azure-files-storage-account"></a>Registro de una cuenta de Azure Files

Para registrar una nueva cuenta Azure Files en el catálogo de datos, haga lo siguiente:

1. Vaya a Purview Data Catalog.
1. En el panel de navegación izquierdo, seleccione **Management Center** (Centro de administración).
1. En **Orígenes de datos**, seleccione **Orígenes y exámenes**.
1. Seleccione **+ Nuevo**.
1. En **Registrar orígenes**, seleccione **Azure Files**. Seleccione **Continuar**.

:::image type="content" source="media/register-scan-azure-files/register-new-data-source.png" alt-text="registrar un nuevo origen de datos" border="true":::

En la pantalla **Registrar orígenes (Azure Files)** , haga lo siguiente:

1. Escriba un **nombre** con el que se mostrará el origen de datos en el catálogo.
1. Elija cómo quiere apuntar a la cuenta de almacenamiento que seleccione:
   1. Seleccione **From Azure subscription** (Desde una suscripción de Azure), seleccione la suscripción adecuada en el cuadro desplegable **Suscripción de Azure** y la cuenta de almacenamiento correspondiente en el cuadro desplegable **Storage account name** (Nombre de la cuenta de almacenamiento).
   1. O bien, puede seleccionar **Escribir manualmente** y especificar un punto de conexión de servicio (URL).
1. **Finalice** el proceso para registrar el origen de datos.

:::image type="content" source="media/register-scan-azure-files/register-sources.png" alt-text="opciones de registro de orígenes" border="true":::

## <a name="set-up-authentication-for-a-scan"></a>Configuración de la autenticación para un examen

Para configurar la autenticación del almacenamiento Azure Files mediante una clave de cuenta, haga lo siguiente:

1. Seleccione el método de autenticación como **Clave de cuenta**.
2. Seleccione la opción **From Azure subscription** (Desde la suscripción de Azure).
3. Elija la suscripción de Azure en la que tenga la cuenta de Azure Files.
4. Seleccione el nombre de la cuenta de almacenamiento en la lista.
5. Haga clic en **Finalizar**

[!INCLUDE [create and manage scans](includes/manage-scans.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Examen del catálogo de datos de Azure Purview](how-to-browse-catalog.md)
- [Búsqueda en el catálogo de datos de Azure Purview](how-to-search-catalog.md)