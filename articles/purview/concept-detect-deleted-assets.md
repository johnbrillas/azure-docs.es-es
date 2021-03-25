---
title: Cómo detectan los exámenes los recursos eliminados
description: En este artículo se explica cómo una cuenta de Azure Purview detecta recursos eliminados durante los exámenes.
author: yaronyg
ms.author: yarong
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 10/16/2020
ms.openlocfilehash: 9b1515ef00355c831e161c01227678197792cc20
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96551344"
---
# <a name="how-scans-detect-deleted-assets"></a>Cómo detectan los exámenes los recursos eliminados

En este artículo se describe cómo Azure Purview usa los resultados del examen para detectar recursos eliminados.

## <a name="background-info"></a>Información de contexto

Un catálogo de Azure Purview conoce el estado de un almacén de datos solo cuando lo examina. Para que el catálogo determine si se ha eliminado un archivo, una tabla o un contenedor, compara la última salida del examen con la salida del examen actual. Por ejemplo, supongamos que la última vez que examinó una cuenta de Azure Data Lake Storage Gen2, incluía una carpeta llamada *carpeta1*. Al volver a examinar la misma cuenta, *folder1* ya no está. Por lo tanto, el catálogo supone que la carpeta se ha eliminado.

## <a name="detecting-deleted-files"></a>Detección de archivos eliminados

La lógica para detectar archivos que faltan funciona para varios exámenes realizados por el mismo usuario y también por distintos usuarios. Por ejemplo, supongamos que un usuario ejecuta un examen puntual en un almacén de datos Data Lake Storage Gen2 en las carpetas A, B y C. Más adelante, otro usuario en la misma cuenta ejecuta un examen puntual en las carpetas C, D y E del mismo almacén de datos. Dado que la carpeta C se ha examinado dos veces, el catálogo la comprueba para detectar posibles eliminaciones. Sin embargo, las carpetas A, B, D y E solo se han examinado una vez y el catálogo no las comprobará para determinar si hay recursos eliminados.

Para mantener los archivos eliminados fuera del catálogo, es importante realizar exámenes periódicamente. El intervalo entre exámenes es importante, ya que el catálogo no puede detectar recursos eliminados hasta que se ejecute otro examen. Por lo tanto, si ejecuta exámenes una vez al mes en un almacén determinado, el catálogo no podrá detectar recursos de datos eliminados en ese almacén hasta que ejecute el siguiente examen un mes más tarde.

Al enumerar almacenes de datos de gran tamaño, como Data Lake Storage Gen2, es posible que se omita información de varias maneras (como errores de enumeración y eventos eliminados). Un examen determinado podría pasar por alto un archivo creado o eliminado. Por lo tanto, a menos que el catálogo tenga la seguridad de que se ha eliminado un archivo, no lo eliminará del catálogo. Esta estrategia implica que podría haber errores cuando un archivo que no existe en el almacén de datos examinado todavía existe en el catálogo. En algunos casos, es posible que sea necesario examinar un almacén de datos dos o tres veces antes de que se detecten determinados recursos eliminados.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con los catálogos de Azure Purview, consulte [Inicio rápido: Creación de una cuenta de Purview](create-catalog-portal.md).
