---
title: Ámbitos de cifrado para Blob Storage (versión preliminar)
description: Los ámbitos de cifrado ofrecen la posibilidad de administrar el cifrado en el nivel del contenedor o de un blob individual. Se pueden usar ámbitos de cifrado para crear límites seguros entre los datos que residen en la misma cuenta de almacenamiento, pero que pertenecen a clientes distintos.
services: storage
author: tamram
ms.service: storage
ms.date: 03/05/2021
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 35395a30f7d58b9edb3aa7622a35e8c4a62dc76f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102211369"
---
# <a name="encryption-scopes-for-blob-storage-preview"></a>Ámbitos de cifrado para Blob Storage (versión preliminar)

Los ámbitos de cifrado ofrecen la posibilidad de administrar el cifrado en el nivel del contenedor o de un blob individual. Se pueden usar ámbitos de cifrado para crear límites seguros entre los datos que residen en la misma cuenta de almacenamiento, pero que pertenecen a clientes distintos.

De forma predeterminada, una cuenta de almacenamiento está cifrada con una clave cuyo ámbito es toda la cuenta de almacenamiento. Con un ámbito de cifrado, puede especificar que uno o varios contenedores estén cifrados con una clave cuyo ámbito sea exclusivo para esos contenedores.

Puede optar por usar claves administradas por Microsoft o claves administradas por el cliente almacenadas en Azure Key Vault para proteger y controlar el acceso a la clave que cifra sus datos. Distintos ámbitos de cifrado de una misma cuenta de almacenamiento pueden usar claves administradas por Microsoft o por el cliente.

Después de crear un ámbito de cifrado, puede especificar ese ámbito de cifrado en una solicitud para crear un contenedor o un blob. Para obtener más información acerca de cómo crear un ámbito de cifrado, consulte [Creación y administración de ámbitos de cifrado (versión preliminar)](encryption-scope-manage.md).

> [!IMPORTANT]
> Los ámbitos de cifrado se encuentran actualmente en **VERSIÓN PERLIMINAR**. Consulte [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para conocer los términos legales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.
>
> Para evitar costos inesperados, asegúrese de deshabilitar los ámbitos de cifrado que no necesite actualmente.
>
> Los ámbitos de cifrado no se admiten con las cuentas de almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) ni con las cuentas de almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS) durante la versión preliminar.

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="create-a-container-or-blob-with-an-encryption-scope"></a>Creación de un contenedor o blob con un ámbito de cifrado

Los blobs que se crean en un ámbito de cifrado se cifran con la clave especificada para ese ámbito. Puede especificar un ámbito de cifrado para un blob individual al crear el blob, o bien puede especificar un ámbito de cifrado predeterminado al crear un contenedor. Al especificar un ámbito de cifrado predeterminado en el nivel de un contenedor, todos los blobs de dicho contenedor se cifran con la clave asociada al ámbito predeterminado.

Al crear un blob en un contenedor que tiene un ámbito de cifrado predeterminado, puede especificar un ámbito de cifrado que invalide el ámbito de cifrado predeterminado si el contenedor está configurado para permitir invalidaciones del ámbito de cifrado predeterminado. Para evitar invalidaciones del ámbito de cifrado predeterminado, configure el contenedor para denegar las invalidaciones para un blob individual.

Las operaciones de lectura en un blob que pertenece a un ámbito de cifrado se producen de forma transparente, siempre y cuando el ámbito de cifrado no esté deshabilitado.

## <a name="disable-an-encryption-scope"></a>Deshabilitación de un ámbito de cifrado

Al deshabilitar un ámbito de cifrado, las operaciones de lectura o escritura posteriores realizadas con el ámbito de cifrado producirán un error con el código de error HTTP 403 (prohibido). Si vuelve a habilitar el ámbito de cifrado, las operaciones de lectura y escritura continuarán con normalidad.

Cuando se deshabilita un ámbito de cifrado, ya no se le facturará. Deshabilite los ámbitos de cifrado que no sean necesarios para evitar cargos innecesarios.

Si el ámbito de cifrado está protegido con claves administradas por el cliente, también puede eliminar la clave asociada en el almacén de claves para deshabilitar el ámbito de cifrado. Tenga en cuenta que las claves administradas por el cliente están protegidas por la protección de eliminación y purga temporal en el almacén de claves, y que una clave eliminada está sujeta al comportamiento definido por esas propiedades. Para más información, consulte uno de los siguientes temas en la documentación de Azure Key Vault:

- [Uso de la eliminación temporal con PowerShell](../../key-vault/general/key-vault-recovery.md)
- [Uso de la eliminación temporal con la CLI](../../key-vault/general/key-vault-recovery.md).

> [!NOTE]
> No es posible eliminar un ámbito de cifrado.

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de Azure Storage para datos en reposo](../common/storage-service-encryption.md)
- [Creación y administración de ámbitos de cifrado (versión preliminar)](encryption-scope-manage.md)
- [Claves administradas por el cliente para el cifrado de Azure Storage](../common/customer-managed-keys-overview.md)
- [¿Qué es Azure Key Vault?](../../key-vault/general/overview.md)