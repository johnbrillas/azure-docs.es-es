---
title: Generación de informes
description: Obtenga información sobre actividad de red, riesgos, ataques y tendencias mediante el uso de las herramientas de informes de Defender para IoT.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/17/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: d0c3f531ede0a590a6ba7bb21c6edbd768c08069
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97836873"
---
# <a name="generate-reports"></a>Generación de informes

Obtenga información sobre actividad de red, riesgos, ataques y tendencias mediante el uso de las herramientas de informes de Azure Defender para IoT. Existen herramientas disponibles para generar informes:

- Basadas en la actividad detectada por sensores individuales.
- Basadas en la actividad detectada por todos los sensores. 

Estos informes se generan desde la consola de administración local.

## <a name="reports-for-sensor-risk-assessment"></a>Informes para la evaluación de riesgos del sensor

Los informes de evaluación de riesgos permiten generar una puntuación de seguridad para cada dispositivo de red, así como una puntuación general de seguridad de red. La puntuación total representa un porcentaje de seguridad del 100 %.

Esta puntuación se basa en los resultados de la inspección de paquetes, los motores de modelado de comportamiento y un diseño de máquina de estados específico de SCADA. Hay disponible una amplia gama de información, por ejemplo:

- Problemas de configuración

- Vulnerabilidad del dispositivo priorizada por nivel de seguridad

- Problemas de seguridad de red

- Problemas operativos de red

- Vectores de ataque

- Conexiones a redes ICS

- Conexiones a Internet

- Indicadores de malware industrial

- Problemas de protocolo

  - Dispositivos seguros: dispositivos con una puntuación de seguridad superior al 90 %.

- **Dispositivos seguros**: dispositivos con una puntuación de seguridad superior al 90 %.

- **Dispositivos vulnerables**: dispositivos con una puntuación de seguridad inferior al 70 %.

- **Dispositivos que necesitan mejorar**: dispositivos con una puntuación de seguridad entre el 70 y el 89 %.

Para crear un informe:

1. Seleccione **Evaluación de riesgos** en el menú lateral.
2. Seleccione un sensor en la lista desplegable **Select Sensor** (Seleccionar sensor).
3. Seleccione **Generar informe**.
4. Seleccione **Descargar** desde la sección Informes archivados.

:::image type="content" source="media/how-to-generate-reports/risk-assessment.png" alt-text="Vista de la evaluación de riesgos.":::

Para importar un logotipo de empresa:

Para importar un logotipo de empresa:

- Seleccione **Import Logo** (Importar logotipo).

## <a name="reports-for-sensor-attack-vector"></a>Informes de vector de ataque de sensor

Los informes de vectores de ataque proporcionan una representación gráfica de una cadena de vulnerabilidades de dispositivos que se pueden infringir. Estas vulnerabilidades pueden conceder a un atacante acceso a dispositivos de red clave. El simulador de vectores de ataque calcula este tipo de vectores en tiempo real y los analiza todos para un destino específico.

Trabajar con un vector de ataque le permite evaluar el efecto de las actividades de mitigación en la secuencia de ataque. Posteriormente, puede determinar, por ejemplo, si una actualización del sistema altera la ruta de acceso del atacante interrumpiendo la cadena de ataque o si permanece una ruta de acceso de ataque alternativa. Esta información ayuda a priorizar las actividades de corrección y mitigación.

:::image type="content" source="media/how-to-generate-reports/control-center.png" alt-text="Vista de las alertas en el centro de control.":::

> [!NOTE]
> Los administradores y los analistas de seguridad pueden llevar a cabo los procedimientos descritos en esta sección.

Para crear una simulación de vector de ataque:

1. Seleccione el :::image type="content" source="media/how-to-generate-reports/plus.png" alt-text="signo más"::: en el menú lateral para agregar una simulación.

 :::image type="content" source="media/how-to-generate-reports/vector.png" alt-text="Simulación del vector de ataque.":::

