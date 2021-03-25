---
title: Línea base de seguridad de Azure para Azure Resource Graph
description: La base de referencia de seguridad de Azure Resource Graph proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: resource-graph
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: e5e01c8d1ac16e5e8be405660a0726796789e645
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101738980"
---
# <a name="azure-security-baseline-for-azure-resource-graph"></a>Línea base de seguridad de Azure para Azure Resource Graph

Esta línea base de seguridad aplica la guía de la [versión 1.0 de Azure Security Benchmark](../../../security/benchmarks/overview-v1.md) a Azure Resource Graph. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por los **controles de seguridad** definidos por Azure Security Benchmark y las instrucciones relacionadas aplicables a Azure Resource Graph. Se han excluido los **controles** no aplicables a Azure Resource Graph.

 
Para ver cómo se adapta por completo Azure Resource Graph a Azure Security Benchmark, consulte el [archivo completo de asignación de línea base de seguridad de Azure Resource Graph](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: identidad y control de acceso](../../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía**: Azure Resource Graph proporciona acceso a tipos de recursos y propiedades según el control de acceso basado en rol de Azure (RBAC de Azure). Audite y revise el acceso concedido a las entidades de seguridad (usuarios, grupos y cuentas de servicio) periódicamente para asegurarse de que las consultas devuelven resultados para los recursos adecuados.

- [Permisos en Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview#permissions-in-azure-resource-graph)

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../../../active-directory/governance/access-reviews-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../../../security/benchmarks/security-control-data-protection.md).*

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Uso de RBAC de Azure para controlar el acceso a los recursos 

**Guía**: uso de RBAC de Azure para controlar el acceso a datos y recursos. Para usar Azure Resource Graph, también debe tener el acceso adecuado a los recursos que desea consultar. El ámbito de este acceso debe ser de solo lectura y solo se puede conceder al personal requerido.

- [Permisos en Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview#permissions-in-azure-resource-graph)

- [Configuración de Azure RBAC](../../../role-based-access-control/role-assignments-rest.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="inventory-and-asset-management"></a>Administración de recursos y del inventario

*Para más información, consulte [Azure Security Benchmark: inventario y administración de recursos](../../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: Use Azure Resource Graph para consultar y detectar todos los recursos admitidos en las suscripciones, los grupos de administración y los inquilinos. Asegúrese de que tiene los permisos adecuados en el suscriptor y de que puede enumerar todas las suscripciones de Azure, así como los recursos de las suscripciones.

- [Creación de consultas con Azure Resource Graph](../first-query-portal.md)

- [Descripción de Azure RBAC](../../../role-based-access-control/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Cree un inventario de los recursos de Azure aprobados y el software aprobado para los recursos de proceso según las necesidades de la organización. Use Azure Resource Graph para consultar los recursos de Azure aprobados y el historial de cambios (versión preliminar) para revisar las instantáneas y ver lo que ha cambiado.

- [Consulta de recursos de Azure con Azure Resource Graph](../first-query-portal.md)

- [Obtención de los cambios del recurso](../how-to/get-resource-changes.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Instrucciones**: Use Azure Resource Graph para consultar y detectar los recursos en las suscripciones, los grupos de administración y los inquilinos. Asegúrese de que todos los recursos de Azure del entorno estén aprobados.

- [Consulta de recursos de Azure con Azure Resource Graph](../first-query-portal.md)

- [Ejemplos: Consultas de inicio de Azure Resource Graph](../samples/starter.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](/azure/security/benchmarks/security-baselines-overview).
