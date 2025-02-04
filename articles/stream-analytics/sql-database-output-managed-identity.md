---
title: 'Uso de identidades administradas para acceder a Azure SQL Database o Azure Synapse Analytics: Azure Stream Analytics'
description: En este artículo se describe cómo usar identidades administradas para autenticar su trabajo de Azure Stream Analytics en la salida de Azure SQL Database o Azure Synapse Analytics.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: how-to
ms.date: 11/30/2020
ms.openlocfilehash: e491c421f4af256b2e74fa61eb442d269bdb9e34
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102487923"
---
# <a name="use-managed-identities-to-access-azure-sql-database-or-azure-synapse-analytics-from-an-azure-stream-analytics-job-preview"></a>Uso de identidades administradas para acceder a Azure SQL Database o Azure Synapse Analytics desde un trabajo de Azure Stream Analytics (versión preliminar)

Azure Stream Analytics admite la [autenticación de identidades administradas](../active-directory/managed-identities-azure-resources/overview.md) para los receptores de salida de Azure SQL Database y Azure Synapse Analytics. Las identidades administradas eliminan las limitaciones de los métodos de autenticación basada en el usuario, como la necesidad de volver a realizar la autenticación debido a los cambios de contraseña o la expiración de tokens de usuario que se produce cada 90 días. Cuando se elimina la necesidad de autenticarse manualmente, las implementaciones de Stream Analytics se pueden automatizar completamente.

Una identidad administrada es una aplicación administrada registrada en Azure Active Directory que representa un trabajo de Stream Analytics determinado. La aplicación administrada se usa para la autenticación en un recurso de destino. En este artículo se muestra cómo habilitar la identidad administrada para las salidas de Azure SQL Database o Azure Synapse Analytics de un trabajo de Stream Analytics a través de Azure Portal.

## <a name="overview"></a>Información general

En este artículo se muestran los pasos necesarios para conectar el trabajo de Stream Analytics al grupo de SQL de Azure SQL Database o Azure Synapse Analytics mediante el modo de autenticación de identidad administrada. 

- En primer lugar, debe crear una identidad administrada asignada por el sistema para el trabajo de Stream Analytics. Esta es la identidad del trabajo en Azure Active Directory.  

- Agregue un administrador de Active Directory al área de trabajo de SQL Server o Synapse, lo que habilita la autenticación de Azure AD (identidad administrada) para ese recurso.

- A continuación, cree un usuario independiente que represente la identidad del trabajo de Stream Analytics en la base de datos. Siempre que el trabajo de Stream Analytics interactúe con el recurso de base de datos SQL o Synapse SQL, esta será la identidad a la que hará referencia para comprobar los permisos que tiene el trabajo de Stream Analytics.

- Conceda permisos al trabajo de Stream Analytics para acceder a los grupos de SQL de Synapse o SQL Database.

- Por último, agregue la instancia de Azure SQL Database o Azure Synapse Analytics como salida en el trabajo de Stream Analytics.

## <a name="prerequisites"></a>Requisitos previos

#### <a name="azure-sql-database"></a>[Azure SQL Database](#tab/azure-sql)

Para usar esta característica se requiere lo siguiente:

- Un trabajo de Azure Stream Analytics.

- Un recurso de Azure SQL Database.

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

Para usar esta característica se requiere lo siguiente:

- Un trabajo de Azure Stream Analytics.

- Un grupo de SQL de Azure Synapse Analytics.

- Una cuenta de Azure Storage [configurada para el trabajo de Stream Analytics](azure-synapse-analytics-output.md).

- Nota: Actualmente no está disponible la integración de la MSI de la cuenta de almacenamiento de Stream Analytics con la MSI de Synapse SQL.

---

## <a name="create-a-managed-identity"></a>Creación de una entidad administrada

En primer lugar, debe crear una identidad administrada para el trabajo de Azure Stream Analytics.