2. Introduzca las propiedades de simulación:

   - **Nombre**: nombre de la simulación.

   - **Maximum vectors** (Máximo de vectores): número máximo de vectores en una sola simulación.

   - **Show in Device map** (Mostrar en el mapa del dispositivos): muestra el vector de ataque como filtro en el mapa del dispositivo.

   - **All Source devices** (Todos los dispositivos de origen): el vector de ataque tendrá en cuenta todos los dispositivos como un origen de ataque.

   - **Attack Source** (Origen de ataque): el vector de ataque solo tendrá en cuenta los dispositivos especificados como un origen de ataque.

   - **All Target devices** (Todos los dispositivos de destino): el vector de ataque tendrá en cuenta todos los dispositivos como un destino de ataque.

   - **Attack Target** (Destino de ataque): el vector de ataque tendrá en cuenta solo los dispositivos especificados como un destino de ataque.

   - **Excluir dispositivos**: los dispositivos especificados se excluirán de la simulación del vector de ataque.

   - **Excluir subredes**: las subredes especificadas se excluirán de la simulación del vector de ataque.

3. Seleccione **Add Simulation** (Agregar simulación). La simulación se agregará a la lista de simulaciones.

   :::image type="content" source="media/how-to-generate-reports/new-simulation.png" alt-text="Agregar una nueva simulación.":::

4. Seleccione :::image type="icon" source="media/how-to-generate-reports/edit-a-simulation-icon.png" border="false"::: si quiere editar la simulación.

   Seleccione :::image type="icon" source="media/how-to-generate-reports/delete-simulation-icon.png" border="false"::: si quiere eliminar la simulación.

   Seleccione :::image type="icon" source="media/how-to-generate-reports/make-a-favorite-icon.png" border="false"::: si quiere marcar la simulación como favorita.

5. Aparece una lista de vectores de ataque que incluye la puntuación vectorial (sobre un total de 100), el dispositivo de origen de ataque y el dispositivo de destino de ataque. Seleccione un ataque específico para la representación gráfica de vectores de ataque.

   :::image type="content" source="media/how-to-generate-reports/sample-attack-vectors.png" alt-text="Vectores de ataque.":::

## <a name="sensor-data-mining-queries"></a>Consultas de minería de datos del sensor

Las herramientas de minería de datos permiten generar información exhaustiva y detallada sobres los dispositivos de red en varias capas. Por ejemplo, puede crear una consulta basada en:

- Períodos de tiempo

- Conexiones a Internet

- Puertos

- Protocolos

- Versiones de firmware

- Comandos de programación

- Inactividad del dispositivo

Puede ajustar el informe en función de los filtros. Por ejemplo, puede consultar una subred específica en la que se ha actualizado el firmware.

:::image type="content" source="media/how-to-generate-reports/active-device-list-v2.png" alt-text="Lista de dispositivos activos.":::

Existen varias herramientas disponibles para administrar consultas. Por ejemplo, puede exportar y guardar.

> [!NOTE]
> Los administradores y los analistas de seguridad tienen acceso a las opciones de minería de datos.

### <a name="dynamic-updates"></a>Actualizaciones dinámicas

Las consultas de minería de datos que se crean se actualizan dinámicamente cada vez que se abren. Por ejemplo:

- Si crea un informe para las versiones de firmware en los dispositivos el 1 de junio y abre el informe de nuevo el 10 de junio, este informe se actualizará con la información precisa para el 10 de junio.

- Si crea un informe para detectar nuevos dispositivos descubiertos en los últimos 30 días el 1 de junio y abre el informe el 30 de junio, se mostrarán los resultados de los últimos 30 días.

### <a name="data-mining-use-cases"></a>Casos de uso de minería de datos

Puede usar consultas para controlar una amplia gama de necesidades de seguridad para varios equipos de seguridad:

- **Respuesta a los incidentes SOC**: genere un informe en tiempo real para ayudar a gestionar la respuesta inmediata a los incidentes. Por ejemplo, genere un informe para una lista de dispositivos que puedan requerir una revisión.

- **Análisis forense**: genere un informe basado en datos históricos de informes de investigación.

- **Integridad de red de TI**: genere un informe que ayude a mejorar la seguridad general de la red. Por ejemplo, genere un informe que muestre los dispositivos con credenciales de autenticación poco seguras.

- **Visibilidad**: genere un informe que abarque todos los elementos de consulta para ver todos los parámetros de línea de base de la red.

### <a name="data-mining-storage"></a>Almacenamiento de la minería de datos

La información de minería de datos se guarda y almacena continuamente, excepto cuando se elimina un dispositivo. Los resultados de minería de datos se pueden exportar y almacenar externamente en un servidor seguro. Además, el sensor realiza copias de seguridad diarias automáticas para garantizar la continuidad del sistema y la conservación de los datos.

