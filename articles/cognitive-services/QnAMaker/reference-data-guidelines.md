---
title: 'Referencia de importación y exportación de datos: QnA Maker'
description: Use esta referencia de importación y exportación para obtener los mejores resultados de la copia de seguridad, el almacenamiento y el reemplazo de la knowledge base.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 01/02/2020
ms.openlocfilehash: d73fd6c7f49c10c8aca1060e91fc69b0e581738b
ms.sourcegitcommit: 17e9cb8d05edaac9addcd6e0f2c230f71573422c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2020
ms.locfileid: "97707510"
---
# <a name="import-and-export-data-reference"></a>Referencia de importación y exportación de datos

Revise esta referencia de importación y exportación para obtener los mejores resultados de la copia de seguridad, el almacenamiento y la sustitución de la base de conocimiento.

## <a name="import-and-export-knowledge-base"></a>Importación y exportación de la base de conocimiento

**Los archivos TSV y XLS**, de las bases de conocimiento exportadas, solo se pueden usar importando los archivos de la página **Configuración** en el portal QnA Maker. No se pueden usar como orígenes de datos durante la creación de la base de conocimiento ni desde las funciones **+ Agregar archivo** o **+ Agregar URL** de la página **Configuración**. 

Al importar la knowledge base mediante estos **archivos TSV y XLS**, los pares de preguntas y respuestas se agregan al origen editorial y no a los orígenes de los que se extrajeron estos pares en la knowledge base exportada. 
