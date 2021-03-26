---
title: Conexión de los registros de auditoría y diagnóstico de bases de datos de Azure SQL a Azure Sentinel
description: Obtenga información sobre cómo conectar los registros de auditoría de seguridad y diagnóstico de bases de datos de Azure SQL a Azure Sentinel.
author: yelevin
manager: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: how-to
ms.date: 01/06/2021
ms.author: yelevin
ms.openlocfilehash: a3a09ceffc75e2d396d7bd7aeedd97b7f2b6ec2b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99807740"
---
# <a name="connect-azure-sql-database-diagnostics-and-auditing-logs"></a>Conexión de los registros de auditoría y diagnóstico de bases de datos de Azure SQL

Azure SQL es un motor de base de datos de plataforma como servicio (PaaS) totalmente administrado que se encarga de la mayoría de las funciones de administración de bases de datos, como actualizar, aplicar revisiones, crear copias de seguridad y supervisar sin la intervención del usuario. 

El conector de Azure SQL Database permite transmitir registros de auditoría y diagnóstico de bases de datos a Azure Sentinel, para poder supervisar continuamente la actividad en todas las instancias.

- La conexión de registros de diagnóstico permite enviar registros de diagnóstico de base de datos de diferentes tipos de datos al área de trabajo de Azure Sentinel.

- La conexión de registros de auditoría permite transmitir los registros de auditoría de seguridad de todas las bases de datos de Azure SQL en el nivel de servidor.

Más información sobre la [supervisión de bases de datos de Azure SQL](../azure-sql/database/metrics-diagnostic-telemetry-logging-streaming-export-configure.md)

## <a name="prerequisites"></a>Requisitos previos

- Debe tener permisos de lectura y escritura en el área de trabajo de Azure Sentinel.

- Para conectar los registros de auditoría, debe tener permisos de lectura y escritura para la configuración de auditoría de Azure SQL Server.

## <a name="connect-to-azure-sql-database"></a>Conexión a una base de datos de Azure SQL
    
1. En el menú de navegación de Azure Sentinel, seleccione **Conectores de datos**.

1. Seleccione **Azure SQL Database** en la galería de conectores de datos y, luego, seleccione **Open Connector Page** (Abrir página del conector) en el panel de vista previa.

1. En la sección **Configuración** de la página del conector, tenga en cuenta las dos categorías de registros que puede conectar.

### <a name="connect-diagnostics-logs"></a>Conexión de registros de diagnóstico

1. En **Registros de diagnóstico**, expanda **Enable diagnostics logs on each of your Azure SQL databases manually** (Habilitar los registros de diagnóstico manualmente en cada base de datos de Azure SQL).

1. Seleccione el vínculo **Open Azure SQL >** (Abrir Azure SQL >) para abrir la hoja de recursos de **Azure SQL**.

1. **(Opcional)** Para encontrar fácilmente el recurso de base de datos, seleccione **Agregar filtro** en la barra de filtros de la parte superior.
    1. En la lista desplegable **Filtro**, seleccione **Tipo de recurso**.
    1. En la lista desplegable **Valor**, desactive **Seleccionar todo** y, después, seleccione **Base de datos SQL**.
    1. Haga clic en **Aplicar**.
    
1. Seleccione el recurso de base de datos cuyos registros de diagnóstico desea enviar a Azure Sentinel.

    > [!NOTE]
    > Para cada recurso de base de datos cuyos registros desea recopilar, debe repetir este proceso a partir de este paso.

1. En la página de recursos de la base de datos que ha seleccionado, en **Supervisión** en el menú de navegación, seleccione **Configuración de diagnóstico**.

    1. Seleccione el vínculo **+ Agregar configuración de diagnóstico** en la parte inferior de la tabla.

    1. En la pantalla **Configuración de diagnóstico**, escriba un nombre en el campo **Nombre de configuración de diagnóstico**.
    
    1. En la columna **Detalles del destino**, marque la casilla **Send to Log Analytics workspace** (Enviar a área de trabajo de Log Analytics). Se mostrarán dos nuevos campos debajo. Elija la **suscripción** y el **área de trabajo de Log Analytics** pertinentes (donde reside Azure Sentinel).

    1. En la columna **Detalles de la categoría**, active las casillas de los tipos de registro y métrica que desee ingerir. Se recomienda seleccionar todos los tipos disponibles en **registro** y **métrica**.

    1. En la parte superior de la pantalla, seleccione **Guardar**.

- Como alternativa, puede usar el **script de PowerShell** proporcionado para conectar los registros de diagnóstico.
    1. En **Registros de diagnósticos**, expanda **Enable by PowerShell script** (Habilitar mediante el script de PowerShell).

    1. Copie el bloque de código y péguelo en PowerShell.

### <a name="connect-audit-logs"></a>Conexión de los registros de auditoría

1. En **Auditing logs (preview)** [Registros de auditoría (versión preliminar)], expanda **Enable auditing logs on all Azure SQL databases (at the server level)** [Habilitar registros de auditoría en todas las bases de datos de Azure SQL (en el nivel de servidor)].

1. Seleccione el vínculo **Open Azure SQL >** (Abrir Azure SQL >) para abrir la hoja de recursos de **Servidores SQL Server**.

1. Seleccione la instancia de SQL Server cuyos registros de auditoría desea enviar a Azure Sentinel.

    > [!NOTE]
    > Para cada recurso de servidor cuyos registros desea recopilar, debe repetir este proceso a partir de este paso.

1. En la página de recursos del servidor que ha seleccionado, en **Seguridad** en el menú de navegación, seleccione **Auditoría**.

    1. Mueva el botón de alternancia **Enable Azure SQL Auditing** (Habilitar auditoría de Azure SQL) a **ON** (Activar).

    1. En **Destino del registro de auditoría**, seleccione **Log Analytics (versión preliminar)** .
    
    1. En la lista de áreas de trabajo que aparece, elija el área de trabajo (donde reside Azure Sentinel).

    1. En la parte superior de la pantalla, seleccione **Guardar**.

- Como alternativa, puede usar el **script de PowerShell** proporcionado para conectar los registros de diagnóstico.
    1. En **Auditing logs** (Registros de auditoría), expanda **Enable by PowerShell script** (Habilitar mediante el script de PowerShell).

    1. Copie el bloque de código y péguelo en PowerShell.


> [!NOTE]
>
> Con este conector de datos concreto, los indicadores de estado de conectividad (una franja de color en la galería de conectores de datos y los iconos de conexión situados junto a los nombres de tipo de datos) se mostrarán como *Conectado* (verde) solo si los datos se han ingerido en algún momento en las dos últimas semanas. Transcurridas dos semanas sin ingesta de datos, el conector se mostrará como desconectado. En el momento en que circulen más datos, se devolverá el estado *conectado*.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar los registros de auditoría y diagnóstico de bases de datos de Azure SQL a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).