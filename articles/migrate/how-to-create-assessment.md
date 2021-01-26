---
title: Creación de una valoración de VM de Azure con la herramienta de valoración de servidores de Azure Migrate | Microsoft Docs
description: En este artículo se describe cómo crear una valoración de VM de Azure con la herramienta de valoración de servidores de Azure Migrate.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 07/15/2019
ms.openlocfilehash: 178bdca78c6f011c607de8e1f5d5eabcdbaab7d4
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567702"
---
# <a name="create-an-azure-vm-assessment"></a>Creación de una valoración de máquina virtual de Azure

En este artículo se describe cómo crear una valoración de VM de Azure para máquinas virtuales locales de VMware o de Hyper-V con Azure Migrate: Server Assessment.

[Azure Migrate](migrate-services-overview.md) le ayuda a migrar a Azure. Azure Migrate proporciona un centro principal para realizar el seguimiento de la detección, valoración y migración de infraestructuras, aplicaciones y datos locales a Azure. El centro proporciona herramientas de Azure para la valoración y migración, así como ofertas de proveedores de software independientes (ISV) externos. 

## <a name="before-you-start"></a>Antes de comenzar

- Asegúrese de que ha [creado](./create-manage-projects.md) un proyecto de Azure Migrate.
- Si ya ha creado un proyecto, asegúrese de que ha [agregado](how-to-assess.md) la herramienta Azure Migrate: Server Assessment.
- Para crear una evaluación, debe configurar una aplicación Azure Migrate para [VMware](how-to-set-up-appliance-vmware.md) o [Hyper-V](how-to-set-up-appliance-hyper-v.md). El dispositivo detecta máquinas locales y envía metadatos y datos de rendimiento a Azure Migrate: Server Assessment. [Más información](migrate-appliance.md).


## <a name="azure-vm-assessment-overview"></a>Introducción a las valoraciones de VM de Azure
Existen dos tipos de criterios de tamaño que puede usar para crear valoraciones de VM de Azure mediante Azure Migrate: Server Assessment.

**Valoración** | **Detalles** | **Data**
--- | --- | ---
**Basada en el rendimiento** | Evaluaciones basadas en los datos de rendimiento recopilados | **Tamaño de máquina virtual recomendado**: se basa en los datos de uso de la CPU y de la memoria.<br/><br/> **Tipo de disco recomendado (disco administrado estándar o Premium**): se basa en IOPS y en el rendimiento de los discos locales.
**Como local** | Evaluaciones que se basan en el tamaño local. | **Tamaño de máquina virtual recomendado**: se basa en el tamaño de la máquina virtual local<br/><br> **Tipo de disco recomendado**: se basa en el valor del tipo de almacenamiento que se selecciona para la evaluación.

[Más información](concepts-assessment-calculation.md) sobre las evaluaciones.

## <a name="run-an-assessment"></a>Ejecución de una evaluación

Las evaluaciones se realizan como se indica a continuación:

1. En la página **Servidores** > **Servidores Windows y Linux**, haga clic en **Evaluar y migrar servidores**.

   ![Ubicación del botón Evaluar y migrar servidores](./media/tutorial-assess-vmware-azure-vm/assess.png)

2. En **Azure Migrate: Server Assessment**, haga clic en **Evaluar**.

    ![Ubicación del botón Evaluar](./media/tutorial-assess-vmware-azure-vm/assess-servers.png)

3. En **Evaluar los servidores** > **Tipo de valoración**, seleccione **Máquina virtual de Azure**.
4. En **Origen de detección**:

    - Si ha detectado máquinas que usan el dispositivo, seleccione **Máquinas detectadas desde el dispositivo de Azure Migrate**.
    - Si ha detectado máquinas que usan un archivo CSV importado, seleccione **Máquinas importadas**. 
    
1. Haga clic en **Editar** para revisar las propiedades de la evaluación.

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vm/assessment-name.png" alt-text="Ubicación del botón Editar para revisar las propiedades de evaluación":::

