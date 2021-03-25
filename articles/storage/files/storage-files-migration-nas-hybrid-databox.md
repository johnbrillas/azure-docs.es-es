---
title: Migración de NAS local a Azure File Sync a través de Azure DataBox
description: Obtenga información sobre cómo migrar archivos de una ubicación de almacenamiento conectado a la red (NAS) local a una implementación de nube híbrida con Azure File Sync a través de Azure DataBox.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 03/5/2021
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 144b2f23e40f315441c3de2482ae8aeffe77ec75
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102583601"
---
# <a name="use-databox-to-migrate-from-network-attached-storage-nas-to-a-hybrid-cloud-deployment-with-azure-file-sync"></a>Uso de DataBox para migrar desde un almacenamiento conectado a la red (NAS) a una implementación de nube híbrida con Azure File Sync

Este artículo de migración es uno de varios que implican las palabras clave NAS, Azure File Sync y Azure DataBox. Compruebe si este artículo se aplica a su escenario:

> [!div class="checklist"]
> * Origen de datos: almacenamiento conectado a la red (NAS)
> * Ruta de migración: &rArr; DataBox &rArr; recurso compartido de archivos de Azure &rArr; sincronización con Windows Server
> * Almacenamiento en caché de archivos locales: sí, el objetivo final es una implementación de Azure File Sync.

