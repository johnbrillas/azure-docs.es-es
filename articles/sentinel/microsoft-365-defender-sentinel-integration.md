---
title: Integración de Microsoft 365 Defender en Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo la integración de Microsoft 365 Defender en Azure Sentinel le ofrece la capacidad de usar Azure Sentinel como la cola de incidentes universales, a la vez que conserva las fortalezas de M365D para ayudar a investigar los incidentes de seguridad de M365. Además, aprenda sobre la ingesta de datos de búsqueda avanzada de componentes de Defender en Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/02/2021
ms.author: yelevin
ms.openlocfilehash: abace18db51a7a571ecc66d50253277fbd2296d3
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743963"
---
# <a name="microsoft-365-defender-integration-with-azure-sentinel"></a>Integración de Microsoft 365 Defender en Azure Sentinel

> [!IMPORTANT]
> El conector Microsoft 365 Defender está actualmente en **VERSIÓN PRELIMINAR**. Consulte [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para conocer los términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

> [!IMPORTANT]
>
> **Microsoft 365 Defender** se conocía antes como **Microsoft Threat Protection** o **MTP**.
>
> **Microsoft Defender para punto de conexión** se conocía antes como **Protección contra amenazas avanzada de Microsoft Defender** o **MDATP**.
>
> Es posible que vea que los nombres antiguos todavía están en uso durante un tiempo.

## <a name="incident-integration"></a>Integración de incidentes

La integración de incidentes de [Microsoft 365 Defender (M365D)](/microsoft-365/security/mtp/microsoft-threat-protection) de Azure Sentinel le permite transmitir todos los incidentes de M365D a Azure Sentinel y mantenerlos sincronizados entre ambos portales. Los incidentes de M365D (anteriormente conocidos como Protección contra amenazas de Microsoft o MTP) incluyen todas las alertas, entidades e información pertinente asociadas, lo que le proporciona un contexto suficiente para realizar la evaluación de errores y una investigación preliminar en Azure Sentinel. Una vez en Sentinel, los incidentes permanecerán sincronizados de manera bidireccional con M365D, lo que le permite aprovechar las ventajas de ambos portales en su investigación de los incidentes.

Esta integración da a los incidentes de seguridad de Microsoft 365 visibilidad para que puedan administrarse desde Azure Sentinel, como parte de la cola de incidentes principales en toda la organización, por lo que puede ver y correlacionar los incidentes de M365 con los de todos los demás sistemas en la nube y locales. Al mismo tiempo, permite aprovechar las ventajas y las funcionalidades únicas de M365D para las investigaciones detalladas y brinda una experiencia específica de M365 en el ecosistema de M365. M365 Defender enriquece y agrupa las alertas de varios productos M365, lo que reduce el tamaño de la cola de incidentes de SOC y reduce el tiempo de resolución. Los servicios de componentes que forman parte de la pila de M365 Defender son:

- **Microsoft Defender para punto de conexión** (MDE, anteriormente MDATP)
- **Microsoft Defender for Identity** (MDI, anteriormente AATP)
- **Microsoft Defender para Office 365** (MDO, anteriormente O365ATP)
- **Microsoft Cloud App Security** (MCAS)

Además de recopilar alertas de estos componentes, M365 Defender genera alertas propias. Crea incidentes a partir de todas estas alertas y los envía a Azure Sentinel.

### <a name="common-use-cases-and-scenarios"></a>Casos de uso y escenarios comunes

- Conexión de un solo clic de incidentes de M365 Defender, incluidas todas las alertas y entidades de los componentes de M365 Defender, en Azure Sentinel.

- Sincronización bidireccional entre incidentes de Sentinel y M365D en el estado, el propietario y el motivo de cierre.

- Aprovechamiento de las funcionalidades de agrupación y enriquecimiento de alertas de M365 Defender en Azure Sentinel, lo que reduce el tiempo de resolución.

- Vínculo profundo en contexto entre un incidente de Azure Sentinel y su incidente paralelo de M365 Defender, para facilitar las investigaciones en ambos portales.

### <a name="connecting-to-microsoft-365-defender"></a>Conexión a Microsoft 365 Defender

Una vez que haya habilitado el conector de datos de Microsoft 365 defender para [recopilar incidentes y alertas](connect-microsoft-365-defender.md), los incidentes de M365D aparecerán en la cola de incidentes de Azure Sentinel, con **Microsoft 365 Defender** en el campo **Nombre de producto**, poco después de que se generen en M365 Defender.
- Pueden pasar hasta 10 minutos entre el momento en que se genere un incidente en M365 Defender y el momento en que aparezca en Azure Sentinel.

- Los incidentes se ingerirán y se sincronizarán sin costo adicional.

Una vez que la integración de M365 Defender esté conectada, todos los conectores de alertas de los componentes (MDE, MDI, MDO, MCAS) se conectarán automáticamente en segundo plano si no lo estaban aún. Si se adquirieron licencias de componentes después de la conexión a M365 Defender, las alertas y los incidentes del nuevo producto seguirán fluyendo hacia Azure Sentinel sin ningún cargo ni configuración adicionales.

### <a name="m365-defender-incidents-and-microsoft-incident-creation-rules"></a>Incidentes de M365 Defender y reglas de creación de incidentes de Microsoft

- Los incidentes generados por M365 Defender, en función de las alertas procedentes de los productos de seguridad de M365, se crean mediante una lógica de M365 personalizada.

- Las reglas de creación de incidentes de Microsoft en Azure Sentinel también crean incidentes a partir de las mismas alertas, mediante (una diferente) lógica de Azure Sentinel personalizada.

- El uso conjunto de ambos mecanismos es totalmente compatible, y esta configuración se puede usar para facilitar la transición a la nueva lógica de creación de incidentes de M365 Defender. Sin embargo, esto creará **incidentes duplicados** para las mismas alertas.

- Para evitar la creación de incidentes duplicados para las mismas alertas, se recomienda que los clientes desactiven todas las **reglas de creación de incidentes de Microsoft** para los productos de M365 (MDE, MDI y MDO; consulte MCAS a continuación) al conectar M365 Defender. Para ello, marque la casilla correspondiente en la página de conectores. Tenga en cuenta que, si lo hace, los filtros aplicados por las reglas de creación de incidentes no se aplicarán a la integración de incidentes de M365 Defender.

- En el caso de las alertas de Microsoft Cloud App Security (MCAS), no todos los tipos de alerta están incorporados actualmente en M365 Defender. Para asegurarse de seguir recibiendo incidentes para todas las alertas de MCAS, debe conservar o crear **reglas de creación de incidentes de Microsoft** para los tipos de alerta *no incorporados* en M365D.

### <a name="working-with-m365-defender-incidents-in-azure-sentinel-and-bi-directional-sync"></a>Trabajo con incidentes de M365 Defender en Azure Sentinel y sincronización bidireccional

Los incidentes de M365 Defender aparecerán en la cola de incidentes de Azure Sentinel con el nombre de producto **Microsoft 365 Defender**, y con detalles y funcionalidades similares a cualquier otro incidente de Sentinel. Cada incidente contiene un vínculo al incidente paralelo en el portal de M365 Defender.

A medida que el incidente evolucione en M365 Defender y se agreguen más alertas o entidades, el incidente de Azure Sentinel se actualizará en consecuencia.

Los cambios realizados en el estado, el motivo de cierre o la asignación de un incidente de M365, ya sea en M365D o en Azure Sentinel, también se actualizarán en consecuencia en la cola de incidentes del otro. La sincronización se llevará a cabo en ambos portales inmediatamente después de que se aplique el cambio en el incidente, sin ningún retraso. Podría ser necesaria una actualización para ver los cambios más recientes.

En M365 Defender, todas las alertas de un incidente se pueden transferir a otro, lo que da lugar a la combinación de los incidentes. Cuando esto sucede, los incidentes de Azure Sentinel reflejarán los cambios. Un incidente contendrá todas las alertas de ambos incidentes originales, y el otro incidente se cerrará automáticamente y se le agregará la etiqueta "redirigido".

> [!NOTE]
> Los incidentes de Azure Sentinel pueden contener un máximo de 150 alertas. Los incidentes de M365D pueden tener más. Si un incidente de M365D con más de 150 alertas se sincroniza con Azure Sentinel, el incidente de Sentinel se mostrará con "150+" alertas y proporcionará un vínculo al incidente paralelo en M365D, donde podrá ver el conjunto completo de alertas.

## <a name="advanced-hunting-event-collection"></a>Colección de eventos de búsqueda avanzada

El nuevo conector Microsoft 365 Defender le permite transmitir eventos de **búsqueda avanzada**, un tipo de datos de eventos sin procesar, desde Microsoft 365 Defender y sus servicios de componentes hacia Azure Sentinel. Actualmente, puede recopilar eventos de [búsqueda avanzada](/windows/security/threat-protection/microsoft-defender-atp/advanced-hunting-overview) de [Microsoft Defender para punto de conexión (MDATP)](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) y transmitirlos directamente a tablas creadas específicamente en el área de trabajo de Azure Sentinel. Estas tablas se basan en el mismo esquema que se usa en el portal de Microsoft 365 Defender, lo que le ofrece acceso pleno al conjunto completo de eventos de búsqueda avanzada y le permite hacer lo siguiente:

- Copiar fácilmente las consultas existentes de búsqueda avanzada de Microsoft Defender para punto de conexión en Azure Sentinel.

- Usar los registros de eventos sin procesar para proporcionar información detallada adicional sobre las alertas, la búsqueda y la investigación, y correlacionar estos eventos con otros orígenes de datos adicionales en Azure Sentinel.

- Almacenar los registros con mayor periodo de retención, por encima de la retención predeterminada de 30 días de Microsoft Defender para punto de conexión o de Microsoft 365 Defender. Para ello, configure la retención del área de trabajo o configure la retención por tabla en Log Analytics.

## <a name="next-steps"></a>Pasos siguientes

En este documento, aprendió la manera de aprovechar el uso de Microsoft 365 Defender junto con Azure Sentinel mediante el conector Microsoft 365 Defender.

- Obtenga instrucciones para [habilitar el conector Microsoft 365 Defender](connect-microsoft-365-defender.md).
- Cree [alertas personalizadas](tutorial-detect-threats-custom.md) o [investigue incidentes](tutorial-investigate-cases.md).