1. En **Assessment properties** (Propiedades de la evaluación)  > **Propiedades de destino**:
    - En **Ubicación de destino**, especifique la región de Azure a la que desee migrar.
        - Las recomendaciones de tamaño y costo se basan en la ubicación que especifique. Una vez que cambie la ubicación de destino predeterminada, se le pedirá que especifique **Instancias reservadas** y **Serie de VM**.
        - En Azure Government, puede dirigir las valoraciones en [estas regiones](migrate-support-matrix.md#supported-geographies-azure-government).
    - En **Tipo de almacenamiento**:
        - Si desea usar datos basados en el rendimiento en la valoración, seleccione **Automático** para que Azure Migrate recomiende un tipo de almacenamiento, en función de la IOPS y del rendimiento del disco.
        - Como alternativa, seleccione el tipo de almacenamiento que desee usar para la máquina virtual en la migración.
    - En **Instancias reservadas**, especifique si desea usar las instancias reservadas para la máquina virtual en la migración.
        - Si selecciona usar una instancia reservada, no puede especificar '**Descuento (%)** ni **Tiempo de actividad de la máquina virtual**. 
        - [Más información](https://aka.ms/azurereservedinstances).
 1. En **Tamaño de VM**:
     - En **Sizing criterion** (Criterios de ajuste de tamaño), seleccione si desea basar la valoración en metadatos o datos de configuración de la máquina, o bien en datos basados en el rendimiento. Si utiliza datos de rendimiento:
        - En **Historial de rendimiento**, indique la duración de los datos en los que desee basar la valoración.
        - En **Uso de percentil**, especifique el valor de percentil que desee utilizar para la muestra de rendimiento. 
    - En **Series de máquinas virtuales**, especifique la serie de máquinas virtuales de Azure que desea considerar.
        - Si usa la valoración basada en el rendimiento, Azure Migrate sugiere un valor.
        - Ajuste la configuración según sea necesario. Por ejemplo, si no tiene un entorno de producción que necesite máquinas virtuales de la serie A en Azure, puede excluir esta serie de la lista.
    - En **Factor de confort**, indique el búfer que desee usar durante la valoración. Tiene en cuenta problemas como el uso estacional, el historial de rendimiento corto y los posibles aumentos en el uso futuro. Por ejemplo, si usa un factor de confort de dos:
    
        **Componente** | **Utilización efectiva** | **Agregar factor de confort (2,0)**
        --- | --- | ---
        Núcleos | 2  | 4
        Memoria | 8 GB | 16 GB
   
1. En **Precios**:
    - En **Oferta**, especifique la [oferta de Azure](https://azure.microsoft.com/support/legal/offer-details/) si está inscrito. Server Assessment calcula el costo de esa oferta.
    - En **Moneda**, seleccione la moneda de facturación para la cuenta.
    - En **Descuento (%)** , agregue cualquier descuento específico de la suscripción que reciba a partir de la oferta de Azure. La configuración predeterminada es 0 %.
    - En **Tiempo de actividad de VM**, especifique el tiempo (días por mes/hora al día) que se ejecutarán las máquinas virtuales.
        - Esto resulta útil para las máquinas virtuales de Azure que no se ejecuten de forma continua.
        - Las estimaciones de costos se basan en la duración especificada.
        - El valor predeterminado es de 31 días al mes y 24 horas al día.
    - En **Suscripción a EA**, especifique si desea tener en cuenta un descuento por suscripción al Contrato Enterprise (EA) para la estimación de costos. 
    - En **Ventaja híbrida de Azure**, especifique si ya tiene una licencia de Windows Server. Si la tiene y está incluido en Active software Assurance de Windows Server Subscriptions, puede solicitar la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/) al incorporar licencias a Azure.

1. Haga clic en **Guardar** si realiza cambios.

    ![Propiedades de la evaluación](./media/tutorial-assess-vmware-azure-vm/assessment-properties.png)

1. En **Evaluar los servidores**, haga clic en **Siguiente**.

1. En **Seleccione las máquinas que quiera evaluar** > **Nombre de la evaluación**, especifique un nombre para la evaluación. 

1. En **Seleccionar o crear un grupo**, seleccione **Crear nuevo** y especifique un nombre de grupo. 

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vm/assess-group.png" alt-text="Adición de máquinas virtuales a un grupo":::

1. Seleccione el dispositivo y las máquinas virtuales que desee agregar al grupo. A continuación, haga clic en **Siguiente**.


1. En **Revisar y crear valoración**, revise los detalles de la valoración y haga clic en **Crear evaluación** para crear el grupo y ejecutar la evaluación.

1. Una vez creada la evaluación, se puede ver en **Servidores** > **Azure Migrate: Server Assessment** > **Evaluaciones**.

1. Haga clic en **Exportar la evaluación** para descargarla como un archivo de Excel.
    > [!NOTE]
    > En el caso de las valoraciones basadas en el rendimiento, se recomienda esperar al menos un día después de iniciar la detección antes de crear una. De este modo, gana tiempo para recopilar los datos de rendimiento con mayor confianza. Lo mejor es que, después de iniciar la detección, espere el tiempo que especifique (día/semana/mes) para que la confianza sea alta.


## <a name="review-an-azure-vm-assessment"></a>Revisión de una valoración de VM de Azure

Una valoración de VM de Azure describe:

- **Preparación para Azure**: si las máquinas virtuales son adecuadas para la migración a Azure.
- **Estimación del costo mensual**: los costos mensuales estimados de proceso y almacenamiento por ejecutar las máquinas virtuales en Azure.
- **Estimación del costo mensual de almacenamiento**: costos estimados del almacenamiento en disco después de la migración.

### <a name="view-an-azure-vm-assessment"></a>Visualización de una valoración de VM de Azure

1. En **Objetivos de migración** >  **Servidores**, haga clic en **Evaluaciones** en **Azure Migrate: Server Assessment**.
2. En **Evaluaciones**, haga clic en una evaluación para abrirla.

    ![Resumen de evaluaciones](./media/how-to-create-assessment/assessment-summary.png)

### <a name="review-azure-readiness"></a>Revisión de la preparación para Azure

1. En **Preparación para Azure**, compruebe si las máquinas virtuales están listas para su migración a Azure.
2. Revise el estado de la máquina virtual:
    - **Preparada para Azure**: Azure Migrate recomienda unas estimaciones de tamaño y costo de las máquinas virtuales en la evaluación.
    - **Preparado con condiciones**: muestra los problemas y las soluciones que se sugieren.
    - **No está preparado para Azure**: muestra los problemas y las soluciones que se sugieren.
    - **Preparación desconocida**: se usa cuando Azure Migrate no puede evaluar la preparación debido a problemas de disponibilidad de datos.

3. Haga clic en cualquiera de los estados de **Preparación para Azure**. Puede ver los detalles de la preparación de la máquina virtual y explorar en profundidad los detalles de esta, entre los que se incluye la configuración de proceso, almacenamiento y red.



### <a name="review-cost-details"></a>Revisión de los datos de costo

Esta vista muestra el costo estimado de almacenamiento y proceso que conlleva ejecutar máquinas virtuales en Azure.

1. Revise los costos mensuales de proceso y almacenamiento. Los costos se agregan para todas las máquinas virtuales del grupo evaluado.

    - Las estimaciones de costo se basan en las recomendaciones de tamaño de una máquina, así como en sus discos y propiedades.
    - Se muestran los costos mensuales estimados de proceso y almacenamiento.
    - La estimación de costos es para ejecutar las máquinas virtuales locales como máquinas virtuales IaaS. Azure Migrate Server Assessment no tiene en cuenta los costos de PaaS o SaaS.

2. Puede revisar las estimaciones del costo de almacenamiento mensual. Esta vista muestra los costos de almacenamiento agregados del grupo evaluado, divididos por los diferentes tipos de discos de almacenamiento.
3. Puede explorar en profundidad para ver los detalles de cada una de las máquinas virtuales.


### <a name="review-confidence-rating"></a>Examen de la clasificación de confianza

Cuando se realizan valoraciones basadas en el rendimiento, se asigna una clasificación de confianza a la evaluación.

![Clasificación de confianza](./media/how-to-create-assessment/confidence-rating.png)

- Se concede una clasificación que oscila entre 1 estrella (la más baja) y 5 estrellas (la más alta).
- La clasificación de confianza sirve de ayuda para calcular la confiabilidad de las recomendaciones de tamaño que proporciona la evaluación.
- La clasificación de confianza se basa en la disponibilidad de los puntos de datos necesarios para calcular tal evaluación.

Estas son las posibles clasificaciones de confianza de una evaluación.

**Disponibilidad del punto de datos** | **Clasificación de confianza**
--- | ---
0 % - 20 % | 1 estrella
21 % - 40 % | 2 estrellas
41 % - 60 % | 3 estrellas
61 % - 80 % | 4 estrellas
81 % - 100 % | 5 estrellas




## <a name="next-steps"></a>Pasos siguientes

- Aprenda a usar la [asignación de dependencias de máquina](how-to-create-group-machine-dependencies.md) para crear grupos de confianza alta.
- [Obtenga más información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones.