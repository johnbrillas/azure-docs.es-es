---
title: Guía de programación de U-SQL para Azure Data Lake
description: Obtenga información general sobre U-SQL y descubra la capacidad de programación de UDF disponible en Azure Data Lake Analytics para que pueda crear scripts de USQL correctos.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: d263f2616607a96a8aa14f1ad1c06330b1b44644
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96510869"
---
# <a name="u-sql-programmability-guide-overview"></a>Introducción a la guía de programación de U-SQL

U-SQL es un lenguaje de consulta diseñado para tipo de macrodatos de cargas de trabajo. Una de las características exclusivas de U-SQL es la combinación del lenguaje declarativo similar a SQL con la extensibilidad y capacidad de programación que proporciona C#. En esta guía, nos centramos en la extensibilidad y capacidad de programación del lenguaje U-SQL que proporciona C#.

## <a name="requirements"></a>Requisitos

Descarga e instalación de [Herramientas de Azure Data Lake para Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="get-started-with-u-sql"></a>Introducción a U-SQL  

Observe el siguiente script U-SQL:

```usql
@a  = 
  SELECT * FROM 
    (VALUES
       ("Contoso",   1500.0, "2017-03-39"),
       ("Woodgrove", 2700.0, "2017-04-10")
    ) AS D( customer, amount, date );

@results =
  SELECT
    customer,
    amount,
    date
  FROM @a;    
```

Este script define dos conjuntos de filas: `@a` y `@results`. El conjunto de filas `@results` se define desde `@a`.

## <a name="c-types-and-expressions-in-u-sql-script"></a>Expresiones y tipos de C# en un script U-SQL

Una expresión U-SQL es una expresión de C# combinada con operaciones lógicas de U-SQL como `AND`, `OR` y `NOT`. Las expresiones U-SQL pueden utilizarse con SELECT, EXTRACT, WHERE, HAVING, GROUP BY y DECLARE. Por ejemplo, el siguiente script analiza una cadena como un valor de DateTime.

```usql
@results =
  SELECT
    customer,
    amount,
    DateTime.Parse(date) AS date
  FROM @a;    
```

El siguiente fragmento de código analiza una cadena de un valor de DateTime en la cláusula DECLARE.

```usql
DECLARE @d = DateTime.Parse("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a>Uso de expresiones de C# para conversiones de tipo de datos

En el ejemplo siguiente se muestra cómo se puede hacer una conversión de datos de fecha y hora mediante el uso de expresiones de C#. En este escenario en particular, los datos de fecha y hora de cadena se convierten en la fecha y hora estándar con la notación de hora de medianoche 00:00:00.

```usql
DECLARE @dt = "2016-07-06 10:23:15";

@rs1 =
  SELECT 
    Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
    dt AS olddt
  FROM @rs0;

OUTPUT @rs1 
  TO @output_file 
  USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a>Uso de expresiones de C# para la fecha de hoy

Para extraer la fecha de hoy, podemos usar la siguiente expresión de C#: `DateTime.Now.ToString("M/d/yyyy")`

Este es un ejemplo de cómo utilizar esta expresión en un script:

```usql
@rs1 =
  SELECT
    MAX(guid) AS start_id,
    MIN(dt) AS start_time,
    MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
    MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
    DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
    user,
    des
  FROM @rs0
  GROUP BY user, des;
```
## <a name="using-net-assemblies"></a>Uso de los ensamblados .NET

El modelo de extensibilidad de U-SQL depende en gran medida de la capacidad de agregar código personalizado a partir de ensamblados .NET. 

### <a name="register-a-net-assembly"></a>Registrar un ensamblado de .NET

Utilice la instrucción `CREATE ASSEMBLY` para colocar un ensamblado .NET en una base de datos U-SQL. Posteriormente, los scripts de U-SQL pueden usar esos ensamblados mediante la instrucción `REFERENCE ASSEMBLY`. 

El código siguiente muestra cómo registrar un ensamblado:

```usql
CREATE ASSEMBLY MyDB.[MyAssembly]
   FROM "/myassembly.dll";
```

El código siguiente muestra cómo hacer referencia a un ensamblado:

```usql
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

Consulte las [instrucciones de registro de un ensamblado](/archive/blogs/azuredatalake/how-to-register-u-sql-assemblies-in-your-u-sql-catalog) que tratan este tema con mayor detalle.


### <a name="use-assembly-versioning"></a>Uso del control de versiones de ensamblado
Actualmente, U-SQL usa la versión 4.7.2 de .NET°Framework. Por lo tanto, asegúrese de que sus propios ensamblados son compatibles con esa versión del tiempo de ejecución.

Como se mencionó anteriormente, U-SQL ejecuta código en un formato de 64 bits (x64). Por lo tanto, asegúrese de que el código se compila para ejecutarse en x64. De lo contrario, se produce el error de formato incorrecto mostrado anteriormente.

El archivo DLL y el de recursos de cada ensamblado cargado como, por ejemplo, un tiempo de ejecución diferente, un ensamblado nativo o un archivo de configuración pueden tener 400 MB como máximo. El tamaño total de recursos implementados, ya sea mediante DEPLOY RESOURCE o mediante referencias a ensamblados y a sus archivos adicionales, no puede superar los 3 GB.

Por último, tenga en cuenta que cada base de datos de U-SQL solo puede contener una versión de cualquier ensamblado dado. Por ejemplo, si necesita tanto la versión 7 como la versión 8 de la biblioteca NewtonSoft Json.NET, debe registrarlos en dos bases de datos diferentes. Además, cada script solo puede hacer referencia a una versión de un archivo DLL de ensamblado dado. En este sentido, U-SQL sigue la semántica de control de versiones y la administración de ensamblados de C#.

## <a name="use-user-defined-functions-udf"></a>Uso de funciones definidas por el usuario: UDF
Las funciones definidas por el usuario de U-SQL o UDF son rutinas programadas que aceptan parámetros, realizan una acción (como un cálculo complejo) y devuelven el resultado de esa acción como un valor. El valor devuelto de UDF solo puede ser un escalar único. Se puede llamar a UDF de U-SQL en un script base U-SQL como a cualquier otra función escalar de C#.

Es recomendable que inicialice las funciones definidas por el usuario de U-SQL como **públicas** y **estáticas**.

```usql
public static string MyFunction(string param1)
{
    return "my result";
}
```

Primero veamos el ejemplo sencillo de creación de una UDF.

En este escenario de caso de uso, es necesario determinar el período fiscal, incluido el trimestre fiscal y el mes fiscal del primer inicio de sesión del usuario específico. El primer mes fiscal del año en nuestro escenario es junio.

Para calcular el período fiscal, se introduce la siguiente función de C#:

```usql
public static string GetFiscalPeriod(DateTime dt)
{
    int FiscalMonth=0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter=0;
    if (FiscalMonth >=1 && FiscalMonth<=3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return "Q" + FiscalQuarter.ToString() + ":P" + FiscalMonth.ToString();
}
```

Simplemente calcula el trimestre y mes fiscal y se devuelve un valor de cadena. Para junio (primer mes del primer trimestre fiscal) se usa "Q1:P1". Para julio: "Q1:P2" y así sucesivamente.

Se trata de una función de C# que vamos a usar en nuestro proyecto U-SQL.

Este es el aspecto de la sección de código subyacente en este escenario:

```usql
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        public static string GetFiscalPeriod(DateTime dt)
        {
            int FiscalMonth=0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter=0;
            if (FiscalMonth >=1 && FiscalMonth<=3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return "Q" + FiscalQuarter.ToString() + ":" + FiscalMonth.ToString();
        }
    }
}
```

Ahora vamos a llamar a esta función desde el script base U-SQL. Para ello, se debe proporcionar un nombre completo de la función, incluido el espacio de nombres que, en este caso, es EspacioDeNombres.Clase.Función(parámetro).
```usql
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

El siguiente es el script base U-SQL real:
```usql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt DateTime,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");

@rs1 =
    SELECT
        MAX(guid) AS start_id,
    MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

El siguiente es el archivo de salida de la ejecución del script:

```output
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

Este ejemplo muestra un uso simple de UDF insertada en U-SQL.

### <a name="keep-state-between-udf-invocations"></a>Mantenimiento del estado entre las invocaciones de UDF
Los objetos de programación de C# de U-SQL pueden ser más sofisticados utilizando interactividad a través de variables globales de código subyacente. Veamos el siguiente escenario de caso de uso empresarial.

En organizaciones grandes, los usuarios pueden cambiar entre variedades de aplicaciones internas. Entre ellas, se encuentran Microsoft Dynamics CRM, Power BI, etc. Es posible que los clientes deseen aplicar un análisis de telemetría de cómo los usuarios cambian entre distintas aplicaciones, cuáles son las tendencias de uso, etc. El objetivo para la empresa es optimizar el uso de las aplicaciones. Es posible que también quieran combinar aplicaciones diferentes o rutinas de inicio de sesión específicas.

Para lograr este objetivo, tenemos que determinar identificadores de sesión y retardo de tiempo entre las últimas sesiones que han tenido lugar.

Es necesario encontrar un inicio de sesión anterior y luego asignar este inicio de sesión a todas las sesiones que se generan en la misma aplicación. El primer desafío es que el script base U-SQL no nos permitirá aplicar cálculos sobre una columna ya calculada con la función LAG. El segundo reto es que se debe mantener la sesión específica para todas las sesiones dentro del mismo período de tiempo.

Para solucionar este problema, vamos a usar una variable global dentro de una sección de código subyacente: `static public string globalSession;`.

Esta variable global se aplica a todo el conjunto de filas durante nuestra ejecución de script.

Esta es la sección de código subyacente de nuestro programa U-SQL:

```csharp
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQLApplication21
{
    public class UserSession
    {
        static public string globalSession;
        static public string StampUserSession(string eventTime, string PreviousRow, string Session)
        {

            if (!string.IsNullOrEmpty(PreviousRow))
            {
                double timeGap = Convert.ToDateTime(eventTime).Subtract(Convert.ToDateTime(PreviousRow)).TotalMinutes;
                if (timeGap <= 60) {return Session;}
                else {return Guid.NewGuid().ToString();}
            }
            else {return Guid.NewGuid().ToString();}

        }

        static public string getStampUserSession(string Session)
        {
            if (Session != globalSession && !string.IsNullOrEmpty(Session)) { globalSession = Session; }
            return globalSession;
        }

    }
}
```

Este ejemplo muestra la variable global `static public string globalSession;` que se usa dentro de la función `getStampUserSession` y se reinicializa cada vez que cambia el parámetro Session.

El script base de U-SQL es como sigue:

```usql
DECLARE @in string = @"\UserSession\test1.tsv";
DECLARE @out1 string = @"\UserSession\Out1.csv";
DECLARE @out2 string = @"\UserSession\Out2.csv";
DECLARE @out3 string = @"\UserSession\Out3.csv";

@records =
    EXTRACT DataId string,
            EventDateTime string,           
            UserName string,
            UserSessionTimestamp string

    FROM @in
    USING Extractors.Tsv();

@rs1 =
    SELECT 
        EventDateTime,
        UserName,
    LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,          
        string.IsNullOrEmpty(LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,           
        USQLApplication21.UserSession.StampUserSession
           (
            EventDateTime,
            LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC),
            LAG(UserSessionTimestamp, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)
           ) AS UserSessionTimestamp
    FROM @records;

@rs2 =
    SELECT 
        EventDateTime,
        UserName,
        LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,
        string.IsNullOrEmpty( LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,
        USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp) AS UserSessionTimestamp
    FROM @rs1
    WHERE UserName != "UserName";

OUTPUT @rs2
    TO @out2
    ORDER BY UserName, EventDateTime ASC
    USING Outputters.Csv();
```

Se llama a la función `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` aquí durante el segundo cálculo del conjunto de filas de memoria. Pasa la columna `UserSessionTimestamp` y devuelve el valor hasta que `UserSessionTimestamp` se cambia.

El archivo de salida tiene el siguiente aspecto:

```output
"2016-02-19T07:32:36.8420000-08:00","User1",,True,"72a0660e-22df-428e-b672-e0977007177f"
"2016-02-17T11:52:43.6350000-08:00","User2",,True,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-17T11:59:08.8320000-08:00","User2","2016-02-17T11:52:43.6350000-08:00",False,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-11T07:04:17.2630000-08:00","User3",,True,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-11T07:10:33.9720000-08:00","User3","2016-02-11T07:04:17.2630000-08:00",False,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-15T21:27:41.8210000-08:00","User3","2016-02-11T07:10:33.9720000-08:00",False,"4d2bc48d-bdf3-4591-a9c1-7b15ceb8e074"
"2016-02-16T05:48:49.6360000-08:00","User3","2016-02-15T21:27:41.8210000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-16T06:22:43.6390000-08:00","User3","2016-02-16T05:48:49.6360000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-17T16:29:53.2280000-08:00","User3","2016-02-16T06:22:43.6390000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T16:39:07.2430000-08:00","User3","2016-02-17T16:29:53.2280000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T17:20:39.3220000-08:00","User3","2016-02-17T16:39:07.2430000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-19T05:23:54.5710000-08:00","User3","2016-02-17T17:20:39.3220000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T05:48:37.7510000-08:00","User3","2016-02-19T05:23:54.5710000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T06:40:27.4830000-08:00","User3","2016-02-19T05:48:37.7510000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T07:27:37.7550000-08:00","User3","2016-02-19T06:40:27.4830000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T19:35:40.9450000-08:00","User3","2016-02-19T07:27:37.7550000-08:00",False,"3f385f0b-3e68-4456-ac74-ff6cef093674"
"2016-02-20T00:07:37.8250000-08:00","User3","2016-02-19T19:35:40.9450000-08:00",False,"685f76d5-ca48-4c58-b77d-bd3a9ddb33da"
"2016-02-11T09:01:33.9720000-08:00","User4",,True,"9f0cf696-c8ba-449a-8d5f-1ca6ed8f2ee8"
"2016-02-17T06:30:38.6210000-08:00","User4","2016-02-11T09:01:33.9720000-08:00",False,"8b11fd2a-01bf-4a5e-a9af-3c92c4e4382a"
"2016-02-17T22:15:26.4020000-08:00","User4","2016-02-17T06:30:38.6210000-08:00",False,"4e1cb707-3b5f-49c1-90c7-9b33b86ca1f4"
"2016-02-18T14:37:27.6560000-08:00","User4","2016-02-17T22:15:26.4020000-08:00",False,"f4e44400-e837-40ed-8dfd-2ea264d4e338"
"2016-02-19T01:20:31.4800000-08:00","User4","2016-02-18T14:37:27.6560000-08:00",False,"2136f4cf-7c7d-43c1-8ae2-08f4ad6a6e08"
```

Este ejemplo muestra un escenario de caso de uso más complicado en el que se usa una variable global dentro de la sección de código subyacente que se aplica al conjunto de filas de memoria completo.

## <a name="next-steps"></a>Pasos siguientes
* [Guía de programación de U-SQL: UDT y UDAGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)
* [Guía de programación de U-SQL: UDO](data-lake-analytics-u-sql-programmability-guide-UDO.md)