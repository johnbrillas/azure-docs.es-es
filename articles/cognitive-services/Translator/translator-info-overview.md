---
title: Servicio Microsoft Translator
titlesuffix: Azure Cognitive Services
description: Integre Translator en aplicaciones, sitios web, herramientas u otras soluciones para proporcionar experiencias de usuario en varios idiomas.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.topic: overview
ms.subservice: translator-text
ms.date: 02/15/2021
ms.author: lajanuar
ms.custom: cog-serv-seo-aug-2020
keywords: translator, text translation, machine translation, translation service
ms.openlocfilehash: 72df700d38cad3ee31ef438d7df1c890acde9e6b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101727981"
---
# <a name="what-is-the-translator-service"></a>¿Qué es el servicio Translator?

Translator es un servicio de traducción automática basado en la nube que forma parte de la familia de API de [Azure Cognitive Services](../../index.yml?panel=ai&pivot=products) utilizadas para crear aplicaciones inteligentes. Translator se integra con facilidad en cualquier aplicación, sitio web, herramienta y solución. Permite agregar experiencias de usuario multilingües en [más de 90 idiomas y dialectos](./language-support.md). Se puede usar en cualquier plataforma de hardware y con cualquier sistema operativo para la traducción de texto.

## <a name="about-microsoft-translator"></a>Acerca de Microsoft Translator

Translator da servicio a muchos productos y servicios de Microsoft, y lo utilizan miles de empresas en todo el mundo en sus aplicaciones y flujos de trabajo.

La traducción de voz, con la tecnología de Translator, también está disponible mediante el [servicio de Voz de Azure](../speech-service/index.yml). Combina la funcionalidad de Translator Speech API y Custom Speech Service en un servicio totalmente personalizable y unificado. 

## <a name="language-support"></a>Compatibilidad con idiomas

Translator proporciona compatibilidad con varios idiomas para traducción de texto, transliteración, detección de idioma y diccionarios. Consulte [Compatibilidad con idiomas](language-support.md) para obtener una lista completa o puede acceder a la lista mediante programación con la [API REST](./reference/v3-0-languages.md).  

## <a name="microsoft-translator-neural-machine-translation"></a>Traducción automática neuronal de Microsoft Translator

La traducción automática neuronal (NMT) es el nuevo estándar para las traducciones automáticas de alta calidad basadas en inteligencia artificial. Este estándar reemplaza la traducción automática estadística (SMT) que alcanzó un nivel estable a mediados de 2010.

NMT proporciona mejores traducciones que SMT, no solo a la hora de puntuar la calidad de la traducción bruta, sino también porque suenan más fluidas y humanas. El motivo principal de esta fluidez es que NMT usa el contexto completo de una frase para traducir las palabras. SMT solo tomaba el contexto inmediato de unas cuantas palabras antes y después de cada palabra.

Los modelos NMT se sitúan en el centro de la API y no son visibles para los usuarios finales. La única diferencia perceptible es una mejor calidad de la traducción, en especial en los idiomas chino, japonés y árabe.

Más información en [¿Qué es la traducción automática?](https://www.microsoft.com/en-us/translator/mt.aspx#nnt).

## <a name="improve-translations-with-custom-translator"></a>Mejora de las traducciones con Traductor personalizado

 Traductor personalizado es una extensión del servicio Translator que se puede usar en combinación con Translator como ayuda para personalizar el sistema de traducción neuronal y mejorar la traducción con su terminología y estilo específicos.

Con Traductor personalizado puede crear sistemas de traducción que administran la terminología usada en su propio negocio o sector. Luego, los sistemas de traducción personalizados se pueden integrar fácilmente en las aplicaciones, flujos de trabajo, sitios web y dispositivos ya existentes, mediante el componente normal Translator, por medio del parámetro de categoría.

Más información acerca de [Traductor personalizado](customization.md).

## <a name="next-steps"></a>Pasos siguientes

- [Cree una instancia del servicio Translator](./translator-how-to-signup.md) para obtener las claves de acceso y el punto de conexión.
- Pruebe nuestra [Guía de inicio rápido](quickstart-translator.md) para aprender a llamar rápidamente al servicio Translator.
- [Referencia de API](./reference/v3-0-reference.md) proporciona la documentación técnica de las API.
- [Detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
