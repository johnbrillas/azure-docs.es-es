---
title: Examen de orígenes de Azure Multiple Source
description: Aprenda a examinar una suscripción o un grupo de recursos completo de Azure en el catálogo de datos de Azure Purview.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 2/26/2021
ms.openlocfilehash: 098f62365971fd634001706ab99fd414a6b25056
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102123571"
---
# <a name="register-and-scan-azure-multiple-sources"></a>Registro y examen de orígenes de Azure Multiple Source

En este artículo se explica cómo registrar un origen de Azure Multiple Source (suscripciones o grupos de recursos de Azure) en Purview y cómo configurar un examen en él.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Azure Multiple Source admite los exámenes para capturar metadatos y esquemas en la mayoría de los tipos de recursos de Azure compatibles con Purview. También clasifica los datos automáticamente en función de las reglas de clasificación personalizadas y del sistema.

## <a name="prerequisites"></a>Requisitos previos

- Antes de registrar los orígenes de datos, cree una cuenta de Azure Purview. Para más información sobre cómo crear una cuenta de Purview, consulte [Inicio rápido: creación de una cuenta de Azure Purview](create-catalog-portal.md).
- Tenga en cuenta que debe ser administrador de los orígenes de datos de Azure Purview.
- Configure la autenticación como se explica en las secciones siguientes.

### <a name="setting-up-authentication-for-enumerating-resources-under-a-subscription-or-resource-group"></a>Configuración de la autenticación para enumerar recursos en una suscripción o un grupo de recursos

1. Vaya a la suscripción o el grupo de recursos en Azure Portal.  
1. Seleccione  **Control de acceso (IAM)**   en el menú de navegación izquierdo. 
1. Debe ser propietario o administrador de acceso de usuarios para agregar un rol a la suscripción o al grupo de recursos. Seleccione el botón *+Agregar*. 
1. Establezca el rol **Lector** y escriba el nombre de la cuenta de Azure Purview (que representa su MSI) en el cuadro de entrada Select (Seleccionar). Haga clic en *Save* (Guardar) para finalizar la asignación de roles.

### <a name="setting-up-authentication-to-scan-resources-under-a-subscription-or-resource-group"></a>Configuración de la autenticación para examinar recursos en una suscripción o un grupo de recursos

Hay dos maneras de configurar la autenticación de Azure Multiple Source:

- Identidad administrada
- Entidad de servicio

> [!NOTE]
> Debe configurar la autenticación en cada recurso de la suscripción o el grupo de recursos que quiere registrar y examinar. Los tipos de recursos de Azure Storage (Azure Blob Storage y Azure Data Lake Storage Gen2) facilitan la tarea de agregar el MSI o la entidad de servicio en el nivel de suscripción o grupo de recursos como lector de datos de Blob Storage. Los permisos se filtran a cada cuenta de almacenamiento dentro de esa suscripción o grupo de recursos. En el caso de todos los demás tipos de recursos, debe aplicar el MSI o la entidad de servicio en cada recurso, o bien desarrollar un script para hacerlo. Aquí se muestra cómo agregar permisos en cada tipo de recurso de una suscripción o un grupo de recursos.
    
