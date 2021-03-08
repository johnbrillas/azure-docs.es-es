---
title: 'Controles de Azure Security Benchmark v2: seguridad de los puntos de conexión'
description: Seguridad de los puntos de conexión de Azure Security Benchmark v2
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 02/22/2021
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 7672f4eb4530dbfb5d039b066fe7cf6eaf79e5a7
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101718648"
---
# <a name="security-control-v2-endpoint-security"></a>Control de seguridad V2: seguridad de los puntos de conexión

La seguridad de los puntos de conexión cubre los controles de detección y respuesta de puntos de conexión. Esto incluye el uso de la detección y respuesta de puntos de conexión (EDR) y el servicio antimalware para los puntos de conexión en entornos de Azure.

Para ver la instancia de Azure Policy integrada aplicable, consulte [Detalles de la iniciativa integrada de cumplimiento normativo de Azure Security Benchmark: seguridad de los puntos de conexión](../../governance/policy/samples/azure-security-benchmark#endpoint-security).

## <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1: uso de la detección y respuesta de puntos de conexión (EDR)

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP 800-53 r4 |
|--|--|--|--|
| ES-1 | 8.1 | SI-2, SI-3, SC-3 |

Habilite las funcionalidades de detección y respuesta de puntos de conexión (EDR) para servidores y clientes, y realice la integración con los procesos de operaciones de seguridad y SIEM.

Microsoft Defender para punto de conexión proporciona la funcionalidad de EDR como parte de una plataforma de seguridad empresarial para puntos de conexión para evitar, detectar, investigar y responder a amenazas avanzadas.

- [Información general de Microsoft Defender para punto de conexión](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)

- [Microsoft Defender para punto de conexión para servidores Windows Server](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)

- [Microsoft Defender para punto de conexión para servidores que no son Windows Server](/windows/security/threat-protection/microsoft-defender-atp/configure-endpoints-non-windows)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security)

- [Información sobre amenazas](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="es-2-use-centrally-managed-modern-anti-malware-software"></a>ES-2: Uso de software antimalware moderno administrado centralmente

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP 800-53 r4 |
|--|--|--|--|
| ES-2 | 8.1 | SI-2, SI-3, SC-3 |

Uso de una solución antimalware para punto de conexión administrada centralmente capaz de realizar análisis periódicos y en tiempo real

Azure Security Center puede identificar automáticamente el uso de varias soluciones antimalware populares para las máquinas virtuales e informar del estado de ejecución de la protección del punto de conexión, así como realizar recomendaciones. 

Microsoft Antimalware para Azure Cloud Services es el antimalware predeterminado para las máquinas virtuales (VM) Windows. En el caso de las VM Linux, use una solución antimalware de terceros. También puede usar la detección de amenazas de Azure Security Center para los servicios de datos con el fin de detectar malware cargado en las cuentas de Azure Storage. 

- [Configuración de Microsoft Antimalware para Cloud Services y Virtual Machines](../fundamentals/antimalware.md)

- [Soluciones de protección de punto de conexión compatibles](../../security-center/security-center-services.md?tabs=features-windows#supported-endpoint-protection-solutions-)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security)

- [Información sobre amenazas](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="es-3-ensure-anti-malware-software-and-signatures-are-updated"></a>ES-3: Asegúrese de que se han actualizado el software y las firmas antimalware

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP 800-53 r4 |
|--|--|--|--|
| ES-3 | 8,2 | SI-2, SI-3 |

Asegúrese de que las firmas antimalware se actualizan de forma rápida y coherente.

Siga las recomendaciones de Azure Security Center: "Proceso y aplicaciones" para asegurarse de que todos los puntos de conexión estén actualizados con las firmas más recientes. De forma predeterminada, Microsoft Antimalware instalará automáticamente las últimas firmas y actualizaciones del motor. En el caso de Linux, asegúrese de que las firmas estén actualizadas en la solución antimalware de terceros.

- [Implementación de Microsoft Antimalware para Azure Cloud Services y Virtual Machines](../fundamentals/antimalware.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security)

- [Información sobre amenazas](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Valoración y recomendaciones de Endpoint Protection en Azure Security Center](../../security-center/security-center-endpoint-protection.md)