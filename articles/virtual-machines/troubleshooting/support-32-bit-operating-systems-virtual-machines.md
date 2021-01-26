---
title: Compatibilidad con sistemas operativos de 32 bits en Azure Virtual Machines | Microsoft Docs
description: Información sobre los sistemas operativos que se admiten en Azure Virtual Machines
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/18/2019
ms.author: v-miegge
ms.openlocfilehash: 81b7efdd6bca0471719c11d130be95405f4d54e1
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2021
ms.locfileid: "98210195"
---
# <a name="support-for-32-bit-operating-systems-in-azure-virtual-machines"></a>Compatibilidad con sistemas operativos de 32 bits en máquinas virtuales de Azure

Microsoft Azure ahora permite a los usuarios traer sus sistemas operativos Windows de 32 bits a Azure. Solo se admiten VHD especializados y las imágenes generalizadas no funcionarán en Azure. Dado que algunos de estos sistemas operativos ya han alcanzado el final de la duración del contrato de soporte técnico, es posible que Microsoft no ofrezca soporte técnico adicional para ellos. Además, no se ofrece soporte técnico para sistemas operativos basados en Linux o en Berkeley Software Distribution (BSD) que se ejecutan en una máquina virtual (VM) de Microsoft Azure.

> [!NOTE]
> La plataforma Azure tiene una limitación de espacio de direcciones de memoria impuesta en máquinas virtuales que ejecutan sistemas operativos de 32 bits según la cual solo puede estar disponible 1 GB de memoria para la máquina virtual (*especialmente en SKU de cliente como Win7 o Win10*) y el resto de la memoria de la máquina virtual se mostrará como reservada en la máquina virtual invitada. Se trata de un problema conocido y actualmente no tenemos una fecha de solución estimada. Se recomienda cambiar a las versiones del sistema operativo de 64 bits.
> 

## <a name="more-information"></a>Más información

Para obtener más información acerca de los sistemas operativos que se admiten en Azure Virtual Machines, vaya a los siguientes artículos de Microsoft Knowledge Base:

* [Soporte técnico del software de servidor de Microsoft para máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)
* [Soporte técnico para Linux y la tecnología de código abierto en Azure](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure)

## <a name="references"></a>Referencias

* [Más información sobre las actualizaciones de seguridad ampliadas gratis para Windows Server 2008/R2 en Azure](https://www.microsoft.com/cloud-platform/windows-server-2008)
* [Más información sobre la compatibilidad con imágenes especializadas de Windows Server 2008 SP2 de 32 bits en Azure](/windows-server/get-started/uploading-specialized-ws08-image-to-azure)
* [Más información sobre la compatibilidad con la migración de imágenes de Windows Server 2008 a Azure mediante Azure Site Recovery](../../site-recovery/migrate-tutorial-windows-server-2008.md)
* [Más información sobre los sistemas operativos compatibles con la extensión de Azure](https://support.microsoft.com/help/4078134/azure-extension-supported-operating-systems)
* [Más información sobre cómo ejecutar Windows Server 2003 en Microsoft Azure](https://support.microsoft.com/help/3206074/running-windows-server-2003-on-microsoft-azure)

## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda con cualquier aspecto de este artículo, póngase en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/forums/).

Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de Soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
