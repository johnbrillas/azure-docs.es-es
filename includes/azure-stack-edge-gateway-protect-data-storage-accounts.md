---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 2e0a7ca8fb9eaafbc50c0ce60799dd68d83b2fa3
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98901190"
---
Su dispositivo está asociado a una cuenta de almacenamiento que se utiliza como destino para los datos en Azure. El acceso a la cuenta de almacenamiento se controla mediante la suscripción y las dos claves de acceso de almacenamiento de 512 bits asociadas a esa cuenta de almacenamiento.

Una de las claves se utiliza para la autenticación cuando el dispositivo Data Box Edge accede a la cuenta de almacenamiento. La otra clave se mantiene en reserva, por lo que puede rotar periódicamente las claves.

Por motivos de seguridad, muchos centros de datos requieren que las claves se roten. Se recomienda seguir estos procedimientos recomendados para la rotación de claves:

- La clave de la cuenta de almacenamiento es similar a la contraseña raíz de la cuenta de almacenamiento. Proteja cuidadosamente la clave de cuenta. No distribuya la contraseña a otros usuarios, no la codifique ni la guarde en cualquier lugar en texto sin formato que sea accesible para otros.
- [Vuelva a generar la clave de cuenta](../articles/storage/common/storage-account-keys-manage.md#manually-rotate-access-keys) mediante Azure Portal si cree podría estar en peligro.
- Periódicamente, el administrador de Azure debe cambiar o volver a generar la clave principal o secundaria mediante la sección Storage de Azure Portal para acceder directamente a la cuenta de almacenamiento.