### <a name="data-mining-and-reports"></a>Minería de datos e informes 

Los informes regulares, a los que se accede desde la opción **Informes**, son informes de minería de datos predefinidos. No son consultas dinámicas como están disponibles en la minería de datos, sino una representación estática de los resultados de la consulta de minería de datos.

Los resultados de la consulta de minería de datos no están disponibles para los usuarios de solo lectura (RO). Los administradores y los analistas de seguridad que quieren que los usuarios de solo lectura tengan acceso a la información generada por las consultas de minería de datos deben guardar la información como informe.

### <a name="predefined-queries"></a>Consultas predefinidas

Están disponibles las siguientes consultas predefinidas. Estas consultas se generan en tiempo real.

- **CVE**: una lista de dispositivos detectados con vulnerabilidades conocidas en las últimas 24 horas.

- **Excluded CVEs** (CVE excluidas): una lista de todas las CVE que se excluyeron manualmente. Para obtener resultados más precisos en los informes de VA y los vectores de ataque, puede personalizar la lista de CVE manualmente incluyendo y excluyendo CVE.

- **Internet activity** (Actividad de Internet): dispositivos que están conectados a Internet.

- **Nonactive devices** (Dispositivos no activos): dispositivos que no han establecido comunicación durante los últimos siete días.

- **Dispositivos activos**: dispositivos de red activos en las últimas 24 horas.

- **Acceso remoto**: dispositivos que se comunican a través de protocolos de sesión remota.

- **Programming commands** (Comandos de programación): dispositivos que envían programación industrial.

Se puede acceder a estos informes automáticamente desde la pantalla **Informes**, donde los usuarios de solo lectura y otros usuarios pueden verlos. Los usuarios de solo lectura no pueden acceder a los informes de minería de datos.

:::image type="content" source="media/how-to-generate-reports/data-mining-screeshot-v2.png" alt-text="Pantalla de minería de datos.":::

### <a name="create-a-data-mining-report"></a>Creación de un informe de minería de datos

Para crear un informe de minería de datos:

1. Seleccione **Minería de datos** en el menú lateral. Los informes sugeridos predefinidos aparecen automáticamente.

 :::image type="content" source="media/how-to-generate-reports/data-mining-screeshot-v2.png" alt-text="Seleccione Minería de datos en el panel lateral.":::

2. Seleccione :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false":::.

3. Seleccione **Nuevo informe** y defina el informe.

   :::image type="content" source="media/how-to-generate-reports/create-new-report-screen.png" alt-text="Cree un informe rellenando esta pantalla.":::

   Los siguientes parámetros están disponibles:

   - Proporcione un nombre y una descripción del informe.

   - En el caso de las categorías, seleccione:

      - **Categorías (todo)** para ver todos los resultados del informe sobre todos los dispositivos de la red.

      - **Genérico** para elegir categorías estándar.

   - Seleccione los parámetros de informe específicos que le interesen.

   - Elija un criterio de ordenación (**Ordenar por**). Ordene los resultados en función de la actividad o categoría.

   - Seleccione **Save to Report Pages** (Guardar en las páginas de informe) para guardar los resultados del informe como un informe al que se puede acceder desde la página **Informes**. Esto permitirá que los usuarios de solo lectura ejecuten el informe que ha creado.

   - Seleccione **Filtros (agregar)** para agregar más filtros. Se admiten solicitudes con comodín.

   - Especifique un grupo de dispositivos (definido en el mapa del dispositivo).

   - Especifique una dirección IP.

   - Especifique un puerto.

   - Especifique una dirección MAC.

4. Seleccione **Guardar**. Los resultados del informe se abren en la página **Minería de datos**.

:::image type="content" source="media/how-to-generate-reports/data-mining-page.png" alt-text="Resultados del informe como se muestran en la página Minería de datos.":::

### <a name="manage-data-mining-reports"></a>Administración de informes de minería de datos

En la tabla siguiente se describen las opciones de administración para la minería de datos:

| Imagen de icono | Descripción |
|--|--|
| :::image type="icon" source="media/how-to-generate-reports/edit-a-simulation-icon.png" border="false"::: | Editar los parámetros de informe. |
| :::image type="icon" source="media/how-to-generate-reports/export-as-pdf-icon.png" border="false"::: | Exportar como PDF. |
| :::image type="icon" source="media/how-to-generate-reports/csv-export-icon.png" border="false"::: |Exportar como CSV. |
| :::image type="icon" source="media/how-to-generate-reports/information-icon.png" border="false"::: | Mostrar información adicional, como la fecha de la última modificación. Use esta característica para crear una instantánea del resultado de la consulta. Es posible que tenga que hacerlo para llevar a cabo una investigación más detallada con los jefes de equipo o analistas de SOC, por ejemplo. |
| :::image type="icon" source="media/how-to-generate-reports/pin-icon.png" border="false"::: | Mostrar en la página **Informes** u ocultar en la página **Informes**. :::image type="content" source="media/how-to-generate-reports/hide-reports-page.png" alt-text="Ocultar o mostrar los informes."::: |
| :::image type="icon" source="media/how-to-generate-reports/delete-simulation-icon.png" border="false"::: | Elimine el informe. |

#### <a name="create-customized-directories"></a>Creación de directorios personalizados 

Puede organizar la amplia información de las consultas de minería de datos creando directorios para las categorías. Por ejemplo, puede crear directorios para protocolos o ubicaciones.

Para crear un directorio nuevo:

1. Seleccione :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false"::: para agregar un directorio nuevo.

2. Seleccione **New Directory** (Nuevo directorio) para mostrar el formulario de directorio nuevo.

3. Asigne un nombre al directorio nuevo.

4. Arrastre los informes necesarios al directorio nuevo. En cualquier momento, puede volver a arrastrar el informe a la vista principal.

5. Haga clic con el botón derecho en el nuevo directorio para abrirlo, editarlo o eliminarlo.

#### <a name="create-snapshots-of-report-results"></a>Creación de instantáneas de los resultados del informe

Es posible que tenga que guardar determinados resultados de la consulta para llevar a cabo una investigación más detallada. Por ejemplo, puede que necesite compartir los resultados con varios equipos de seguridad.

Las instantáneas se guardan en los resultados del informe y no generan consultas dinámicas.

:::image type="content" source="media/how-to-generate-reports/report-results-report.png" alt-text="Instantáneas.":::

Para crear una instantánea:

1. Abra el informe requerido.

2. Seleccione el icono de información :::image type="icon" source="media/how-to-generate-reports/information-icon.png" border="false":::.

3. Seleccione **Take New** (Crear nueva).

4. Escriba un nombre para la instantánea y seleccione **Guardar**.

:::image type="content" source="media/how-to-generate-reports/take-a-snapshot.png" alt-text="Realizar una instantánea.":::

#### <a name="customize-the-cve-list"></a>Personalización de la lista de CVE

Puede personalizar manualmente la lista de CVE de la siguiente manera:

  - Incluir o excluir CVE

  - Cambiar la puntuación de CVE

Para realizar cambios manuales en el informe de CVE:

1.  Seleccione **Minería de datos** en el menú lateral.

2. Seleccione :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false"::: en la esquina superior izquierda de la ventana **Minería de datos**. Seleccione **Nuevo informe**.

   :::image type="content" source="media/how-to-generate-reports/create-a-new-report-screen.png" alt-text="Crear un informe nuevo.":::

3. En el panel izquierdo, seleccione una de las opciones siguientes:

   - **Known Vulnerabilities** (Vulnerabilidades conocidas): selecciona ambas opciones y presenta los resultados en las dos tablas del informe, una con CVE y la otra con las CVE excluidas.

   - **CVE**: seleccione esta opción para presentar una lista de todas las CVE.

   - **Excluded CVEs** (CVE excluidas): Seleccione esta opción para presentar una lista de todas las CVE excluidas.

4. Rellene la información de **Nombre** y **Descripción** y seleccione **Guardar**. El nuevo informe aparece en la ventana **Minería de datos**.

5. Para excluir CVE, abra el informe de minería de datos para las CVE. Aparece la lista de todas las CVE.

   :::image type="content" source="media/how-to-generate-reports/cves.png" alt-text="Informe de CVE.":::

