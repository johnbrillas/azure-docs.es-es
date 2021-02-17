---
title: 'Tutorial: Comprobaciones de cumplimiento normativo: Azure Security Center'
description: 'Tutorial: Aprenda a mejorar el cumplimiento de reglamentaciones mediante Azure Security Center.'
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5f50c4dc-ea42-418d-9ea8-158ffeb93706
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2021
ms.author: memildin
ms.openlocfilehash: 20a464011e5a8d37a6215b222323ca989e02ac04
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550930"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Tutorial: Mejora del cumplimiento normativo

Azure Security Center resulta de gran ayuda para simplificar el proceso necesario para cumplir los requisitos de cumplimiento normativo, para lo que se usa el **panel de cumplimiento normativo.** . 

Security Center evalúa continuamente el entorno de nube híbrida para analizar los factores de riesgo con arreglo a los controles y los procedimientos recomendados establecidos en los estándares que se aplican a las suscripciones. El panel refleja el estado de cumplimiento con respecto a estos estándares. 

Cuando habilite Security Center en una suscripción de Azure, se le asignará automáticamente el estándar [Azure Security Benchmark](../security/benchmarks/introduction.md). Este punto de referencia ampliamente respetado está basado en los controles del [Centro de seguridad de Internet (CIS)](https://www.cisecurity.org/benchmark/azure/) y del [National Institute of Standards and Technology (NIST)](https://www.nist.gov/) y hace hincapié en la seguridad centrada en la nube.

En el panel de cumplimiento normativo se indica el estado de todas estas valoraciones realizadas en el entorno conforme a las normativas y los estándares elegidos. Al actuar sobre las recomendaciones y reducir los factores de riesgo en su entorno, su estado de cumplimiento normativo mejora.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Evaluar su cumplimiento normativo mediante el panel de cumplimiento normativo.
> * Mejorar su estado de cumplimiento normativo mediante la realización de acciones sobre las recomendaciones
> * Configurar alertas sobre los cambios en la estado de cumplimiento
> * Exportar los datos de cumplimiento como una secuencia continua y como instantáneas semanales

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para recorrer las características descritas en este tutorial:

- [Azure Defender](azure-defender.md) debe estar habilitado. Puede probar Azure Defender gratis durante 30 días.
- Debe haber iniciado sesión con una cuenta que tenga acceso de lectura a los datos de cumplimiento de directivas (**Lector de seguridad** no es suficiente). El rol **Lector global** para la suscripción funcionará. Como mínimo, necesitará tener asignados los roles **Colaborador de directivas de recursos** y **Administrador de seguridad**.

##  <a name="assess-your-regulatory-compliance"></a>Mejora del cumplimiento de reglamentaciones

El panel de cumplimiento normativo muestra los estándares normativos seleccionados, con todos sus requisitos. Los requisitos compatibles se asignarán a las evaluaciones de seguridad correspondientes. El estado de estas evaluaciones refleja el estado de cumplimiento con respecto a los estándares.

Utilice el panel de cumplimiento normativo para centrarse en las deficiencias relativas al cumplimiento de las normativas y estándares elegidos. Esta vista focalizada también le permite supervisar continuamente su puntuación de cumplimiento a lo largo del tiempo para entornos híbrido y de nube dinámicos.

1. En el menú de Security Center, seleccione **Cumplimiento normativo**.

    En la parte superior de la pantalla hay un panel con información general sobre su estado de cumplimiento con respecto a las normativas compatibles. Allí se indica la puntuación global de cumplimiento y el número de valoraciones aprobadas y suspendidas asociadas a cada estándar.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-dashboard.png" alt-text="Panel de cumplimiento normativo" lightbox="./media/security-center-compliance-dashboard/compliance-dashboard.png":::

1. Seleccione la pestaña de la norma que le interese (1). Verá las suscripciones en las que se aplica la norma (2) y la lista de todos los controles de esa norma (3). Para sabe qué controles se pueden aplicar, puede ver los detalles de las valoraciones aprobadas y suspendidas asociadas a ese control (4) y la cantidad de recursos afectados (5). Algunos controles aparecen atenuados, no tienen valoraciones de Security Center asociadas. Compruebe los requisitos de estos y valórelos en su entorno. Es posible que algunos de ellos estén relacionados con los procesos y no sean técnicos.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-drilldown.png" alt-text="Exploración de los detalles de cumplimiento con una norma específica":::

1. Para generar un informe PDF con un resumen de su estado de cumplimiento actual en relación con un estándar concreto, seleccione **Descargar informe**.

    El informe proporciona un resumen de alto nivel del estado de cumplimiento del estándar seleccionado, para lo que usa los datos de las evaluaciones de Security Center, y se organiza según los controles de dicho estándar concreto. El informe se puede compartir con las partes interesadas competentes y puede proporcionar evidencia a los auditores internos y externos.

    :::image type="content" source="./media/security-center-compliance-dashboard/download-report.png" alt-text="Descarga del informe de cumplimiento":::

## <a name="improve-your-compliance-posture"></a>Mejora de su estado de cumplimiento de la norma

El panel contiene información que le ayudará a mejorar el cumplimiento normativo y le permitirá resolver las recomendaciones directamente en él.

1.  Haga clic en cualquiera de las valoraciones suspensas que aparecen en el panel para ver los detalles de dicha recomendación. Cada recomendación incluye un conjunto de pasos de corrección que se deben seguir si se desea resolver el problema.

1.  Seleccione un recurso concreto para ver más detalles y resolver la recomendación relacionada. <br>Por ejemplo, en el estándar **Azure CIS 1.1.0**, seleccione la recomendación **El cifrado de disco se debe aplicar en las máquinas virtuales**.

    :::image type="content" source="./media/security-center-compliance-dashboard/sample-recommendation.png" alt-text="Al seleccionar una recomendación de una norma se va directamente a la página de detalles de la recomendación.":::

1. En este ejemplo, si selecciona **Realizar acción** en la página de detalles de la recomendación, accederá Azure Portal, a las páginas de la máquina virtual de Azure, donde podrá abrir la pestaña **Seguridad** y habilitar el cifrado:

    :::image type="content" source="./media/security-center-compliance-dashboard/encrypting-vm-disks.png" alt-text="Botón Realizar acción en la página de detalles de la recomendación que conduce a las opciones de corrección":::

    Para más información sobre cómo aplicar las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md).