- [Azure Blob Storage](register-scan-azure-blob-storage-source.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen2](register-scan-adls-gen2.md#setting-up-authentication-for-a-scan)
- [Azure SQL Database](register-scan-azure-sql-database.md)
- [Instancia administrada de Azure SQL Database](register-scan-azure-sql-database-managed-instance.md#setting-up-authentication-for-a-scan)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#setting-up-authentication-for-a-scan)
 
## <a name="register-an-azure-multiple-source"></a>Registro de un origen de Azure Multiple Source

Para registrar un nuevo origen de Azure Multiple Source en el catálogo de datos, haga lo siguiente:

1. Vaya a la cuenta de Purview.
1. Seleccione **Sources** (Orígenes) en el panel de navegación izquierdo.
1. Seleccione **Registrar**.
1. En **Register sources** (Registrar orígenes), seleccione **Azure (Multiple)** .
1. Seleccione **Continuar**

   :::image type="content" source="media/register-scan-azure-multiple-sources/register-azure-multiple.png" alt-text="Registro de un origen de Azure Multiple Source":::

En la pantalla **Register sources (Azure multiple)** (Registrar orígenes (Azure Multiple Source)), haga lo siguiente:

1. Escriba un **Name** (Nombre) con el que se muestre el origen de datos en el catálogo. 
1. Opcionalmente, seleccione un **Management group** (Grupo de administración) para filtrar.
1. **Seleccione una suscripción o un grupo de recursos concreto** en una suscripción determinada de la lista desplegable. El ámbito de registro se establece en la suscripción o el grupo de recursos seleccionados.  
1. Seleccione una **Collection** (Colección) o cree una nueva (opcional).
1. Seleccione **Finish** (Finalizar) para registrar el origen de datos.

   :::image type="content" source="media/register-scan-azure-multiple-sources/azure-multiple-source-setup.png" alt-text="Configuración de Azure Multiple Source":::

## <a name="creating-and-running-a-scan"></a>Creación y ejecución de un examen

Para crear y ejecutar un nuevo examen, siga estos pasos:

1. Vaya a la sección **Sources** (Orígenes).

1. Seleccione el origen de datos que ha registrado.

1. Haga clic en View details (Ver detalles) y seleccione **+ New scan** (+ Nuevo examen) o use el icono de acción rápida en el icono del origen.

1. Rellene el *Name* (Nombre) y seleccione todos los tipos de recursos que quiere examinar en este origen.

    1. Puede dejarlo como *All* (Todos). (Esto incluye tipos de recursos futuros que pueden no existir actualmente en esa suscripción o ese grupo de recursos).
    1. Puede **seleccionar específicamente los tipos de recursos** que quiere examinar. Si elige esta opción, los tipos de recursos futuros que puedan crearse en esta suscripción o este grupo de recursos no se van a incluir en los exámenes, a menos que el examen se edite explícitamente en el futuro.
    
    :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-source-scan.png" alt-text="Examen de Azure Multiple Source":::

1. Seleccione la credencial para conectarse a los recursos del origen de datos. 
    1. Puede seleccionar una **credencial en el nivel primario** como MSI o una credencial de tipo de entidad de servicio determinada, que puede usar para todos los tipos de recursos de la suscripción o el grupo de recursos.
    1. También puede **seleccionar el tipo de recurso específicamente y aplicar otra credencial** para ese tipo de recurso.
    1. Cada credencial se considera el método de autenticación de todos los recursos de un tipo determinado.
    1. Debe establecer la credencial elegida en los recursos para examinarlos correctamente, como se explica en la [sección](#setting-up-authentication-to-scan-resources-under-a-subscription-or-resource-group) anterior.
1. En cada tipo puede optar por examinar todos los recursos o un subconjunto de ellos por nombre.
    1. Si deja la opción como **All** (Todos), los recursos futuros de ese tipo también se examinarán en próximas ejecuciones de exámenes.
    1. Si selecciona cuentas de almacenamiento o bases de datos SQL específicas, los recursos futuros creados de ese tipo en esta suscripción o grupo de recursos no se incluirán en los exámenes, a menos que el examen se edite de manera explícita en el futuro.
 
1.  Haga clic en **Continuar** para seguir. Vamos a probar el acceso para comprobar si se ha aplicado el MSI de Purview como lector en la suscripción o el grupo de recursos. Si aparece un mensaje de error, siga las instrucciones que se indican [aquí](#setting-up-authentication-for-enumerating-resources-under-a-subscription-or-resource-group).

1.  Seleccione los **conjuntos de reglas de examen** de cada tipo de recurso elegido en el paso anterior. También puede crear conjuntos de reglas de examen insertados.
  :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-scan-rule-set.png" alt-text="Selección de conjunto de reglas de examen de Azure Multiple":::

1. Elija el desencadenador del examen. Puede programarlo para que se ejecute **semanalmente/mensualmente** o **una vez**.

1. Revise el examen y seleccione Save (Guardar) para completar la configuración.   

## <a name="viewing-your-scans-and-scan-runs"></a>Visualización de los exámenes y las ejecuciones de exámenes

1. Para ver los detalles del origen, haga clic en **View details** (Ver detalles) en el icono de la sección de orígenes. 

      :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-source-detail.png" alt-text="Detalles de Azure Multiple Source"::: 

1. Para ver los detalles de la ejecución del examen, vaya a la página **Scan details** (Detalles del examen).
    1. La *barra de estado* es un breve resumen sobre el estado de ejecución de los recursos secundarios. Aparece en el nivel de suscripción o de grupo de recursos.
    1. El color verde significa que se ha realizado correctamente, mientras que el rojo significa error. El gris significa que el examen sigue su curso.
    1. Puede hacer clic en cada examen para ver detalles más precisos.

      :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-scan-full-details.png" alt-text="Detalles de examen de Azure Multiple":::

1. Vea un resumen de las ejecuciones de exámenes recientes con errores en la parte inferior de la página de detalles del origen. También puede hacer clic para ver detalles más precisos relacionados con estas ejecuciones.

## <a name="manage-your-scans---edit-delete-or-cancel"></a>Administración de exámenes: editar, eliminar o cancelar
Para administrar o eliminar un examen, haga lo siguiente:

- Vaya al centro de administración. Seleccione Data sources (Orígenes de datos) en la sección Sources and scanning (Orígenes y exámenes) y luego el origen de datos deseado.

- Seleccione el examen que desea administrar. Seleccione Edit (Editar) para editar el examen.

- Seleccione Delete (Eliminar) para eliminar el examen.
- Si se está ejecutando un examen, también puede cancelarlo.

## <a name="next-steps"></a>Pasos siguientes

- [Examen del catálogo de datos de Azure Purview](how-to-browse-catalog.md)
- [Búsqueda en el catálogo de datos de Azure Purview](how-to-search-catalog.md)    
