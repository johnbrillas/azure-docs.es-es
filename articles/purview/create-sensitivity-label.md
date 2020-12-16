---
title: Aplicación automática de etiquetas de confidencialidad a los datos
description: Obtenga información acerca de cómo crear etiquetas de confidencialidad y aplicarlas automáticamente a los datos durante un examen.
author: batamig
ms.author: bagol
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 12/03/2020
ms.openlocfilehash: 195ddfcc3c781c1866a7751f989c9460463edbb9
ms.sourcegitcommit: 65a4f2a297639811426a4f27c918ac8b10750d81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96558171"
---
# <a name="automatically-label-your-data-in-azure-purview"></a>Etiquetado automático de los datos en Azure Purview

En este artículo se describe cómo crear etiquetas de confidencialidad de Microsoft Information Protection (MIP) y cómo aplicarlas automáticamente a los recursos de Azure en Azure Purview.

## <a name="what-are-sensitivity-labels"></a>¿Qué son las etiquetas de confidencialidad? 

Para trabajar, las personas de la organización colaboran con otras dentro y fuera de la organización. Los datos no siempre permanecen en la nube y, a menudo, se mueven por todas partes, en dispositivos, aplicaciones y servicios. 

Cuando los datos se mueven, debe hacerlo de una manera segura y protegida que satisfaga las directivas empresariales y de cumplimiento de la organización.

La aplicación de etiquetas de confidencialidad permite indicar el grado de confidencialidad de determinados datos de la organización. Por ejemplo, un nombre de proyecto específico puede ser muy confidencial dentro de la organización, mientras que el mismo término no es confidencial para otras organizaciones. 

### <a name="sensitivity-labels-in-azure-purview"></a>Etiquetas de confidencialidad en Azure Purview

En Purview, las clasificaciones son similares a las etiquetas de asunto y se usan para marcar e identificar los datos de un tipo específico que se encuentra dentro de la información de datos durante el análisis.

Purview usa las mismas clasificaciones, también conocidas como tipos de información confidencial, que Microsoft 365.  Las etiquetas de confidencialidad de MIP se crean en el Centro de seguridad y cumplimiento (SCC) de Microsoft 365. Esto le permite extender las etiquetas de confidencialidad existentes a través de los recursos de Azure Purview.

> [!NOTE]
> Mientras que las clasificaciones coinciden directamente (un número del seguro social tiene una clasificación de **Número del seguro social**), se aplican etiquetas de confidencialidad cuando se encuentran una o varias clasificaciones y escenarios juntos. 
> 

Las etiquetas de confidencialidad de Azure Purview se pueden usar para aplicar automáticamente etiquetas a archivos y columnas de bases de datos.

Para más información, consulte:

- [Información sobre las etiquetas de confidencialidad](/microsoft-365/compliance/sensitivity-labels) en la documentación de Microsoft 365
- [¿Qué son las reglas de etiquetado automático?](#what-are-autolabeling-rules)
- [Tipos de datos admitidos para las etiquetas de confidencialidad en Azure Purview](#supported-data-types-for-sensitivity-labels-in-azure-purview)

#### <a name="what-are-autolabeling-rules"></a>¿Qué son las reglas de etiquetado automático?

Los datos aumentan y cambian constantemente. El seguimiento de los datos que no tienen etiqueta actualmente y la acción de aplicar etiquetas manualmente no solo es difícil, sino que también es innecesario. 

Las reglas de etiquetado automático son condiciones que especifica el usuario, que indica cuándo se debe aplicar una etiqueta determinada. Cuando se cumplen estas condiciones, la etiqueta se asigna automáticamente a los datos, conservando etiquetas de confidencialidad coherentes en los datos a gran escala.

Al crear las etiquetas, asegúrese de definir reglas de etiquetado automático para los [archivos](#define-autolabeling-rules-for-files) y las [columnas de bases de datos](#define-autolabeling-rules-for-database-columns) para aplicar las etiquetas automáticamente con cada examen de datos. 

Después de examinar los datos en Purview, puede ver las etiquetas que se aplican automáticamente en el catálogo de Purview y los informes de conclusiones.

#### <a name="supported-data-types-for-sensitivity-labels-in-azure-purview"></a>Tipos de datos admitidos para las etiquetas de confidencialidad en Azure Purview

Las etiquetas de confidencialidad se admiten en Azure Purview para los siguientes tipos de datos:

|Tipo de datos  |Orígenes  |
|---------|---------|
|Etiquetado automático de archivos     |      : Blob Storage de Azure  </br>- Azure Data Lake Storage Gen 1 y Gen 2  |
|Etiquetado automático de columnas de bases de datos     |  - SQL Server </br>- Azure SQL Database </br>- Instancia administrada de Azure SQL Database   <br> - Azure Synapse  <br> : Azure Cosmos DB   |
| | |

## <a name="how-to-create-sensitivity-labels-in-microsoft-365"></a>Cómo crear etiquetas de confidencialidad en Microsoft 365

Si aún no tiene etiquetas de confidencialidad, deberá crearlas y ponerlas a disposición de Azure Purview. También se pueden modificar las etiquetas de confidencialidad existentes para que estén disponibles para Azure Purview.

Para más información, consulte:

- [Requisitos de concesión de licencia](#licensing-requirements)
- [Extensión de etiquetas de confidencialidad a Azure Purview](#extending-sensitivity-labels-to-azure-purview)
- [Creación de nuevas etiquetas de confidencialidad o modificación de las etiquetas existentes](#creating-new-sensitivity-labels-or-modifying-existing-labels)
### <a name="licensing-requirements"></a>Requisitos de concesión de licencia

Las etiquetas de confidencialidad de MIP se crean y administran en el Centro de seguridad y cumplimiento (SCC) de Microsoft 365. Para crear etiquetas de confidencialidad para su uso en Azure Purview, debe tener una licencia activa de Microsoft 365 E5.

Si aún no tiene la licencia necesaria, puede registrarse para obtener una prueba de [Microsoft 365 E5](https://www.microsoft.com/microsoft-365/business/compliance-solutions#midpagectaregion).

### <a name="extending-sensitivity-labels-to-azure-purview"></a>Extensión de etiquetas de confidencialidad a Azure Purview

De manera predeterminada, las etiquetas de confidencialidad de MIP solo están disponibles para los recursos de Microsoft 365, donde puede aplicarlas a archivos y correos electrónicos.

Para aplicar etiquetas de confidencialidad de MIP a recursos de Azure en Azure Purview, debe dar su consentimiento explícito para extender las etiquetas y seleccionar las etiquetas específicas que quiere que estén disponibles en Purview.

Mediante la extensión de las etiquetas de confidencialidad de MIP con Azure Purview, las organizaciones ya pueden detectar, clasificar y obtener información sobre la confidencialidad a través de una variedad más amplia de orígenes de datos, lo que minimiza el riesgo de cumplimiento.

> [!NOTE]
> Como Microsoft 365 y Azure Purview son servicios independientes, existe la posibilidad de que se implementen en regiones diferentes. Los nombres de etiqueta y los nombres de tipo de información confidencial se consideran datos de cliente y se mantienen dentro de la misma ubicación geográfica de manera predeterminada para proteger la confidencialidad de los datos y para evitar la legislación de RGPD.
>
> Por este motivo, las etiquetas y los tipos de información confidencial personalizada no se comparten en Azure Purview de manera predeterminada y requieren su consentimiento para usarlos en Azure Purview.

> [!IMPORTANT]
> Su consentimiento permite a Microsoft compartir el nombre de la etiqueta y el nombre del tipo de información confidencial personalizada *tanto en* Azure Purview como en Azure Security Center (ASC). Microsoft usa la información de la etiqueta de Azure Purview para enriquecer sus recomendaciones y alertas en ASC. 
>
> El consentimiento del centro de cumplimiento de Microsoft 365 se aplica al uso compartido de estos datos con ambos servicios. Actualmente, no hay ninguna opción para compartir la información de etiquetado solo con Azure Purview.

**Para extender las etiquetas de confidencialidad a Purview:**

En Microsoft 365, vaya a la página **Information Protection**. En **Extender la etiqueta a los recursos de Azure Purview**, seleccione el botón **Activar** y, a continuación, seleccione **Sí** en el cuadro de diálogo de confirmación que se muestra.

Por ejemplo:

:::image type="content" source="media/create-sensitivity-label/extend-sensitivity-labels-to-purview-small.png" alt-text="Selección de **Activar** para extender las etiquetas de confidencialidad a Purview" lightbox="media/create-sensitivity-label/extend-sensitivity-labels-to-purview.png":::
 
Una vez que extienda el etiquetado a los recursos de Azure Purview, puede seleccionar las etiquetas que quiere que estén disponibles en Purview. Para obtener más información, consulte [Creación de nuevas etiquetas de confidencialidad o modificación de las etiquetas existentes](#creating-new-sensitivity-labels-or-modifying-existing-labels).
### <a name="creating-new-sensitivity-labels-or-modifying-existing-labels"></a>Creación de nuevas etiquetas de confidencialidad o modificación de las etiquetas existentes

1. Abra el [Centro de seguridad y cumplimiento de Microsoft 365](https://protection.office.com/homepage). 

1. En **Soluciones**, seleccione **Information Protection** y, a continuación, seleccione **Crear una etiqueta**. 

    :::image type="content" source="media/create-sensitivity-label/create-sensitivity-label-full-small.png" alt-text="Creación de etiquetas de confidencialidad en el Centro de seguridad y cumplimiento de Microsoft 365" lightbox="media/create-sensitivity-label/create-sensitivity-label-full.png":::

1. Asigne un nombre a la etiqueta. A continuación, en **Definir el ámbito de esta etiqueta**, seleccione **Archivos y correos electrónicos** y **Recursos de Azure Purview**.
    
    :::image type="content" source="media/create-sensitivity-label/create-label-scope-small.png" alt-text="Creación de la etiqueta en el Centro de seguridad y cumplimiento de Microsoft 365" lightbox="media/create-sensitivity-label/create-label-scope.png":::

1. Siga el resto de mensajes del asistente para la configuración de la etiqueta. 

    En concreto, defina reglas de etiquetado automático para archivos y columnas de bases de datos:

    - [Definición de reglas de etiquetado automático para archivos](#define-autolabeling-rules-for-files)
    - [Definición de reglas de etiquetado automático para columnas de base de datos](#define-autolabeling-rules-for-database-columns)

    Para obtener más información sobre las opciones del asistente, consulte [Qué pueden hacer las etiquetas de confidencialidad](/microsoft-365/compliance/sensitivity-labels#what-sensitivity-labels-can-do) en la documentación de Microsoft 365.

1. Repita los pasos indicados anteriormente para crear etiquetas adicionales. 

    Para crear una subetiqueta, seleccione la etiqueta primaria > **...**  > **Más acciones** > **Agregar subetiqueta**.

1. Para modificar las etiquetas existentes, vaya a **Information Protection** > **Etiquetas** y seleccione la etiqueta. 

    A continuación, seleccione **Editar etiqueta** para abrir de nuevo el asistente **Editar etiqueta de confidencialidad** con todas las opciones que se definieron al crear la etiqueta.

    :::image type="content" source="media/create-sensitivity-label/edit-sensitivity-label-full-small.png" alt-text="Edición de una etiqueta de confidencialidad existente" lightbox="media/create-sensitivity-label/edit-sensitivity-label-full.png":::

1. Cuando haya terminado de crear todas las etiquetas, asegúrese de ver el orden de las etiquetas y reordenarlas según sea necesario. 

    Para cambiar el orden de una etiqueta, seleccione **...** **> Más acciones** > **Subir** o **Bajar**. 

    Para obtener más información, consulte [Prioridad de etiqueta (el orden importa)](/microsoft-365/compliance/sensitivity-labels#label-priority-order-matters) en la documentación de Microsoft 365.

> [!IMPORTANT]
> No elimine una etiqueta a menos que comprenda el impacto en los usuarios. 
>
> Para obtener más información, consulte [Quitar y eliminar etiquetas](/microsoft-365/compliance/create-sensitivity-labels#removing-and-deleting-labels) en la documentación de Microsoft 365.

Continúe [examinando los datos para aplicar etiquetas automáticamente](#scan-your-data-to-apply-labels-automatically) y, a continuación:

- [Visualización de las etiquetas en los recursos](#view-labels-on-assets)
- [Visualización de los informes de conclusiones para las clasificaciones y las etiquetas de confidencialidad](#view-insight-reports-for-the-classifications-and-sensitivity-labels)

#### <a name="define-autolabeling-rules-for-files"></a>Definición de reglas de etiquetado automático para archivos

Defina las reglas de etiquetado automático para los archivos en el asistente cuando cree o edite la etiqueta. 

En la página **Etiquetado automático para aplicaciones de Office**, habilite el **etiquetado automático para las aplicaciones de Office** y, a continuación, defina las condiciones en las que quiere que la etiqueta se aplique automáticamente a los datos.

Por ejemplo:

:::image type="content" source="media/create-sensitivity-label/create-auto-labeling-rules-files-small.png" alt-text="Definición de reglas de etiquetado automático para archivos en el Centro de seguridad y cumplimiento de Microsoft 365" lightbox="media/create-sensitivity-label/create-auto-labeling-rules-files.png":::
 
Para obtener más información, consulte [Aplicación automática de una etiqueta de confidencialidad a los datos](/microsoft-365/compliance/apply-sensitivity-label-automatically#how-to-configure-auto-labeling-for-office-apps) en la documentación de Microsoft 365. 

#### <a name="define-autolabeling-rules-for-database-columns"></a>Definición de reglas de etiquetado automático para columnas de base de datos

Defina las reglas de etiquetado automático para las columnas de base de datos en el asistente cuando cree o edite la etiqueta. 

En la opción **Recursos de Azure Purview (versión preliminar)** :

1. Seleccione el control deslizante **Etiquetado automático para las columnas de bases de datos**.

1. Seleccione **Comprobar tipos de información confidencial** para elegir los tipos de información confidencial que quiere aplicar a la etiqueta.

Por ejemplo:
        
:::image type="content" source="media/create-sensitivity-label/create-auto-labeling-rules-db-columns-small.png" alt-text="Definición de reglas de etiquetado automático para columnas de SQL en el Centro de seguridad y cumplimiento de Microsoft 365" lightbox="media/create-sensitivity-label/create-auto-labeling-rules-db-columns.png":::

## <a name="scan-your-data-to-apply-labels-automatically"></a>Examen de los datos para aplicar etiquetas automáticamente

Examine los datos en Azure Purview para aplicar automáticamente las etiquetas que ha creado, según las reglas de etiquetado automático que haya definido. 

Para obtener más información sobre cómo configurar exámenes en varios recursos de Azure Purview, consulte:

|Source  |Referencia  |
|---------|---------|
|**Azure Blob Storage**     |[Registro y examen de Azure Blob Storage](register-scan-azure-blob-storage-source.md)         |
|**Almacén de Azure Data Lake**     |[Registro y examen de Azure Data Lake Storage Gen1](register-scan-adls-gen1.md) </br>[Registro y examen de Azure Data Lake Storage Gen2](register-scan-adls-gen2.md)         |
|**Instancias de Azure SQL Database**|[Registro y examen de una instancia de Azure SQL Database](register-scan-azure-sql-database.md) </br>[Registro y examen de una Instancia administrada de Azure SQL Database](register-scan-azure-sql-database-managed-instance.md)|
|**Cuentas de almacenamiento detrás de firewalls**     |[Examen de cuentas de almacenamiento detrás de un firewall en Azure Purview](scan-sqlresource-firewall.md)         |
| | |

## <a name="view-labels-on-assets"></a>Visualización de las etiquetas en los recursos

Una vez que se han definido las reglas de etiquetado automático para las etiquetas en Microsoft 365 y se han examinado los datos en Azure Purview, las etiquetas se aplican automáticamente a los recursos. 

**Para ver las etiquetas aplicadas a los recursos en el catálogo de Azure Purview:**

En el catálogo de Azure Purview, use las opciones de filtrado de **etiquetas** para mostrar solo los archivos con etiquetas específicas. Por ejemplo: 

:::image type="content" source="media/create-sensitivity-label/filter-search-results-small.png" alt-text="Buscar recursos por etiqueta" lightbox="media/create-sensitivity-label/filter-search-results.png":::

Por ejemplo:

:::image type="content" source="media/create-sensitivity-label/view-labeled-files-blob-storage-small.png" alt-text="Visualización de una etiqueta de confidencialidad en un archivo en Azure Blob Storage" lightbox="media/create-sensitivity-label/view-labeled-files-blob-storage.png":::

## <a name="view-insight-reports-for-the-classifications-and-sensitivity-labels"></a>Visualización de los informes de conclusiones para las clasificaciones y las etiquetas de confidencialidad

Busque información sobre los datos clasificados y etiquetados en Azure Purview mediante los informes de **clasificación** y **etiquetas de confidencialidad**.

> [!div class="nextstepaction"]
> [Conclusiones de clasificaciones](./classification-insights.md)

> [!div class="nextstepaction"]
> [Conclusiones de etiquetas de confidencialidad](sensitivity-insights.md)