6. Para habilitar la selección de elementos de la lista, seleccione :::image type="icon" source="media/how-to-generate-reports/enable-selecting-icon.png" border="false"::: y seleccione las CVE que quiere personalizar. La barra **Operaciones** aparece en la parte inferior.

   :::image type="content" source="media/how-to-generate-reports/operations-bar-appears.png" alt-text="Captura de pantalla de la barra Operaciones de Minería de datos.":::

7. Seleccione las CVE que quiere excluir y, posteriormente, seleccione **Eliminar registros**. Las CVE que ha seleccionado no aparecen en la lista de CVE y se mostrarán en la lista de CVE excluidas al generar una.

8. Para incluir las CVE excluidas en la lista de CVE, genere el informe para las CVE excluidas y elimine de esa lista los elementos que quiere incluir de nuevo en la lista de CVE.

9. Seleccione las CVE en las que quiere cambiar la puntuación y, posteriormente, seleccione **Update CVE Score** (Actualizar puntuación de CVE).

   :::image type="content" source="media/how-to-generate-reports/set-new-score-screen.png" alt-text="Actualice la puntuación de CVE.":::

10. Escriba la nueva puntuación y seleccione **Aceptar**. La puntuación actualizada aparece en las CVE que ha seleccionado.

### <a name="reports-based-on-data-mining"></a>Informes basados en la minería de datos

Los informes reflejan la información generada por los resultados de la consulta de minería de datos. Esto incluye los informes de minería de datos predeterminados, que están disponibles en la vista Informes. El administrador y los analistas de seguridad también pueden generar consultas de minería de datos personalizadas y guardarlas como informes. Estos informes también están disponibles para los usuarios de solo lectura.

Para generar un informe:

1. Seleccione **Informes** en el menú lateral.

2. Elija el informe necesario para mostrar. La elección puede ser de informes de tipo **Personalizado** o **Generado automáticamente**, como **Programming Commands** (Comandos de programación) y **Acceso remoto**.

3. Puede exportar el informe seleccionando uno de los iconos de la parte superior derecha de la pantalla:

   :::image type="icon" source="media/how-to-generate-reports/export-to-pdf-icon.png" border="false"::: Exportar a un archivo PDF.

   :::image type="icon" source="media/how-to-generate-reports/export-to-csv-icon.png" border="false"::: Exportar a un archivo CSV.

> [!NOTE] 
> El usuario de solo lectura solo puede ver los informes creados para esta categoría de usuarios.

:::image type="content" source="media/how-to-generate-reports/select-a-report-screen.png" alt-text="Seleccionar el informe que se generará.":::

:::image type="content" source="media/how-to-generate-reports/remote-access-report.png" alt-text="Informe de acceso remoto generado.":::

## <a name="reports-for-sensor-trends-and-statistics"></a>Informes de estadísticas y tendencias del sensor

Puede crear gráficos de widgets y gráficos circulares para obtener información sobre estadísticas y tendencias de red. Los widgets se pueden agrupar en paneles definidos por el usuario.

> [!NOTE]
> Solo los administradores y los analistas de seguridad pueden realizar los procedimientos de esta sección.

Para ver los paneles y los widgets, seleccione **Tendencias y estadísticas** en el menú lateral.

:::image type="content" source="media/how-to-generate-reports/investigation-screenshot.png" alt-text="Captura de pantalla de una investigación.":::

El panel consta de widgets que describen gráficamente los siguientes tipos de información:

- Tráfico por puerto
- Ancho de banda de canal
- Ancho de banda total
- Conexión TCP activa
- Dispositivos:
  - Dispositivos nuevos
  - Dispositivos ocupados
  - Dispositivos por proveedor
  - Dispositivos por SO
  - Dispositivo desconectados
- Caídas de conectividad por horas
- Alertas de incidentes por tipo
- Acceso de tablas de base de datos
- Widgets de disección de protocolos
- Dirección IP y Ethernet:
  - Tráfico de direcciones IP y Ethernet por servicio de CIP
  - Tráfico de direcciones IP y Ethernet por clase de CIP
  - Tráfico de direcciones IP y Ethernet por comando
- OPC:
  - Principales operaciones de administración de OPC
  - Principales operaciones de E/S de OPC
- Siemens S7:
  - Tráfico S7 por función de control
  - Tráfico S7 por subfunción
- SRTP:
  - Tráfico SRTP por código de servicio
  - Errores SRTP por día
- SuiteLink:
  - Principales etiquetas de SuiteLink consultadas
  - Comportamiento de las etiquetas numéricas de SuiteLink
