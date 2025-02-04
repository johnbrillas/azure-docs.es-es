---
title: Adición de una audiencia preliminar para la oferta de Aplicación de Azure
description: Aprenda a agregar una audiencia preliminar para la oferta de Aplicación de Azure en el Centro de partners.
author: aarathin
ms.author: aarathin
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 11/06/2020
ms.openlocfilehash: 1d41e9dc39b50b92351fe38a86587a67811c5eec
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "94369866"
---
# <a name="how-to-add-a-preview-audience-for-your-azure-application-offer"></a>Adición de una audiencia preliminar para la oferta de Aplicación de Azure

En este artículo se describe cómo configurar una audiencia preliminar para una oferta de Aplicación de Azure en el marketplace comercial. Debe definir una audiencia preliminar que pueda revisar la descripción de la oferta antes de publicarla.

## <a name="define-a-preview-audience"></a>Definición de una audiencia preliminar

En la pestaña **Preview audience** (Audiencia preliminar), puede definir una audiencia limitada que revise la oferta de Aplicación de Azure antes de publicarla para la audiencia más amplia del marketplace. Especifique la audiencia preliminar mediante los identificadores de suscripción de Azure, junto con una descripción opcional de cada uno. Los clientes no pueden ver ninguno de estos campos. Puede encontrar el identificador de suscripción de Azure en la página **Suscripciones** de Azure Portal.

Agregue entre uno y diez identificadores de suscripción de Azure, ya sea de forma individual (hasta 10) o mediante la carga de un archivo .csv (hasta 100) para definir quién recibirá una versión preliminar de la oferta antes de publicarla. Si la oferta ya está publicada, todavía puede definir un público preliminar para probar los cambios o las actualizaciones de la oferta.

> [!NOTE]
> Un público preliminar no es el mismo que un público privado. A una audiencia preliminar se le permite el acceso a la oferta antes de que se publique en las tiendas en línea. Podrá ver y validar todos los planes, incluidos aquellos que estarán disponibles solo para un público privado después de que su oferta se publique completamente en el marketplace. Puede hacer que un plan esté disponible solo para una audiencia privada. Una audiencia privada (definida en la pestaña **Disponibilidad** del plan) tiene acceso exclusivo a un plan determinado.

### <a name="add-email-addresses-manually"></a>Agregar direcciones de correo electrónico manualmente

1. En la página **Preview audience** (Audiencia preliminar), agregue un único identificador de suscripción de Azure y una descripción opcional en los cuadros proporcionados.
1. Para agregar otra dirección de correo electrónico, seleccione el vínculo **Add ID (Max 10)** (Agregar identificador [10 como máximo]).
1. Seleccione **Guardar borrador** antes de pasar a la siguiente pestaña: Configuración técnica.
1. Vaya a [Pasos siguientes](#next-steps).

### <a name="add-email-addresses-using-the-csv-file"></a>Agregar direcciones de correo electrónico mediante el archivo CSV

1. En la página **Preview audience** (Audiencia preliminar), seleccione el vínculo **Export Audience (csv)** (Exportar audiencia [csv]).
1. Abra el archivo CSV en una aplicación, como Microsoft Excel.
1. En el archivo .csv, en la columna **Id.** , escriba los identificadores de la suscripción de Azure que quiere agregar a la audiencia preliminar.
1. En la columna **Descripción**, tiene la opción de agregar una descripción para cada dirección de correo electrónico.
1. En la columna **Tipo**, agregue **SubscriptionID** a cada fila que tenga una dirección de correo electrónico.
1. Guarde el archivo como un archivo CSV.
1. En la página **Preview audience** (Audiencia preliminar), seleccione el vínculo **Import Audience (csv)** (Importar audiencia [csv]).
1. En el cuadro de diálogo **Confirmar**, seleccione **Sí**.
1. Seleccione el archivo CSV y, después, elija **Abrir**.
1. Seleccione **Guardar borrador** antes de pasar a la siguiente pestaña: Configuración técnica.

## <a name="next-steps"></a>Pasos siguientes

- [Adición de detalles técnicos para la oferta de Aplicación de Azure](create-new-azure-apps-offer-technical.md)
