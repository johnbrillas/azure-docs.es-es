---
title: Uso de directivas de acceso en Azure HPC Cache
description: Creación y aplicación de directivas de acceso personalizadas para limitar el acceso de cliente a los destinos de almacenamiento en Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 12/28/2020
ms.author: v-erkel
ms.openlocfilehash: 795b194eb7cd31e633128c22ddffe808b32e07da
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97802426"
---
# <a name="use-client-access-policies"></a>Uso de directivas de acceso de cliente

En este artículo se explica cómo crear y aplicar directivas de acceso de cliente personalizadas para los destinos de almacenamiento.

Las directivas de acceso de cliente controlan cómo los clientes pueden conectarse a las exportaciones del destino de almacenamiento. Puede controlar aspectos como el uso de root_squash y el acceso de lectura y escritura en el nivel de host cliente o de red.

Las directivas de acceso se aplican a una ruta de acceso de espacio de nombres, lo que significa que puede usar diferentes directivas de acceso para dos exportaciones diferentes de un sistema de almacenamiento NFS.

Esta característica está destinada a flujos de trabajo en los que es necesario controlar el modo en que los distintos grupos de clientes acceden a los destinos de almacenamiento.

Si no necesita un control preciso sobre el acceso al destino de almacenamiento, puede usar la directiva predeterminada o puede personalizarla con reglas adicionales.

## <a name="create-a-client-access-policy"></a>Creación de una directiva de acceso de cliente

Use la página **Directivas de acceso de cliente** de Azure Portal para crear y administrar directivas. <!-- is there AZ CLI for this? -->

[![Captura de pantalla de la página de directivas de acceso de cliente. Se definen varias directivas y otras se expanden para mostrar sus reglas](media/policies-overview.png)](media/policies-overview.png#lightbox)

Cada directiva se compone de reglas. Las reglas se aplican a los hosts en orden desde el ámbito más pequeño (el host) al más grande (valor predeterminado). La primera regla que coincide se aplica y se omiten las reglas posteriores.

Para crear una nueva directiva de acceso, haga clic en el botón **+ Agregar directiva de acceso** en la parte superior de la lista. Asigne un nombre a la nueva directiva de acceso y especifique al menos una regla.

![captura de pantalla de la hoja de edición de directivas de acceso con varias reglas rellenas. Haga clic en Aceptar para guardar la regla.](media/add-policy.png)

En el resto de esta sección se explican los valores que puede usar en las reglas.

### <a name="scope"></a>Ámbito

El valor del ámbito y el filtro de direcciones funcionan de forma conjunta para definir qué clientes se ven afectados por la regla.

Úselos para especificar si la regla se aplica a un cliente individual (host), a un intervalo de direcciones IP (red) o a todos los clientes (valor predeterminado).

Seleccione el valor adecuado de **ámbito** para su regla:

* **Host**: la regla se aplica a un cliente individual.
* **Red**: la regla se aplica a los clientes de un intervalo de direcciones IP.
* **Predeterminado**: la regla se aplica a todos los clientes.

Las reglas de una directiva se evalúan en ese orden. Una vez que una solicitud de montaje de cliente coincide con una regla, se omiten las demás.

### <a name="address-filter"></a>Filtro de direcciones

El valor **Filtro de direcciones** especifica qué clientes coinciden con la regla.

Si establece el ámbito en **Host**, solo podrá especificar una dirección IP en el filtro. En el caso de establecer el ámbito en el valor **Predeterminado**, no se podrá especificar ninguna dirección IP en el campo **Filtro de direcciones** porque el ámbito predeterminado coincide con todos los clientes.

Especifique la dirección IP o el intervalo de direcciones para esta regla. Utilice la notación CIDR (por ejemplo: 0.1.0.0/16) para especificar un intervalo de direcciones.

### <a name="access-level"></a>Nivel de acceso

Establezca qué privilegios se conceden a los clientes que coinciden con el ámbito y el filtro.

Las opciones son **lectura/escritura**, **solo lectura** o **sin acceso**.

### <a name="suid"></a>SUID

Active la casilla **SUID** para permitir que los archivos del almacenamiento establezcan los identificadores de usuario al acceder.

En general, se usa SUID para aumentar los privilegios de un usuario de forma temporal para que este pueda realizar una tarea relacionada con ese archivo.

### <a name="submount-access"></a>Acceso al submontaje

Active esta casilla para permitir que los clientes especificados monten directamente los subdirectorios de esta exportación.

### <a name="root-squash"></a>Uso de root_squash

Elija si desea establecer o no el uso de root_squash para los clientes que coincidan con esta regla.

Este valor le permite autorizar el uso de root_squash en el nivel de exportación del almacenamiento. También puede [establecer el uso de root_squash en el nivel de caché](configuration.md#configure-root-squash).

Si activa el uso de root_squash, también debe establecer el valor del identificador de usuario anónimo en una de estas opciones:

* **-2** (nadie)
* **65534** (nadie)
* **-1** (sin acceso)
* **65535** (sin acceso)
* **0** (raíz sin privilegios)

## <a name="update-access-policies"></a>Actualización de directivas de acceso

Puede editar o eliminar directivas de acceso de la tabla en la página **Directivas de acceso de cliente**.

Haga clic en el nombre de la directiva para abrirla para su edición.

Para eliminar una directiva, marque la casilla situada junto a su nombre en la lista y, a continuación, haga clic en el botón **Eliminar** en la parte superior de la lista. No se puede eliminar la directiva denominada "default".

> [!NOTE]
> No se puede eliminar una directiva de acceso que esté en uso. Quite las directivas de cualquier ruta de acceso de espacio de nombres que la incluya antes de intentar eliminarla.

## <a name="next-steps"></a>Pasos siguientes

* Aplique directivas de acceso en las rutas de acceso del espacio de nombres de los destinos de almacenamiento. Lea [Configuración del espacio de nombres agregado](add-namespace-paths.md) para aprender cómo hacerlo.
