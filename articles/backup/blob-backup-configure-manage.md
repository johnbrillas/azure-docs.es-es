---
title: Configuración de la copia de seguridad operativa para blobs de Azure
description: Aprenda a configurar y administrar la copia de seguridad operativa para blobs de Azure (en versión preliminar)
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: 0dc490842389ba9286799aef5d37c1cf7c1ba64e
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051079"
---
# <a name="configure-operational-backup-for-azure-blobs-in-preview"></a>Configuración de la copia de seguridad operativa para blobs de Azure (en versión preliminar)

Azure Backup le permite configurar fácilmente la copia de seguridad operativa para proteger los blobs en bloques en las cuentas de almacenamiento. En este artículo se explica cómo configurar la copia de seguridad operativa en una o varias cuentas de almacenamiento mediante Azure Portal. En este artículo se tratan los temas siguientes:

- Cosas que hay que saber antes de empezar
- Creación de un almacén de Backup
- Concesión de permisos al almacén de Backup en las cuentas de almacenamiento que se van a proteger
- Creación de una directiva de Backup
- Configuración de una copia de seguridad operativa en una o varias cuentas de almacenamiento
- Efectos en las cuentas de almacenamiento de copia de seguridad

## <a name="before-you-start"></a>Antes de comenzar

- La copia de seguridad operativa de blobs es una solución de copia de seguridad local que mantiene los datos durante un tiempo especificado en la propia cuenta de almacenamiento de origen. Esta solución no mantiene una copia adicional de los datos en el almacén.
- Permite conservar los datos para su restauración durante un máximo de 360 días. Sin embargo, las duraciones de retención largas pueden dar lugar a que la operación de restauración tarde más tiempo.
- La solución se puede usar para realizar restauraciones solo en la cuenta de almacenamiento de origen y puede que se sobrescriban datos.
- Si elimina un contenedor de la cuenta de almacenamiento llamando a la operación Eliminar contenedor, dicho contenedor no se puede restaurar con una operación de restauración. En lugar de eliminar un contenedor completo, elimine blobs individuales, por si desea restaurarlos más adelante. Además, Microsoft recomienda habilitar la eliminación temporal de los contenedores, además de la copia de seguridad operativa, como protección contra la eliminación accidental de contenedores.
- Consulte la [matriz de compatibilidad](blob-backup-support-matrix.md) para más información sobre los escenarios admitidos, las limitaciones y la disponibilidad.

## <a name="create-a-backup-vault"></a>Creación de un almacén de Backup

Un [almacén de Backup](backup-vault-overview.md) es una entidad de administración que almacena los puntos de recuperación creados a lo largo del tiempo y proporciona una interfaz para realizar operaciones relacionadas con la copia de seguridad. Esto incluye realizar copias de seguridad a petición, realizar restauraciones y crear directivas de copia de seguridad. Aunque la copia de seguridad operativa de blobs es una copia de seguridad local y no "almacena" datos en el almacén, este es necesario para varias operaciones de administración.

>[!NOTE]
>El almacén de Backup es un nuevo recurso que se usa para realizar copias de seguridad de nuevas cargas de trabajo compatibles y es diferente del almacén de Recovery Services existente.

