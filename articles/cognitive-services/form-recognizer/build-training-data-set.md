---
title: Creación de un conjunto de datos de aprendizaje para un modelo personalizado (Form Recognizer)
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo asegurarse de que el conjunto de datos de aprendizaje está optimizado para entrenar un modelo de Form Recognizer.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: lajanuar
ms.openlocfilehash: b33ac3cb710a2d2a9d92efadf14dc829cb5da6e8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103467500"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Creación de un conjunto de datos de aprendizaje para un modelo personalizado

Cuando se usa el modelo personalizado de Form Recognizer, puede proporcionar sus propios datos de entrenamiento a la operación [Train Custom Model](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/TrainCustomModelAsync) (Entrenar modelo personalizado), para que el modelo pueda entrenar según los formularios específicos del sector. Siga esta guía para obtener información sobre cómo recopilar y preparar los datos para entrenar el modelo de forma eficaz.

Necesita al menos cinco formularios rellenados del mismo tipo.

Si quiere usar datos de entrenamiento etiquetados manualmente, tiene que empezar con al menos cinco formularios del mismo tipo. Podrá seguir usando formularios sin etiquetar además del conjunto de datos requerido.

## <a name="custom-model-input-requirements"></a>Requisitos de entrada del modelo personalizado

En primer lugar, asegúrese de que el conjunto de datos de aprendizaje también cumpla con los requisitos de entrada para Form Recognizer.

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="training-data-tips"></a>Sugerencias sobre los datos de aprendizaje

Siga estas sugerencias adicionales para optimizar aún más el conjunto de datos para el entrenamiento.

* Si es posible, use documentos PDF de texto en lugar de documentos basados en imágenes. Los archivos PDF digitalizados se tratan como imágenes.
* En el caso de los formularios rellenados, use ejemplos en los que estén todos los campos rellenados.
* Use formularios con valores distintos en cada campo.
* Si las imágenes de los formularios son de menor calidad, use un conjunto de datos más grande (con entre 10 y 15 imágenes, por ejemplo).

## <a name="upload-your-training-data"></a>Carga de los datos de aprendizaje

Una vez que recopila el conjunto de documentos de formularios que usará para el entrenamiento, deberá cargarlo a un contenedor de Azure Blob Storage. Si no sabe cómo crear una cuenta de almacenamiento de Azure con un contenedor, siga el [inicio rápido de Azure Storage para Azure Portal](../../storage/blobs/storage-quickstart-blobs-portal.md). Use el nivel de rendimiento estándar.

Si desea usar datos etiquetados manualmente, también tendrá que cargar los archivos *.labels.json* y *.ocr.json* correspondientes a los documentos de entrenamiento. Puede usar la [herramienta de etiquetado de ejemplo](./quickstarts/label-tool.md) (o su propia interfaz de usuario) para generar estos archivos.

### <a name="organize-your-data-in-subfolders-optional"></a>Organización de los datos en subcarpetas (opcional)

De manera predeterminada, la API de [Train Custom Model](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/TrainCustomModelAsync) (Entrenar modelo personalizado) solo usará documentos de formularios que se encuentren en la raíz del contenedor de almacenamiento. Sin embargo, puede realizar el entrenamiento con los datos de las subcarpetas si lo especifica así en la llamada API. Por lo general, el cuerpo de la llamada [Train Custom Model](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/TrainCustomModelAsync) (Entrenar modelo personalizado) tiene el formato siguiente, donde `<SAS URL>` es la dirección URL de la firma de acceso compartido del contenedor:

```json
{
  "source":"<SAS URL>"
}
```

Si agrega el contenido siguiente al cuerpo de la solicitud, la API se entrenará con los documentos ubicados en las subcarpetas. El campo `"prefix"` es opcional y limitará el conjunto de datos de aprendizaje a los archivos cuyas rutas de acceso empiezan con la cadena determinada. Por lo tanto, un valor de `"Test"`, por ejemplo, hará que la API examine solo los archivos o las carpetas que empiecen con la palabra "Test" (Prueba).

```json
{
  "source": "<SAS URL>",
  "sourceFilter": {
    "prefix": "<prefix string>",
    "includeSubFolders": true
  },
  "useLabelFile": false
}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que aprendió a crear un conjunto de datos de aprendizaje, siga un inicio rápido para entrenar a un modelo personalizado de Form Recognizer y empezar a usarlo en los formularios.

* [Entrenamiento de un modelo y extracción de datos de formularios mediante la biblioteca cliente o la API de REST](./quickstarts/client-library.md)
* [Entrenamiento con etiquetas mediante la herramienta de etiquetado de ejemplo](./quickstarts/label-tool.md)

## <a name="see-also"></a>Consulte también

* [¿Qué es Form Recognizer?](./overview.md)