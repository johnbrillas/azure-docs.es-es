---
title: 'Copia de seguridad y restauración en Azure Database for PostgreSQL: servidor único mediante Azure Portal'
description: En este artículo se describe cómo restaurar un servidor en Azure Database for PostgreSQL con un único servidor mediante Azure Portal.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 6/30/2020
ms.openlocfilehash: 82cec4cc448f0ec30aecf6f8a69f399e0abbdde0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97706956"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>Copia de seguridad y restauración de un servidor en Azure Database for PostgreSQL con un único servidor mediante Azure Portal

## <a name="backup-happens-automatically"></a>Las copias de seguridad se realizan automáticamente
Periódicamente, se realizan copias de seguridad de los servidores de Azure Database for PostgreSQL para habilitar las características de restauración. Con esta característica, puede restaurar el servidor y todas sus bases de datos en un servidor nuevo a un momento dado anterior.

## <a name="set-backup-configuration"></a>Configuración de copia de seguridad

Elija la configuración del servidor para copias de seguridad con redundancia local o con redundancia geográfica en el momento de crear el servidor, en la ventana **Plan de tarifa**.

> [!NOTE]
> Después de crear un servidor, no se puede cambiar el tipo de redundancia elegido, redundancia geográfica o redundancia local.
>

Al crear un servidor mediante Azure Portal, en la ventana **Plan de tarifa** se selecciona **Redundancia local** o **Redundancia geográfica** para las copias de seguridad del servidor. En esta ventana se selecciona también el **Período de retención de copia de seguridad**, es decir, durante cuánto tiempo se almacenarán las copias de seguridad del servidor.

   :::image type="content" source="./media/howto-restore-server-portal/pricing-tier.png" alt-text="Plan de tarifa: elija la redundancia de las copias de seguridad":::

Para más información acerca de cómo establecer estos valores durante la creación, consulte la [guía de inicio rápido del servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md).

Para cambiar el período de retención de copia de seguridad de un servidor, siga estos pasos:
1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).
2. Seleccione su servidor de Azure Database for PostgreSQL. Esta acción abre la página **Información general**.
3. Seleccione **Plan de tarifa** en el menú, en **Configuración**. Con el control deslizante puede cambiar el **Período de retención de copia de seguridad** entre 7 y 35 días.
En la captura de pantalla siguiente, se ha aumentado a 34 días.
:::image type="content" source="./media/howto-restore-server-portal/3-increase-backup-days.png" alt-text="Aumento del período de retención de copia de seguridad":::

4. Haga clic en **Aceptar** para confirmar el cambio.

El período de retención de copia de seguridad rige durante cuánto tiempo se puede realizar una restauración a un momento dado, porque se basa en las copias de seguridad disponibles. La restauración a un momento dado se describe con más detalle en la sección siguiente. 

## <a name="point-in-time-restore"></a>Restauración a un momento dado
Azure Database for PostgreSQL permite restaurar el servidor a un momento dado y en una nueva copia del servidor. Puede usar este nuevo servidor para recuperar los datos o hacer que las aplicaciones cliente apunten a este nuevo servidor.

Por ejemplo, si una tabla se quitó accidentalmente a mediodía de hoy, podría restaurar al momento justo antes de mediodía y recuperar la tabla y los datos que faltan desde esa copia nueva del servidor. La restauración a un momento dado se realiza en el nivel del servidor, no en el nivel de base de datos.

Los siguientes pasos restauran el servidor de ejemplo a un momento dado:
1. En Azure Portal, seleccione el servidor de Azure Database for PostgreSQL. 

2. En la barra de herramientas de la página **Información general** del servidor, seleccione **Restaurar**.

   :::image type="content" source="./media/howto-restore-server-portal/2-server.png" alt-text="Azure Database for PostgreSQL - Información general - Botón Restaurar":::

3. Rellene el formulario Restaurar con la información necesaria:

   :::image type="content" source="./media/howto-restore-server-portal/3-restore.png" alt-text="Azure Database for PostgreSQL - Información sobre restauración":::
   - **Punto de restauración**: seleccione el momento al que desea restaurar.
   - **Servidor de destino:** : proporcione un nombre para el nuevo servidor.
   - **Ubicación**: no se puede seleccionar la región. De manera predeterminada, es el mismo que el del servidor de origen.
   - **Plan de tarifa**: estos parámetros no se pueden cambiar al realizar una restauración a un momento dado. Es el mismo que el del servidor de origen. 

4. Haga clic en **Aceptar** para restaurar el servidor a un momento dado. 

5. Una vez finalizada la restauración, busque el nuevo servidor que se crea para comprobar que los datos se restauraron del modo esperado.

