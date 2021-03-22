---
title: Creación de una valoración de Azure SQL
description: Aprenda a evaluar las instancias de SQL que se van a migrar a Azure SQL Managed Instance y Azure SQL Database
author: rashi-ms
ms.author: rajosh
ms.topic: tutorial
ms.date: 02/07/2021
ms.openlocfilehash: 14197516c0669055f756614b9559f1423703c6a8
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102053682"
---
# <a name="create-an-azure-sql-assessment"></a>Creación de una valoración de Azure SQL

Como parte del recorrido de la migración a Azure, puede evaluar las cargas de trabajo locales para medir la preparación de la nube, identificar los riesgos y hacer una estimación de los costos y la complejidad.
En este artículo, se explica cómo evaluar las instancias de SQL detectadas que están preparándose para migrar a Azure SQL utilizando la herramienta de detección y evaluación de Azure Migrate.

> [!Note]
> La herramienta de detección y evaluación de las bases de datos e instancias de SQL Server que se ejecutan en el entorno de VMware se encuentran ahora en versión preliminar. Para probar esta característica, use [**este vínculo**](https://aka.ms/AzureMigrate/SQL) para crear un proyecto en la región **Este de Australia**. Si ya tiene un proyecto en la región Este de Australia y desea probar esta característica, asegúrese de que ha completado estos [**requisitos previos**](how-to-discover-sql-existing-project.md) en el portal.

## <a name="before-you-start"></a>Antes de comenzar

- Asegúrese de que ha [creado](./create-manage-projects.md) un proyecto de Azure Migrate y de que ha agregado la herramienta de detección y evaluación de Azure Migrate.
- Si desea crear una evaluación, debe configurar una aplicación de Azure Migrate para [VMware](how-to-set-up-appliance-vmware.md). El dispositivo detecta los servidores locales y envía los metadatos y los datos de rendimiento a Azure Migrate. [Más información](migrate-appliance.md)

## <a name="azure-sql-assessment-overview"></a>Introducción a las evaluaciones de Azure SQL
Puede crear una evaluación de Azure SQL con criterios relacionados con el dimensionamiento, como el criterio **basado en el rendimiento**. 

**Criterio de tamaño** | **Detalles** | **Data**
--- | --- | ---
**Basada en el rendimiento** | Evaluaciones basadas en los datos de rendimiento recopilados | La **configuración de Azure SQL** recomendada se basa en los datos de rendimiento de las instancias y las bases de datos de SQL, como el uso de la CPU, el número de núcleos, la organización y el tamaño de los archivos de la base de datos, la entrada y salida de archivos, las consultas por lotes que se realizan cada segundo, el tamaño de la memoria y el uso de la base de datos que hace cada consulta.

Obtenga [más información](concepts-azure-sql-assessment-calculation.md) sobre las evaluación de Azure SQL.

## <a name="run-an-assessment"></a>Ejecución de una evaluación
Las evaluaciones se realizan como se indica a continuación:
1. En la página **Información general** > **Windows, Linux and SQL Server** (Windows, Linux y SQL Server), haga clic en **Evaluar y migrar servidores**.
    :::image type="content" source="./media/tutorial-assess-sql/assess-migrate.png" alt-text="Página de información general de Azure Migrate":::
2. En **Azure Migrate: Discovery and assessment**, (Azure Migrate: Detección y evaluación), haga clic en **Evaluar** y elija el tipo de evaluación **Azure SQL**.
    :::image type="content" source="./media/tutorial-assess-sql/assess.png" alt-text="Lista desplegable en la que se puede elegir el tipo de evaluación Azure SQL":::
3. En **Evaluar los servidores**, podrá ver que el tipo de evaluación preseleccionado es **Azure SQL** y que el origen de detección se ha establecido de forma predeterminada en **Servers discovered from Azure Migrate appliance** (Servidores detectados desde el dispositivo de Azure Migrate).

4. Haga clic en **Editar** para revisar las propiedades de la evaluación.
     :::image type="content" source="./media/tutorial-assess-sql/assess-servers-sql.png" alt-text="Botón Editar desde el que se pueden personalizar las propiedades de la evaluación":::
5. En Propiedades de evaluación > **Propiedades de destino**:
    - En **Ubicación de destino**, especifique la región de Azure a la que desee migrar. 
        - La configuración de Azure SQL y los costos recomendados dependen de la ubicación que especifique. 
    - En **Target deployment type** (Tipo de implementación de destino):
        - Seleccione **Opción recomendada** si desea que Azure Migrate evalúe el grado de preparación de las instancias de SQL para su migración a Azure SQL MI y Azure SQL DB, y recomiende la opción de implementación de destino, el nivel de destino, la configuración de Azure SQL y las estimaciones mensuales que mejor se ajusten. [Más información](concepts-azure-sql-assessment-calculation.md)
        - Seleccione **Azure SQL DB** si quiere evaluar el grado de preparación que tienen las instancias de SQL para migrar exclusivamente a Azure SQL Database y revisar el nivel de destino, la configuración de Azure SQL y las estimaciones mensuales.
        - Seleccione **Azure SQL MI** si quiere evaluar el grado de preparación que tienen las instancias de SQL para migrar exclusivamente a Azure SQL Managed Instance y revisar el nivel de destino, la configuración de Azure SQL y las estimaciones mensuales.
    - En **Capacidad reservada**, especifique si desea usar la capacidad reservada para el servidor SQL Server después de la migración.
        - Si selecciona una opción de capacidad reservada, no podrá especificar "Descuento (%)".

6. En Propiedades de evaluación > **Assessment criteria** (Criterios de evaluación):
    - El valor de "Criterio de tamaño" está establecido de forma predeterminada en **Basado en el rendimiento**, lo que significa que Azure Migrate recopilará las métricas de rendimiento relativas a las instancias y la bases de datos de SQL que administra para recomendar una configuración de Azure SQL Database y Azure SQL Managed Instance que tenga el tamaño óptimo. Puede especificar:
        - Un valor en **Historial de rendimiento** para indicar la duración de los datos en los que desea basar la evaluación. (El valor predeterminado es un día).
        - Un valor en **Utilización del percentil** para especificar el valor de percentil que desea utilizar con la muestra de rendimiento. (El valor predeterminado es el percentil 95).
    - En **Factor de confort**, indique el búfer que desee usar durante la valoración. Tiene en cuenta problemas como el uso estacional, el historial de rendimiento corto y los posibles aumentos en el uso futuro. Por ejemplo, si usa un factor de confort de dos: 
        
        **Componente** | **Utilización efectiva** | **Agregar factor de confort (2,0)**
        --- | --- | ---
        Núcleos | 2  | 4
        Memoria | 8 GB | 16 GB
   
7. En **Precios**:
    - En **Offer/Licensing program** (Oferta/programa de licencias), especifique la oferta de Azure en la que está inscrito. En la actualidad, solo puede elegir entre "pago por uso" y "desarrollo/pruebas de pago por uso". 
        - Puede beneficiarse de descuentos adicionales combinando la capacidad reservada y la Ventaja híbrida de Azure con la oferta de pago por uso. 
        - Puede combinar la Ventaja híbrida de Azure con la opción de desarrollo/pruebas de pago por uso. Actualmente, la evaluación no permite combinar la capacidad reservada con la oferta de desarrollo/pruebas de pago por uso.
    - En **Nivel de servicio**, elija la opción de nivel de servicio que mejor se ajuste a las necesidades de la empresa para migrar a Azure SQL Database o Azure SQL Managed Instance: 
        - Seleccione **Opción recomendada** si desea que Azure Migrate recomiende el nivel de servicio que se ajuste mejor a los servidores. Esta opción podría ser Uso general o Crucial para la empresa. Más información
        - Seleccione **Uso general** si desea utilizar una configuración de Azure SQL diseñada para cargas de trabajo en las que hay presupuestos implicados.
        - Seleccione **Crucial para la empresa** si desea utilizar una configuración de Azure SQL diseñada para cargas de trabajo de baja latencia con alta resistencia a errores y conmutaciones por error rápidas.
    - En **Descuento (%)** , agregue cualquier descuento específico de la suscripción que reciba a partir de la oferta de Azure. La configuración predeterminada es 0 %.
    - En **Moneda**, seleccione la moneda de facturación para la cuenta.
    - En **Ventaja híbrida de Azure**, especifique si ya tiene una licencia de SQL Server. Si la tiene y está incluida en las suscripciones activas de Software Assurance para SQL Server, podrá solicitar la Ventaja híbrida de Azure cuando traiga sus licencias a Azure.
    - Haga clic en Guardar si realiza cambios.
     :::image type="content" source="./media/tutorial-assess-sql/view-all.png" alt-text="Botón Guardar de Propiedades de evaluación":::
8. En **Evaluar los servidores**, haga clic en Siguiente.
9.  En **Select servers to assess**(Seleccionar servidores que se van a evaluar) > **Nombre de la evaluación**, especifique el nombre de la evaluación.
10. En **Seleccionar o crear un grupo**, seleccione **Crear nuevo** y especifique un nombre de grupo.
     :::image type="content" source="./media/tutorial-assess-sql/assessment-add-servers.png" alt-text="Ubicación del botón para crear un nuevo grupo":::
11. Seleccione el dispositivo y los servidores que desee agregar al grupo. A continuación, haga clic en Siguiente.
12. En **Revisar y crear valoración**, revise los detalles de la valoración y haga clic en Crear evaluación para crear el grupo y ejecutar la evaluación.
     :::image type="content" source="./media/tutorial-assess-sql/assessment-create.png" alt-text="Ubicación del botón Revisar y crear valoración.":::
13. Una vez creada la evaluación, vaya al icono **Windows, Linux and SQL Server**(Windows, Linux y SQL Server) > **Azure Migrate: Discovery and assessment** (Azure Migrate: Detección y evaluación) y haga clic en el número que aparece junto a evaluación de Azure SQL.
     :::image type="content" source="./media/tutorial-assess-sql/assessment-sql-navigation.png" alt-text="Ruta de navegación de la evaluación creada":::
15. Haga clic en el nombre de la evaluación que desea ver.

> [!NOTE]
> Como las evaluaciones de Azure SQL están basadas en el rendimiento, es preferible que, después de iniciar la detección, espere al menos un día para crear la evaluación. De este modo, gana tiempo para recopilar los datos de rendimiento con mayor confianza. Si la detección aún está en curso, el nivel de preparación de las instancias de SQL aparecerá como **desconocido**. Lo mejor es que, después de iniciar la detección, **espere el tiempo que haya especificado para evaluar el rendimiento (día/semana/mes)** y conseguir así un alto grado de confianza. 

## <a name="review-an-assessment"></a>Revisión de una evaluación

**Para ver una evaluación**:

1. Vaya a **Windows, Linux and SQL Server** (Windows, Linux y SQL Server) > **Azure Migrate: Discovery and assessment** (Azure Migrate: Detección y evaluación) y haga clic en el número situado junto a la evaluación de Azure SQL.
2. Haga clic en el nombre de la evaluación que desea ver. Veamos un ejemplo (las estimaciones y los costos son ficticios): :::image type="content" source="./media/tutorial-assess-sql/assessment-sql-summary.png" alt-text="Información general sobre la evaluación de SQL":::
3. Revise el resumen de valoraciones. También puede editar las propiedades de la evaluación o volver a calcularla.

#### <a name="discovered-items"></a>Elementos detectados

Indica el número de servidores, instancias y bases de datos de SQL que se han incluido en esta evaluación.
    
#### <a name="azure-readiness"></a>Preparación para Azure

Indica la distribución de las instancias de SQL evaluadas: 
    
**Tipo de implementación de destino (en las propiedades de evaluación)** | **Preparación**   
--- | --- |
**Recomendado** |  Las instancias están preparadas para Azure SQL Database y Azure SQL Managed Instance, potencialmente preparadas para Azure VM o se desconoce su nivel de preparación (en caso de que la detección esté en curso o haya que solucionar algún problema con la detección)
**Azure SQL DB** o **Azure SQL MI** | Las instancias están preparadas para Azure SQL Database o Azure SQL Managed Instance, no están preparadas para Azure SQL Database o Azure SQL Managed Instance, o se desconoce su nivel de preparación (en caso de que la detección esté en curso o haya que solucionar algún problema con la detección)
     
Puede profundizar para obtener más detalles sobre los problemas o advertencias de la migración que puede corregir antes de migrar a Azure SQL. Si necesita [más información](concepts-azure-sql-assessment-calculation.md), también puede consultar las configuraciones de Azure SQL recomendadas para migrar a bases de datos o instancias administradas de Azure SQL.
    
#### <a name="azure-sql-database-and-managed-instance-cost-details"></a>Costos de Azure SQL Database y Azure SQL Managed Instance

En la estimación de costos mensuales, se incluyen los costos de proceso y almacenamiento de las configuraciones de Azure SQL que corresponden al tipo de implementación recomendada de Azure SQL Database o Azure SQL Managed Instance. [Más información](concepts-azure-sql-assessment-calculation.md#calculate-monthly-costs)


### <a name="review-readiness"></a>Revisión de la preparación

1. Haga clic en **Azure SQL readiness** (Preparación para Azure SQL).
    :::image type="content" source="./media/tutorial-assess-sql/assessment-sql-readiness.png" alt-text="Detalles de la preparación para Azure SQL":::
1. En Azure SQL Readiness (Preparación para Azure SQL), consulte el grado de preparación de las instancia de SQL evaluadas para **Azure SQL DB** y **Azure SQL MI**:
    - **Preparada**: la instancia está preparada para migrarse a Azure SQL DB/MI sin ningún problema o advertencia en relación con la migración. 
        - Preparada (hipervínculo en azul): la instancia está preparada para migrarse a Azure SQL DB/MI sin ningún problema de migración, pero tiene algunas advertencias de migración que es necesario revisar. Puede hacer clic en el hipervínculo para consultar las advertencias de migración y las instrucciones de corrección recomendadas: :::image type="content" source="./media/tutorial-assess-sql/assess-ready.png" alt-text="Evaluación con el estado &quot;Preparada&quot;":::.       
    - **No está lista**: la instancia tiene uno o varios problemas que impiden que pueda migrarse a Azure SQL DB o Azure SQL MI. Puede hacer clic en el hipervínculo para consultar los problemas de migración y ver las instrucciones de corrección recomendadas.
    - **Se desconoce**: Azure Migrate no puede evaluar la preparación porque la detección está en curso o surgieron problemas durante la detección que deben corregirse desde la hoja de notificaciones. Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft. 
1. Consulte el tipo de implementación recomendado para la instancia de SQL de acuerdo con la matriz siguiente:

    - **Tipo de implementación de destino** (el que se seleccionó en las propiedades de evaluación): **Opción recomendada**

        **Preparación para Azure SQL DB** | **Preparación para Azure SQL MI** | **Tipo de implementación recomendado** | **¿Se han calculado los costos estimados y la configuración de Azure SQL?**
         --- | --- | --- | --- |
        Ready | Ready | Azure SQL DB o Azure SQL MI [Más información](concepts-azure-sql-assessment-calculation.md#recommended-deployment-type) | Sí
        Ready | No está lista o se desconoce | Azure SQL DB | Sí
        No está lista o se desconoce | Ready | Azure SQL MI  | Sí
        No está listo | No está listo | Potencialmente preparada para Azure VM [Más información](concepts-azure-sql-assessment-calculation.md#potentially-ready-for-azure-vm) | No
        No está lista o se desconoce | No está lista o se desconoce | Unknown | No
    
    - **Tipo de implementación de destino** (el que se seleccionó en las propiedades de evaluación): **Azure SQL DB**
    
        **Preparación para Azure SQL DB** | **¿Se han calculado los costos estimados y la configuración de Azure SQL?**
        --- | --- |
        Ready | Sí
        No está listo | No
        Desconocido | No
    
    - **Tipo de implementación de destino** (el que se seleccionó en las propiedades de evaluación): **Azure SQL MI**
    
        **Preparación para Azure SQL MI** | **¿Se han calculado los costos estimados y la configuración de Azure SQL?**
         --- | --- |
        Ready | Sí
        No está listo | No
        Desconocido | No

4. Haga clic en el nombre de la instancia para acceder a los detalles y ver el número de bases de datos de usuarios, los datos de la instancia (como sus propiedades) y los datos sobre la capacidad de almacenamiento (solo de la instancia) y de almacenamiento de la base de datos de origen.
5. Haga clic en el número de bases de datos de usuarios para consular la lista de bases de datos y sus detalles. Veamos un ejemplo (las estimaciones y los costos son ficticios): :::image type="content" source="./media/tutorial-assess-sql/assessment-db.png" alt-text="Detalles de la instancia de SQL":::
5. En la columna Migration issues (Problemas de migración), haga clic en Ver detalles para consultar las advertencias y los problemas relacionados con la migración de un tipo de implementación de destino concreto. 
    :::image type="content" source="./media/tutorial-assess-sql/assessment-db-issues.png" alt-text="Problemas y advertencias de la migración de bases de datos":::

### <a name="review-cost-estimates"></a>Revisar estimaciones de costos
En el resumen de la evaluación se muestran los costos de procesamiento y almacenamiento de las configuraciones de Azure SQL correspondientes al tipo de implementación recomendado de Azure SQL Database o Azure SQL Managed Instance.

1. Revise los costos totales mensuales. Se suman los costos de todas las instancias de SQL del grupo evaluado.
    - Las estimaciones de costos dependen de la configuración de Azure SQL recomendada para una instancia. 
    - Se muestran los costos mensuales estimados de proceso y almacenamiento. Veamos un ejemplo (las estimaciones y los costos son ficticios):
    
    :::image type="content" source="./media/tutorial-assess-sql/assessment-sql-cost.png" alt-text="Detalles del costo":::

1. Puede profundizar hasta el nivel de la instancia para ver la configuración de Azure SQL y las estimaciones de costos en ese nivel.  
1. También puede profundizar en la lista de bases de datos para consultar la configuración de Azure SQL y las estimaciones de costos de cada base de datos cuando la configuración recomendada es la de Azure SQL Database.

### <a name="review-confidence-rating"></a>Examen de la clasificación de confianza
Azure Migrate asigna una calificación de confianza a todas las evaluaciones de Azure SQL en función de la disponibilidad de los puntos de datos de rendimiento y uso necesarios para calcular la evaluación de todas las instancias y las bases de datos de SQL evaluadas. La clasificación es de una estrella (más baja) a cinco estrellas (más alta).
 
La clasificación de confianza sirve de ayuda para calcular la confiabilidad de las recomendaciones de tamaño de la evaluación.  Las clasificaciones de confianza son las siguientes.

**Disponibilidad del punto de datos** | **Clasificación de confianza**
--- | ---
0 % - 20 % | 1 estrella
21 % - 40 % | 2 estrellas
41 % - 60 % | 3 estrellas
61 % - 80 % | 4 estrellas
81 % - 100 % | 5 estrellas

[Obtenga más información](concepts-azure-sql-assessment-calculation.md#confidence-ratings) sobre la clasificación de confianza.

## <a name="next-steps"></a>Pasos siguientes

- [Más información](concepts-azure-sql-assessment-calculation.md) sobre el cálculo de las evaluaciones de Azure SQL.
- Comience a migrar instancias y bases de datos de SQL con [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview).