Si el escenario es diferente, examine la [tabla de guías de migración](storage-files-migration-overview.md#migration-guides).

Azure File Sync funciona en ubicaciones de almacenamiento de conexión directa (DAS) y no admite la sincronización con ubicaciones de almacenamiento conectado a la red (NAS).
Es por esto que resulta necesario migrar los archivos y en este artículo encontrará una guía para planear y ejecutar dicha migración.

## <a name="migration-goals"></a>Objetivos de la migración

El objetivo es trasladar los recursos compartidos que tiene en el dispositivo NAS a una instancia de Windows Server. Después, usará Azure File Sync para una implementación de nube híbrida. Esta migración se debe realizar de forma que garantice la integridad de los datos de producción y la disponibilidad durante la migración. Esta última requiere que el tiempo de inactividad sea mínimo, para ajustarse o solo superar ligeramente las ventanas de mantenimiento regulares.

## <a name="migration-overview"></a>Información general sobre la migración

El proceso de migración consta de varias fases. Tendrá que implementar las cuentas de almacenamiento y los recursos compartidos de archivos de Azure, implementar una instancia de Windows Server local, configurar Azure File Sync, migrar con RoboCopy y, por último, realizar la migración. En las secciones siguientes se describen detalladamente las fases del proceso de migración.

> [!TIP]
> Si vuelve a este artículo, use la navegación del lado derecho para ir a la fase de migración en la que se quedó.

## <a name="phase-1-identify-how-many-azure-file-shares-you-need"></a>Fase 1: Identificación de cuántos recursos compartidos de archivos de Azure se necesitan

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

## <a name="phase-2-deploy-azure-storage-resources"></a>Fase 2: Implementación de recursos de almacenamiento de Azure

En esta fase, consulte la tabla de asignación de la fase 1 y úsela para aprovisionar el número correcto de cuentas de almacenamiento de Azure y recursos compartidos de archivos que contienen.

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

## <a name="phase-3-determine-how-many-azure-databox-appliances-you-need"></a>Fase 3: Determinación del número de dispositivos de Azure DataBox que necesita

Inicie este paso solo cuando haya completado la fase anterior. Los recursos de almacenamiento de Azure Storage (cuentas de almacenamiento y recursos compartidos de archivos) se deben crear en este momento. Durante el pedido de DataBox, debe especificar a qué cuentas de almacenamiento se mueven los datos de DataBox.

En esta fase, debe asignar los resultados del plan de migración de la fase anterior a los límites de las opciones de DataBox disponibles. Estas consideraciones le ayudarán a crear un plan para las opciones de DataBox que debe elegir, así como cuántas necesitará para mover los recursos compartidos de NAS a recursos compartidos de archivos de Azure.

Para determinar el número de dispositivos que necesita de cada tipo, tenga en cuenta estos límites importantes:

* Cualquier instancia de Azure DataBox puede mover datos a un máximo de 10 cuentas de almacenamiento. 
* Cada opción de DataBox tiene su propia capacidad utilizable. Consulte [Opciones de DataBox](#databox-options).

Consulte el plan de migración para conocer el número de cuentas de almacenamiento que ha decidido crear y los recursos compartidos en cada una de ellas. A continuación, examine el tamaño de cada uno de los recursos compartidos de NAS. La combinación de esta información le permitirá optimizar y decidir qué dispositivo debe enviar datos a las cuentas de almacenamiento. Puede tener dos dispositivos de DataBox para trasladar los archivos a la misma cuenta de almacenamiento, pero no divida el contenido de un solo recurso compartido de archivos en dos instancias de DataBox.

### <a name="databox-options"></a>Opciones de DataBox

Para una migración estándar, se debe elegir una combinación de estas tres opciones de DataBox: 

* DataBox Disks: Microsoft le enviará de uno y a cinco discos SSD con una capacidad de 8 TiB cada uno, con un total de 40 TiB como máximo. La capacidad utilizable es aproximadamente un 20 % menos debido a la sobrecarga del sistema de archivos y el cifrado. Para obtener más información, consulte la [Documentación de DataBox Disks](../../databox/data-box-disk-overview.md).
* DataBox: esta es la opción más común. Se le enviará un dispositivo DataBox resistente, que funciona de forma similar a un NAS. Tiene una capacidad utilizable de 80 TiB. Para obtener más información, consulte la [Documentación de DataBox](../../databox/data-box-overview.md).
* DataBox Heavy: esta opción presenta un dispositivo DataBox resistente en ruedas, que funciona de forma similar a un NAS, con una capacidad de 1 PiB. La capacidad utilizable es aproximadamente un 20 % menos debido a la sobrecarga del sistema de archivos y el cifrado. Para obtener más información, consulte la [Documentación de DataBox Heavy](../../databox/data-box-heavy-overview.md).

## <a name="phase-4-provision-a-suitable-windows-server-on-premises"></a>Fase 4: Aprovisionamiento de una instancia de Windows Server adecuada en el entorno local

Mientras espera a que lleguen los dispositivos Azure DataBox, ya puede empezar a revisar las necesidades de una o más instancias de Windows Server que va a usar con Azure File Sync.

* Cree un servidor de Windows Server 2019 (como mínimo de la edición 2012 R2), como una máquina virtual o un servidor físico. También se admite un clúster de conmutación por error de Windows Server.
* Aprovisione o agregue almacenamiento de conexión directa (DAS frente a NAS, que no se admite).

La configuración de recursos (de proceso y RAM) de la instancia de Windows Server que implemente depende principalmente del número de elementos (archivos y carpetas) que se van a sincronizar. Se recomienda trabajar con una configuración de rendimiento más alto si tiene problemas.

[Aprenda a cambiar el tamaño de un servidor de Windows Server según el número de elementos (archivos y carpetas) que necesite sincronizar.](storage-sync-files-planning.md#recommended-system-resources)

> [!NOTE]
> El artículo vinculado anteriormente presenta una tabla con un intervalo de memoria del servidor (RAM). Puede orientarse hacia el número más pequeño para el servidor, pero espere que la sincronización inicial tarde mucho más.

## <a name="phase-5-copy-files-onto-your-databox"></a>Fase 5: Copia de archivos en el dispositivo DataBox

Cuando llegue el dispositivo DataBox, debe configurarlo en la línea de visión del dispositivo NAS. Siga la documentación de configuración del tipo de DataBox que solicitó.

* [Configuración de Data Box](../../databox/data-box-quickstart-portal.md)
* [Configuración de Data Box Disk](../../databox/data-box-disk-quickstart-portal.md)
* [Configuración de Data Box Heavy](../../databox/data-box-heavy-quickstart-portal.md)

En función del tipo de DataBox, puede que haya herramientas de copia de DataBox disponibles. En este momento, no se recomiendan para las migraciones a recursos compartidos de archivos de Azure, ya que no copian los archivos con plena fidelidad en el dispositivo DataBox. En su lugar, use RoboCopy.

Cuando llegue el dispositivo DataBox, tendrá recursos compartidos de SMB aprovisionados previamente disponibles para cada cuenta de almacenamiento que especificó en el momento del pedido.

* Si los archivos entran en un recurso compartido de archivos prémium de Azure, habrá un recurso compartido de SMB por cuenta de almacenamiento de "almacenamiento de archivos" prémium.
* Si los archivos entran en una cuenta de almacenamiento estándar, habrá tres recursos compartidos de SMB por cuenta de almacenamiento estándar (GPv1 y GPv2). Solo los recursos compartidos de archivos que terminan con `_AzFiles` son relevantes para la migración. Omita los recursos compartidos de blob en bloques y en páginas.

Siga los pasos descritos en la documentación de Azure DataBox:

1. [Conexión a un dispositivo Data Box](../../databox/data-box-deploy-copy-data.md)
1. Copia de datos a un dispositivo Data Box
1. [Preparación del dispositivo DataBox para su salida a Azure](../../databox/data-box-deploy-picked-up.md)

La documentación vinculada de DataBox especifica un comando RoboCopy. Sin embargo, el comando no es adecuado para conservar la fidelidad total de archivos y carpetas. En su lugar, use este comando:

[!INCLUDE [storage-files-migration-robocopy](../../../includes/storage-files-migration-robocopy.md)]


## <a name="phase-6-deploy-the-azure-file-sync-cloud-resource"></a>Fase 6: Implementación del recurso de nube de Azure File Sync

Antes de continuar con esta guía, espere hasta que todos los archivos hayan llegado a los recursos compartidos de archivos de Azure correctos. El proceso de envío e ingesta de datos de DataBox llevará tiempo.

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

## <a name="phase-7-deploy-the-azure-file-sync-agent"></a>Fase 7: Implementación del agente de Azure File Sync

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

## <a name="phase-8-configure-azure-file-sync-on-the-windows-server"></a>Fase 8: Configuración de Azure File Sync en el servidor de Windows Server

El servidor registrado de Windows Server debe estar preparado y conectado a Internet para este proceso.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

Active la característica de nube por niveles y seleccione "Solo espacio de nombres" en la sección de descarga inicial.

> [!IMPORTANT]
> La nube por niveles es la característica de Azure File Sync que permite al servidor local tener menos capacidad de almacenamiento de la que está almacenada en la nube y, a pesar de ello, disponer de todo el espacio de nombres. Los datos de interés local también se almacenan en caché para conseguir un rendimiento de acceso rápido. La nube por niveles es una característica opcional de cada "punto de conexión de servidor" de Azure File Sync. Debe usar esta característica si no tiene suficiente capacidad de disco local en Windows Server para contener todos los datos en la nube y si quiere evitar la descarga de todos los datos de la nube.

Repita los pasos de creación de grupos de sincronización y adición de la carpeta de servidor correspondiente como un punto de conexión de servidor para todos los recursos compartidos de archivos de Azure o las ubicaciones del servidor, que deban configurarse para la sincronización. Espere hasta que se complete la sincronización del espacio de nombres. En la siguiente sección se detallará cómo puede asegurarse de ello.

> [!NOTE]
> Después de crear un punto de conexión de servidor, la sincronización funciona. Sin embargo, la sincronización debe enumerar (detectar) los archivos y las carpetas que se movieron a través de DataBox al recurso compartido de archivos de Azure. Dependiendo del tamaño del espacio de nombres, el espacio de nombres de la nube puede tardar bastante tiempo en empezar a aparecer en el servidor.

## <a name="phase-9-wait-for-the-namespace-to-fully-appear-on-the-server"></a>Fase 9: Espera para que el espacio de nombres aparezca por completo en el servidor

Para realizar los pasos siguientes de la migración, es imperativo esperar a que el servidor haya descargado por completo el espacio de nombres del recurso compartido de nube. Si empieza a mover archivos demasiado pronto al servidor, puede poner en riesgo cargas innecesarias e incluso se pueden producir conflictos de sincronización de archivos.

Para saber si el servidor ha completado la sincronización inicial de la descarga, abra el Visor de eventos en la instancia de Windows Server que se está sincronizando y use el registro de eventos de telemetría de Azure File Sync.
El registro de eventos de telemetría se encuentra en el Visor de eventos en Applications and Services\Microsoft\FileSync\Agent.

Busque el evento 9102 más reciente. El id. de evento 9102 se registra una vez finalizada una sesión de sincronización. En el texto del evento, hay un campo para la dirección de sincronización de la descarga. (`HResult` debe ser cero y archivos descargados también).
 
Debería ver dos eventos consecutivos de este tipo y contenido para saber que el servidor ha terminado de descargar el espacio de nombres. No hay ningún problema si se activan distintos eventos entre los dos eventos 9102.

## <a name="phase-10-catch-up-robocopy-from-your-nas"></a>Fase 10: Puesta al día de RoboCopy a partir del NAS

Una vez que el servidor haya completado la sincronización inicial del espacio de nombres completo desde el recurso compartido en la nube, puede continuar con este paso. Es imperativo que ese paso se complete antes de continuar con este. Consulte la sección anterior para obtener más información.

En este paso, ejecutará trabajos de RoboCopy para poner al día los recursos compartidos en la nube con los cambios más recientes del NAS desde el momento en que bifurcó los recursos compartidos en el dispositivo DataBox.
Esta puesta al día de RoboCopy puede finalizar rápidamente o tardar unos minutos en función de la cantidad de abandonos que haya ocurrido en los recursos compartidos de NAS.

> [!WARNING]
> Debido a un comportamiento de RoboCopy con regresión en Windows Server 2019, el modificador /MIR de RoboCopy no es compatible con un directorio de destino en capas. No debe usar Windows Server 2019 ni el cliente Windows 10 para esta fase de la migración. Use RoboCopy en Windows Server 2016 intermedio.

El enfoque de migración básico es una ejecución de RoboCopy del dispositivo NAS a Windows Server y de Azure File Sync a los recursos compartidos de archivos de Azure.

Ejecute la primera copia local en la carpeta de destino de Windows Server:

1. Identifique la primera ubicación en el dispositivo NAS.
1. Identifique la carpeta coincidente en el servidor de Windows Server, que ya tiene Azure File Sync configurado.
1. Inicie la copia con RoboCopy.

El comando de RoboCopy siguiente solo copiará las diferencias (archivos y carpetas actualizados) desde el almacenamiento NAS a la carpeta de destino de la instancia de Windows Server. A continuación, Windows Server los sincronizará con los recursos compartidos de archivos de Azure. 

[!INCLUDE [storage-files-migration-robocopy](../../../includes/storage-files-migration-robocopy.md)]

Si ha aprovisionado menos almacenamiento en la instancia de Windows Server que el que ocupan los archivos en el dispositivo NAS, ha configurado la nube por niveles. A medida que el volumen local de Windows Server se llena, la [nube por niveles](storage-sync-cloud-tiering-overview.md) se iniciará y organizará por niveles los archivos que ya se han sincronizado correctamente. La nube por niveles generará espacio suficiente para continuar con la copia desde el dispositivo NAS. La nube por niveles realiza comprobaciones cada hora para averiguar qué se ha sincronizado y para liberar espacio en disco para alcanzar el 99 % de espacio libre del volumen.
Es posible que RoboCopy tenga que mover numerosos archivos, más que el almacenamiento local del que dispone en Windows Server. Como promedio, puede esperar que RoboCopy se mueva mucho más rápido de lo que Azure File Sync puede cargar los archivos y cambiarlos de nivel del volumen local. Se producirá un error en RoboCopy. Se recomienda trabajar con los recursos compartidos en una secuencia que lo evite. Por ejemplo, no inicie trabajos de RoboCopy para todos los recursos compartidos al mismo tiempo, o bien transfiera solo los recursos compartidos que se ajusten a la cantidad actual de espacio disponible en la instancia de Windows Server, por mencionar algunas soluciones. La buena noticia es que el modificador /MIR solo moverá archivos delta y, una vez que uno de estos archivos se haya movido, no será necesario volver a moverlo cuando se reinicie un trabajo.

### <a name="user-cut-over"></a>Migración total de los usuarios

Al ejecutar por primera vez el comando RoboCopy, los usuarios y las aplicaciones siguen accediendo a los archivos en la ubicación de NAS y pueden modificarlos. Es posible que RoboCopy haya procesado un directorio, pase al siguiente y, después, un usuario en la ubicación de origen (NAS) agregue, cambie o elimine un archivo que no se procesará en esta ejecución actual de RoboCopy. Este comportamiento es normal.

La primera ejecución consiste en mover la mayor parte de los datos perdidos a la instancia de Windows Server y, después, a la nube a través de Azure File Sync. Esta primera copia puede tardar mucho tiempo, en función de lo siguiente:

* El ancho de banda de carga.
* La velocidad de la red local y el número de subprocesos de RoboCopy que coincidan de forma óptima con ella.
* El número de elementos (archivos y carpetas) que deben procesar RoboCopy y Azure File Sync.

Una vez completada la ejecución inicial, vuelva a ejecutar el comando.

La segunda vez que ejecute RoboCopy para el mismo recurso compartido se completará más rápido, ya que solo necesita transportar los cambios posteriores a la última ejecución. Puede ejecutar trabajos repetidos para el mismo recurso compartido.

Si considera que el tiempo de inactividad es aceptable, debe quitar el acceso de usuario a los recursos compartidos basados en NAS. Para ello, siga los pasos que impidan que los usuarios cambien el contenido y la estructura de archivos y carpetas. Un ejemplo es dirigir DFS-Namespace a una ubicación no existente o cambiar las ACL raíz del recurso compartido.

Ejecute una última ronda de RoboCopy. Recuperará todos los cambios que puedan haberse omitido.
El tiempo necesario para realizar este último paso dependerá de la velocidad del análisis de RoboCopy. Para realizar un cálculo estimado del tiempo (que equivale al tiempo de inactividad) averigüe cuánto tardó en realizarse la ejecución anterior.

Cree un recurso compartido en la carpeta de Windows Server y, eventualmente, ajuste esta carpeta como destino de la implementación de DFS-N. Asegúrese de establecer los mismos permisos de nivel de recurso compartido que en el recurso compartido de SMB de NAS. Si tuviera un NAS unido a un dominio de clase empresarial, los SID de usuario coincidirían automáticamente a medida que los usuarios se encuentren en Active Directory y RoboCopy copia los archivos y metadatos con total fidelidad. Si ha utilizado usuarios locales en la ubicación de NAS, debe volver a crearlos como usuarios locales de Windows Server y asignar los SID existentes que RoboCopy ha transferido a la instancia de Windows Server a los SID de los nuevos usuarios locales de Windows Server.

Ha terminado de migrar un recurso compartido o un grupo de recursos compartidos a un volumen o una raíz comunes. (Según la asignación de la fase 1)

Puede intentar ejecutar algunas de estas copias en paralelo. Se recomienda procesar el ámbito de un recurso compartido de archivos de Azure a la vez.

## <a name="troubleshoot"></a>Solución de problemas

El problema que puede experimentar más probablemente, es que el comando de RoboCopy produzca el error *"Volumen lleno"* en el lado de Windows Server. La nube por niveles actúa una vez cada hora para evacuar el contenido del disco local de Windows Server, que se ha sincronizado. Su objetivo es alcanzar el 99 % de espacio libre en el volumen.

Permita que el progreso de la sincronización y la nube por niveles liberen espacio en disco. Puede observarlo en el Explorador de archivos en Windows Server.

Cuando Windows Server tenga capacidad suficiente disponible, el problema se resolverá al volver a ejecutar el comando. Si se da esta situación, no se interrumpe nada y puede avanzar con confianza. La única consecuencia es el inconveniente de tener que volver a ejecutar el comando.

Consulte el vínculo de la sección siguiente para solucionar problemas de Azure File Sync.

## <a name="next-steps"></a>Pasos siguientes

Hay más información sobre los recursos compartidos de archivos de Azure y Azure File Sync. Los artículos siguientes le ayudarán a comprender las opciones avanzadas, los procedimientos recomendados y también contienen ayuda para la solución de problemas. Estos artículos se vinculan a la [documentación de recursos compartidos de archivos de Azure](storage-files-introduction.md) según corresponda.

* [Información general acerca de la migración](storage-files-migration-overview.md)
* [Información general sobre AFS](./storage-sync-files-planning.md)
* [Guía de implementación de AFS](./storage-how-to-create-file-share.md)
* [Solución de problemas de AFS](storage-sync-files-troubleshoot.md)
