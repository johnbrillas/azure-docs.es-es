---
title: Administración de un servidor mediante la CLI de Azure en Azure Database for MySQL
description: Aprenda a administrar un servidor de Azure Database for MySQL desde la CLI de Azure.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 9/22/2020
ms.openlocfilehash: bd068f0cf76a8edefca854d72d5240c0becaf8fc
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "94542072"
---
# <a name="manage-an-azure-database-for-mysql-single-server-using-the-azure-cli"></a>Administración de un servidor de Azure Database for MySQL mediante la CLI de Azure

En este artículo se muestra cómo administrar los servidores únicos implementados en Azure. Entre las tareas de administración se incluyen el escalado de proceso y almacenamiento, el restablecimiento de contraseñas de administración y la visualización de detalles del servidor.

## <a name="prerequisites"></a>Requisitos previos
Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar. En este artículo es necesario que ejecute la versión 2.0 de la CLI de Azure, o cualquier versión posterior, de forma local. Para ver la versión instalada, ejecute el comando `az --version`. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

Será preciso que inicie sesión en su cuenta mediante el comando [az login](/cli/azure/reference-index#az-login). Tenga en cuenta la propiedad **id**, que hace referencia al **identificador de suscripción** para su cuenta de Azure.

```azurecli-interactive
az login
```

Seleccione la suscripción específica en su cuenta mediante el comando [az account set](/cli/azure/account). Anote el valor de **id** de la salida de **az login** para usarlo como valor del argumento **subscription** del comando. Si tiene varias suscripciones, elija la suscripción adecuada en la que se debe facturar el recurso. Para obtener todas las suscripciones, use [az account list](/cli/azure/account#az-account-list).

```azurecli
az account set --subscription <subscription id>
```

Si aún no ha creado un servidor, consulte esta [guía de inicio rápido](quickstart-create-mysql-server-database-using-azure-cli.md) para crear uno.

## <a name="scale-compute-and-storage"></a>Escalado de proceso y almacenamiento
Puede escalar verticalmente su plan de tarifa de forma sencilla, así como el proceso y el almacenamiento con el siguiente comando. Puede ver todas las operaciones del servidor que puede realizar con [az mysql server overview](/cli/azure/mysql/server).

```azurecli-interactive
az mysql server update --resource-group myresourcegroup --name mydemoserver --sku-name GP_Gen5_4 --storage-size 6144
```

Estos son los detalles de los argumentos anteriores:

**Configuración** | **Valor de ejemplo** | **Descripción**
---|---|---
name | mydemoserver | Escriba un nombre único para el servidor de Azure Database for MySQL. El nombre del servidor solo puede contener letras minúsculas, números y el carácter de guion (-). Debe contener entre 3 y 63 caracteres.
resource-group | myresourcegroup | Especifique el nombre del grupo de recursos de Azure.
sku-name|GP_Gen5_2|Escriba el nombre del plan de tarifa y la configuración del proceso. Sigue la convención {plan de tarifa} _{generación de procesos}_ {núcleos virtuales} en forma abreviada. Para más información, consulte los [planes de tarifa](./concepts-pricing-tiers.md).
storage-size | 6144 | La capacidad de almacenamiento del servidor (la unidad es megabytes). El mínimo es 5120 y aumenta en incrementos de 1024.

> [!Important]
> - El almacenamiento se puede escalar verticalmente, pero no reducir.
> - No se permite el escalado vertical de un plan de tarifa Básico a uno de uso general ni a uno optimizado para memoria. Puede escalar verticalmente de forma manual [mediante un script de Bash](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/upgrade-from-basic-to-general-purpose-or-memory-optimized-tiers/ba-p/830404) o [con MySQL Workbench](https://techcommunity.microsoft.com/t5/azure-database-support-blog/how-to-scale-up-azure-database-for-mysql-from-basic-tier-to/ba-p/369134).


## <a name="manage-mysql-databases-on-a-server"></a>Administración de bases de datos de MySQL en un servidor
Puede usar cualquiera de estos comandos para crear, eliminar, enumerar y ver las propiedades de una base de datos en el servidor.

| Cmdlet | Uso| Descripción |
| --- | ---| --- |
|[az mysql db create](/cli/azure/sql/db#az-mysql-db-create)|```az mysql db create -g myresourcegroup -s mydemoserver -n mydatabasename``` |Crea una base de datos.|
|[az mysql db delete](/cli/azure/sql/db#az-mysql-db-delete)|```az mysql db delete -g myresourcegroup -s mydemoserver -n mydatabasename```|Elimina la base de datos del servidor. Este comando no elimina el servidor. |
|[az mysql db list](/cli/azure/sql/db#az-mysql-db-list)|```az mysql db list -g myresourcegroup -s mydemoserver```|Enumera todas las bases de datos del servidor.|
|[az mysql db show](/cli/azure/sql/db#az-mysql-db-show)|```az mysql db show -g myresourcegroup -s mydemoserver -n mydatabasename```|Muestra más detalles de la base de datos.|

## <a name="update-admin-password"></a>Actualización de la contraseña del administrador
La contraseña del rol Administrador se puede cambiar con este comando.
```azurecli-interactive
az mysql server update --resource-group myresourcegroup --name mydemoserver --admin-password <new-password>
```

> [!Important]
>  Asegúrese de que tiene su longitud oscila entre 8 y 128 caracteres.
> Debe contener caracteres de tres de las categorías siguientes: Letras del alfabeto inglés mayúsculas y minúsculas, números y caracteres no alfanuméricos.

## <a name="delete-a-server"></a>Eliminación de un servidor
Si solo quiere eliminar el servidor único de MySQL, puede ejecutar el comando [az mysql server delete](/cli/azure/mysql/server#az-mysql-server-delete).

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Pasos siguientes
- [Reinicio de un servidor](howto-restart-server-cli.md)
- [Restauración de un servidor en mal estado](howto-restore-server-cli.md)
- [Supervisión y ajuste del servidor](concepts-monitoring.md)