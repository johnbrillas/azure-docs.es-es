---
title: Guía de programación de UDT y UDAGG con U-SQL para Azure Data Lake
description: Obtenga información sobre la programación de UDT y UDAGG con U-SQL en Azure Data Lake Analytics para crear scripts de USQL correctos.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: 10fcce9a667d9a08318f5adab804f482387052ff
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97606664"
---
# <a name="u-sql-programmability-guide---udt-and-udagg"></a>Guía de programación de U-SQL: UDT y UDAGG



## <a name="use-user-defined-types-udt"></a>Uso de tipos definidos por el usuario: UDT
Los tipos de definidos por el usuario o UDT constituyen otra característica de programación de U-SQL. UDT de U-SQL actúa como un tipo de definido por el usuario de C# normal. C# es un lenguaje fuertemente tipado que permite el uso de tipos integrados y definidos por el usuario personalizados.

U-SQL no puede serializar ni deserializar de forma implícita UDT arbitrarios mientras el UDT se pasa entre vértices de conjuntos de filas. Esto implica que el usuario debe proporcionar un formateador explícito mediante la interfaz de IFormatter. Esto proporcionará a U-SQL los métodos parar serializar o deserializar el UDT.

> [!NOTE]
> En la actualidad, los extractores y outputters integrados de U-SQL no pueden serializar ni deserializar los datos de UDT en archivos ni desde estos ni siquiera con el conjunto IFormatter. Por ello, cuando esté escribiendo datos UDT en un archivo con la instrucción OUTPUT o leyéndolos con un extractor, tiene que pasarlos como una cadena o matriz de bytes. A continuación, llama al código de serialización y deserialización explícitamente (es decir, al método ToString() del UDT). Por otra parte, los extractores y outputters definidos por el usuario pueden leer y escribir UDT.

Si se intenta utilizar UDT en EXTRACTOR o OUTPUTTER (fuera de SELECT anterior), como se muestra a continuación:

```usql
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

Recibirá el error siguiente:

```output
Error   1   E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used to output column myfield of type
MyNameSpace.Myfunction_Returning_UDT.

Description:

Outputters.Text only supports built-in types.

Resolution:

Implement a custom outputter that knows how to serialize this type, or call a serialization method on the type in
the preceding SELECT.   C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
USQL-Programmability\Types.usql 52  1   USQL-Programmability
```

Para trabajar con UDT en outptutter tenemos que serializar en una cadena con el método ToString() o crear un outputter personalizado.

Actualmente los UDT no se pueden usar en GROUP BY. Si UDT se usa en GROUP BY, se produce el siguiente error:

```output
Error   1   E_CSC_USER_INVALIDTYPEINCLAUSE: GROUP BY doesn't support type MyNameSpace.Myfunction_Returning_UDT
for column myfield

Description:

GROUP BY doesn't support UDT or Complex types.

Resolution:

Add a SELECT statement where you can project a scalar column that you want to use with GROUP BY.
C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\USQL-Programmability\Types.usql
62  5   USQL-Programmability
```

Para definir un UDT, tenemos que:

1. Agregue los siguientes espacios de nombres:

```csharp
using Microsoft.Analytics.Interfaces
using System.IO;
```

2. Agregue `Microsoft.Analytics.Interfaces`, necesario para las interfaces UDT. También se podría necesitar `System.IO` para definir la interfaz de IFormatter.

3. Defina un tipo definido por el usuario con el atributo SqlUserDefinedType.

**SqlUserDefinedType** se utiliza para marcar una definición de tipo en un ensamblado como un tipo definido por el usuario (UDT) en U-SQL. Las propiedades del atributo reflejan las características físicas del UDT. No se puede heredar esta clase.

SqlUserDefinedType es un atributo necesario para la definición de UDT.

El constructor de la clase:  

* SqlUserDefinedTypeAttribute (formateador de tipos)

* Formateador de tipos: parámetro necesario para definir un formateador UDT. En concreto, el tipo de interfaz `IFormatter` que se debe pasar aquí.

```csharp
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* El UDT típico también requerirá la definición de la interfaz IFormatter, tal como se muestra en el ejemplo siguiente:

```csharp
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

La interfaz `IFormatter` serializa y deserializa un grafo de objetos con el tipo de raíz de \<typeparamref name="T">.

\<typeparam name="T">El tipo de raíz del grafo de objetos para serializar y deserializar.

* **Deserialize**: deserializa los datos en el flujo proporcionado y reconstituye el grafo de objetos.

* **Serialize**: serializa un objeto o un grafo de objetos con el directorio raíz especificado en el flujo proporcionado.

Instancia de `MyType`: instancia del tipo.  
Escritor `IColumnWriter`/lector `IColumnReader`: flujo de la columna subyacente.  
Contexto `ISerializationContext`: enumeración que define un conjunto de marcas que especifica el contexto de origen o destino para el flujo durante la serialización.

* **Intermediate**: especifica que el contexto de origen o destino no es un almacén persistente.

* **Persistence**: especifica que el contexto de origen o destino es un almacén persistente.

Como un tipo de C# normal, la definición de UDT de U-SQL puede incluir invalidaciones para los operadores como +/==/!=. Puede incluir también métodos estáticos. Por ejemplo, si vamos a usar este UDT como un parámetro para la función de agregado MIN de U-SQL, tenemos que definir la invalidación del operador <.

Anteriormente en esta guía se mostró un ejemplo para la identificación del período fiscal a partir de la fecha específica con el formato `Qn:Pn (Q1:P10)`. En el ejemplo siguiente se muestra cómo definir un tipo personalizado para los valores del período fiscal.

A continuación aparece un ejemplo de sección de código subyacente con interfaz IFormatter y UDT personalizados:

```csharp
[SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
public struct FiscalPeriod
{
    public int Quarter { get; private set; }

    public int Month { get; private set; }

    public FiscalPeriod(int quarter, int month):this()
    {
    this.Quarter = quarter;
    this.Month = month;
    }

    public override bool Equals(object obj)
    {
    if (ReferenceEquals(null, obj))
    {
        return false;
    }

    return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
    }

    public bool Equals(FiscalPeriod other)
    {
return this.Quarter.Equals(other.Quarter) && this.Month.Equals(other.Month);
    }

    public bool GreaterThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
    }

    public bool LessThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
    }

    public override int GetHashCode()
    {
    unchecked
    {
        return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
    }
    }

    public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
    {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
    }

    public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.Equals(c2);
    }

    public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
    {
    return !c1.Equals(c2);
    }
    public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.GreaterThan(c2);
    }
    public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.LessThan(c2);
    }
    public override string ToString()
    {
    return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
    }

}

public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
{
    public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
    {
    using (var binaryWriter = new BinaryWriter(writer.BaseStream))
    {
        binaryWriter.Write(instance.Quarter);
        binaryWriter.Write(instance.Month);
        binaryWriter.Flush();
    }
    }

    public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
    {
    using (var binaryReader = new BinaryReader(reader.BaseStream))
    {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
        return result;
    }
    }
}
```

El tipo definido incluye dos números: trimestre y mes. Los operadores `==/!=/>/<` y el método estático `ToString()` se definen aquí.

Como se mencionó anteriormente, UDT se puede utilizar en expresiones SELECT, pero no se puede usar en OUTPUTTER/EXTRACTOR sin serialización personalizada. Debe serializarse como una cadena con `ToString()` o usarse con OUTPUTTER/EXTRACTOR personalizado.

Ahora vamos a analizar el uso de UDT. En una sección de código subyacente, cambiamos nuestra función GetFiscalPeriod a la siguiente:

```csharp
public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
{
    int FiscalMonth = 0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter = 0;
    if (FiscalMonth >= 1 && FiscalMonth <= 3)
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

    return new FiscalPeriod(FiscalQuarter, FiscalMonth);
}
```

Como puede ver, devuelve el valor de nuestro tipo FiscalPeriod.

Vea este ejemplo de cómo se usa más en el script base de U-SQL. En este ejemplo se muestran distintas maneras de invocación de UDT a partir del script U-SQL.

```usql
DECLARE @input_file string = @"c:\work\cosmos\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"c:\work\cosmos\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid string,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT 
        guid AS start_id,
        dt,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Quarter AS fiscalquarter,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Month AS fiscalmonth,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt) + new USQL_Programmability.CustomFunctions.FiscalPeriod(1,7) AS fiscalperiod_adjusted,
        user,
        des
    FROM @rs0;

@rs2 =
    SELECT 
        start_id,
        dt,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        fiscalquarter,
        fiscalmonth,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS fiscalperiod,

       // This user-defined type was created in the prior SELECT.  Passing the UDT to this subsequent SELECT would have failed if the UDT was not annotated with an IFormatter.
           fiscalperiod_adjusted.ToString() AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    TO @output_file 
    USING Outputters.Text();
```

Este es un ejemplo de una sección de código subyacente completa:

```csharp
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.IO;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        static public DateTime? ToDateTime(string dt)
        {
            DateTime dtValue;

            if (!DateTime.TryParse(dt, out dtValue))
                return Convert.ToDateTime(dt);
            else
                return null;
        }

        public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
        {
            int FiscalMonth = 0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter = 0;
            if (FiscalMonth >= 1 && FiscalMonth <= 3)
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

            return new FiscalPeriod(FiscalQuarter, FiscalMonth);
        }        [SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
        public struct FiscalPeriod
        {
            public int Quarter { get; private set; }

            public int Month { get; private set; }

            public FiscalPeriod(int quarter, int month):this()
            {
                this.Quarter = quarter;
                this.Month = month;
            }

            public override bool Equals(object obj)
            {
                if (ReferenceEquals(null, obj))
                {
                    return false;
                }

                return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
            }

            public bool Equals(FiscalPeriod other)
            {
return this.Quarter.Equals(other.Quarter) &&    this.Month.Equals(other.Month);
            }

            public bool GreaterThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
            }

            public bool LessThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
            }

            public override int GetHashCode()
            {
                unchecked
                {
                    return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
                }
            }

            public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
            {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
            }

            public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.Equals(c2);
            }

            public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
            {
                return !c1.Equals(c2);
            }
            public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.GreaterThan(c2);
            }
            public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.LessThan(c2);
            }
            public override string ToString()
            {
                return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
            }

        }

        public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
        {
public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
            {
                using (var binaryWriter = new BinaryWriter(writer.BaseStream))
                {
                    binaryWriter.Write(instance.Quarter);
                    binaryWriter.Write(instance.Month);
                    binaryWriter.Flush();
                }
            }

public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
            {
                using (var binaryReader = new BinaryReader(reader.BaseStream))
                {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
                    return result;
                }
            }
        }
    }
}
```

## <a name="use-user-defined-aggregates-udagg"></a>Uso de agregados definidos por el usuario: UDAGG
Los agregados definidos por el usuario son todas las funciones relacionadas con la agregación que no se distribuyen de fábrica con U-SQL. El ejemplo puede ser un agregado para realizar un cálculo matemático personalizado, realizar concatenaciones de cadenas, manipulaciones con cadenas, etc.

La definición de clase base agregada definida por el usuario es la siguiente:

```csharp
    [SqlUserDefinedAggregate]
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
    {
        protected IAggregate();

        public abstract void Accumulate(T1 t1, T2 t2);
        public abstract void Init();
        public abstract TResult Terminate();
    }
```

**SqlUserDefinedAggregate** indica que el tipo se debe registrar como un agregado definido por el usuario. No se puede heredar esta clase.

El atributo SqlUserDefinedType es **opcional** para la definición de UDAGG.


La clase base le permite pasar tres parámetros abstractos: dos de entrada y uno de resultado. Los tipos de datos son variables y se deben definirse durante la herencia de clases.

```csharp
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    { … }

    public override void Accumulate(string guid, string user)
    { … }

    public override string Terminate()
    { … }
}
```

* **Init** se invoca una vez para cada grupo durante el cálculo. Proporciona la rutina de inicialización para cada grupo de agregación.  
* **Accumulate** se ejecuta una vez para cada valor. Proporciona la funcionalidad principal para el algoritmo de agregación. Puede utilizarse para agregar valores con varios tipos de datos definidos durante la herencia de clases. Puede aceptar dos parámetros de tipos de datos de variable.
* **Terminate** se ejecuta una vez por cada grupo de agregación al final del procesamiento para generar el resultado para cada grupo.

Para declarar tipos de datos de entrada y salida correctos, utilice la definición de clase de la siguiente manera:

```csharp
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* T1: segundo parámetro para acumular.
* T2: Segundo parámetro para acumular.
* TResult: tipo de valor de terminación devuelto.

Por ejemplo:

```csharp
public class GuidAggregate : IAggregate<string, int, int>
```

or

```csharp
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a>Uso de UDAGG en U-SQL
Para usar UDAGG, defínalo primero en código subyacente o haga referencia a él desde el archivo DLL de programación existente como se indicó anteriormente.

Después use la sintaxis siguiente:

```csharp
AGG<UDAGG_functionname>(param1,param2)
```

A continuación se muestra un ejemplo de UDAGG:

```csharp
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    {
        guid_agg = "";
    }

    public override void Accumulate(string guid, string user)
    {
        if (user.ToUpper()== "USER1")
        {
        guid_agg += "{" + guid + "}";
        }
    }

    public override string Terminate()
    {
        return guid_agg;
    }

}
```

Y el script base U-SQL:

```usql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @" \usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    SELECT
        user,
        AGG<USQL_Programmability.GuidAggregate>(guid,user) AS guid_list
    FROM @rs0
    GROUP BY user;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

En este escenario de caso de uso, se concatenan los GUID de clase para los usuarios específicos.

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a la guía de programación de U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
* [Guía de programación de U-SQL: UDO](data-lake-analytics-u-sql-programmability-guide-UDO.md)
