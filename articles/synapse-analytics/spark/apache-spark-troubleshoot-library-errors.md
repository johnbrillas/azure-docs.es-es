---
title: Solución de errores de instalación de biblioteca
description: En este tutorial se proporciona información general sobre cómo solucionar errores de instalación de la biblioteca.
services: synapse-analytics
author: midesa
ms.author: midesa
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: conceptual
ms.date: 01/04/2021
ms.openlocfilehash: e812fa47d35889a9cf8c671a4df6034812272a6a
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101670630"
---
# <a name="troubleshoot-library-installation-errors"></a>Solución de errores de instalación de biblioteca 
Para que el código de terceros o el código compilado localmente esté disponible para las aplicaciones, puede instalar una biblioteca en uno de los grupos de Apache Spark sin servidor. Los paquetes que aparecen en el archivo requirements.txt se descargan de PyPi cuando se inicia el grupo. Este archivo de requisitos se usa cada vez que se crea una instancia de Spark desde ese grupo de Spark. Una vez instalada una biblioteca para un grupo de Spark, está disponible para todas las sesiones que usan el mismo grupo. 

En algunos casos, puede encontrar que la biblioteca que intenta instalar no aparece en el grupo de Apache Spark. Esto suele ocurrir si hay un error en el archivo requirements.txt proporcionado o en las bibliotecas especificadas. Cuando se produce un error en el proceso de instalación de la biblioteca, el grupo de Apache Spark se revertirá a las bibliotecas especificadas en el runtime base de Synapse.

El objetivo de este documento es proporcionar información sobre problemas comunes y ayudarlo a depurar los errores de instalación de la biblioteca.

## <a name="force-update-your-apache-spark-pool"></a>Forzar la actualización del grupo de Apache Spark
Al actualizar las bibliotecas del grupo de Apache Spark, estos cambios se seleccionarán una vez que el grupo se reinicia. Si tiene trabajos activos, estos trabajos seguirán ejecutándose en la versión original del grupo de Spark.

Para forzar la aplicación de los cambios, seleccione la opción para **forzar la nueva configuración**. Esta configuración terminará todas las sesiones actuales del grupo de Spark seleccionado. Una vez finalizadas las sesiones, tendrá que esperar a que el grupo se reinicie. 

![Adición de bibliotecas de Python](./media/apache-spark-azure-portal-add-libraries/update-libraries.png "Adición de bibliotecas de Python")

## <a name="validate-your-permissions"></a>Validación de los permisos
Para instalar y actualizar las bibliotecas, debe tener los permisos de tipo **Colaborador de datos de Storage Blob** o **Propietario de datos de Storage Blob** en la cuenta de almacenamiento principal de Azure Data Lake Storage Gen2 que está vinculada con el área de trabajo de Azure Synapse Analytics.

Para validar que tiene estos permisos, puede ejecutar el código siguiente:

```python
from pyspark.sql.types import StructType,StructField, StringType, IntegerType
data2 = [("James","Smith","Joe","4355","M",3000),
    ("Michael","Rose","Edward","40288","F",4000)
  ]

schema = StructType([ \
    StructField("firstname",StringType(),True), \
    StructField("middlename",StringType(),True), \
    StructField("lastname",StringType(),True), \
    StructField("id", StringType(), True), \
    StructField("gender", StringType(), True), \
    StructField("salary", IntegerType(), True) \
  ])
 
df = spark.createDataFrame(data=data2,schema=schema)

df.write.csv("abfss://<<ENTER NAME OF FILE SYSTEM>>@<<ENTER NAME OF PRIMARY STORAGE ACCOUNT>>.dfs.core.windows.net/validate_permissions.csv")

```
Si recibe un error, es probable que le falten los permisos necesarios. Para saber cómo obtener los permisos necesarios, consulte este documento: [Asignación de los permisos Colaborador de datos de Storage Blob o Propietario de datos de Storage Blob](../../storage/common/storage-auth-aad-rbac-portal.md#assign-an-azure-built-in-role).

Además, si ejecuta una canalización, el MSI del área de trabajo también debe tener los permisos Propietario de datos de Storage Blob o Colaborador de datos de Storage Blob. Para información sobre cómo conceder este permiso a su identidad de área de trabajo, visite: [Concesión de permisos a una identidad administrada de área de trabajo](../security/how-to-grant-workspace-managed-identity-permissions.md).

## <a name="check-the-requirements-file"></a>Comprobación del archivo de requisitos
Se puede usar un archivo ***requirements.txt*** (salida del comando pip freeze) para actualizar el entorno virtual. Este archivo sigue el formato descrito en la documentación de referencia de [pip freeze](https://pip.pypa.io/en/stable/reference/pip_freeze/).

Es importante tener en cuenta las restricciones siguientes:
   -  El nombre del paquete PyPI debe aparecer junto con una versión exacta. 
   -  El contenido del archivo de requisitos no debe incluir líneas o caracteres en blanco adicionales. 
   -  El [entorno de ejecución de Synapse](apache-spark-version-support.md) incluye un conjunto de bibliotecas instalado previamente en cada grupo de Apache Spark sin servidor. Los paquetes instalados previamente en el entorno de ejecución base no pueden cambiar a una versión anterior. Los paquetes solo se pueden agregar o actualizar.
   -  No se admite la modificación de la versión de PySpark, Python, Scala/Java, .NET o Spark.

En el fragmento de código siguiente se muestra el formato requerido del archivo de requisitos.

```
absl-py==0.7.0
adal==1.2.1
alabaster==0.7.10
```

## <a name="validate-wheel-files"></a>Validación de los archivos Wheel
Los grupos de Apache Spark sin servidor de Synapse se basan en la distribución de Linux. Al descargar e instalar archivos Wheel directamente de PyPI, asegúrese de seleccionar la versión que se basa en Linux y que se ejecuta en la misma versión de Python que el grupo de Spark.

>[!IMPORTANT]
>Los paquetes personalizados se pueden agregar o modificar entre sesiones. Sin embargo, deberá esperar a que el grupo y la sesión se reinicien para ver el paquete actualizado.

## <a name="check-for-dependency-conflicts"></a>Comprobación de conflictos de dependencia
 En general, la resolución de dependencias de Python puede ser difícil de administrar. Para ayudar a depurar localmente los conflictos de dependencia, puede crear su propio entorno virtual basado en el entorno de ejecución de Synapse y validar los cambios.

Para volver a crear el entorno y validar las actualizaciones, haga lo siguiente:
 1. [Descargue](https://github.com/Azure-Samples/Synapse/blob/main/Spark/Python/base_environment.yml) la plantilla para volver a crear el entorno de ejecución de Synapse de manera local. Puede haber pequeñas diferencias entre la plantilla y el entorno de Synapse real.
   
 2. Use [estas instrucciones](https://docs.conda.io/projects/conda/latest/user-guide/tasks/manage-environments.html) para crear un entorno virtual. Este entorno le permite crear una instalación de Python aislada con la lista de bibliotecas especificada. 
    
    ```
    conda myenv create -f environment.yml
    conda activate myenv
    ```
   
 3. Use ``pip install -r <provide your req.txt file>`` para actualizar el entorno virtual con los paquetes especificados. Si se produce un error en la instalación, puede haber un conflicto entre lo que se instaló previamente en el entorno de ejecución base de Synapse y lo que se especifica en el archivo de requisitos proporcionado. Es necesario resolver estos conflictos de dependencia para obtener las bibliotecas actualizadas en el grupo de Apache Spark sin servidor.

## <a name="next-steps"></a>Pasos siguientes
- Visualización de las bibliotecas predeterminadas: [Compatibilidad de las versiones de Azure Spark](apache-spark-version-support.md)