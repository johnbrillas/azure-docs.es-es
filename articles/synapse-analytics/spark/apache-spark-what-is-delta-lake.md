---
title: ¿Qué es Delta Lake?
description: Información general de Delta Lake y su funcionamiento como parte de Azure Synapse Analytics
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: spark
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: 23a269fe9498295a3ff325a261c6539f5e1e31aa
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101676230"
---
# <a name="what-is-delta-lake"></a>¿Qué es Delta Lake?

Azure Synapse Analytics es compatible con Delta Lake de Linux Foundation. Delta Lake es una capa de almacenamiento de código abierto que ofrece transacciones ACID (atomicidad, coherencia, aislamiento y durabilidad) para cargas de trabajo de macrodatos de Apache Spark.

La versión actual de Delta Lake incluida con Azure Synapse tiene compatibilidad de lenguaje con Scala, PySpark y .NET. Hay vínculos en la parte inferior de la página para obtener ejemplos y documentación más detallados.

## <a name="key-features"></a>Principales características

| Característica | Descripción |
| --- | --- |
| **Transacciones ACID** | Los lagos de datos normalmente se rellenan mediante varios procesos y canalizaciones, algunos de los cuales escriben datos simultáneamente con las lecturas. Antes de Delta Lake y la adición de las transacciones, los ingenieros de datos tenían que realizar un proceso manual propenso a errores para garantizar la integridad de los datos. Delta Lake aporta las transacciones ACID conocidas a los lagos de datos. Ofrece serialización, que es el mayor nivel de aislamiento. Obtenga más información en [Profundización en Delta Lake: desempaquetado del registro de transacciones](https://databricks.com/blog/2019/08/21/diving-into-delta-lake-unpacking-the-transaction-log.html)|
| **Control escalable de metadatos** | En términos de macrodatos, incluso los metadatos pueden ser "macrodatos". Delta Lake trata los metadatos de la misma forma que a los datos y aprovecha la capacidad de procesamiento distribuido de Spark para administrar todos sus metadatos. Como resultado, Delta Lake puede administrar tablas de escala de petabytes con miles de millones de archivos y particiones fácilmente. |
| **Viaje en el tiempo (control de versiones de datos)** | La capacidad de "deshacer" un cambio o volver a una versión anterior es una de las características clave de las transacciones. Delta Lake proporciona instantáneas de los datos, lo que le permite revertir a versiones anteriores de datos para las auditorías, las reversiones o la reproducción de experimentos. Obtenga más información en [Introducción al viaje en el tiempo de Delta Lake para los lagos de datos a gran escala](https://databricks.com/blog/2019/02/04/introducing-delta-time-travel-for-large-scale-data-lakes.html). |
| **Formato abierto** | Apache Parquet es el formato de referencia para Delta Lake, lo que le permite aprovechar los eficientes esquemas de compresión y codificación que son nativos para el formato. |
| **Lote unificado y origen y receptor de streaming** | Una tabla en Delta Lake es una tabla de lotes, así como un origen y un receptor de streaming. La ingesta de datos de streaming, la reposición histórica de lotes y las consultas interactivas funcionan de manera integral. |
| **Aplicación de esquemas** | La aplicación de esquemas ayuda a garantizar que los tipos de datos son correctos y que las columnas necesarias están presentes; de este modo, se evita que los datos incoherentes generen incoherencias en los datos. Para obtener más información, consulte [Profundización en Delta Lake: aplicación y evolución de esquemas](https://databricks.com/blog/2019/09/24/diving-into-delta-lake-schema-enforcement-evolution.html) |
| **Evolución del esquema** | Delta Lake le permite realizar cambios en un esquema de tabla que se puede aplicar automáticamente, sin tener que escribir DDL de migración. Para obtener más información, consulte [Profundización en Delta Lake: aplicación y evolución de esquemas](https://databricks.com/blog/2019/09/24/diving-into-delta-lake-schema-enforcement-evolution.html) |
| **Historial de auditorías** | Los registros de transacciones de Delta Lake registran los detalles de todos los cambio realizados en los datos, lo que proporciona un registro de auditoría completo de los cambios. |
| **Actualizaciones y eliminaciones** | Delta Lake es compatible con las API de Scala/Java/Python y SQL para una gran variedad de funcionalidades. La compatibilidad con las operaciones MERGE, UPDATE y DELETE le ayuda a satisfacer los requisitos de cumplimiento. Para obtener más información, consulte [Presentación de la versión 0.6.1 de Delta Lake](https://delta.io/news/delta-lake-0-6-1-released/), [Presentación de la versión 0.7 de Delta Lake](https://delta.io/news/delta-lake-0-7-0-released/) y [Operaciones de upsert y eliminaciones simples y confiables en tablas de Delta Lake con las API de Python](https://databricks.com/blog/2019/10/03/simple-reliable-upserts-and-deletes-on-delta-lake-tables-using-python-apis.html), que incluye fragmentos de código para los comandos DML MERGE, UPDATE y DELETE. |
| **100 % compatible con la API de Apache Spark** | Los desarrolladores pueden usar Delta Lake con sus canalizaciones de datos actuales realizando cambios mínimos, ya que es totalmente compatible con las implementaciones de Spark existentes. |

Para ver la documentación completa, consulte la [página de la documentación de Delta Lake](https://docs.delta.io/latest/delta-intro.html).

Para obtener más información, consulte [Proyecto Delta Lake](https://github.com/delta-io/delta).

## <a name="next-steps"></a>Pasos siguientes

- [Documentación de .NET para Apache Spark](/dotnet/spark)
- [Azure Synapse Analytics](../index.yml)