El nuevo servidor creado mediante restauración a un momento dado tiene el mismo nombre de inicio de sesión y contraseña de administrador del servidor que el servidor existente tenía en ese momento dado. Puede cambiar la contraseña en la página **Información general** del nuevo servidor.

El servidor creado durante una restauración no tiene las reglas de firewall o los puntos de conexión de servicio VNet que existían en el servidor original. Estas reglas deben configurarse por separado para este nuevo servidor.

Si el servidor PostgreSQL de origen está cifrado con claves administradas por el cliente, consulte la [documentación](concepts-data-encryption-postgresql.md) para obtener más información.

## <a name="geo-restore"></a>Restauración geográfica

Si ha configurado el servidor para copias de seguridad con redundancia geográfica, se puede crear un nuevo servidor a partir de la copia de seguridad de ese servidor existente. Este nuevo servidor puede crearse en cualquier región en la que Azure Database for PostgreSQL esté disponible.  

1. Seleccione el botón **Crear un recurso** (+) de la esquina superior izquierda del portal. Seleccione **Bases de datos** > **Azure Database for PostgreSQL**.

   :::image type="content" source="./media/howto-restore-server-portal/1-navigate-to-postgres.png" alt-text="Navegación a Azure Database for PostgreSQL.":::

2. Seleccione la opción de implementación de **servidor único**.

   :::image type="content" source="./media/howto-restore-server-portal/2-select-deployment-option.png" alt-text="Seleccione la opción de implementación Azure Database for PostgreSQL: servidor único.":::
 
3. Proporcione la suscripción, el grupo de recursos y el nombre del nuevo servidor. 

4. Seleccione **Copia de seguridad** como el **Origen de datos**. Esta acción carga un menú desplegable en el que se proporciona una lista de servidores que tienen habilitadas copias de seguridad con redundancia geográfica.
   
   :::image type="content" source="./media/howto-restore-server-portal/4-geo-restore.png" alt-text="Seleccione el origen de datos.":::
    
   > [!NOTE]
   > Al crear por primera vez un servidor, puede que no esté disponible para la restauración geográfica inmediatamente. Los metadatos pueden tardar unas horas en rellenarse.
   >

5. Seleccione la lista desplegable **Copia de seguridad**.
   
   :::image type="content" source="./media/howto-restore-server-portal/5-geo-restore-backup.png" alt-text="Selección de la lista desplegable Copia de seguridad.":::

6. Seleccione el servidor de origen desde el que se va a restaurar.
   
   :::image type="content" source="./media/howto-restore-server-portal/6-select-backup.png" alt-text="Selección de la copia de seguridad.":::

7. El servidor usará de forma predeterminada los valores para el número de **Núcleos virtuales**, **Período de retención de copia de seguridad**, la opción **Redundancia de copia de seguridad**, la **versión del motor** y las **credenciales de administrador**. Seleccione **Continuar**. 
   
   :::image type="content" source="./media/howto-restore-server-portal/7-accept-backup.png" alt-text="Continuación con la copia de seguridad.":::

8. Rellene el resto del formulario con sus preferencias. Puede seleccionar cualquier valor en **Ubicación**.

    Después de seleccionar la ubicación, puede seleccionar **Configurar servidor** para actualizar **Generación de procesos** (si está disponible en la región que ha elegido), el número de **Núcleos virtuales**, **Período de retención de copia de seguridad** y la opción  **Redundancia de copia de seguridad**. No se permite cambiar el **Plan de tarifa** (Básico, Uso general o Memoria optimizada) ni el tamaño de **Almacenamiento** durante la restauración.

   :::image type="content" source="./media/howto-restore-server-portal/8-create.png" alt-text="Cumplimentación del formulario."::: 

9. Seleccione **Review + create** (Revisar y crear) para revisar las selecciones. 

10. Seleccione **Crear** para realizar el aprovisionamiento del servidor. Esta operación puede tardar algunos minutos.

El nuevo servidor creado mediante restauración geográfica tiene el mismo nombre de inicio de sesión y contraseña de administrador del servidor que el servidor existente tenía cuando se inició la restauración. La contraseña se puede cambiar en la página **Información general** del nuevo servidor.

El servidor creado durante una restauración no tiene las reglas de firewall o los puntos de conexión de servicio VNet que existían en el servidor original. Estas reglas deben configurarse por separado para este nuevo servidor.

Si el servidor PostgreSQL de origen está cifrado con claves administradas por el cliente, consulte la [documentación](concepts-data-encryption-postgresql.md) para obtener más información.


## <a name="next-steps"></a>Pasos siguientes
- Más información acerca de las [copias de seguridad](concepts-backup.md) del servicio.
- Más información sobre las opciones de [continuidad del negocio](concepts-business-continuity.md).