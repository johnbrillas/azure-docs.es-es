---
title: Administración de recursos y cuotas
titleSuffix: Azure Purview
description: Obtenga información sobre las cuotas y límites de los recursos de Azure Purview y cómo solicitar aumentos de cuota.
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/12/2020
ms.openlocfilehash: 3b0a413db304b4f9d2c50a3d221c480f1e9dc37a
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96550679"
---
# <a name="manage-and-increase-quotas-for-resources-with-azure-purview"></a>Administración y aumento de las cuotas de los recursos con Azure Purview
 
Azure Purview es un servicio en la nube disponible para usuarios de datos. Azure Purview permite administrar de forma centralizada la gobernanza de datos en todo el patrimonio de datos, desde entornos locales hasta en la nube. El servicio permite a los analistas de negocios buscar datos pertinentes mediante el uso de términos empresariales significativos. Para elevar los límites al máximo de la suscripción, póngase en contacto con el servicio de soporte técnico.
 
## <a name="azure-purview-limits"></a>Límites de Azure Purview
 
|**Recurso**|  **Límite predeterminado**  |**Límite máximo**|
|---|---|---|
|Cuentas de Purview por inquilino (todas las suscripciones combinadas)|3|Ponerse en contacto con el servicio de soporte técnico|
|Núcleos virtuales disponibles para el examen, por cuenta*|160|160|
|Exámenes simultáneos, por cuenta en un momento dado. El límite se basa en el tipo de orígenes de datos examinados*|5 | 10 |
|Tiempo máximo durante el que se puede ejecutar un examen|7 días|7 días|
|Llamadas API, por cuenta|10 millones de API/mes para una plataforma de 4 unidades de capacidad. <br>40 millones de API/mes para una plataforma de 16 unidades de capacidad. |10 millones de API/mes para una plataforma de 4 unidades de capacidad. <br>40 millones de API/mes para una plataforma de 16 unidades de capacidad.|
|Almacenamiento, por cuenta|10 GB para una plataforma de 4 unidades de capacidad. <br>40 GB para una plataforma de 16 unidades de capacidad. |10 GB para una plataforma de 4 unidades de capacidad. <br> 40 GB para una plataforma de 16 unidades de capacidad. |
|Tamaño de los recursos por cuenta|100 millones de recursos físicos |Ponerse en contacto con el servicio de soporte técnico|
|Tamaño máximo de un recurso en un catálogo|2 MB|2 MB|
|Longitud máxima de un nombre de recurso y nombre de clasificación|4 KB|4 KB|
|Longitud máxima del nombre y valor de la propiedad del recurso|32 KB|32 KB|
|Longitud máxima del nombre y valor del atributo de clasificación|32 KB|32 KB|
|Número máximo de términos del glosario, por cuenta|100 k|100 k|
 
*Los escenarios de entorno de ejecución de integración autohospedado están fuera del ámbito de los límites definidos en la tabla anterior. 
 
## <a name="next-steps"></a>Pasos siguientes
 
> [!div class="nextstepaction"]
>[Tutorial: Examen de datos con Azure Purview](tutorial-scan-data.md)

> [!div class="nextstepaction"]
>[Tutorial: Desplazamiento por la página principal y búsqueda de un recurso](tutorial-asset-search.md)

> [!div class="nextstepaction"]
>[Tutorial: Examen de recursos y visualización de su linaje](tutorial-browse-and-view-lineage.md)
