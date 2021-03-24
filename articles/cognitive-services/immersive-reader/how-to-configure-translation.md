---
title: Configurar la traducción
titleSuffix: Azure Cognitive Services
description: En este artículo se muestra cómo configurar las distintas opciones de traducción.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: how-to
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: bb90cb3a86acb6a94fa92c33d78ec596659e105f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102608706"
---
# <a name="how-to-configure-translation"></a>Configuración de la traducción

En este artículo se muestra cómo configurar las distintas opciones de traducción en el Lector inmersivo.

## <a name="configure-translation-language"></a>Configuración del idioma de traducción

El parámetro `options` contiene todas las marcas que se pueden usar para configurar la traducción. Establezca el parámetro `language` en el idioma al que desea traducir. Consulte la [Compatibilidad de idiomas](./language-support.md) para obtener la lista completa de los idiomas admitidos.

```typescript
const options = {
    translationOptions: {
        language: 'fr-FR'
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="automatically-translate-the-document-on-load"></a>Traducción automática del documento en carga

Establezca `autoEnableDocumentTranslation` en `true` para habilitar la traducción automática de todo el documento cuando se cargue el Lector inmersivo.

```typescript
const options = {
    translationOptions: {
        autoEnableDocumentTranslation: true
    }
};
```

## <a name="automatically-enable-word-translation"></a>Habilitación automática de la traducción de palabras

Establezca `autoEnableWordTranslation` en `true` para habilitar la traducción de una sola palabra.

```typescript
const options = {
    translationOptions: {
        autoEnableWordTranslation: true
    }
};
```

## <a name="next-steps"></a>Pasos siguientes

* Lea la [Referencia del SDK del Lector inmersivo](./reference.md)