1. En [Azure Portal](https://portal.azure.com), abra el trabajo de Azure Stream Analytics.

1. En el menú de navegación izquierdo, seleccione **Identidad administrada** en **Configurar**. A continuación, active la casilla situada junto a **Usar la identidad administrada asignada por el sistema** y seleccione **Guardar**.

   ![Selección de la identidad administrada asignada por el sistema](./media/sql-db-output-managed-identity/system-assigned-managed-identity.png)

   Se crea una entidad de servicio para la identidad del trabajo de Stream Analytics en Azure Active Directory. El ciclo de vida de la identidad recién creada lo administrará Azure. Cuando se elimina el trabajo de Stream Analytics, Azure elimina automáticamente la identidad asociada (es decir, la entidad de servicio).

1. Cuando se guarda la configuración, el id. de objeto (OID) de la entidad de servicio aparece como id. de entidad de seguridad, tal como se muestra a continuación: 

   ![Id. de objeto mostrado como id. de entidad de seguridad](./media/sql-db-output-managed-identity/principal-id.png)

   La entidad de servicio se llama igual que el trabajo de Stream Analytics. Por ejemplo, si es el nombre del trabajo es *MyASAJob*, el nombre de la entidad de servicio también será *MyASAJob*.

## <a name="select-an-active-directory-admin"></a>Selección de un administrador de Active Directory

Después de crear una identidad administrada, seleccione un administrador de Active Directory.

1. Vaya al recurso del grupo de SQL de Azure SQL Database o Azure Synapse Analytics y seleccione la instancia de SQL Server o el área de trabajo de Synapse en la que se encuentra el recurso, respectivamente. Puede encontrar el vínculo a estos en la página de información general del recurso, junto a *Nombre del servidor* o *Nombre del área de trabajo*.

1. Seleccione **Administrador de Active Directory** o **Administrador de Active Directory de SQL** en **Configuración**, para SQL Server y el área de trabajo de Synapse, respectivamente. A continuación, seleccione **Establecer administrador**.

   ![Página Administrador de Active Directory](./media/sql-db-output-managed-identity/active-directory-admin-page.png)

1. En la página Administrador de Active Directory, busque un usuario o un grupo para que sea administrador de SQL Server y haga clic en **Seleccionar**. Este será el usuario que podrá crear el **usuario de base de datos independiente** en la sección siguiente.

   ![Adición de un administrador de Active Directory](./media/sql-db-output-managed-identity/add-admin.png)

   En la hoja Administrador de Active Directory se muestran todos los miembros y grupos de Active Directory. Los usuarios o grupos que aparecen atenuados no se pueden seleccionar porque no se admiten como administradores de Azure Active Directory. Vea la lista de administradores admitidos en la sección  **Características y limitaciones de Azure Active Directory** de  [Uso de la autenticación de Azure Active Directory con SQL Database o Azure Synapse](../azure-sql/database/authentication-aad-overview.md#azure-ad-features-and-limitations).

1. Seleccione **Guardar** en la página **Administrador de Active Directory**. El proceso para cambiar el administrador tarda unos minutos.

## <a name="create-a-contained-database-user"></a>Creación de un usuario de base de datos independiente

A continuación, cree un usuario de base de datos independiente en su base de datos de Azure SQL o Azure Synapse que esté asignado a la identidad de Azure Active Directory. El usuario de base de datos independiente no tiene un inicio de sesión para la base de datos principal, sino que se asigna a una identidad del directorio que está asociada a la base de datos. La identidad de Azure Active Directory puede ser una cuenta de usuario individual o un grupo. En este caso, puede crear un usuario de base de datos independiente para el trabajo de Stream Analytics. 

Para más información, consulte el siguiente artículo sobre la integración de Azure AD: [Uso de la autenticación multifactor de Azure Active Directory](../azure-sql/database/authentication-mfa-ssms-overview.md).

1. Conéctese a la base de datos de Azure SQL o Azure Synapse mediante SQL Server Management Studio. El **nombre de usuario** es un usuario de Azure Active Directory con el permiso **ALTER ANY USER**. El administrador que estableció en SQL Server es un ejemplo. Utilice la opción de autenticación **Azure Active Directory - Universal con MFA** 

   ![Conectar a SQL Server](./media/sql-db-output-managed-identity/connect-sql-server.png)

   El nombre del servidor `<SQL Server name>.database.windows.net` puede ser diferente en distintas regiones. Por ejemplo, la región de China debe utilizar `<SQL Server name>.database.chinacloudapi.cn`.
 
   Para especificar una base de datos de Azure SQL o Azure Synapse específica, vaya a **Opciones > Propiedades de conexión > Conectar con base de datos**.  

   ![Propiedades de conexión de SQL Server:](./media/sql-db-output-managed-identity/sql-server-connection-properties.png)

1. Cuando se conecta por primera vez, puede que se muestre la siguiente ventana:

   ![Ventana Nueva regla de firewall](./media/sql-db-output-managed-identity/new-firewall-rule.png)

   1. En ese caso, vaya al recurso de SQL Server o del área de trabajo de Synapse en Azure Portal. En la sección **Seguridad**, abra la página **Firewalls y red virtual/Firewalls**. 
   1. Agregue una nueva regla con cualquier nombre.
   1. Use la dirección IP *De* de la ventana **Nueva regla de firewall** para *IP de inicio*.
   1. Use la dirección IP de *A* de la ventana **Nueva regla de firewall** para *IP final*. 
   1. Seleccione **Guardar** e intente conectarse de nuevo desde SQL Server Management Studio. 

1. Una vez que esté conectado, cree el usuario de base de datos independiente. El siguiente comando de SQL crea un usuario de base de datos independiente que tiene el mismo nombre que el trabajo de Stream Analytics. Asegúrese de incluir los corchetes alrededor de *ASA_JOB_NAME*. Use la siguiente sintaxis de T-SQL y ejecute la consulta. 

   ```sql
   CREATE USER [ASA_JOB_NAME] FROM EXTERNAL PROVIDER; 
   ```
   
    Para comprobar si ha agregado el usuario de base de datos independiente correctamente, ejecute el siguiente comando en SSMS en la base de datos correspondiente y compruebe si el valor de *ASA_JOB_NAME* está en la columna "Name" (Nombre).

   ```sql
   SELECT * FROM <SQL_DB_NAME>.sys.database_principals 
   WHERE type_desc = 'EXTERNAL_USER' 
   ```

1. Para que Azure Active Directory de Microsoft compruebe si el trabajo de Stream Analytics tiene acceso a la instancia de SQL Database, es necesario concederle permiso para comunicarse con la base de datos. Para ello, vaya de nuevo a la página "Firewalls y red virtual"/"Firewalls" de Azure Portal y habilite "Permitir que los servicios y recursos de Azure accedan a este servidor" o "Permitir que los servicios y recursos de Azure accedan a esta área de trabajo".

   ![Firewall y red virtual](./media/sql-db-output-managed-identity/allow-access.png)

## <a name="grant-stream-analytics-job-permissions"></a>Concesión de permisos del trabajo de Stream Analytics

#### <a name="azure-sql-database"></a>[Azure SQL Database](#tab/azure-sql)

Una vez que se haya creado un usuario de base de datos independiente y se le haya proporcionado acceso a los servicios de Azure en el portal como se ha descrito en la sección anterior, el trabajo de Stream Analytics tiene permiso de identidad administrada para **CONECTARSE** al recurso de base de datos de Azure SQL a través de la identidad administrada. Se recomienda conceder los permisos **SELECT** e **INSERT** al trabajo de Stream Analytics, ya que se necesitarán más adelante en el flujo de trabajo de Stream Analytics. El permiso **SELECT** permite que el trabajo pruebe su conexión con la tabla de la instancia de base de datos de Azure SQL. El permiso **INSERT** permite probar las consultas de Stream Analytics de un extremo a otro una vez que haya configurado una entrada y la salida de la base de datos de Azure SQL.

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

Una vez que se haya creado un usuario de base de datos independiente y se le haya proporcionado acceso a los servicios de Azure en el portal como se ha descrito en la sección anterior, el trabajo de Stream Analytics tiene permiso de identidad administrada para **CONECTARSE** al recurso de base de datos de Azure Synapse a través de la identidad administrada. Se recomienda además conceder los permisos **SELECT**, **INSERT** y **ADMINISTER DATABASE BULK OPERATIONS** al trabajo de Stream Analytics, ya que se necesitarán más adelante en el flujo de trabajo de Stream Analytics. El permiso **SELECT** permite que el trabajo pruebe su conexión con la tabla de la instancia de base de datos de Azure Synapse. Los permisos **INSERTAR** y **ADMINISTER DATABASE BULK OPERATIONS** permiten probar las consultas de Stream Analytics de un extremo a otro una vez que haya configurado una entrada y la salida de la base de datos de Azure Synapse.

Para conceder el permiso **ADMINISTER DATABASE BULK OPERATIONS**, tendrá que conceder todos los permisos etiquetados como **CONTROL** que aparecen en la columna [Implícito en el permiso de base de datos](/sql/t-sql/statements/grant-database-permissions-transact-sql?view=azure-sqldw-latest&preserve-view=true#remarks) al trabajo de Stream Analytics. Necesita este permiso porque el trabajo de Stream Analytics realiza la instrucción **COPY**, que requiere los permisos [ADMINISTER DATABASE BULK OPERATIONS e INSERT](/sql/t-sql/statements/copy-into-transact-sql).

---

Puede conceder estos permisos al trabajo de Stream Analytics mediante SQL Server Management Studio. Para obtener más información, consulte la referencia de GRANT (Transact-SQL).

Para conceder solo permiso a una tabla o un objeto determinado de la base de datos, use la siguiente sintaxis de T-SQL y ejecute la consulta. 

#### <a name="azure-sql-database"></a>[Azure SQL Database](#tab/azure-sql)

```sql
GRANT CONNECT, SELECT, INSERT ON OBJECT::TABLE_NAME TO ASA_JOB_NAME;
```

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

```sql
GRANT CONNECT, SELECT, INSERT, CONTROL, ADMINISTER DATABASE BULK OPERATIONS OBJECT::TABLE_NAME TO ASA_JOB_NAME;
```

---

Como alternativa, puede hacer clic con el botón derecho en su base de datos de Azure SQL o Azure Synapse en SQL Server Management Studio y seleccionar **Propiedades > Permisos**. En el menú de permisos, puede ver el trabajo de Stream Analytics que agregó anteriormente y conceder o denegar los permisos manualmente según considere oportuno.

Para ver todos los permisos que ha agregado al usuario *ASA_JOB_NAME*, ejecute el siguiente comando en SSMS en la base de datos correspondiente: 

```sql
SELECT dbprin.name, dbprin.type_desc, dbperm.permission_name, dbperm.state_desc, dbperm.class_desc, object_name(dbperm.major_id) 
FROM sys.database_principals dbprin 
LEFT JOIN sys.database_permissions dbperm 
ON dbperm.grantee_principal_id = dbprin.principal_id 
WHERE dbprin.name = '<ASA_JOB_NAME>' 
```

## <a name="create-an-azure-sql-database-or-azure-synapse-output"></a>Creación de una salida de Azure SQL Database o Azure Synapse

#### <a name="azure-sql-database"></a>[Azure SQL Database](#tab/azure-sql)

Ahora que la identidad administrada está configurada, está listo para agregar una salida de Azure SQL Database o Azure Synapse a su trabajo de Stream Analytics.

Asegúrese de haber creado una tabla en la instancia de SQL Database con el esquema de salida adecuado. El nombre de esta tabla es una de las propiedades necesarias que se debe rellenar cuando se agrega la salida de SQL Database al trabajo de Stream Analytics. Además, asegúrese de que el trabajo tiene permisos **SELECT** e **INSERT** para probar la conexión y ejecutar consultas de Stream Analytics. Consulte la sección [Concesión de permisos de trabajos de Stream Analytics](#grant-stream-analytics-job-permissions) si todavía no lo ha hecho. 

1. Vuelva al trabajo de Stream Analytics y vaya a la página **Salidas** en **Topología de trabajo**. 

1. Seleccione **Agregar > SQL Database**. En la ventana de propiedades de salida del receptor de salida de SQL Database, seleccione **Identidad administrada** en la lista desplegable Modo de autenticación.

1. Rellene el resto de las propiedades. Para más información acerca de cómo crear una salida de SQL Database, consulte [Creación de una salida de SQL Database con Stream Analytics](sql-database-output.md). Cuando haya terminado, seleccione **Guardar**.

1. Después de hacer clic en **Guardar**, se debe desencadenar automáticamente una prueba de conexión para el recurso. Una vez que se complete correctamente, habrá configurado correctamente el trabajo de Stream Analytics para conectarse a la base de datos de Azure SQL o Synapse SQL mediante el modo de autenticación de identidad administrada. 

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

Ahora que la identidad administrada y la cuenta de almacenamiento están configuradas, está listo para agregar una salida de Azure SQL Database o Azure Synapse a su trabajo de Stream Analytics.

Asegúrese de haber creado una tabla en la base de datos de Azure Synapse con el esquema de salida adecuado. El nombre de esta tabla es una de las propiedades necesarias que se debe rellenar cuando se agrega la salida de Azure Synapse al trabajo de Stream Analytics. Además, asegúrese de que el trabajo tiene permisos **SELECT** e **INSERT** para probar la conexión y ejecutar consultas de Stream Analytics. Consulte la sección [Concesión de permisos de trabajos de Stream Analytics](#grant-stream-analytics-job-permissions) si todavía no lo ha hecho.

1. Vuelva al trabajo de Stream Analytics y vaya a la página **Salidas** en **Topología de trabajo**.

1. Seleccione **Agregar > Azure Synapse Analytics**. En la ventana de propiedades de salida del receptor de salida de SQL Database, seleccione **Identidad administrada** en la lista desplegable Modo de autenticación.

1. Rellene el resto de las propiedades. Para más información sobre la creación de una salida de Azure Synapse, consulte [Salida de Azure Synapse Analytics desde Azure Stream Analytics](azure-synapse-analytics-output.md). Cuando haya terminado, seleccione **Guardar**.

1. Después de hacer clic en **Guardar**, se debe desencadenar automáticamente una prueba de conexión para el recurso. Una vez que se complete correctamente, estará listo para continuar con el uso de la identidad administrada para el recurso de Azure Synapse Analytics con Stream Analytics. 

---

## <a name="remove-managed-identity"></a>Eliminación de una identidad administrada

La identidad administrada creada para un trabajo de Stream Analytics se elimina solo cuando se elimina el trabajo. No hay ninguna manera de eliminar la identidad administrada sin eliminar el trabajo. Si ya no va a usar la identidad administrada, puede cambiar el método de autenticación de la salida. La identidad administrada seguirá existiendo hasta que se elimine el trabajo y se utilizará si decide usar de nuevo la autenticación de identidad administrada.

## <a name="next-steps"></a>Pasos siguientes

* [Información sobre las salidas desde Azure Stream Analytics](stream-analytics-define-outputs.md)
* [Salida de Azure Stream Analytics a Azure SQL Database](stream-analytics-sql-output-perf.md)
* [Salida de Azure Synapse Analytics desde Azure Stream Analytics](azure-synapse-analytics-output.md)