- Tráfico IEC-60870 por ASDU
- Tráfico DNP3 por función
- Tráfico MMS por servicio
- Tráfico Modbus por función
- Tráfico OPC-UA por servicio

> [!NOTE]
>  La hora en los widgets se establece según la hora del sensor.

## <a name="reports-for-the-on-premises-management-console"></a>Informes para la consola de administración local

La consola de administración local permite generar informes para cada sensor que está conectado a ella. Los informes se basan en las consultas de minería de datos en el sensor que se realizan.

Se pueden generar los siguientes informes:

- **Active Devices (Last 24 Hours)** (Dispositivos activos (últimas 24 horas)): presenta una lista de dispositivos que muestran actividad de red en un período de 24 horas.

- **Non-Active Devices (Last 7 Days)** (Dispositivos no activos (últimos 7 días)): presenta una lista de dispositivos que no muestran ninguna actividad de red en los últimos siete días.

- **Programming Commands** (Comandos de programación): presenta una lista de dispositivos que han enviado comandos de programación en las últimas 24 horas.

- **Acceso remoto**: presenta una lista de dispositivos a los que se ha accedido desde orígenes remotos en las últimas 24 horas.

:::image type="content" source="media/how-to-generate-reports/reports-view.png" alt-text="Captura de pantalla de la vista de informes.":::

Al elegir el sensor desde la consola de administración local, todos los informes personalizados configurados en ese sensor aparecen en la lista de informes. Para cada sensor, puede generar un informe predeterminado o un informe personalizado configurado en ese sensor.

Para generar un informe:

1. En el panel izquierdo, seleccione **Informes**. Aparecerá la ventana **Informes**.

2. En la lista desplegable **Sensores**, seleccione el sensor para el que quiere generar el informe.

   :::image type="content" source="media/how-to-generate-reports/sensor-drop-down-list.png" alt-text="Captura de pantalla de la vista de sensores.":::

3. En la lista desplegable de la derecha, seleccione el informe que quiere generar.

4. Para crear un PDF de los resultados del informe, seleccione :::image type="icon" source="media/how-to-generate-reports/pdf-report-icon.png" border="false":::.

## <a name="risk-assessment-reports-for-the-on-premises-management-console"></a>Informes de evaluación de riesgos para la consola de administración local

Genere un informe de evaluación de riesgos para cada sensor conectado a la consola de administración local. Este informe proporciona información sobre cada una de las redes que los sensores supervisan.

Los informes de evaluación de riesgos permiten generar una puntuación de seguridad para cada dispositivo de red, así como una puntuación general de seguridad de red. La puntuación global se basa en una inspección de paquetes profunda, los motores de modelado de comportamiento y un diseño de máquina de estados específico de SCADA. Hay disponible una amplia gama de información. Por ejemplo:

- Problemas de configuración

- Vulnerabilidad del dispositivo priorizada por nivel de seguridad

- Problemas de seguridad de red

- Problemas operativos de red

- Vectores de ataque

- Conexiones a redes ICS

- Conexiones a Internet

- Indicadores de malware industrial

- Problemas de protocolo

El informe proporciona recomendaciones de mitigación que le ayudarán a mejorar la puntuación de seguridad.

- Dispositivos seguros: dispositivos con una puntuación de seguridad superior al 90 %.

- **Dispositivos vulnerables**: dispositivos con una puntuación de seguridad inferior al 70 %.

- **Dispositivos que necesitan mejorar**: dispositivos con una puntuación de seguridad entre el 70 y el 89 %.

Para crear un informe:

1. Seleccione **Evaluación de riesgos** en el menú lateral.

2. Seleccione un sensor en la lista desplegable **Select Sensor** (Seleccionar sensor).

3. Seleccione **Generar informe**.

4. Seleccione **Descargar** desde la sección **Informes archivados**.

Para importar un logotipo de empresa:

- Seleccione **Import Logo** (Importar logotipo).

:::image type="content" source="media/how-to-generate-reports/import-logo-screenshot.png" alt-text="Importar un logotipo a través de la vista de evaluación de riesgos.":::

## <a name="see-also"></a>Consulte también
[Trabajar con vistas de mapa del sitio](how-to-gain-insight-into-global-regional-and-local-threats.md#work-with-site-map-views)
