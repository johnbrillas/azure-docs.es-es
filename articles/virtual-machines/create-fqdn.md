---
title: Creación de un nombre de dominio completo para una VM en Azure Portal
description: Aprenda a crear un nombre de dominio completo para una máquina virtual en Azure Portal.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 1/12/2020
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aadf4000da4ab88a3c3b4dee37d2179eb49d39d7
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98132071"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Creación de un nombre de dominio completo en Azure Portal para una máquina virtual Linux

Cuando crea una máquina virtual (VM) en [Azure Portal](https://portal.azure.com), se crea automáticamente un recurso de IP pública para la máquina virtual. Use esta dirección IP para acceder de forma remota a la máquina virtual. Aunque el portal no crea [un nombre de dominio completo](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), o FQDN, puede agregar uno cuando se crea la máquina virtual. Este artículo muestra los pasos para crear un nombre DNS o FQDN. 

## <a name="create-a-fqdn"></a>Creación de un FQDN
En este artículo se supone que ya ha creado una máquina virtual. Si es necesario, puede crear una máquina virtual [Linux](./linux/quick-create-portal.md) o [Windows](./windows/quick-create-portal.md) en el portal. Una vez que la máquina virtual está en funcionamiento, siga estos pasos:


1. Seleccione la máquina virtual en el portal. 
1. En el menú de la izquierda, seleccione **Configuración**.
1. En **Etiqueta de nombre de DNS**, escriba el prefijo que desea usar.
1. En la parte superior de la página, seleccione **Guardar**.
1. Vuelva a la hoja de información general de la máquina virtual; para ello, seleccione **Información general** en el menú de la izquierda. 
1. Compruebe que el *nombre DNS* se muestra correctamente. 

## <a name="next-steps"></a>Pasos siguientes

También puede administrar DNS con [zonas de Azure DNS](../dns/dns-getstarted-portal.md).

