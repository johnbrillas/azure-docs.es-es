---
title: Informes de Azure Monitor para contenedores
description: Se describen los informes disponibles para analizar los datos recopilados por Azure Monitor para contenedores.
ms.topic: conceptual
ms.date: 12/07/2020
ms.openlocfilehash: 3cc2f8fb9bfaa278ce06b4a8cd6d379397b7129a
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96907425"
---
# <a name="reports-in-azure-monitor-for-containers"></a>Informes de Azure Monitor para contenedores
Los informes de Azure Monitor para contenedores son [libros de Azure](../platform/workbooks-overview.md) recomendados listos para usar. En este artículo se describen los diferentes informes que están disponibles y cómo acceder a ellos.

## <a name="viewing-reports"></a>Visualización de informes
En el menú **Azure Monitor** de Azure Portal, seleccione **Contenedores**. Seleccione **Información detallada** en la sección **Supervisión**, elija un clúster determinado y, luego, seleccione la página **Informes (versión preliminar)** . 

[![Página Informes](media/container-insights-reports/reports-page.png)](media/container-insights-reports/reports-page.png#lightbox)

## <a name="create-a-custom-workbook"></a>Creación de un libro personalizado
Para crear un libro personalizado basado en cualquiera de estos libros, seleccione la lista desplegable **Ver libros** y, luego, **Ir a la galería de AKS** en la parte inferior de la lista desplegable. Para más información sobre los libros y el uso de plantillas de libro, consulte [Libros de Azure Monitor](../platform/workbooks-overview.md).

[![Galería de AKS](media/container-insights-reports/aks-gallery.png)](media/container-insights-reports/aks-gallery.png#lightbox)

## <a name="node-workbooks"></a>Libros de nodo

- **Capacidad de disco**: gráficos interactivos de uso de cada disco que se presenta al nodo de un contenedor por las siguientes perspectivas:

    - Porcentaje de uso de disco (para todos los discos).
    - Espacio libre en disco (para todos los discos).
    - Una cuadrícula que muestra el disco de cada nodo, su porcentaje de espacio usado, la tendencia del porcentaje de espacio usado, el espacio libre en el disco (GiB) y la tendencia de espacio libre en el disco (GiB). Cuando se selecciona una fila de la tabla, a continuación se muestran debajo de dicha fila el porcentaje de espacio usado y el espacio libre en el disco (GiB).

- **E/S de disco**: gráficos interactivos de uso de cada uno de los discos que se presenta al nodo dentro de un contenedor por las siguientes perspectivas:

    - Resumen de E/S de disco en todos los discos por bytes de lectura por segundo, bytes de escritura por segundo y tendencias de lectura y escritura en bytes por segundo.
    - Ocho gráficos de rendimiento que muestran indicadores clave de rendimiento que le ayudarán a medir e identificar los cuellos de botella de E/S de disco.

- **GPU**: gráficos interactivos de uso de cada nodo de clúster de Kubernetes compatible con GPU.

## <a name="resource-monitoring-workbooks"></a>Libros de supervisión de recursos

- **Implementaciones**: estado de las implementaciones y Horizontal Pod Autoscaler (HPA), que incluye el HPA personalizado. 
  
- **Detalles de la carga de trabajo**: gráficos interactivos que muestran las estadísticas de rendimiento de las cargas de trabajo de un espacio de nombres. Incluye varias pestañas:

  - Overview of CPU (Información general de CPU) y Memory usage by POD (Uso de memoria por POD).
  - POD/Container Status (Estado del contenedor o POD) que muestra la tendencia de reinicio del POD, la tendencia de reinicio del contenedor y el estado del contenedor de los POD.
  - Kubernetes Events (Eventos de Kubernetes) que muestra un resumen de eventos del controlador.

- **Kubelet**: incluye dos cuadrículas que muestran las estadísticas operativas del nodo principal:

    - La información general sobre la cuadrícula de nodos resume el número total de operaciones, errores y operaciones correctas en porcentaje, junto con la tendencia de cada nodo.
    - La información general por tipo de operación resume para cada operación el número total de operaciones, de errores y de operaciones correctas en porcentaje, y la tendencia.
## <a name="billing-workbooks"></a>Libros de facturación

- **Uso de datos**: le ayuda a visualizar el origen de los datos sin tener que crear su propia biblioteca de consultas de lo que compartimos en nuestra documentación. En este libro, hay gráficos con los que puede ver los datos facturables desde perspectivas como:

  - Datos facturables totales ingeridos en GB por solución
  - Datos facturables ingeridos por registros de contenedor (registros de aplicación)
  - Datos de registros de contenedores facturables ingeridos por espacio de nombres de Kubernetes
  - Datos de registros de contenedores facturables ingeridos segregados por nombre de clúster
  - Datos de registros de contenedores facturables ingeridos por entrada de origen de registro
  - Datos de diagnóstico facturables ingeridos por registros de nodo maestro de diagnóstico

## <a name="networking-workbooks"></a>Libros de redes

- **Configuración de NPM**:  supervisión de las configuraciones de red que se configuran mediante el Administrador de directivas de redes (NPM).

  - Información de resumen sobre la complejidad de la configuración general.
  - Número de directivas, reglas y grupos a lo largo del tiempo, lo que permite conocer la relación entre los tres y agregar una dimensión de tiempo para depurar una configuración.
  - Número de entradas de todos los IPSets y de cada uno de ellos.
  - Rendimiento de los casos peores y medios por nodo para agregar componentes a la configuración de red.

- **Network** (Red): gráficos interactivos de uso de la red del adaptador de red de cada nodo y una cuadrícula que presenta los indicadores clave de rendimiento para ayudar a medir el rendimiento de los adaptadores de red.



## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los libros de Azure Monitor, consulte [Libros de Azure Monitor](../platform/workbooks-overview.md).
