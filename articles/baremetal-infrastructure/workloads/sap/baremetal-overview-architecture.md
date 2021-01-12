---
title: Información general sobre la versión preliminar de BareMetal Infrastructure en Azure
description: Información general sobre la infraestructura de BareMetal en Azure.
ms.custom: references_regions
ms.topic: conceptual
ms.date: 1/4/2021
ms.openlocfilehash: eb4dc129719dc410f7101598e3d72e68f17809c1
ms.sourcegitcommit: aeba98c7b85ad435b631d40cbe1f9419727d5884
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860990"
---
#  <a name="what-is-baremetal-infrastructure-preview-on-azure"></a>¿Qué es la versión preliminar de BareMetal Infrastructure de Azure?

BareMetal Infrastructure de Azure proporciona una solución segura para migrar cargas de trabajo personalizadas empresariales. Las instancias de BareMetal se corresponden con el hardware de servidor o host no compartido que se le ha asignado. Desbloquea el traslado de la solución local con cargas de trabajo especializadas que requieren hardware certificado, licencias y contratos de soporte técnico. Azure se encarga automáticamente de la supervisión y el mantenimiento de la infraestructura, mientras que la supervisión del sistema operativo (SO) invitado y la supervisión de aplicaciones son responsabilidad del usuario.

BareMetal Infrastructure proporciona una ruta para modernizar el panorama de la infraestructura a la vez que mantiene las inversiones y la arquitectura existentes. Con BareMetal Infrastructure, puede trasladar cargas de trabajo especializadas a Azure, lo que le permite acceder a los servicios de Azure e integrarlos con baja latencia.

## <a name="sku-availability-in-azure-regions"></a>Disponibilidad de SKU en regiones de Azure
Está disponible BareMetal Infrastructure para cargas de trabajo especializadas y de uso general, empezando por cuatro regiones basadas en las marcas de la revisión 4.2 (rev. 4.2):
- Oeste de Europa
- Norte de Europa
- Este de EE. UU. 2
- Centro-sur de EE. UU.

>[!NOTE]
>**Rev 4.2** es la infraestructura BareMetal Infrastructure cuyo nombre se ha cambiado más reciente que usa la arquitectura Rev 4 existente.  Rev 4 proporciona mayor proximidad a los hosts de máquina virtual (VM) de Azure. Presenta mejoras significativas en la latencia de red entre las máquinas virtuales de Azure y las unidades de instancia de BareMetal implementadas en los sellos o filas de Rev 4.  Puede tener acceso a sus instancias de BareMetal y administrarlas en Azure Portal. 

## <a name="support"></a>Soporte técnico
BareMetal Infrastructure es compatible con ISO 27001, ISO 27017, SOC 1 y SOC 2.  También usa un modelo Traiga su propia licencia (BYOL): SO, carga de trabajo especializada y aplicaciones de terceros.  

En cuanto recibe acceso a la raíz y control total, asume la responsabilidad de los siguientes aspectos:
- Diseñar e implementar soluciones de copia de seguridad y recuperación, alta disponibilidad y recuperación ante desastres
- Proporcionar licencias, seguridad y soporte técnico para el sistema operativo y el software de terceros

Microsoft es responsable de lo siguiente:
- Proporcionar el hardware para cargas de trabajo especializadas 
- Aprovisionar el SO

:::image type="content" source="media/baremetal-support-model.png" alt-text="Modelo de soporte técnico de BareMetal Infrastructure" border="false":::

## <a name="compute"></a>Proceso
BareMetal Infrastructure ofrece varias SKU para cargas de trabajo especializadas. Las SKU disponibles abarcan desde el sistema de dos sockets más pequeño hasta el sistema de veinticuatro sockets. Use las SKU específicas de la carga de trabajo para la carga de trabajo especializada.

El sello de instancias de BareMetal en sí combina los componentes siguientes:

- **Computación**: servidores basados en una generación distinta de procesadores Intel Xeon que proporcionan la funcionalidad de computación necesaria y poseen certificación para la carga de trabajo especializada.

- **Red:** tejido de red unificada de alta velocidad que interconecta los componentes de computación, almacenamiento y LAN.

- **Storage:** infraestructura a la que se accede por medio de un tejido de red unificada.

Dentro de la infraestructura multiinquilino del sello de BareMetal, los clientes se implementan en inquilinos aislados. Al implementar un inquilino, asigne un nombre a una suscripción de Azure dentro de la inscripción de Azure. Esta suscripción de Azure es en la que se facturan las instancias de BareMetal.

>[!NOTE]
>Un cliente implementado en la instancia de BareMetal se aísla en un inquilino. Un inquilino se aísla en el nivel de redes, almacenamiento y proceso de otros inquilinos. Las unidades de almacenamiento y proceso asignadas a los distintos inquilinos no pueden verse ni comunicarse entre sí en las instancias de BareMetal.

## <a name="os"></a>SO
Durante el aprovisionamiento de la instancia de BareMetal, puede seleccionar el sistema operativo que desea instalar en los equipos. 

>[!NOTE]
>Recuerde que BareMetal Infrastructure es un modelo de BYOL.

Las versiones del sistema operativo Linux disponibles son:
- Red Hat Enterprise Linux (RHEL) 7.6
- SUSE Linux Enterprise Server (SLES)
   - SLES 12 SP2
   - SLES 12 SP3
   - SLES 12 SP4
   - SLES 12 SP5
   - SLES 15 SP1

## <a name="storage"></a>Storage
Las instancias de BareMetal basadas en un tipo de SKU específico incluyen un almacenamiento de NFS predefinido para el tipo de carga de trabajo específico. Al aprovisionar BareMetal, puede aprovisionar más almacenamiento en función del crecimiento estimado mediante el envío de una solicitud de soporte técnico. Todo el almacenamiento incluye un disco All-Flash en la revisión 4.2 con compatibilidad con NFSv3 y NFSv4. Estará disponible la última versión del SSD NVMe de la revisión 4.5. Para más información sobre el tamaño del almacenamiento, consulte la sección [Tipo de carga de trabajo de BareMetal](../../../virtual-machines/workloads/sap/get-started.md).

>[!NOTE]
>El almacenamiento usado para BareMetal cumple los requisitos de los [estándares federales de procesamiento de la información (FIPS), publicación 140-2](/microsoft-365/compliance/offering-fips-140-2) estadounidenses, que ofrecen cifrado en reposo de forma predeterminada. Los datos se almacenan de forma segura en los discos.

## <a name="networking"></a>Redes
La arquitectura del servicio de red de Azure es un componente clave para implementar las carga de trabajo especializadas correctamente en instancias de BareMetal. Es probable que no todos los sistemas de TI ya se encuentren en Azure. Azure ofrece tecnología de red, de forma que se asemeja a un centro de datos virtual para las implementaciones de software locales. La funcionalidad de red de Azure necesaria para las instancias de BareMetal es la siguiente:

- Las redes virtuales de Azure se conectan al circuito ExpressRoute que conecta con los recursos de red locales.
- Un circuito ExpressRoute que conecta el entorno local a Azure debería tener un ancho de banda mínimo de 1 Gbps o más.
- Active Directory y DNS ampliados en Azure o completamente en ejecución en Azure.

Usar ExpressRoute permite ampliar la red local a la nube de Microsoft mediante una conexión privada con la ayuda de un proveedor de conectividad. Puede habilitar **ExpressRoute Premium** para ampliar la conectividad a través de los límites geopolíticos o usar **ExpressRoute Local** para una transferencia de datos rentable entre la ubicación cercana a la región de Azure que desee.

Las instancias de BareMetal se aprovisionan en el intervalo de direcciones IP del servidor de red virtual de Azure.

:::image type="content" source="media/baremetal-infrastructure-portal/baremetal-infrastructure-diagram.png" alt-text="Diagrama de BareMetal Infrastructure de Azure" lightbox="media/baremetal-infrastructure-portal/baremetal-infrastructure-diagram.png" border="false":::

La arquitectura que se muestra se divide en tres secciones:
- **Izquierda**: muestra la infraestructura local del cliente que ejecuta aplicaciones diferentes, que se conecta a través del enrutador perimetral de asociados o local, como Equinix. Para más información, vea [Ubicaciones y proveedores de conectividad: Azure ExpressRoute](../../../expressroute/expressroute-locations.md).
- **Centro**: muestra [ExpressRoute](../../../expressroute/expressroute-introduction.md) aprovisionado con la suscripción de Azure que ofrece conectividad con la red perimetral de Azure.
- **Derecha**: muestra IaaS de Azure y, en este caso, el uso de máquinas virtuales para hospedar las aplicaciones, que se aprovisionan en la red virtual de Azure.
- **Abajo**: muestra cómo usar la puerta de enlace de ExpressRoute habilitada con [FastPath de ExpressRoute](../../../expressroute/about-fastpath.md) para la conectividad de BareMetal con una latencia baja.   
   >[!TIP]
   >Para admitir esto, la puerta de enlace de ExpressRoute debe ser UltraPerformance.  Para más información, vea [Acerca de las puertas de enlace de red virtual de ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md).

## <a name="next-steps"></a>Pasos siguientes

El paso siguiente consiste en identificar e interactuar con las unidades de instancia de BareMetal mediante Azure Portal.

> [!div class="nextstepaction"]
> [Administración de instancias de BareMetal mediante Azure Portal](baremetal-infrastructure-portal.md)