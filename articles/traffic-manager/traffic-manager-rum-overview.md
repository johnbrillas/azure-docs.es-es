---
title: Mediciones de usuario reales en Azure Traffic Manager
description: En esta introducción, aprenderá cómo funcionan las Mediciones de usuario reales de Azure Traffic Manager.
services: traffic-manager
documentationcenter: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: duau
ms.custom: ''
ms.openlocfilehash: 618f8fff532da0f6ae315ad9e4cda35a289949d1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98183717"
---
# <a name="traffic-manager-real-user-measurements-overview"></a>Introducción a Real User Measurements en Traffic Manager

Al configurar un perfil de Traffic Manager para usar el método de enrutamiento de rendimiento, el servicio examina de dónde proceden las solicitudes de consulta DNS y toma decisiones de enrutamiento para dirigir a los solicitantes a la región de Azure que proporcione la latencia más baja. Esto se logra mediante el uso de la información de latencia de red que Traffic Manager mantiene para distintas redes de usuario final.

Real User Measurements le permite medir la latencia de red de regiones de Azure desde las aplicaciones cliente que utilizan los usuarios finales y hacer que Traffic Manager considere también esta información al tomar decisiones de enrutamiento. Si opta por usar Real User Measurements, puede aumentar la precisión de la ruta para las solicitudes procedentes de las redes en las que residen los usuarios finales. 

## <a name="how-real-user-measurements-work"></a>Cómo funciona Real User Measurements

Mediciones de usuario reales funciona haciendo que las aplicaciones cliente midan la latencia de las regiones de Azure tal como se ve desde las redes de usuario final en las que se utilizan. Por ejemplo, si tiene una página web a la que tienen acceso usuarios de distintas ubicaciones (por ejemplo, en las regiones de Norteamérica), puede usar Mediciones de usuario reales con el método de enrutamiento de rendimiento para dirigirlos a la mejor de las regiones de Azure en las que la aplicación está hospedada.

Se comienza por insertar un código JavaScript proporcionado por Azure (que contiene una clave única) en las páginas web. Una vez hecho, siempre que un usuario visita la página web, el código JavaScript consulta a Traffic Manager para obtener información acerca de las regiones de Azure que debe medir. El servicio devuelve un conjunto de puntos de conexión al script que, a continuación, mide estas regiones consecutivamente mediante la descarga de una imagen de un solo píxel hospedada en esas regiones de Azure y anotando la latencia entre el momento en el que se envió la solicitud y la hora a la que se recibió el primer byte. Estas mediciones, a continuación, se notifican de vuelta al servicio Traffic Manager.

Con el tiempo, esto ocurre muchas veces y en muchas redes, proporcionando a Traffic Manager información más precisa sobre las características de latencia de las redes en las que residen los usuarios finales. Esta información comienza a incluirse en las decisiones de enrutamiento realizadas por Traffic Manager. Como resultado, se producirá una mayor precisión en esas decisiones basadas en las mediciones de usuarios reales enviadas.

Cuando use Real User Measurements, se le facturará en función del número de mediciones enviadas a Traffic Manager. Para más detalles sobre los precios, visite la [página de precios de Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faqs"></a>Preguntas más frecuentes

* [¿Cuáles son las ventajas de usar Real User Measurements?](./traffic-manager-faqs.md#what-are-the-benefits-of-using-real-user-measurements)

* [¿Se puede usar Real User Measurements con regiones que no son de Azure?](./traffic-manager-faqs.md#can-i-use-real-user-measurements-with-non-azure-regions)

* [¿Qué método de enrutamiento se beneficia de Real User Measurements?](./traffic-manager-faqs.md#which-routing-method-benefits-from-real-user-measurements)

* [¿Es necesario habilitar Real User Measurements para cada perfil por separado?](./traffic-manager-faqs.md#do-i-need-to-enable-real-user-measurements-each-profile-separately)

* [¿Cómo se desactiva Real User Measurements en una suscripción?](./traffic-manager-faqs.md#how-do-i-turn-off-real-user-measurements-for-my-subscription)

* [¿Se puede usar Real User Measurements con aplicaciones cliente que no sean páginas web?](./traffic-manager-faqs.md#can-i-use-real-user-measurements-with-client-applications-other-than-web-pages)

* [¿Cuántas medidas se realizan cada vez que se representa la página web habilitada para Real User Measurements?](./traffic-manager-faqs.md#how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered)

* [¿Existe un retraso antes de que el script de Real User Measurements se ejecute en la página web?](./traffic-manager-faqs.md#is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage)

* [¿Se pueden usar las Mediciones de usuario reales solo con las regiones que se quieren medir?](./traffic-manager-faqs.md#can-i-use-real-user-measurements-with-only-the-azure-regions-i-want-to-measure)

* [¿Se puede limitar el número de medidas realizadas a un número específico?](./traffic-manager-faqs.md#can-i-limit-the-number-of-measurements-made-to-a-specific-number)

* [¿Se pueden ver las medidas realizadas por mis aplicaciones cliente como parte de Real User Measurements?](./traffic-manager-faqs.md#can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements)

* [¿Se puede modificar el script de medida proporcionado por Traffic Manager?](./traffic-manager-faqs.md#can-i-modify-the-measurement-script-provided-by-traffic-manager)

* [¿Podrán ver otros usuarios la clave que se usa con Real User Measurements?](./traffic-manager-faqs.md#will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements)

* [¿Pueden otros usuarios hacer un uso inapropiado de la clave RUM?](./traffic-manager-faqs.md#can-others-abuse-my-rum-key)

* [¿Es necesario poner el código JavaScript de medida en todas las páginas web?](./traffic-manager-faqs.md#do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages)

* [¿Puede Traffic Manager identificar la información sobre mis usuarios finales si uso Real User Measurements?](./traffic-manager-faqs.md#can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements)

* [¿Es necesario que la página web que realiza las medidas de usuarios reales use Traffic Manager para el enrutamiento?](./traffic-manager-faqs.md#does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing)

* [¿Es necesario hospedar algún servicio en regiones de Azure para usar Real User Measurements?](./traffic-manager-faqs.md#do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements)

* [¿Aumentará el uso de ancho de banda de Azure debido a la utilización de Real User Measurements?](./traffic-manager-faqs.md#will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements)

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información acerca de cómo usar [Real User Measurements con páginas web](traffic-manager-create-rum-web-pages.md)
- Aprenda [cómo funciona el Administrador de tráfico](traffic-manager-overview.md)
- Obtenga más información sobre [Mobile Center](/mobile-center/).
- Aprenda más sobre los [métodos de enrutamiento de tráfico](traffic-manager-routing-methods.md) que admite el Administrador de tráfico.
- Aprenda a [crear un perfil del Administrador de tráfico](./quickstart-create-traffic-manager-profile.md)