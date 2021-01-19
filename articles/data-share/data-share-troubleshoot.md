---
title: Solución de problemas de Azure Data Share
description: Aprenda a solucionar problemas con invitaciones y errores al crear o recibir recursos compartidos de datos en Azure Data Share.
services: data-share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: troubleshooting
ms.date: 12/16/2020
ms.openlocfilehash: 3aa1c0b8579bd37d2bb51cbde70997131c696813
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97964514"
---
# <a name="troubleshoot-common-problems-in-azure-data-share"></a>Solucionar problemas comunes de Azure Data Share 

En este artículo se explica cómo solucionar problemas comunes de Azure Data Share. 

## <a name="azure-data-share-invitations"></a>Invitaciones de Azure Data Share 

En algunos casos, cuando un usuario nuevo selecciona **Accept Invitation** (Aceptar invitación) en una invitación de correo electrónico, es posible que vea una lista de invitaciones vacía. 

:::image type="content" source="media/no-invites.png" alt-text="Captura de pantalla que muestra una lista de invitaciones vacía.":::

Este problema puede deberse a uno de los siguientes motivos:

* **El servicio Azure Data Share no está registrado como proveedor de recursos de ninguna suscripción de Azure en el inquilino de Azure.** Este problema se produce si no hay ningún recurso compartido de datos en su inquilino de Azure. 

    Cuando se crea un recurso de Azure Data Share, se registra automáticamente el proveedor de recursos en la suscripción de Azure. Puede registrar manualmente el servicio Data Share siguiendo estos pasos. Para completar estos pasos, debe tener el [rol Colaborador](../role-based-access-control/built-in-roles.md#contributor)de la suscripción de Azure. 

    1. En Azure Portal, vaya a **Suscripciones**.
    1. Seleccione la suscripción de Azure que quiere usar para crear un recurso de Azure Data Share.
    1. Seleccione **Proveedores de recursos**.
    1. Busque **Microsoft.DataShare**.
    1. Seleccione **Registrar**.

* **La invitación se envía a su alias de correo electrónico en lugar de a su dirección de correo de inicio de sesión de Azure.** Si ya ha registrado el servicio de Azure Data Share o ya ha creado un recurso compartido de datos en el inquilino de Azure, pero sigue sin poder ver la invitación, es posible que su alias de correo electrónico aparezca como destinatario. Póngase en contacto con el proveedor de datos y asegúrese de que la invitación ha sido enviada a su dirección de correo electrónico de inicio de sesión de Azure, y no a su alias de correo electrónico.

* **La invitación ya se aceptó.** El vínculo del correo electrónico le lleva a la página **Invitaciones de Data Share** en Azure Portal. En esta página solo aparecen las invitaciones pendientes. Las invitaciones aceptadas no se ven en la página. Para ver los recursos compartidos recibidos y configurar la configuración del clúster de Azure Data Explorer de destino, diríjase al recurso Data Share que usó para aceptar la invitación.

## <a name="creating-and-receiving-shares"></a>Al crear y recibir recursos compartidos

Los siguientes errores pueden aparecer cuando crea un nuevo recurso compartido, agrega conjuntos de datos o asigna conjuntos de datos:

* No se pudo agregar conjunto de datos.
* No se pudo asignar conjunto de datos.
* No se puede conceder acceso de x para y al recurso de Data Share.
* No tiene los permisos apropiados para x.
* No hemos podido agregar permisos de escritura para la cuenta de Azure Data Share en uno o más de sus recursos seleccionados.

Es posible que vea uno de estos errores si no tiene permisos suficientes en el almacén de datos de Azure. Para más información, consulte los [roles y requisitos](concepts-roles-permissions.md). 

Necesita permiso de escritura para compartir o recibir datos de un almacén de datos de Azure. Por lo general, este permiso existe en el rol Colaborador. 

Si esta es la primera vez que va a compartir o recibir datos del almacén de datos de Azure, también necesita el permiso *Microsoft.Authorization/role assignments/write*. Este permiso es, normalmente, parte del rol Propietario. Aunque haya creado el recurso del almacén de datos de Azure, usted no es automáticamente el propietario del recurso. 

Cuando tiene los permisos adecuados, el servicio de Azure Data Share concede automáticamente a la identidad administrada del recurso compartido de datos acceso al almacén de datos. Este proceso puede tardar unos minutos. Si experimenta un error debido a este retraso, inténtelo de nuevo pasados unos minutos.

El uso compartido basado en SQL necesita permisos adicionales. Para obtener información sobre los requisitos previos, consulte [Compartir desde orígenes SQL](how-to-share-from-sql.md).

## <a name="snapshots"></a>Instantáneas
Una instantánea puede fallar por diversos motivos. Abra un mensaje de error detallado seleccionando la hora de inicio de la instantánea y luego el estado de cada conjunto de datos. 

Normalmente se produce un error en una instantánea por estos motivos:

* Data Share no tiene permiso para leer desde el almacén de datos de origen, y tampoco tiene permiso para escribir en el almacén de datos de destino. Para más información, consulte los [roles y requisitos](concepts-roles-permissions.md). Si es la primera vez que toma una instantánea, puede que el recurso de Data Share necesite unos minutos para obtener acceso al almacén de datos de Azure. Espere unos minutos y vuelva a intentarlo.
* El firewall bloquea la conexión de Data Share al almacén de datos de origen o al almacén de datos de destino.
* Se eliminó un conjunto de datos compartido o el almacén de datos de origen o de destino.

En el caso de las cuentas de almacenamiento, se puede producir un error en una instantánea porque se está actualizando un archivo en el origen mientras se crea la instantánea. Esto puede dar lugar a un archivo de 0 bytes en el destino. Una vez realizada la actualización en el origen, la instantánea debería poder hacerse correctamente.

En el caso de los orígenes SQL, se puede producir un error en una instantánea por uno de estos motivos:

* El script SQL de origen o destino que concede permiso para Data Share no se ha ejecutado. O para Azure SQL Database o Azure Synapse Analytics (anteriormente Azure SQL Data Warehouse), el script se ejecuta mediante la autenticación de SQL en lugar de la autenticación de Azure Active Directory.  
* El almacén de datos de origen o los datos de destino SQL están en pausa.
* El proceso de hacer la instantánea o el almacén de datos de destino son incompatibles con los tipos de datos SQL. Para obtener más información, consulte [Compartir desde orígenes de SQL](how-to-share-from-sql.md#supported-data-types).
* El almacén de datos de origen o los datos de destino SQL están bloqueados por otros procesos. Azure Data Share no bloquea estos almacenes de datos. Los bloqueos existentes en estos almacenes de datos pueden hacer que la instantánea falle.
* Una restricción de clave externa hace referencia a la tabla SQL de destino. Durante una instantánea, si una tabla de destino tiene el mismo nombre que una tabla en los datos de origen, Azure Data Share descarta la tabla y crea una nueva tabla. Si una restricción de clave externa hace referencia a la tabla SQL de destino, la tabla no se puede descartar.
* Se genera un archivo CSV de destino, pero los datos no se pueden leer en Excel. Esto puede ocurrir cuando la tabla SQL de origen contiene datos con caracteres no ingleses. En Excel, seleccione la pestaña **Obtener datos** y elija el archivo CSV. Seleccione el origen del archivo como **65001: Unicode (UTF-8)** , y después cargue los datos.

## <a name="updated-snapshot-schedules"></a>Programación actualizada de instantáneas
Una vez que el proveedor de datos actualiza la programación de la instantánea para el recurso compartido enviado, el consumidor de datos debe deshabilitar la anterior programación de la instantánea. A continuación, ha de habilitar la programación actualizada de instantáneas para el recurso compartido recibido. 

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo empezar a compartir datos, continúe con el tutorial [Compartir datos](share-your-data.md). 

Para información sobre cómo recibir datos, continúe con el tutorial [Aceptación y recepción de datos](subscribe-to-data-share.md).