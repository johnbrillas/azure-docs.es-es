---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 12/01/2020
ms.author: larryfr
ms.openlocfilehash: e92d3ac9ed4330cc1680428a426e881538cb0e78
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102623350"
---
* Al crear una nueva área de trabajo, puede crear automáticamente los servicios necesarios para esa área de trabajo o usar los servicios ya existentes. Si quiere usar los __servicios ya existentes de una suscripción de Azure diferente__ al área de trabajo, debe registrar el espacio de nombres de Azure Machine Learning en la suscripción que contiene esos servicios. Por ejemplo, al crear un área de trabajo en la suscripción A que usa una cuenta de almacenamiento de la suscripción B, el espacio de nombres de Azure Machine Learning debe estar registrado en la suscripción B para poder usar la cuenta de almacenamiento con el área de trabajo.

    El proveedor de recursos para Azure Machine Learning es __Microsoft.MachineLearningService__. Para obtener información sobre cómo comprobar si está registrado y cómo realizar el registro, consulte el artículo [Proveedores de recursos de Azure y sus tipos](../articles/azure-resource-manager/management/resource-providers-and-types.md).

    > [!IMPORTANT]
    > Esta opción solo se aplica a los recursos proporcionados durante la creación del área de trabajo para cuentas de Azure Storage, el registro de contenedores de Azure, Azure Key Vault y Application Insights.