1.  Una vez realizadas las acciones necesarias para resolver las recomendaciones, podrá comprobar su efecto en el informe del panel de cumplimiento, ya que la puntuación de cumplimiento mejora.

    > [!NOTE]
    > Las valoraciones se ejecutan aproximadamente cada 12 horas, por lo que el efecto sobre los datos de cumplimiento solo se constatará tras la ejecución siguiente de la valoración en cuestión.


## <a name="export-your-compliance-status-data"></a>Exportación de los datos de estado de cumplimiento

Si desea hacer un seguimiento del estado de cumplimiento con otras herramientas de supervisión de su entorno, Security Center dispone de un mecanismo de exportación que facilita esta tarea. Configure la **exportación continua** para enviar una selección de los datos a un centro de eventos de Azure o un área de trabajo de Log Analytics.

Utilice los datos de exportación continua con un centro de eventos de Azure o un área de trabajo de Log Analytics:

- Exporte todos los datos de cumplimiento normativo en un **flujo continuo**:

    :::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-stream.png" alt-text="Exportar ininterrumpidamente un flujo de datos de cumplimiento normativo" lightbox="media/security-center-compliance-dashboard/export-compliance-data-stream.png":::

- Exporte **instantáneas semanales** de los datos de cumplimiento normativo:

    :::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-snapshot.png" alt-text="Exportar ininterrumpidamente una instantánea semanal de los datos de cumplimiento normativo" lightbox="media/security-center-compliance-dashboard/export-compliance-data-snapshot.png":::

También puede exportar un **informe PDF/CSV** de los datos de cumplimiento directamente desde el panel:

:::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-report.png" alt-text="Exportar los datos de cumplimiento normativo como un informe PDF o CSV" lightbox="media/security-center-compliance-dashboard/export-compliance-data-report.png":::

Para más información, consulte este artículo sobre la [exportación continua de datos de Security Center](continuous-export.md).


## <a name="run-workflow-automations-when-there-are-changes-to-your-compliance"></a>Ejecución de automatizaciones de flujos de trabajo cuando se producen cambios en el cumplimiento

La característica de automatización de flujos de trabajo de Security Center puede desencadenar Logic Apps cada vez que una de las evaluaciones de cumplimiento normativo cambie el estado.

Por ejemplo, si quiere que Security Center envíe un correo electrónico a un usuario específico cuando no se supere una valoración de cumplimiento, primero tendrá que crear la aplicación lógica (mediante [Azure Logic Apps](../logic-apps/logic-apps-overview.md)) y después tendrá que configurar el desencadenador de una nueva automatización de flujos de trabajo, tal y como se explica en [Automatización de respuestas a desencadenadores de Security Center](workflow-automation.md).

:::image type="content" source="media/release-notes/regulatory-compliance-triggers-workflow-automation.png" alt-text="Uso de los cambios en las valoraciones de cumplimiento normativo para desencadenar la automatización de un flujo de trabajo" lightbox="media/release-notes/regulatory-compliance-triggers-workflow-automation.png":::

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a utilizar el panel de cumplimiento normativo de Security Center para:

- Vea y supervise su estado de cumplimiento con arreglo a los estándares y regulaciones que le interesen.
- Mejorar el estado de cumplimiento mediante la resolución de las recomendaciones pertinentes y la observación de la mejora en la puntuación del cumplimiento.

El panel de cumplimiento normativo puede simplificar considerablemente el proceso de cumplimiento y reducir en gran medida el tiempo necesario para recopilar las pruebas de cumplimiento en un entorno de Azure, un entorno híbrido y un entorno con varias nubes.

Para más información, consulte los artículos relacionados:

- [Personalización del conjunto de estándares en el panel de cumplimiento normativo](update-regulatory-compliance-packages.md): aprenda a seleccionar los estándares que aparecen en el panel de cumplimiento normativo. 
- [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
- [Administración de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md): aprenda a usar las recomendaciones de Azure Security Center como ayuda para la protección de los recursos de Azure.