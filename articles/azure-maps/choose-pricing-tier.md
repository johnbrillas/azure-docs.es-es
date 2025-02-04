---
title: Elección del plan de tarifa adecuado para Microsoft Azure Maps
description: Obtenga información sobre los planes de tarifa de Azure Maps. Vea qué características se ofrecen en qué niveles y consulte las consideraciones clave para elegir un plan de tarifa.
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: cb57e424642141e73588a61d026d1ff2f1a8b096
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96905305"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Elección del plan de tarifa adecuado de Azure Maps

Azure Maps ofrece dos planes de tarifa: S0 y S1. El propósito de este artículo es ayudarle a elegir el plan de tarifa adecuado para satisfacer sus necesidades. Para elegir el plan de tarifa adecuado, plantéese las dos preguntas siguientes.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>¿Cuántos usuarios simultáneos tiene previsto admitir?

Los planes de tarifa S0 y S1 manipulan diferentes volúmenes de rendimiento de datos. El plan de tarifa S0 manipula hasta **50 consultas por segundo**. Por el contrario, el plan de tarifa S1 administra **más de 50 consultas por segundo**.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>¿Qué funcionalidades geoespaciales tiene previsto usar?

Si las API geoespaciales básicas satisfacen los requisitos del servicio, elija el plan de tarifa S0. Si desea funcionalidades más avanzadas para su aplicación, considere la posibilidad de optar por el plan de tarifa S1. Las funcionalidades avanzadas son: imágenes aéreas e híbridas, obtención del alcance de la ruta y geocodificación por lotes. Para seleccionar el plan de tarifa más apropiado para la aplicación, consulte la tabla con las **funcionalidades del plan de tarifa**  a continuación:

### <a name="pricing-tier-capabilities"></a>Funcionalidades de los planes de tarifa

| Capacidad                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Representación de mapas                              | ✓                   | ✓       |
| Imágenes de satélite                       |                     | ✓        |
| Search                                  | ✓                    | ✓        |
| Búsqueda por lotes                            |                     | ✓        |
| Enrutar                                   | ✓                    |✓        |
| Enrutamiento por lotes                            |                    | ✓        |
| Enrutamiento de matriz                          |                     | ✓        |
| Alcance de ruta (isocronas)                |                     | ✓        |
| Tráfico                                |✓                    |✓        |
| Zona horaria                               |✓                    |✓        |
| Geolocalización (versión preliminar)                    |✓                   |✓        |
| Spatial Operations                        |                    |✓        |
| Geovalla                                |                    |✓        |
| Datos de Azure Maps (versión preliminar)                |                     | ✓        |
| Movilidad (versión preliminar)                       |                     | ✓        |
| Meteorología (versión preliminar)                        |✓                    |✓        |
|  Creator (versión preliminar)                         |                   |✓        |
|  Elevation (versión preliminar)                        |                   |✓        |

Tenga en cuenta también estos otros aspectos:

* ¿Qué tipo de empresa tiene?
* ¿Es crítica la aplicación?

### <a name="pricing-tier-targeted-customers"></a>Clientes objetivo de los planes de tarifa

Consulte la tabla **Clientes objetivo de los planes de tarifa** para obtener una visión general de los planes de tarifa S0 y S1. Para más información, consulte [Precios de Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/). 

| Plan de tarifa  |     Clientes objetivo                                                                |
|-----------------|:-----------------------------------------------------------------------------------------|
| S0            |    El plan de tarifa S0 funciona para las aplicaciones en todas las fases de producción, desde el desarrollo de la prueba de concepto y la primera fase de pruebas hasta la puesta en producción e implementación de la aplicación. Sin embargo, este nivel está diseñado para el desarrollo a pequeña escala, para clientes con pocos usuarios simultáneos o ambos. 
| S1            |    El plan de tarifa S1 está orientado a clientes con aplicaciones empresariales a gran escala, aplicaciones críticas para la empresa o grandes volúmenes de usuarios simultáneos. También es para aquellos clientes que requieren servicios geoespaciales avanzados.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo ver y cambiar los planes de tarifa:

> [!div class="nextstepaction"]
> [Administración de un plan de tarifa](how-to-manage-pricing-tier.md)
