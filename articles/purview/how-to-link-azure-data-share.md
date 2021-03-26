---
title: Conexión a Azure Data Share
description: En este artículo se describe cómo conectar una cuenta de Azure Data Share con Azure Purview para buscar recursos y realizar el seguimiento del linaje de datos.
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/25/2020
ms.openlocfilehash: edea881d67d5a677339c6ff357c1ac45f5dfd420
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96551340"
---
# <a name="how-to-connect-azure-data-share-and-azure-purview"></a>Conexión de Azure Data Share y Azure Purview

En este artículo se describe cómo conectar la cuenta de [Azure Data Share](../data-share/overview.md) con Azure Purview y controlar los conjuntos de datos compartidos (tanto salientes como entrantes) de su patrimonio de datos. Hay varios roles de gobernanza de datos que pueden detectar y realizar un seguimiento del linaje de datos más allá de límites como organizaciones, departamentos e incluso centros de datos.

## <a name="common-scenarios"></a>Escenarios comunes

El linaje de Data Share tiene como objetivo proporcionar información detallada para el análisis de la causa principal y el análisis de impacto.

### <a name="scenario-1-360-view-of-datasets-shared-inout-for-a-partner-organization-or-internal-department"></a>Escenario 1: 360 vistas de conjuntos de datos compartidos de entrada y salida con una organización asociada o un departamento interno

Los responsables de datos pueden ver una lista de todos los conjuntos de datos que se comparten bidireccionalmente con las organizaciones asociadas. Pueden buscar y detectar los conjuntos de datos por nombre de la organización y disponen de una vista completa de todos los recursos compartidos de entrada y salida.

### <a name="scenario-2-root-cause-analysis---upstream-dependency-on-datasets-coming-into-organization-consumer-view-of-incoming-shares"></a>Escenario 2: Análisis de la causa principal: dependencia ascendente en conjuntos de datos que entran en la organización (vista de consumidor de los recursos compartidos entrantes)

Un informe tiene información incorrecta debido a problemas de datos ascendentes procedentes de una cuenta de Data Share externa. Los ingenieros de datos pueden comprender los errores ascendentes, estar informados de los motivos y ponerse en contacto con el propietario del recurso compartido para corregir los problemas que causan la discrepancia de datos.

### <a name="scenario-3-impact-analysis-on-datasets-going-outside-organization-provider-view-of-outgoing-shares"></a>Escenario 3: Análisis de impacto en conjuntos de datos que van fuera de la organización (vista de proveedor de los recursos compartidos salientes)

Los productores de datos quieren saber quién se verá afectado al realizar un cambio en el conjunto de datos. Mediante el linaje, un productor de datos puede comprender fácilmente el impacto en los asociados internos o externos descendentes que consumen los datos mediante Azure Data Share.

## <a name="azure-data-share-and-purview-connected-experience"></a>Experiencia conectada de Azure Data Share y Purview

Para conectar Azure Data Share y la cuenta de Azure Purview, haga lo siguiente:

1. Cree una cuenta de Purview. Toda la información de linaje de Data Share se recopilará mediante una cuenta de Purview. Puede usar una existente o crear una nueva cuenta de Purview.

1. Conecte el recurso de Azure Data Share a la cuenta de Purview.

    1. En el portal de Purview, vaya a **Management Center** (Centro de administración) y conecte el recurso de Azure Data Share en la sección **External connections** (Conexiones externas).
    1. Seleccione **+ New** (+ Nuevo) en la barra superior, busque el recurso de Azure Data Share en la barra lateral emergente y agregue la cuenta de Data Share. Ejecute un trabajo de instantánea después de conectar el recurso compartido de datos a la cuenta de Purview, para que los recursos del recurso compartido de datos y la información de linaje sean visibles en Purview.

       :::image type="content" source="media/how-to-link-azure-data-share/connect-to-data-share.png" alt-text="Centro de administración para vincular Azure Data Share":::

1. Ejecute la instantánea en Azure Data Share.

    - Una vez que se establece la conexión de Azure Data Share con Azure Purview, puede ejecutar una instantánea para los recursos compartidos existentes. 
    - Si no tiene ningún recurso compartido existente, vaya al portal de Azure Data Share para [compartir los datos](../data-share/share-your-data.md) [y suscribirse a un recurso compartido de datos](../data-share/subscribe-to-data-share.md).
    - Una vez completada la instantánea del recurso compartido, puede ver los recursos de Data Share asociados y el linaje en Purview.

1. Detecte cuentas de Data Share y comparta información en la cuenta de Purview.

    - En la página principal de la cuenta de Purview, seleccione **Browse by asset type** (Examinar por tipo de recurso) y seleccione el icono de **Azure Data Share**. Puede buscar un nombre de cuenta, un nombre de recurso compartido, una instantánea de recurso compartido o una organización asociada. De lo contrario, aplique filtros en la página de resultados de la búsqueda para el nombre de cuenta o el tipo de recurso compartido (recursos compartidos enviados o recibidos).

       :::image type="content" source="media/how-to-link-azure-data-share/azure-data-share-search-result-page.png" alt-text="Recurso de Azure Data Share en la página de resultados de la búsqueda":::

    >[!Important]
    >Para que los recursos de Data Share se muestren en Purview, se debe ejecutar un trabajo de instantánea después de conectar el recurso de Data Share a Purview.

1. Realice un seguimiento del linaje de los conjuntos de datos compartidos con Azure Data Share.

    - En la página de resultados de la búsqueda de Purview, elija la instantánea del recurso compartido de datos (recibido o enviado) y seleccione la pestaña **Lineage** (Linaje) para ver un gráfico de linaje con dependencias ascendentes y descendentes.

    :::image type="content" source="media/how-to-link-azure-data-share/azure-data-share-lineage.png" alt-text="Linaje de los conjuntos de datos compartidos con Azure Data Share":::

## <a name="next-steps"></a>Pasos siguientes

- [Guía de usuario del linaje del catálogo de datos de Azure Purview](catalog-lineage-user-guide.md)
- [Cómo conectar Azure Data Factory y Azure Purview](how-to-link-azure-data-factory.md)
