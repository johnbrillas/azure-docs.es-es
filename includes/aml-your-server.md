---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 03/05/2020
ms.openlocfilehash: 0d7622217d192a100fd809c32c9e85970cf61fd7
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102510632"
---
1. Use las instrucciones del [SDL de Azure Machine Learning](/python/api/overview/azure/ml/install) para instalar el SDK de Azure Machine Learning para Python

1. Cree un [área de trabajo de Azure Machine Learning](../articles/machine-learning/how-to-manage-workspace.md).

1. Escriba un [archivo de configuración](../articles/machine-learning/how-to-configure-environment.md#workspace) (**aml_config/config.json**).

1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Inicie el servidor del cuaderno desde el directorio clonado.

    ```bash
    jupyter notebook
    ```