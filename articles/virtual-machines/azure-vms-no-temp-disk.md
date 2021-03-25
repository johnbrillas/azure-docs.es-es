---
title: Preguntas más frecuentes sobre los tamaños de máquina virtual de Azure sin disco temporal local
description: En este artículo se proporcionan respuestas a las preguntas más frecuentes (P+F) sobre los tamaños de máquina virtual de Microsoft Azure que no tienen disco temporal local.
author: brbell
ms.service: virtual-machines
ms.topic: conceptual
ms.subservice: sizes
ms.author: brbell
ms.reviewer: mimckitt
ms.date: 06/15/2020
ms.openlocfilehash: bd4dcbdc7ab13d18ef7f2d7102c56d1bd8d8758d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104582109"
---
# <a name="azure-vm-sizes-with-no-local-temporary-disk"></a>Tamaños de máquina virtual de Azure sin disco temporal local 
En este artículo se proporcionan respuestas a las preguntas frecuentes sobre los tamaños de máquina virtual de Azure que no tienen disco temporal local (por ejemplo, sin disco temporal local). Para más información sobre estos tamaños de máquina virtual, consulte [Especificaciones para las series Dv4 y Dsv4 (cargas de trabajo de uso general)](dv4-dsv4-series.md) o [Especificaciones para las series Ev4 y Esv4 (cargas de trabajo optimizadas para memoria)](ev4-esv4-series.md).

## <a name="what-does-no-local-temp-disk-mean"></a>¿Qué significa que no hay disco temporal local? 
Tradicionalmente, teníamos tamaños de máquina virtual (por ejemplo, Standard_D2s_v3 y Standard_E48_v3) que incluían un pequeño disco local (por ejemplo, una unidad D:). Ahora, con estos nuevos tamaños de máquina virtual, ese pequeño disco local ya no existe; sin embargo, todavía puede adjuntar discos HDD estándar, SSD Premium o SSD Ultra.

## <a name="what-if-i-still-want-a-local-temp-disk"></a>¿Qué ocurre si quiero un disco temporal local?
Si la carga de trabajo requiere un disco temporal local, también tenemos disponibles los nuevos tamaños de máquina virtual [Ddv4 y Ddsv4](ddv4-ddsv4-series.md) o [Edv4 y Edsv4](edv4-edsv4-series.md). Estos tamaños ofrecen un disco temporal un 50 % mayor en comparación con los tamaños de la serie v3 anteriores.

> [!NOTE]
> El disco temporal local no es persistente; para asegurarse de que los datos sean persistentes, use las opciones HDD estándar, SSD Premium o SSD Ultra. 

## <a name="what-are-the-differences-between-these-new-vm-sizes-and-the-general-purpose-dv3dsv3-or-the-memory-optimized-ev3esv3-vm-sizes-that-i-am-used-to"></a>¿Cuáles son las diferencias entre estos nuevos tamaños de máquina virtual y el tamaño Dv3/Dsv3 De uso general o los tamaños de máquina virtual Ev3/Esv3 optimizados para memoria a los que estoy acostumbrado? 
| Familias de máquinas virtuales v3 con disco temporal local   | Nuevas familias v4 con disco temporal local | Nuevas familias v4 sin disco temporal local |
|---|---|---|
| Dv3   | Ddv4 | Dv4 |
| Dsv3 | Ddsv4  | Dsv4 |
| Ev3   | Edv4  | Ev4 |
| Esv3 | Edsv4 |    Esv4 | 

## <a name="can-i-resize-a-vm-size-that-has-a-local-temp-disk-to-a-vm-size-with-no-local-temp-disk"></a>¿Puedo cambiar el tamaño de un tamaño de máquina virtual que tiene un disco temporal local a un tamaño de máquina virtual sin disco temporal local?  
No. Las únicas combinaciones permitidas para el cambio de tamaño son: 

1. Máquina virtual (con disco temporal local)-> máquina virtual (con disco temporal local) y 
2. Máquina virtual (sin disco temporal local)-> máquina virtual (sin disco temporal local). 

Si le interesa una solución alternativa, consulte la siguiente pregunta.

> [!NOTE]
> Si una imagen depende del disco de recursos o si existe un archivo de paginación o un archivo de intercambio en el disco temporal local, las imágenes sin disco no funcionarán; en su lugar, use la alternativa "con disco". 

## <a name="how-do-i-migrate-from-a-vm-size-with-local-temp-disk-to-a-vm-size-with-no-local-temp-disk"></a>¿Cómo puedo migrar de un tamaño de máquina virtual que tiene un disco temporal local a un tamaño de máquina virtual sin disco temporal local?  
Para realizar la migración, siga estos pasos: 

1. Conéctese a la máquina virtual que tenga un disco temporal local (por ejemplo, una unidad D:) como administrador local.
2. Siga las instrucciones de la sección "Mover temporalmente pagefile.sys a la unidad C" de [Uso de la unidad de disco D: como unidad de datos en una máquina virtual Windows](./windows/change-drive-letter.md) para mover el archivo de paginación del disco temporal local (unidad D:) a la unidad C:.

   > [!NOTE]
   > Siga las instrucciones de la sección "Mover temporalmente el archivo pagefile.sys a la unidad C" de Uso de la unidad de disco D: como unidad de datos en una máquina virtual Windows para mover el archivo de paginación del disco temporal local (unidad D:) a la unidad C:. **La desviación de los pasos descritos dará lugar al mensaje de error: "No se puede cambiar el tamaño de la VM porque el cambio del tamaño de la VM de disco de recursos a disco que no es de recursos y viceversa no se permite.**

3. Para realizar una instantánea de la máquina virtual, siga los pasos descritos en [Creación de una instantánea mediante el portal o la CLI de Azure](./linux/snapshot-copy-managed-disk.md). 
4. Use la instantánea para crear una nueva máquina virtual sin disco (por ejemplo, de las series Dv4, Dsv4, Ev4, Esv4) siguiendo los pasos descritos en [Creación de una máquina virtual a partir de una instantánea con la CLI](/previous-versions/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot). 

## <a name="do-these-vm-sizes-support-both-linux-and-windows-operating-systems-os"></a>¿Estos tamaños de máquina virtual admiten sistemas operativos (SO) Linux y Windows?
Sí.

## <a name="will-this-break-my-custom-scripts-custom-images-or-os-images-that-have-scratch-files-or-page-files-on-a-local-temp-disk"></a>¿Se interrumpirán mis scripts personalizados, imágenes personalizadas o imágenes de sistema operativo que tengan archivos temporales o archivos de paginación en un disco temporal local?
Si la imagen de sistema operativo personalizada apunta al disco temporal local, es posible que la imagen no funcione correctamente con este tamaño sin disco.

## <a name="have-questions-or-feedback"></a>¿Tiene preguntas o comentarios?
Rellene el [formulario de comentarios]( https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRUMzdCQkw0OVVRTldFUUtXSTlLQVBPUkVHSy4u). 

## <a name="next-steps"></a>Pasos siguientes 
En este documento ha obtenido más información sobre las preguntas frecuentes relacionadas con las máquinas virtuales de Azure sin disco temporal local. Para más información sobre estos tamaños de máquina virtual, consulte los siguientes artículos:

- [Especificaciones para las series Dv4 y Dsv4 (carga de trabajo de uso general)](dv4-dsv4-series.md)
- [Especificaciones para las series Ev4 y Esv4 (carga de trabajo optimizada para memoria)](ev4-esv4-series.md)