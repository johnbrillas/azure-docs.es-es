---
title: Análisis de costos y presupuestos
description: Obtenga información sobre cómo obtener un análisis de costos, establecer un presupuesto y reducir los costos para los recursos de proceso subyacentes y las licencias de software que se usan para ejecutar las cargas de trabajo de Batch.
ms.topic: how-to
ms.date: 01/21/2021
ms.openlocfilehash: 1a950f23b7ecee11dd7da5414b7eed9e87277850
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98679327"
---
# <a name="cost-analysis-and-budgets-for-azure-batch"></a>Análisis de costos y presupuestos para Azure Batch

No se aplica ningún costo por usar Azure Batch, aunque puede haber cargos por los recursos de proceso subyacentes y las licencias de software que se usan para ejecutar las cargas de trabajo de Batch. Se puede incurrir en costos desde las máquinas virtuales (VM) en un grupo, la transferencia de datos desde la VM o cualquier dato de entrada o salida almacenado en la nube. Este tema le ayudará a entender de dónde proceden los costos, cómo establecer un presupuesto para un grupo o cuenta de Batch, y cómo reducir los costos de las cargas de trabajo de Batch.

## <a name="batch-resources"></a>Recursos de Batch

Las máquinas virtuales son el recurso más importante que se usa para el procesamiento por lotes. El costo de usar máquinas virtuales para Batch se calcula en función del tipo, la cantidad y la duración del uso. Las opciones de facturación de las máquinas virtuales incluyen [Pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/) o [reserva](../cost-management-billing/reservations/save-compute-costs-reservations.md) (pago por adelantado). Ambas opciones de pago tienen distintas ventajas en función de la carga de trabajo del proceso y afectarán a la factura de manera distinta.

Cuando se implementan aplicaciones en los nodos de Batch (máquinas virtuales) mediante [paquetes de aplicación](batch-application-packages.md), se le cobrarán los recursos de Azure Storage que los paquetes de aplicación consumen. También se le cobrará el almacenamiento de cualquier archivo de entrada o salida, como los archivos de recursos u otros datos de registro. En general, el costo de los datos de almacenamiento asociados con Batch es mucho menor que el costo de los recursos de proceso. Cada máquina virtual de un grupo que se crea con [Configuración de máquina virtual](nodes-and-pools.md#virtual-machine-configuration) tiene un disco de sistema operativo asociado que usa discos administrados de Azure. Los discos administrados de Azure tienen un costo adicional y otros niveles de rendimiento de discos tienen también distintos costos.

Los grupos de Batch usan recursos de red. En concreto, para los grupos de **VirtualMachineConfiguration** se usan equilibradores de carga estándar, que requieren direcciones IP estáticas. Los equilibradores de carga que Batch usa son visibles para las cuentas de **Suscripción del usuario**, pero no para las cuentas de **Servicio de Batch**. Los equilibradores de carga estándar incurren en gastos por todos los datos transmitidos desde y hacia las máquinas virtuales de los grupos de Batch. Las API de Batch selectas que recuperan datos desde los nodos de grupo (como Get Task/Node File), los paquetes de aplicación de tareas, los archivos de recursos o de salida y las imágenes de contenedor incurrirán en cargos.

### <a name="additional-services"></a>Servicios adicionales

Los servicios que no incluyen máquinas virtuales ni almacenamiento se pueden tener en cuenta para el costo de la cuenta de Batch.

Otros servicios que se usan habitualmente con Batch pueden incluir:

- Application Insights
- Data Factory
- Azure Monitor
- Virtual Network
- Máquinas virtuales con aplicaciones de gráficos

En función de los servicios que usa con la solución de Batch, es posible que incurra en tarifas adicionales. Consulte la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) para determinar el costo de cada servicio adicional.

## <a name="cost-analysis-and-budget-for-a-pool"></a>Análisis de costos y presupuesto para un grupo

En Azure Portal, puede crear presupuestos y alertas de gastos para los grupos o cuentas de Batch. Los presupuestos y las alertas son útiles para notificar a las partes interesadas de cualquier riesgo de exceso de gasto, aunque es posible que haya un retraso en las alertas de gasto y que se supere ligeramente el presupuesto.

En este ejemplo, veremos el análisis de costos de un grupo de Batch individual.

1. En Azure Portal, escriba o seleccione **Cost Management + Billing**.
1. Seleccione su suscripción en la sección **Ámbitos de facturación**.
1. En **Cost Management**, seleccione **Análisis de costos**.
1. Seleccione **Agregar filtro**. En la primera lista desplegable, seleccione **Recurso**.
1. En la segunda lista desplegable, seleccione el grupo de Batch. Una vez seleccionado el grupo, verá el análisis de costos del grupo, similar al ejemplo que se muestra aquí.
    ![Captura de pantalla que muestra el análisis de costos de un grupo en Azure Portal.](./media/batch-budget/pool-cost-analysis.png)

