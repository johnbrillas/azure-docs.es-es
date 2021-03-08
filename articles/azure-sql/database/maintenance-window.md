---
title: Ventana de mantenimiento
description: Aprenda a configurar la ventana de mantenimiento de Azure SQL Database y SQL Managed Instance.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.topic: conceptual
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: sstein
ms.custom: references_regions
ms.date: 03/02/2021
ms.openlocfilehash: 9dc4d17ea95362dd915bd1dfdfd82f4cdec611b8
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692817"
---
# <a name="maintenance-window-preview"></a>Ventana de mantenimiento (versión preliminar)
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

La característica de ventana de mantenimiento permite configurar las programaciones de ventanas de mantenimiento predecibles de [Azure SQL Database](sql-database-paas-overview.md) y [SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md). 

Para más información sobre los eventos de mantenimiento, consulte [Planeación de eventos de mantenimiento de Azure en Azure SQL Database e Instancia administrada de Azure SQL](planned-maintenance.md).

## <a name="overview"></a>Información general

Azure ejecuta periódicamente actualizaciones de mantenimiento planeadas en los recursos de Azure SQL Database y SQL Managed Instance. Suelen incluirse actualizaciones del hardware o el software subyacente, como el sistema operativo y el motor de SQL. Durante una actualización de mantenimiento, los recursos están completamente disponibles y accesibles, pero algunas de las actualizaciones de mantenimiento requieren una conmutación por error, ya que Azure deja las instancias sin conexión durante un breve período de tiempo para ejecutar las actualizaciones de mantenimiento (ocho segundos de promedio).  Las actualizaciones de mantenimiento planeadas se producen una vez cada 35 días de promedio, lo que significa que el cliente puede esperar aproximadamente un evento de mantenimiento planeado al mes por instancia de Azure SQL Database o SQL Managed Instance, y solo durante las franjas de tiempo de la ventana de mantenimiento seleccionadas por el cliente.   

La ventana de mantenimiento se ha concebido para cargas de trabajo empresariales que no son resistentes a incidencias de conectividad intermitente que pueden producirse como resultado de los eventos de mantenimiento planeados.

La ventana de mantenimiento se puede configurar mediante Azure Portal, PowerShell, la CLI o la API de Azure. Se puede configurar durante la creación, o bien para instancias existentes de Azure SQL Database y SQL Managed Instance.

### <a name="gain-more-predictability-with-maintenance-window"></a>Ganar en previsibilidad con la ventana de mantenimiento

De forma predeterminada, las instancias de Azure SQL Database y SQL Managed Instance se actualizan solo entre las 17:00 y las 8:00 (hora local) diariamente, para evitar interrupciones durante el horario comercial con mayor actividad. La hora local viene determinada por la [región de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) que hospeda el recurso. Para ajustar aún más el horario de las actualizaciones de mantenimiento de su base de datos, elija entre otras dos franjas de tiempo de la ventana de mantenimiento:

* Ventana **predeterminada**: de 17:00 a 8:00 (hora local) de lunes a domingo. 
* Ventana de día de la semana: de 22:00 a 6:00 (hora local) de lunes a jueves.
* Ventana de fin de semana: de 22:00 a 6:00 (hora local) de viernes a domingo.

Una vez seleccionada la ventana de mantenimiento, las actualizaciones de mantenimiento planeadas solo se producirán durante la ventana que haya elegido.   

> [!Note]
> Además de las actualizaciones de mantenimiento planeadas, en circunstancias excepciones los eventos de mantenimiento no planeados pueden dar lugar a la falta de disponibilidad. 

### <a name="cost-and-eligibility"></a>Costo y elegibilidad

La elección de una ventana de mantenimiento no tiene coste alguno para los siguientes [tipos de oferta](https://azure.microsoft.com/support/legal/offer-details/) de suscripción: pago por uso, proveedor de soluciones en la nube (CSP), Microsoft Enterprise o Contrato de cliente de Microsoft.

> [!Note]
> Una oferta de Azure es el tipo de la suscripción a Azure que tiene. Algunos ejemplos de ofertas de Azure son una suscripción con [tarifas de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/), [Azure bajo licencia Open](https://azure.microsoft.com/en-us/offers/ms-azr-0111p/) y [Visual Studio Enterprise](https://azure.microsoft.com/en-us/offers/ms-azr-0063p/). Cada uno de los planes u ofertas tienen diferentes términos y ventajas. La oferta o el plan se muestra en la información general de la suscripción. Para obtener más información sobre cómo cambiar su suscripción a una oferta distinta, consulte [Cambio de la suscripción de Azure a una oferta distinta](/azure/cost-management-billing/manage/switch-azure-offer).

## <a name="advance-notifications"></a>Notificaciones anticipadas

Las notificaciones de mantenimiento se pueden configurar para avisar a los clientes sobre los próximos eventos de mantenimiento planeados con 24 horas de antelación, en el momento del mantenimiento y cuando la ventana de mantenimiento termina. Para más información, consulte [Notificaciones anticipadas](advance-notifications.md).

## <a name="availability"></a>Disponibilidad

### <a name="supported-service-level-objectives"></a>Objetivos de nivel de servicio admitidos

La elección de una ventana de mantenimiento que no sea la predeterminada está disponible para todos los objetivos de nivel de servicio, **excepto por**:
* Hiperescala 
* Núcleo virtual Gen4 heredado
* Básico, S0 y S1 
* DC, Fsv2, serie M

### <a name="azure-region-support"></a>Compatibilidad con regiones de Azure

La elección de una ventana de mantenimiento que no sea la predeterminada está disponible actualmente en las siguientes regiones:

- Este de Australia
- Sudeste de Australia
- Sur de Brasil
- Centro de Canadá
- Centro de EE. UU.
- Este de EE. UU.
- Este de EE. UU. 2
- Japón Oriental
- Centro-Norte de EE. UU.
- Norte de Europa
- Centro-Sur de EE. UU.
- Sudeste de Asia
- Sur de Reino Unido 2
- Oeste de Europa
- Oeste de EE. UU.
- Oeste de EE. UU. 2

## <a name="gateway-maintenance-for-azure-sql-database"></a>Mantenimiento de la puerta de enlace para Azure SQL Database

Para aprovechar al máximo la ventaja de las ventanas de mantenimiento, asegúrese de que las aplicaciones cliente usen la directiva de conexión de redireccionamiento. Redirigir es la directiva de conexión recomendada, mediante la que los clientes establecen conexiones directamente con el nodo que hospeda la base de datos. De este modo, se reduce la latencia y se mejora el rendimiento.  

* En Azure SQL Database, cualquier conexión que use la directiva de conexión de proxy podría verse afectada por la ventana de mantenimiento elegida y por una ventana de mantenimiento del nodo de puerta de enlace. Sin embargo, las conexiones de cliente que usan la directiva de conexión de redireccionamiento recomendada no se ven afectadas por la conmutación por error de mantenimiento del nodo de puerta de enlace. 

* En Azure SQL Managed Instance, los nodos de puerta de enlace se encuentran [en el clúster virtual](../../azure-sql/managed-instance/connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture) y tienen la misma ventana de mantenimiento que la instancia administrada, por lo que el uso de la directiva de conexión de proxy no expone potencialmente las conexiones a una ventana de mantenimiento adicional.

Para obtener más información sobre la directiva de conexión de cliente en Azure SQL Database consulte la [directiva de conexión de Azure SQL Database](../database/connectivity-architecture.md#connection-policy). 

Para obtener más información sobre la directiva de conexión de cliente en Azure SQL Managed Instance, consulte los [tipos de conexión de Azure SQL Managed Instance](../../azure-sql/managed-instance/connection-types-overview.md).


## <a name="next-steps"></a>Pasos siguientes

* [Notificaciones anticipadas](advance-notifications.md)
* [Configuración de ventanas de mantenimiento](maintenance-window-configure.md)

## <a name="learn-more"></a>Más información

* [P+F sobre la ventana de mantenimiento](maintenance-window-faq.yml)
* [Azure SQL Database](sql-database-paas-overview.md) 
* [Instancia administrada de SQL](../managed-instance/sql-managed-instance-paas-overview.md)
* [Planeación de eventos de mantenimiento de Azure en Azure SQL Database e Instancia administrada de Azure SQL](planned-maintenance.md)