Para obtener instrucciones sobre cómo crear un almacén de Backup, consulte la [documentación del almacén de Backup](backup-vault-overview.md#create-a-backup-vault).

## <a name="grant-permissions-to-the-backup-vault-on-storage-accounts"></a>Concesión de permisos al almacén de Backup en cuentas de almacenamiento

La copia de seguridad operativa también protege la cuenta de almacenamiento (que contiene los blobs que se van a proteger) de cualquier eliminación accidental mediante la aplicación de un bloqueo de eliminación propiedad de Backup. Esto requiere que el almacén de Backup tenga ciertos permisos en las cuentas de almacenamiento que deben protegerse. Para mayor comodidad de uso, estos permisos se han consolidado en el rol Colaborador de Backup de la cuenta de almacenamiento. Siga las instrucciones que se indican a continuación para las cuentas de almacenamiento que deben protegerse:

1. En la cuenta de almacenamiento que se va a proteger, vaya a la **pestaña Control de acceso (IAM)** en el panel de navegación izquierdo.
1. Seleccione **Agregar asignaciones de roles** para asignar el rol requerido.

    ![Agregar asignación de roles](./media/blob-backup-configure-manage/add-role-assignments.png)

1. En el panel Agregar asignación de roles:

    1. En **Rol**, elija **Storage Account Backup Contributor** (Colaborador de copia de seguridad de cuenta de almacenamiento).
    1. En **Asignar acceso a**: elija **User, group or service principal** (Usuario, grupo o entidad de servicio).
    1. Escriba el **nombre del almacén de Backup** en el que quiere proteger los blobs de esta cuenta de almacenamiento y seleccione el mismo en los resultados de la búsqueda.
    1. Cuando haya terminado, seleccione **Save** (Guardar).

        ![Opciones de asignación de roles](./media/blob-backup-configure-manage/role-assignment-options.png)

        >[!NOTE]
        >Espere hasta 10 minutos para que la asignación de roles surta efecto.

## <a name="create-a-backup-policy"></a>Crear una directiva de copia de seguridad

Normalmente, una directiva de copia de seguridad rige la retención y la programación de las copias de seguridad. Dado que la copia de seguridad operativa de los blobs es continua por naturaleza, no necesita una programación para realizar copias de seguridad. La directiva es básicamente necesaria para especificar el período de retención. Puede usar y reutilizar la directiva de copia de seguridad para configurar la copia de seguridad de varias cuentas de almacenamiento en un almacén.

Estos son los pasos para crear una directiva de copia de seguridad para la copia de seguridad operativa de los blobs:

1. En el almacén de Backup, vaya a **Directivas de copia de seguridad** y seleccione **+Agregar** para empezar a crear una directiva de copia de seguridad.

    ![Directivas de copia de seguridad](./media/blob-backup-configure-manage/backup-policies.png)

1. En la pestaña **Datos básicos**, proporcione un nombre para la directiva de copia de seguridad y seleccione **Azure Blobs** como el tipo de origen de datos. También puede ver los detalles del almacén seleccionado.

    ![Creación de una directiva de copia de seguridad](./media/blob-backup-configure-manage/create-backup-policy.png)

    >[!NOTE]
    >Aunque verá la **redundancia de almacenamiento de copia de seguridad** del almacén, la redundancia no se aplica realmente a la copia de seguridad operativa de blobs, ya que la copia de seguridad es local por naturaleza y no se almacena ningún dato en el almacén de Backup. Este almacén de Backup es la entidad de administración para ayudarle a administrar la protección de los blobs en bloques en las cuentas de almacenamiento.

1. En la pestaña **Directiva de Backup** se especifican los detalles de la retención. Verá que ya hay una regla de retención denominada **Predeterminada** con un período de retención de 30 días. Si quiere editar la duración de la retención, use el icono de **edición de regla de retención** para editar y especificar la duración de la conservación de los datos que quiera. Puede especificar una retención de hasta 360 días.

    ![Pestaña Directiva de Backup](./media/blob-backup-configure-manage/backup-policy-tab.png)

    >[!NOTE]
    >La restauración de duraciones largas puede dar lugar a que las operaciones de restauración tarden más en completarse. Además, el tiempo que se tarda en restaurar un conjunto de datos se basa en el número de operaciones de escritura y eliminación realizadas durante el período de restauración. Por ejemplo, una cuenta con un millón de objetos con 3000 objetos agregados al día y 1000 objetos eliminados al día requerirá aproximadamente dos horas para restaurar a un período de 30 días determinado del pasado. No se recomendarían un período de retención ni una restauración superiores a 90 días en el pasado para una cuenta con esta tasa de cambio.

1. En el panel **Revisar y crear**, compruebe todos los detalles de la directiva y seleccione **Crear** cuando haya finalizado para terminar de crear la directiva. Una notificación confirmará que se ha creado la directiva de Backup y que está lista para usarse.

    ![Revisión y creación de directiva](./media/blob-backup-configure-manage/review-create.png)

## <a name="configure-backup"></a>Configuración de la copia de seguridad

La copia de seguridad de blobs se configura en el nivel de cuenta de almacenamiento. Por lo tanto, todos los blobs de la cuenta de almacenamiento están protegidos con la copia de seguridad operativa.

Para iniciar la configuración de la copia de seguridad:

1. Busque **Centro de Backup** en la barra de búsqueda.
1. Vaya a **Información general** ->  **+Copia de seguridad**.

    ![Información general del Centro de copias de seguridad](./media/blob-backup-configure-manage/backup-center-overview.png)

1. En la pestaña **Iniciar: configurar copia de seguridad**, elija **Azure Blobs (Azure Storage)** como el tipo de origen de datos.

    ![Pestaña Iniciar: configurar copia de seguridad](./media/blob-backup-configure-manage/initiate-configure-backup.png)

1. En la pestaña **Datos básicos**, especifique **Azure Blobs (Azure Storage)** como tipo de **Origen de datos** y seleccione el almacén de Backup al que desea asociar las cuentas de almacenamiento. Puede ver los detalles del almacén seleccionado en el panel.

    ![Pestaña Aspectos básicos](./media/blob-backup-configure-manage/basics-tab.png)

1. A continuación, seleccione la directiva de copia de seguridad que quiere usar para especificar la retención. Puede ver los detalles de la directiva seleccionada en la parte inferior de la pantalla. La columna del almacén de datos operativos muestra la retención definida en la directiva. "Operativo" significa que los datos se mantienen localmente en la propia cuenta de almacenamiento de origen.

    ![Elegir directiva de copia de seguridad](./media/blob-backup-configure-manage/choose-backup-policy.png)

    También puede crear una nueva directiva de copia de seguridad. Para ello, seleccione **Crear nueva** y siga estos pasos:

    1. Escriba un nombre para la directiva que quiere crear. Asegúrese de que los demás cuadros muestran el tipo de origen de datos y el nombre de almacén correctos.
    1. En la pestaña **Directiva de Backup**, seleccione el icono de edición de regla de retención para editar y especificar la duración de tiempo que quiere retener los datos. Puede especificar una retención de hasta 360 días. La restauración de duraciones largas puede dar lugar a que las operaciones de restauración tarden más en completarse.

        ![Creación de una directiva de copia de seguridad](./media/blob-backup-configure-manage/new-backup-policy.png)

    1. Una vez hecho esto, seleccione **Revisar y crear** para crear la directiva de copia de seguridad.

1. A continuación, se le pedirá que elija las cuentas de almacenamiento para las que quiere configurar la protección de blobs. Puede elegir varias cuentas de almacenamiento a la vez y elegir **Seleccionar**.

    Sin embargo, asegúrese de que el almacén que ha elegido tiene los permisos necesarios para configurar la copia de seguridad en las cuentas de almacenamiento, tal y como se ha descrito anteriormente en [Concesión de permisos al almacén de Backup en cuentas de almacenamiento](#grant-permissions-to-the-backup-vault-on-storage-accounts).

    ![Seleccionar recursos para realizar copias de seguridad](./media/blob-backup-configure-manage/select-resources.png)

    Backup comprueba si el almacén tiene permisos suficientes para permitir la configuración de la copia de seguridad en las cuentas de almacenamiento seleccionadas.

    ![Backup valida los permisos](./media/blob-backup-configure-manage/validate-permissions.png)

    Si la validación produce errores (como con una de las cuentas de almacenamiento de la captura de pantalla), vaya a las cuentas de almacenamiento seleccionadas y asigne los roles adecuados, tal como se detalla [aquí](#grant-permissions-to-the-backup-vault-on-storage-accounts), y seleccione **Volver a validar**. La asignación de roles nuevos puede tardar hasta 10 minutos en surtir efecto.

1. Una vez que la validación sea correcta para todas las cuentas de almacenamiento seleccionadas, continúe en **Revisión y configuración** para configurar la copia de seguridad. Verá notificaciones que le informan sobre el estado de la configuración de la protección y su finalización.

## <a name="effects-on-backed-up-storage-accounts"></a>Efectos en las cuentas de almacenamiento de copia de seguridad

Una vez configurada la copia de seguridad, se realiza un seguimiento de los cambios que se realizan en los blobs en bloques de las cuentas de almacenamiento y los datos se retienen según la directiva de copia de seguridad. Observará los siguientes cambios en las cuentas de almacenamiento para las que se ha configurado la copia de seguridad:

- Están habilitadas las siguientes funcionalidades en la cuenta de almacenamiento. Se pueden ver en la pestaña **Protección de datos** de la cuenta de almacenamiento.
  - Restauración a un momento dado para contenedores: con retención tal y como se especifica en la directiva de copia de seguridad
  - Eliminación temporal para blobs: con retención tal y como se especifica en la directiva de copia de seguridad más 5 días
  - Control de versiones para blobs
  - Fuente de cambios de blobs

  Si la cuenta de almacenamiento configurada para la copia de seguridad ya tenía habilitadas las opciones **Point in time restore for containers** (Restauración a un momento dado para contenedores) o **Soft delete for blobs** (Eliminación temporal para blobs) (antes de configurar la copia de seguridad), Backup garantiza que la retención sea al menos tal como se define en la directiva de copia de seguridad. Por lo tanto, para cada propiedad:

  - Si la retención en la directiva de copia de seguridad es mayor que la retención presente originalmente en la cuenta de almacenamiento, la retención de la cuenta de almacenamiento se modifica según la directiva de copia de seguridad.
  - Si la retención en la directiva de copia de seguridad es menor que la retención presente originalmente en la cuenta de almacenamiento, la retención de la cuenta de almacenamiento se deja sin modificar en la duración establecida originalmente.

  ![Pestaña Protección de datos](./media/blob-backup-configure-manage/data-protection.png)

- Backup aplica un **bloqueo de eliminación** en la cuenta de almacenamiento. El bloqueo está diseñado como protección frente a casos de eliminación accidental de la cuenta de almacenamiento. Se puede ver en **Cuenta de almacenamiento** > **Bloqueos**.

    ![Eliminación de bloqueos](./media/blob-backup-configure-manage/delete-lock.png)

## <a name="manage-operational-backup"></a>Administración de copias de seguridad operativas

Puede usar el Centro de Backup como el único panel para administrar todas las copias de seguridad. Con respecto a la copia de seguridad operativa para blobs de Azure, puede usar el Centro de copias de seguridad para realizar lo siguiente:

- Como hemos visto anteriormente, puede usarlo para crear almacenes y directivas de Backup. También puede ver todos los almacenes y directivas en las suscripciones seleccionadas.
- El Centro de Backup ofrece una manera sencilla de supervisar el estado de la protección de las cuentas de almacenamiento protegidas, así como las cuentas de almacenamiento para las que la copia de seguridad no está configurada actualmente.
- Puede configurar la copia de seguridad de cualquier cuenta de almacenamiento mediante el botón **+Copia de seguridad**.
- Puede iniciar las restauraciones mediante el botón **Restaurar** y realizar un seguimiento de las restauraciones mediante **Trabajos de Backup**. Para más información sobre cómo realizar restauraciones, consulte [Restauración de blobs de Azure](blob-backup-support-matrix.md).
- Analice el uso de copias de seguridad mediante informes de copia de seguridad.

    ![Centro de copia de seguridad](./media/blob-backup-configure-manage/backup-center.png)

Para obtener más información, consulte [Información general sobre el Centro de copias de seguridad](backup-center-overview.md).

## <a name="next-steps"></a>Pasos siguientes

- [Restauración de blobs de Azure](blob-restore.md)