El análisis de costos resultante muestra el costo del grupo, así como los recursos que contribuyen a este costo. En este ejemplo, las máquinas virtuales que se usan en el grupo son el recurso más costoso.

Para crear un presupuesto para el grupo, seleccione **Presupuesto: ninguno** y, luego, seleccione **Crear presupuesto >** . Ahora use la ventana para [configurar un presupuesto](../cost-management-billing/costs/tutorial-acm-create-budgets.md) específicamente para el grupo.

> [!NOTE]
> El servicio Azure Batch se basa en la tecnología de Azure Cloud Services y Azure Virtual Machines. Al elegir **Configuración de Cloud Services**, se le cobrará según la estructura de precios de Cloud Services. Al elegir **Configuración de la máquina virtual**, se le cobrará según la estructura de precios de máquinas virtuales. En el ejemplo de esta página se usa la **Configuración de máquina virtual**, que es la recomendada para la mayoría de los grupos de Batch.

## <a name="minimize-cost"></a>Minimización del costo

Usar varias máquinas virtuales y servicios de Azure durante largos períodos de tiempo puede ser costoso. Considere la posibilidad de usar estas estrategias para maximizar la eficacia de las cargas de trabajo y reducir los costos.

### <a name="low-priority-virtual-machines"></a>Máquinas virtuales de prioridad baja

Las [VM de prioridad baja](batch-low-pri-vms.md) disminuyen el costo de las cargas de trabajo de Batch al usar la capacidad de proceso sobrante en Azure. Cuando se especifican máquinas virtuales de prioridad baja en los grupos, Batch usa este excedente para ejecutar la carga de trabajo. Puede lograr grandes ahorros de costos al usar VM de prioridad baja en lugar de VM dedicadas.

### <a name="virtual-machine-os-disk-type"></a>Tipo de disco de sistema operativo de máquina virtual

Azure ofrece varios [tipos de discos de sistema operativo de VM](../virtual-machines/disks-types.md). Muchas de las series de máquinas virtuales tienen tamaños que permiten almacenamiento premium y estándar. Cuando se selecciona un tamaño de VM "s" para un grupo, Batch configura discos de SO SSD prémium. Cuando se selecciona un tamaño de VM distinto de "s", se usa el tipo de disco HDD estándar más barato. Por ejemplo, los discos de SO SSD Premium se usan para `Standard_D2s_v3` y los discos de SO HDD estándar se usan para `Standard_D2_v3`.

Los discos de sistema operativo SSD prémium son más caros, pero tienen un mayor rendimiento. Las VM con discos prémium se pueden iniciar ligeramente más rápido que las VM con discos de SO HDD estándar. Con Batch, en general el disco de SO no se usa tanto, ya que las aplicaciones y archivos de tareas se encuentran en el disco SSD temporal de la VM. Por este motivo, a menudo puede seleccionar el tamaño de VM distinto de "s" para evitar el mayor costo para el SSD prémium que se aprovisiona cuando se especifica un tamaño de VM "s".

### <a name="reserved-virtual-machine-instances"></a>Instancias reservadas de máquina virtual

Si piensa usar Batch durante largo tiempo, puede reducir el costo de las VM si usa [Reservas de Azure](../cost-management-billing/reservations/save-compute-costs-reservations.md) para las cargas de trabajo. Una tarifa de reserva es considerablemente menor que una tarifa de pago por uso. Las instancias de máquina virtual que se usan sin una reserva se cobran según la tarifa de pago por uso. Al comprar una reserva, se aplica el descuento de la reserva.

### <a name="automatic-scaling"></a>Escalado automático

El [escalado automático](batch-automatic-scaling.md) escala de manera dinámica el número de máquinas virtuales en el grupo de Batch en función de las demandas del trabajo actual. Al escalar el grupo según la duración de un trabajo, el escalado automático garantiza que las VM se escalen verticalmente y se usen solo cuando haya que realizar un trabajo. Cuando el trabajo se complete o no haya trabajos, las VM se reducen verticalmente de manera automáticamente para ahorrar recursos de proceso. El escalamiento permite disminuir los costos generales de la solución de Batch mediante el uso solo de los recursos que necesita.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las [herramientas y API de Batch](batch-apis-tools.md) disponibles para compilar y supervisar las soluciones de Batch.  
- Más información sobre las [VM de prioridad baja con Batch](batch-low-pri-vms.md).
