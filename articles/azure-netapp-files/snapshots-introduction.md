---
title: Funcionamiento de las instantáneas de Azure NetApp Files | Microsoft Docs
description: Explica el funcionamiento de las instantáneas de Azure NetApp Files, incluidos los métodos para crear instantáneas, restaurarlas y cómo usarlas en la configuración de la replicación entre regiones.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/12/2021
ms.author: b-juche
ms.openlocfilehash: beadd250ec4472b894f0f474b1057ad44cf474ed
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98133521"
---
# <a name="how-azure-netapp-files-snapshots-work"></a>Funcionamiento de las instantáneas de Azure NetApp Files

En este artículo se explica cómo funcionan las instantáneas de Azure NetApp Files. La tecnología de instantáneas de Azure NetApp Files ofrece estabilidad, escalabilidad y capacidad de recuperación más rápida sin ningún impacto en el rendimiento. La tecnología de instantáneas de Azure NetApp Files proporciona la base para las soluciones de protección de datos, incluidas las restauraciones de archivos únicas, las restauraciones de volumen y clones y la replicación entre regiones. 

Para conocer los pasos sobre el uso de instantáneas de volumen, consulte [Administración de instantáneas mediante Azure NetApp Files](azure-netapp-files-manage-snapshots.md). Para conocer las consideraciones sobre la administración de instantáneas en la replicación entre regiones, consulte [Requisitos y consideraciones del uso de la replicación entre regiones](cross-region-replication-requirements-considerations.md).

## <a name="what-volume-snapshots-are"></a>Concepto de instantáneas de volumen  

Una instantánea de Azure NetApp Files es una imagen del sistema de archivos (volumen) en un momento dado. Puede funcionar como una copia de seguridad en línea ideal. Puede usar una instantánea para [crear un nuevo volumen](azure-netapp-files-manage-snapshots.md#restore-a-snapshot-to-a-new-volume), [restaurar un archivo](azure-netapp-files-manage-snapshots.md#restore-a-file-from-a-snapshot-using-a-client) o [revertir un volumen](azure-netapp-files-manage-snapshots.md#revert-a-volume-using-snapshot-revert). En datos de aplicación específicos que se almacenan en volúmenes de Azure NetApp Files, pueden ser necesarios pasos adicionales para garantizar la coherencia de la aplicación.  

Las instantáneas de sobrecarga baja son posibles gracias a las características únicas de la tecnología de virtualización de volúmenes subyacente que es parte de Azure NetApp Files. Como una base de datos, esta capa usa punteros a los bloques de datos reales en el disco. Sin embargo, a diferencia de una base de datos, no reescribe los bloques existentes; en cambio, escribe los datos actualizados en un nuevo bloque y cambia el puntero. Una instantánea de Azure NetApp Files simplemente manipula los punteros a un bloque, de modo que crea una vista "congelada" de un volumen. Esta permite las aplicaciones accedan a versiones anteriores de los archivos y jerarquías de directorios sin una programación especial. No se copian los bloques de datos reales. Como tal, las instantáneas ahorran tiempo al crearse; se generan casi al instante, independientemente del tamaño del volumen. Las instantáneas también ahorran espacio de almacenamiento, ya que no consumen espacio y solo se conservan los bloques delta entre las instantáneas y el volumen activo. 

Los siguientes diagramas ilustran estos conceptos:  

![Diagramas que muestran los conceptos básicos de las instantáneas](../media/azure-netapp-files/snapshot-concepts.png)

En los diagramas, se toma una instantánea en la Figura 1a. En la Figura 1b, los datos modificados se escriben en un *nuevo bloque* y el puntero se actualiza. Pero el puntero de la instantánea sigue señalando al *bloque escrito anteriormente*, lo que ofrece una vista en directo de los datos y una histórica. En la Figura 1c se toma otra instantánea. Ahora tiene acceso a tres generaciones de datos (los datos en directo, la instantánea 2 y la instantánea 1, por orden de antigüedad) sin ocupar el espacio de volumen que sería necesario para tres copias completas. 

Una instantánea solo toma una copia de los metadatos del volumen (*tabla de inode*). Tarda unos segundos en crearse, independientemente del tamaño del volumen, de la capacidad usada o del nivel de actividad en el volumen. Por lo tanto, tomar una instantánea de un volumen 100 TiB requiere el mismo tiempo (casi nada) necesario para tomar una instantánea de un volumen de 100 GiB. Después de crear una instantánea, los cambios en los archivos de datos se reflejan en la versión activa de los archivos, como siempre.

Mientras tanto, los bloques de datos a los que se apunta desde una instantánea permanecen estables e inmutables. Debido a la naturaleza del "redireccionamiento en escritura" de los volúmenes de Azure NetApp Files, una instantánea no incurre en la sobrecarga de rendimiento y no consume ningún espacio. Puede almacenar hasta 255 instantáneas por volumen a lo largo del tiempo, todas las cuales están disponibles como versiones de solo lectura y en línea de los datos. Esto que consume tan poca capacidad como el número de bloques cambiados entre cada instantánea. Los bloques modificados se almacenan en el volumen activo. Los bloques a los que se apunta en las instantáneas se conservan (como elementos de solo lectura) en el volumen para protegerlos y se transforman únicamente cuando se han borrado todos los punteros (en el volumen activo y las instantáneas). Por lo tanto, el uso de volumen aumentará con el tiempo, ya sea por nuevos bloques de datos o por bloques de datos (modificados) que estén almacenado en instantáneas.

 En el siguiente diagrama se muestran las instantáneas de un volumen y el espacio utilizado a lo largo del tiempo: 

![Diagrama que muestran las instantáneas de un volumen y el espacio utilizado a lo largo del tiempo](../media/azure-netapp-files/snapshots-used-space-over-time.png)

Dado que una instantánea de volumen registra solo los cambios de bloque a partir de la última instantánea, proporciona las siguientes ventajas clave:

* Las instantáneas ***ahorran almacenamiento** _.   
    Las instantáneas consumen un espacio de almacenamiento mínimo porque no copian los bloques de datos de todo el volumen. Dos instantáneas tomadas en secuencia solo se diferencian en los bloques agregados o modificados en el intervalo de tiempo entre ambas. Este comportamiento de aumento de bloques limita el consumo de la capacidad de almacenamiento asociada. Muchas implementaciones de instantáneas alternativas consumen volúmenes de almacenamiento iguales al sistema de archivos activo, lo que aumenta los requisitos de capacidad de almacenamiento. En función de las tasas de cambio diario en el _nivel de bloques* de la aplicación, las instantáneas de Azure NetApp Files consumirán más o menos capacidad, pero solo en los datos modificados. El consumo medio diario de instantáneas va de solo el 1 al 5 % de la capacidad de volumen usada para muchos volúmenes de aplicaciones o hasta el 20 a 30 % para volúmenes como los volúmenes de base de datos de SAP HANA. Asegúrese de [supervisar el uso del volumen y las instantáneas](azure-netapp-files-metrics.md#volumes) para conocer el consumo de la capacidad de instantáneas en relación con el número de instantáneas creadas y conservadas.   

* Las instantáneas son de ***rápida creación, replicación, restauración o clonación** _.   
    Solo toma unos segundos crear, replicar, restaurar o clonar una instantánea, independientemente del tamaño del volumen y el nivel de actividades. Puede crear una instantánea de volumen [a petición](azure-netapp-files-manage-snapshots.md#create-an-on-demand-snapshot-for-a-volume). También puede usar las [directivas de instantáneas](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies) para especificar cuándo Azure NetApp Files debe crear automáticamente una instantánea y el número de instantáneas que se conservarán para un volumen.  Puede lograr la coherencia de la aplicación mediante la orquestación de instantáneas con el nivel de aplicación; por ejemplo, mediante la [herramienta AzAcSnap](azacsnap-introduction.md) para SAP HANA.

Las _instantáneas no tienen ningún impacto en el ***rendimiento** _ del volumen.   
    Debido a la naturaleza del "redireccionamiento en escritura" de la tecnología subyacente, el almacenamiento o la retención de las instantáneas de Azure NetApp Files no tiene ningún impacto en el rendimiento, incluso con una actividad de datos intensiva. En la mayoría de los casos, la eliminación de una instantánea también tiene poco impacto en el rendimiento. 

Las _instantáneas proporcionan ***escalabilidad** _ porque se pueden crear con frecuencia y se pueden conservar muchas.   
    Los volúmenes de Azure NetApp Files admiten hasta 255 instantáneas. La capacidad de almacenar un gran número de instantáneas de bajo impacto que se crean con frecuencia aumenta la probabilidad de que se pueda recuperar correctamente la versión deseada de los datos.

Las _instantáneas proporcionan ***visibilidad del usuario** _ y la _*_capacidad de recuperación de archivos_*_.   
El alto rendimiento, la escalabilidad y la estabilidad de la tecnología de instantáneas de Azure NetApp Files ofrecen una copia de seguridad en línea ideal para la recuperación controlada por el usuario. Las instantáneas pueden estas disponibles para el usuario para fines de restauración de archivos, directorios o volúmenes. Las soluciones adicionales permiten realizar copias de las copias de seguridad en almacenamiento sin conexión o [realizar réplicas entre regiones](cross-region-replication-introduction.md) para fines de retención o recuperación ante desastres.

## <a name="ways-to-create-snapshots"></a>Métodos para crear instantáneas   

Puede usar varios métodos para crear y mantener las instantáneas:

_ Manualmente (a petición), mediante:   
    * Las herramientas de [Azure Portal](azure-netapp-files-manage-snapshots.md#create-an-on-demand-snapshot-for-a-volume), la [API REST](/rest/api/netapp/snapshots), la [CLI de Azure](/cli/azure/netappfiles/snapshot) o [PowerShell](/powershell/module/az.netappfiles/new-aznetappfilessnapshot).
    * Los scripts (consulte [ejemplos](azure-netapp-files-solution-architectures.md#sap-tech-community-and-blog-posts)).

* Automáticamente, mediante:
    * Las directivas de instantánea a través de las herramientas de [Azure Portal](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies), la [API REST](/rest/api/netapp/snapshotpolicies), la [CLI de Azure](/cli/azure/netappfiles/snapshot/policy) o [PowerShell](/powershell/module/az.netappfiles/new-aznetappfilessnapshotpolicy).
    * Herramientas de instantáneas coherentes con aplicaciones, como [AzAcSnap](azacsnap-introduction.md).

## <a name="how-volumes-and-snapshots-are-replicated-cross-region-for-dr"></a>Cómo se replican los volúmenes y las instantáneas entre regiones para la recuperación ante desastres  

Azure NetApp Files admite la [replicación entre regiones](cross-region-replication-introduction.md) con fines de recuperación ante desastres. La replicación de Azure NetApp Files entre regiones usa la tecnología SnapMirror. Así que solo los bloques modificados se envían a través de la red en un formato comprimido y eficaz. Una vez que se inicia una replicación entre regiones entre los volúmenes, todo el contenido del volumen (es decir, los bloques de datos almacenados reales) se transfiere una sola vez. Esta operación se denomina de *transferencia de base de referencia*. Después de la transferencia inicial, solo se transferirán los bloques modificados (capturados en instantáneas). El resultado es una réplica asincrónica 1:1 del volumen de origen, incluidas todas las instantáneas. Este comportamiento sigue un mecanismo de replicación completo y siempre incremental. Esta tecnología reduce la cantidad de datos necesarios para replicar entre regiones, lo que ahorra costos de transferencia de datos. También reduce el tiempo de replicación. Puede lograr un objetivo de punto de recuperación (RPO) más pequeño, ya que se pueden crear y transferir más instantáneas con mayor frecuencia con transferencias de datos limitadas. Además, elimina la necesidad de los mecanismos de replicación basados en host al evitar el costo de las licencias de software y máquinas virtuales.

En el siguiente diagrama se muestra el tráfico de instantáneas en los escenarios de replicación entre regiones: 

![Diagrama que muestra el tráfico de instantáneas en los escenarios de replicación entre regiones](../media/azure-netapp-files/snapshot-traffic-cross-region-replication.png)

## <a name="ways-to-restore-data-from-snapshots"></a>Métodos de restauración de datos desde instantáneas  

La tecnología de instantáneas de Azure NetApp Files mejora en gran medida la frecuencia y la confiabilidad de las copias de seguridad. Incurre en una sobrecarga de rendimiento mínima y se puede crear de manera segura en un volumen activo. Las instantáneas de Azure NetApp Files permiten realizar restauraciones casi instantáneas, seguras y, opcionalmente, administradas por el usuario. En esta sección se describen las distintas formas en que se puede acceder a los datos o restaurarlos desde las instantáneas de Azure NetApp Files.

### <a name="restoring-files-or-directories-from-snapshots"></a>Restauración de archivos o directorios desde instantáneas 

Si la [visibilidad de la ruta de acceso de la instantánea](azure-netapp-files-manage-snapshots.md#edit-the-hide-snapshot-path-option) no se establece en `hidden`, los usuarios podrán acceder directamente a las instantáneas para recuperarse de la eliminación accidental, el daño o la modificación de los datos. La seguridad de los archivos y directorios se conserva en la instantánea, y las instantáneas son de solo lectura por diseño. Por lo tanto, la restauración es segura y sencilla. 

En el diagrama siguiente se muestra el acceso de archivo o directorio a una instantánea: 

![Diagrama que muestra el acceso de archivo o directorio a una instantánea](../media/azure-netapp-files/snapshot-file-directory-access.png)

En el diagrama, la instantánea 1 solo consume los bloques delta entre el volumen activo y el momento en que se creó la instantánea. Pero cuando se accede a la instantánea a través de la ruta de acceso de la instantánea de volumen, los datos *aparecen* como si se tratase de la capacidad de volumen total al crear la instantánea. Al acceder a las carpetas de instantáneas, los usuarios pueden restaurar los datos al copiar los archivos y directorios de una instantánea específica.

Del mismo modo, se puede obtener acceso de solo lectura a las instantáneas de volúmenes de replicación entre regiones de destino para la recuperación de datos en la región de recuperación ante desastres.  

En el siguiente diagrama se muestra el acceso a instantáneas en los escenarios de replicación entre regiones: 

![Diagrama que muestra el acceso a instantáneas en los escenarios de replicación entre regiones](../media/azure-netapp-files/snapshot-access-cross-region-replication.png)

Consulte [Restauración de un archivo desde una instantánea mediante un cliente](azure-netapp-files-manage-snapshots.md#restore-a-file-from-a-snapshot-using-a-client) acerca de la restauración de archivos o directorios individuales desde instantáneas.

### <a name="restoring-cloning-a-snapshot-to-a-new-volume"></a>Restauración (clonación) de una instantánea a un nuevo volumen

Puede restaurar las instantáneas de Azure NetApp Files en un volumen independiente y separado. Esta operación es casi instantánea, independientemente del tamaño del volumen y de la capacidad consumida. El volumen recién creado está accesible casi inmediatamente, mientras que el volumen y los bloques de datos reales de la instantánea se están copiando. En función del tamaño y la capacidad del volumen, este proceso puede tardar un tiempo considerable durante el que no se puede eliminar el volumen y la instantánea primarios. No obstante, se puede acceder al volumen después de su creación inicial, mientras el proceso de copia está en curso en segundo plano. Esta funcionalidad permite crear volúmenes rápidamente para la recuperación de datos o la clonación de volúmenes con fines de pruebas y desarrollo. Dada la naturaleza del proceso de copia de datos, el consumo del grupo de capacidades de almacenamiento se duplicará cuando se complete la restauración y el nuevo volumen mostrará la capacidad activa completa de la instantánea original. Una vez completado este proceso, el volumen será independiente y se desasociará del volumen original, y tanto el volumen como la instantánea de origen podrán administrarse o quitarse de manera independiente del nuevo volumen.

En el siguiente diagrama se muestra un nuevo volumen que se creó mediante la restauración (clonación) de una instantánea:   

![Diagrama que muestra un nuevo volumen que se creó al restaurar una instantánea](../media/azure-netapp-files/snapshot-restore-clone-new-volume.png)

Se pueden realizar las mismas operaciones en las instantáneas replicadas en un volumen de recuperación ante desastres (DR). Cualquier instantánea se puede restaurar en un volumen nuevo, incluso cuando la replicación entre regiones permanece activa o está en curso. Esta funcionalidad permite crear sin interrupciones entornos de prueba y desarrollo en una región de recuperación ante desastres, con lo que los datos comienzan a usarse, mientras que los volúmenes replicados podrían usarse solo para fines de recuperación ante desastres. Este caso de uso permite aislar las pruebas y el desarrollo de la producción, lo que elimina el impacto potencial en los entornos de producción.  

En el diagrama siguiente se muestra la restauración (clonación) de volúmenes mediante las instantáneas del volumen de destino para la recuperación ante desastres mientras se lleva a cabo la replicación entre regiones:  

![Diagrama que muestra la restauración de volúmenes mediante una instantánea del volumen de destino para recuperación ante desastres](../media/azure-netapp-files/snapshot-restore-clone-target-volume.png)

Consulte la sección [Restauración de una instantánea a un nuevo volumen](azure-netapp-files-manage-snapshots.md#restore-a-snapshot-to-a-new-volume) acerca de las operaciones de restauración de volúmenes.

### <a name="restoring-reverting-a-snapshot-in-place"></a>Restauración (reversión) de una instantánea en contexto

En algunos casos, dado que el volumen nuevo usará la capacidad de almacenamiento, quizá no se necesite o no resulte adecuado crear un nuevo volumen a partir de una instantánea. Para recuperar rápidamente los datos dañados (por ejemplo, daños en una base de datos o ataques de ransomware), puede ser más adecuado restaurar una instantánea dentro del propio volumen. Esta operación se puede realizar mediante la funcionalidad de reversión de instantáneas de Azure NetApp Files. Esta funcionalidad le permite revertir rápidamente un volumen al estado en que se encontraba cuando se tomó una instantánea determinada. En la mayoría de los casos, la reversión de un volumen es mucho más rápida que la restauración de archivos individuales a partir de una instantánea en el sistema de archivos activo, sobre todo en volúmenes de gran tamaño con varios TiB. 

La reversión de una instantánea de volumen es casi instantánea y solo tarda unos segundos en completarse, incluso en el caso de los volúmenes de mayor tamaño. Los metadatos del volumen activo (*tabla de inode*) se sustituyen por los metadatos de la instantánea en el momento de creación de dicha instantánea, lo que revierte el volumen a ese punto específico en el tiempo. No es necesario copiar ningún bloque de datos para que la reversión surta efecto. De este modo, requiere menos espacio que la restauración de una instantánea en un nuevo volumen. 

En el diagrama siguiente se muestra la reversión de un volumen a una instantánea anterior:  

![Diagrama que muestra la reversión de un volumen a una instantánea anterior](../media/azure-netapp-files/snapshot-volume-revert.png)

> [!IMPORTANT]
> Los datos del sistema de archivos activo que se escribieron y las instantáneas que se tomaron después de tomar la instantánea seleccionada se perderán. La operación de reversión de instantáneas reemplazará todos los datos del volumen de destino por los datos de la instantánea seleccionada. Debe prestar atención al contenido y a la fecha de creación de la instantánea al seleccionar una instantánea. No se puede deshacer la operación de reversión de instantáneas.  

Consulte la sección [Reversión de un volumen mediante la reversión de instantáneas](azure-netapp-files-manage-snapshots.md#revert-a-volume-using-snapshot-revert) sobre cómo usar esta característica.

## <a name="how-snapshots-are-deleted"></a>Cómo se eliminan las instantáneas 

Las instantáneas usan la capacidad de almacenamiento. Por lo tanto, normalmente no se conservan indefinidamente. Para la protección de datos, la retención y la capacidad de recuperación, normalmente se conserva un número de instantáneas (creadas en varios momentos) en línea durante cierto tiempo en función de los requisitos de RPO, RTO y el Acuerdo de Nivel de Servicio. No obstante, no suele ser necesario conservar las instantáneas más antiguas en el servicio de almacenamiento y es posible que deban eliminarse para liberar espacio. Un administrador puede eliminar cualquier instantánea (no necesariamente en su orden de creación) en cualquier momento. 

> [!IMPORTANT]
> La operación de eliminación de instantáneas no se puede deshacer. Debe conservar las copias sin conexión del volumen para fines de retención y protección de datos. 

Cuando se elimine una instantánea, se quitarán todos los punteros de esa instantánea que apuntan a los bloques de datos existentes. Cuando un bloque de datos no tiene ningún puntero que apunte a él (ya sea desde el volumen activo u otras instantáneas del volumen), dicho bloque de datos se devuelve al espacio libre del volumen para usarse en el futuro. Por lo tanto, la eliminación de instantáneas normalmente libera más capacidad en un volumen que eliminar datos del volumen activo, ya que los bloques de datos suelen estar capturados en instantáneas creadas previamente. 

En el diagrama siguiente se muestra el impacto en el consumo de almacenamiento al eliminar la instantánea 3 de un volumen:  

![Diagrama que muestra el impacto en el consumo de almacenamiento al eliminar instantáneas](../media/azure-netapp-files/snapshot-delete-storage-consumption.png)

Asegúrese de [supervisar el consumo de los volumen e instantáneas](azure-netapp-files-metrics.md#volumes) y de comprender cómo interactúa el consumo de la aplicación, el volumen activo y la instantánea. 

Consulte la sección [Eliminar instantáneas](azure-netapp-files-manage-snapshots.md#delete-snapshots) sobre cómo administrar la eliminación de instantáneas. Consulte la sección [Administración de directivas de instantánea](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies) sobre cómo automatizar este proceso.

## <a name="next-steps"></a>Pasos siguientes

* [Administración de instantáneas mediante Azure NetApp Files](azure-netapp-files-manage-snapshots.md)
* [Supervisión de métricas de volumen e instantáneas](azure-netapp-files-metrics.md#volumes)
* [Solución de problemas de directivas de instantáneas](troubleshoot-snapshot-policies.md)
* [Límites de recursos para Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Vídeo Instantáneas de Azure NetApp Files 101](https://www.youtube.com/watch?v=uxbTXhtXCkw)
* [Instantánea de NetApp Snapshot: biblioteca de vídeos de NetApp](https://tv.netapp.com/detail/video/2579133646001/snapshot)



