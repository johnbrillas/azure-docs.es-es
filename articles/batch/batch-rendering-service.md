---
title: Introducción a la representación
description: Introducción al uso de Azure para la representación e información general de las funcionalidades de representación con Azure Batch
author: mscurrell
ms.author: markscu
ms.date: 01/14/2021
ms.topic: how-to
ms.openlocfilehash: 1cd07f9322837c03e15aaeabec993820deb3170a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98232121"
---
# <a name="rendering-using-azure"></a>Representación mediante Azure

La representación es el proceso de tomar los modelos 3D y convertirlos en imágenes 2D. Los archivos de escena 3D se crean en aplicaciones como Autodesk 3ds Max, Autodesk Maya y Blender.  Las aplicaciones de representación, como Autodesk Maya, Autodesk Arnold, Chaos Group V-Ray y Blender Cycles, generan imágenes 2D.  A veces, las imágenes individuales se crean a partir de los archivos de escena. Sin embargo, es común modelar y representar varias imágenes y luego combinarlas en una animación.

La carga de trabajo de representación se usa mucho como efectos especiales (VFX) en el sector multimedia y de entretenimiento. La representación también se usa en muchos otros sectores como la publicidad, el mercado minorista, petróleo y gas y fabricación.

El proceso de representación es muy intenso desde el punto de vista computacional; puede haber muchos fotogramas e imágenes para generar y cada imagen puede tardar muchas horas en representarse.  Por lo tanto, la representación es una carga de trabajo de procesamiento por lotes perfecta que puede aprovechar Azure para ejecutar muchas representaciones en paralelo y usar una amplia variedad de hardware, incluidas las GPU.

## <a name="why-use-azure-for-rendering"></a>¿Por qué usar Azure para la representación?

Por muchas razones, la representación es una carga de trabajo perfectamente adecuada para Azure:

* Los trabajos de representación se pueden dividir en muchas partes que se pueden ejecutar en paralelo mediante varias máquinas virtuales:
  * Las animaciones constan de muchos fotogramas y cada fotograma se puede representar en paralelo.  Cuantas más máquinas virtuales haya disponibles para procesar cada fotograma, más rápido podrán generarse todos los fotogramas y la animación.
  * Algunos programas de software de representación permiten que los fotogramas individuales se dividan en varias partes, como iconos o segmentos.  Cada parte se puede representar por separado y luego combinarse en la imagen final cuando todas las partes han terminado.  Cuantas más máquinas virtuales haya disponibles, más rápido se podrá representar un fotograma.
* Los proyectos de representación puede requerir una gran escala:
  * Los fotogramas individuales pueden ser complejos y requerir muchas horas para representarse, incluso en hardware de gama alta; las animaciones pueden constar de cientos de miles de fotogramas.  Se requiere una gran inmensa cantidad de proceso para representar las animaciones de alta calidad en un período de tiempo razonable.  En algunos casos, se han utilizado más de 100 000 núcleos para representar miles de fotogramas en paralelo.
* Los proyectos de representación se basan en proyecto y requieren cantidades variables de proceso:
  * Asignar capacidad de proceso y almacenamiento cuando sea necesario, escalarla o reducirla verticalmente según la carga durante un proyecto y quitarla cuando haya finalizado un proyecto.
  * Pagar por capacidad cuando se asigna, pero no pagar por ella cuando no hay ninguna carga, como entre proyectos.
  * Preste atención a las ráfagas debidas a cambios inesperados; escale más alto si hay cambios inesperados tarde en un proyecto y esos cambios se deben procesar en un horario ajustado.
* Elija entre una gran variedad de hardware conforme a la aplicación, la carga de trabajo y el período de tiempo:
  * Hay una gran variedad de hardware disponible en Azure que se puede asignar y administrar con Batch.
  * Según el proyecto, el requisito puede ser la mejor relación precio/rendimiento o el mejor rendimiento global.  Diferentes escenas y/o aplicaciones de representación tendrán requisitos de memoria distintos.  Alguna aplicación de representación puede aprovechar las GPU para el mejor rendimiento o determinadas características. 
* Las máquinas virtuales de prioridad baja o [al contado](https://azure.microsoft.com/pricing/spot/) reducen el costo:
  * Las máquinas virtuales de prioridad baja y al contado están disponibles con un gran descuento en comparación con las máquinas virtuales convencionales, y son adecuadas para algunos tipos de trabajos.
  
## <a name="existing-on-premises-rendering-environment"></a>Entorno de representación local existente

El caso más común es el de una granja de representaciones local administrada mediante una aplicación de administración de representaciones, como PipelineFX Qube, Royal Render, Thinkbox Deadline o una aplicación personalizada.  El requisito es ampliar la capacidad de la granja de representación local mediante máquinas virtuales de Azure.

Los servicios y la infraestructura de Azure se usan para crear un entorno híbrido donde Azure se emplea para complementar la capacidad local. Por ejemplo:

* Use una [red virtual](../virtual-network/virtual-networks-overview.md) para colocar los recursos de Azure en la misma red que la granja de representaciones local.
* Use [Avere vFXT for Azure](../avere-vfxt/avere-vfxt-overview.md) o [Azure HPC Cache](../hpc-cache/hpc-cache-overview.md) para almacenar en caché los archivos de origen de Azure con el fin de reducir el uso y la latencia del ancho de banda y aumentar el rendimiento.
* Asegúrese de que el servidor de licencias existente se encuentre en la red virtual y compre las licencias que sean necesarias para satisfacer la capacidad adicional basada en Azure.

## <a name="no-existing-render-farm"></a>Granja de representación no existente

Las estaciones de trabajo cliente pueden estar realizando la representación, pero la carga de representación aumenta y se tarda demasiado en usar únicamente la capacidad de la estación de trabajo.

Hay dos opciones principales disponibles:

* Implementar un administrador de representaciones local, como Royal Render, y configurar un entorno híbrido para usar Azure cuando se necesite más capacidad o rendimiento. Un administrador de representaciones está específicamente adaptado a las cargas de trabajo de representación e incluye complementos para las aplicaciones cliente más conocidas, lo que permite el envío sencillo de los trabajos de representación.

* Una solución personalizada que use Azure Batch para asignar y administrar la capacidad de proceso, así como para proporcionar la programación de trabajos para ejecutar los trabajos de representación.

## <a name="next-steps"></a>Pasos siguientes

 Aprenda a [usar la infraestructura y los servicios de Azure para ampliar una granja de representaciones local existente](https://azure.microsoft.com/solutions/high-performance-computing/rendering/).

Más información sobre las [funcionalidades de representación de Azure Batch](batch-rendering-functionality.md).
