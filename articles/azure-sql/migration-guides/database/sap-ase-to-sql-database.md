---
title: 'De SAP ASE a Azure SQL Database: guía de migración'
description: En esta guía se explica cómo migrar las bases de datos de SAP ASE a Azure SQL Database mediante SQL Server Migration Assistant para SAP Adaptive Server Enterprise.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 81956a16142f314f54afd9d5a1b9055a559e906c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103564704"
---
# <a name="migration-guide-sap-ase-to-azure-sql-database"></a>Guía de migración: de SAP ASE a Azure SQL Database
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

En esta guía se explica cómo migrar las bases de datos de SAP ASE a Azure SQL Database mediante SQL Server Migration Assistant para SAP Adaptive Server Enterprise.

Para ver otras guías de migración, consulte [Migración de bases de datos](https://datamigration.microsoft.com/). 

## <a name="prerequisites"></a>Requisitos previos 

Para migrar la base de datos de SAP ASE a Azure SQL Database, necesita lo siguiente:

- Comprobar que el entorno de origen es compatible. 
- [SQL Server Migration Assistant para SAP Adaptive Server Enterprise (anteriormente, SAP Sybase ASE)](https://www.microsoft.com/en-us/download/details.aspx?id=54256). 

## <a name="pre-migration"></a>Antes de la migración

Una vez cumplidos los requisitos previos, estará a punto para detectar la topología de su entorno y evaluar la viabilidad de la migración.

### <a name="assess"></a>Evaluar

Use [SQL Server Migration Assistant (SSMA) para SAP Adaptive Server Enterprise (anteriormente, SAP Sybase ASE)](https://www.microsoft.com/en-us/download/details.aspx?id=54256) para revisar los datos y objetos de base de datos, evaluar las bases de datos para la migración, migrar objetos de base de datos de Sybase a Azure SQL Database y, después, migrar datos a Azure SQL Database. Para obtener más información, consulte [SQL Server Migration Assistant para Sybase (SybaseToSQL)](/sql/ssma/sybase/sql-server-migration-assistant-for-sybase-sybasetosql).

Para crear una evaluación, siga estos pasos: 

1. Abra **SSMA para Sybase**. 
1. Seleccione **Archivo** y, luego, elija **Nuevo proyecto**. 
1. Facilite el nombre del proyecto y una ubicación para guardarlo y, luego, seleccione Azure SQL Database como destino de la migración en la lista desplegable. Seleccione **Aceptar**.
1. Escriba los valores de los detalles de la conexión de SAP en el cuadro de diálogo **Conectarse a Sybase**. 
1. Haga clic con el botón derecho en la base de datos de SAP que quiera migrar y, luego, elija **Crear informe**. Se generará un informe HTML.
1. Revise el informe en HTML para comprender las estadísticas de conversión, y los errores o advertencias. También puede abrir el informe en Excel para obtener un inventario de objetos DB2 y conocer el esfuerzo necesario para realizar las conversiones de esquema. La ubicación predeterminada del informe es la carpeta de informes dentro de SSMAProjects.

   Por ejemplo: `drive:\<username>\Documents\SSMAProjects\MyDB2Migration\report\report_<date>`. 


### <a name="validate-type-mappings"></a>Validación de las asignaciones de tipos

Antes de llevar a cabo la conversión de esquemas, valide las asignaciones de tipos de datos predeterminadas o cámbielas en función de los requisitos. Para ello, vaya al menú **Herramientas** y seleccione **Configuración del proyecto**, o bien cambie la asignación de tipos de cada tabla seleccionando la tabla en el **Explorador de metadatos de SAP ASE**.


### <a name="convert-schema"></a>Convertir esquema

Para convertir el esquema, siga estos pasos:

1. (Opcional) Para convertir consultas dinámicas o ad hoc, haga clic con el botón derecho en el nodo y elija **Agregar instrucción**. 
1. Seleccione **Conectarse a Azure SQL Database** en la barra de navegación de la línea superior y proporcione los detalles de Azure SQL Database. Puede optar por conectarse a una base de datos existente o proporcionar un nombre nuevo, en cuyo caso se creará una base de datos en el servidor de destino.
1. Haga clic con el botón derecho en el esquema de SAP ASE en el **Explorador de metadatos de Sybase** y seleccione **Convertir esquema**. Como alternativa, puede seleccionar **Convertir esquema** en la barra de navegación superior. 
1. Compare y revise la estructura del esquema para identificar posibles problemas. 

   Después de convertir el esquema, puede guardar este proyecto localmente para un ejercicio de corrección de esquema sin conexión. Seleccione **Guardar proyecto** en el menú **Archivo**. Esto le ofrece la oportunidad de evaluar los esquemas de origen y de destino sin conexión, y hacer correcciones para poder publicar el esquema en Azure SQL Database.

Para obtener más información, consulte [Conversión de esquema](/sql/ssma/sybase/converting-sybase-ase-database-objects-sybasetosql).


## <a name="migrate"></a>Migrar 

Una vez que haya satisfecho los requisitos previos necesarios y completado las tareas asociadas a la fase de **migración previa**, estará a punto para llevar a cabo el esquema y la migración de datos.

Para publicar el esquema y migrar los datos, siga estos pasos: 

1. Haga clic con el botón derecho en la base de datos del **Explorador de metadatos de Azure SQL Database** y elija **Sincronizar con base de datos**.  Esta acción publica el esquema de SAP ASE en la instancia de Azure SQL Database.
1. Haga clic con el botón derecho en el esquema de SAP ASE en el **Explorador de metadatos de SAP ASE** y seleccione **Migrar datos**.  Como alternativa, puede seleccionar **Migrar datos** en la barra de navegación superior.  
1. Una vez finalizada la migración, vea el **informe de migración de datos**: 
1. Para validar la migración, revise los datos y el esquema en la instancia de Azure SQL Database mediante Azure SQL Database Management Studio (SSMS).


## <a name="post-migration"></a>Etapa posterior a la migración 

Cuando haya completado correctamente la fase de **migración**, deberá realizar una serie de tareas posteriores para asegurarse de que todo funcione de la forma más fluida y eficaz posible.

### <a name="remediate-applications"></a>Corrección de las aplicaciones

Cuando se hayan migrado los datos al entorno de destino, todas las aplicaciones que antes utilizaban el origen deben empezar a utilizar el destino. Lograr esto requerirá en algunos casos realizar cambios en las aplicaciones.

### <a name="perform-tests"></a>Realización de pruebas

El enfoque de prueba para la migración de bases de datos consiste en realizar las siguientes actividades:

1. **Desarrollar pruebas de validación**. para probar la migración de bases de datos, debe utilizar consultas SQL. Debe crear las consultas de validación para que se ejecuten en las bases de datos de origen y destino. Las consultas de validación deben abarcar el ámbito definido.

2. **Configurar un entorno de prueba**. el entorno de prueba debe contener una copia de la base de datos de origen y la base de datos de destino. Asegúrese de aislar el entorno de prueba.

3. **Ejecutar pruebas de validación**. ejecute las pruebas de validación en el origen y el destino y, luego, analice los resultados.

4. **Ejecutar pruebas de rendimiento**. ejecute la prueba de rendimiento en el origen y el destino y, luego, analice y compare los resultados.

### <a name="optimize"></a>Optimización

La fase posterior a la migración es fundamental para reconciliar cualquier problema de precisión de datos y comprobar su integridad, así como para solucionar problemas de rendimiento con la carga de trabajo.

> [!NOTE]
> Para obtener detalles adicionales sobre estos problemas y los pasos específicos para mitigarlos, consulte la [Guía de optimización y validación posterior a la migración](/sql/relational-databases/post-migration-validation-and-optimization-guide).


## <a name="next-steps"></a>Pasos siguientes

- Para obtener una matriz de los servicios y las herramientas de Microsoft y de otros fabricantes que están disponibles para ayudarlo en diversos escenarios de migración de datos y bases de datos, además de las tareas especializadas, consulte [Servicios y herramientas de migración de datos](../../../dms/dms-tools-matrix.md).

- Para más información acerca de Azure SQL Database, consulte:
   - [Información general sobre SQL Database](../../database/sql-database-paas-overview.md)
   - [Calculadora del costo total de propiedad de Azure](https://azure.microsoft.com/pricing/tco/calculator/) 


- Para más información sobre el marco y el ciclo de adopción de las migraciones en la nube, consulte:
   -  [Cloud Adoption Framework para Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Procedimientos recomendados para la gestión de los costos y los ajustes de tamaño de las cargas de trabajo migradas a Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Para evaluar el nivel de acceso de la aplicación, consulte [Data Access Migration Toolkit (versión preliminar)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Para más información sobre cómo realizar pruebas A/B en la capa de acceso a datos, consulte [Información general del Asistente para experimentación con bases de datos](/sql/dea/database-experimentation-assistant-overview).
