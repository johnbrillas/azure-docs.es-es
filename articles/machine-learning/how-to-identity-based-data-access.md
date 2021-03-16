---
title: Acceso a datos basado en identidad para los servicios de almacenamiento de Azure
titleSuffix: Azure Machine Learning
description: Aprenda a usar el acceso a datos basado en identidad para conectarse a los servicios de almacenamiento de Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 02/22/2021
ms.custom: how-to, contperf-fy21q1, devx-track-python, data4ml
ms.openlocfilehash: 68d07481e228b1d1b2f4571a783f925add261cff
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102520020"
---
# <a name="connect-to-storage-with-identity-based-data-access-preview"></a>Conexión al almacenamiento mediante el acceso a datos basado en identidad (versión preliminar)

>[!IMPORTANT]
> Las funcionalidades que se presentan en este artículo se encuentran en versión preliminar y deben considerarse características [experimentales](/python/api/overview/azure/ml/#stable-vs-experimental) en versión preliminar que pueden cambiar en cualquier momento.

En este artículo, aprenderá a conectarse a los servicios de almacenamiento de Azure mediante el acceso a datos basado en identidad y los almacenes de datos de Azure Machine Learning con el [SDK de Azure Machine Learning para Python](/python/api/overview/azure/ml/intro).  

Normalmente, los almacenes de datos usan el acceso a datos basado en credenciales para confirmar que dispone de permiso para acceder al servicio de almacenamiento. Conservan la información de conexión, como el identificador de suscripción y la autorización de token, en la instancia de [Key Vault](https://azure.microsoft.com/services/key-vault/) asociada el área de trabajo. Cuando se crea un almacén de datos que usa el acceso a datos basado en identidad, se usa el inicio de sesión de Azure ([token de Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)) para confirmar que tiene permiso de acceso al servicio de almacenamiento. En este escenario, no se guardan las credenciales de autenticación y solo se almacena la información de la cuenta de almacenamiento en el almacén de datos. 

Para crear almacenes de datos que usen la autenticación basada en credenciales, como con las claves de acceso o las entidades de servicio, consulte [Conexión a los servicios de almacenamiento en Azure](how-to-access-data.md).

## <a name="identity-based-data-access-in-azure-machine-learning"></a>Acceso a datos basado en identidad en Azure Machine Learning

Hay dos áreas para aplicar el acceso a datos basado en identidad en Azure Machine Learning. Especialmente, cuando se trabaja con datos confidenciales y se necesita una administración del acceso a datos más granular. 

1. Acceso a servicios de almacenamiento.
1. Entrenamiento de modelos de aprendizaje automático con datos privados.

### <a name="accessing-storage-services"></a>Acceso a servicios de almacenamiento

Puede conectarse a los servicios de almacenamiento mediante el acceso a datos basado en identidad con almacenes de datos de Azure Machine Learning o [conjuntos de datos de Azure Machine Learning](how-to-create-register-datasets.md). 

Normalmente, las credenciales de autenticación se conservan en un almacén de datos, que se usa para asegurarse de que tiene permiso de acceso al servicio de almacenamiento. Cuando estas credenciales se registran con almacenes de datos, cualquier usuario con el rol *Lector* en el área de trabajo puede recuperarlas, lo que puede suponer un problema de seguridad para algunas organizaciones. [Más información sobre el rol *Lector* del área de trabajo](how-to-assign-roles.md#default-roles). 

Cuando se usa el acceso a datos basado en identidad, Azure Machine Learning le solicita su token de Azure Active Directory para la autenticación de acceso a datos, en lugar de mantener sus credenciales en el almacén de datos. Esto permite la administración del acceso a datos en el nivel de almacenamiento y mantiene las credenciales de forma confidencial. 

Ese mismo comportamiento se aplica cuando:

* [Cree un conjunto de datos directamente a partir de las direcciones URL de almacenamiento](#use-data-in-storage). 
* Trabaje con datos de forma interactiva mediante un cuaderno de Jupyter Notebook en el equipo local o en la [instancia de proceso](concept-compute-instance.md).

> [!NOTE]
> Las credenciales almacenadas mediante la autenticación basada en credenciales incluyen: identificador de suscripción, tokens de firma de acceso compartido (SAS), claves de acceso de almacenamiento e información de la entidad de servicio como el identificador de cliente y el identificador de inquilino.

### <a name="model-training-on-private-data"></a>Entrenamiento de modelos con datos privados

Algunos escenarios de aprendizaje automático implican el entrenamiento de modelos con datos privados. En estos casos, los científicos de datos deben ejecutar los flujos de trabajo de entrenamiento sin exponer los datos de entrada confidenciales. En este escenario, se usa una identidad administrada de la instancia de proceso de entrenamiento para la autenticación del acceso a datos. De este modo, los administradores del almacenamiento pueden conceder acceso de **Lector de datos de Storage Blob** a la identidad administrada que usa la instancia de proceso de entrenamiento para ejecutar el trabajo de entrenamiento, en lugar de a los científicos de datos individuales. Obtenga información sobre cómo [configurar la identidad administrada en una instancia de proceso](how-to-create-attach-compute-cluster.md#managed-identity).


## <a name="prerequisites"></a>Requisitos previos

- Suscripción a Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

- Una cuenta de Azure Storage con un tipo de almacenamiento compatible. Se admiten los siguientes tipos de almacenamiento en la versión preliminar. 
    - [Azure Blob Storage](../storage/blobs/storage-blobs-overview.md)
    - [Azure Data Lake Gen 1](../data-lake-store/index.yml)
    - [Azure Data Lake Gen 2](../storage/blobs/data-lake-storage-introduction.md)
    - [Azure SQL Database](../azure-sql/database/sql-database-paas-overview.md)

- El [SDK de Azure Machine Learning para Python](/python/api/overview/azure/ml/install).

- Un área de trabajo de Azure Machine Learning.
  
  [Cree un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md) o use una [existente mediante el SDK para Python](how-to-manage-workspace.md#connect-to-a-workspace). 

## <a name="storage-access-permissions"></a>Permisos de acceso al almacenamiento

Para asegurarse de que se conecta de forma segura al servicio de almacenamiento de Azure, Azure Machine Learning requiere que tenga permiso para acceder al almacenamiento de datos correspondiente.

El acceso a datos basado en identidad solo admite conexiones a los siguientes servicios de almacenamiento:

* Azure Blob Storage
* Azure Data Lake Generation 1
* Azure Data Lake Generation 2
* Azure SQL Database

Para acceder a estos servicios de almacenamiento, debe tener como mínimo acceso de **Lector de datos de Storage Blob**. Obtenga más información sobre el [Lector de datos de Storage Blob](../role-based-access-control/built-in-roles.md#storage-blob-data-reader). Solo los propietarios de la cuenta de almacenamiento pueden [cambiar el nivel de acceso mediante Azure Portal](../storage/common/storage-auth-aad-rbac-portal.md).

Si va a entrenar un modelo en un destino de proceso remoto, se debe conceder a la identidad de proceso al menos el rol **Lector de datos de Storage Blob** del servicio de almacenamiento. Obtenga información sobre cómo [configurar la identidad administrada en una instancia de proceso](how-to-create-attach-compute-cluster.md#managed-identity).

## <a name="work-with-virtual-networks"></a>Trabajo con redes virtuales

De forma predeterminada, Azure Machine Learning no pueden comunicarse con una cuenta de almacenamiento que esté detrás de un firewall o dentro de una red virtual.

Las cuentas de almacenamiento se pueden configurar para permitir el acceso únicamente desde redes virtuales específicas, lo que requiere configuraciones adicionales para garantizar que los datos no se fuguen fuera de la red. Este comportamiento es el mismo para el acceso a datos basado en credenciales, consulte [qué configuraciones son necesarias y cómo aplicarlas a escenarios de redes virtuales](how-to-access-data.md#virtual-network). 

## <a name="create-and-register-datastores"></a>Creación y registro de almacenes de datos

Cuando se registra un servicio de almacenamiento de Azure como almacén de datos, se crea y registra automáticamente ese almacén de datos en un área de trabajo específica. Revise estas secciones: [Permisos de acceso al almacenamiento](#storage-access-permissions) para obtener instrucciones sobre los tipos de permisos necesarios y [Trabajo con redes virtuales](#work-with-virtual-networks) para más información sobre cómo conectarse al almacenamiento de datos detrás de redes virtuales.

Observe la ausencia de parámetros de autenticación en el código siguiente, como `sas_token`, `account_key`, `subscription_id` o el valor de `client_id` de la entidad de servicio. Esta omisión indica que Azure Machine Learning utiliza el acceso a datos basado en identidad para la autenticación. Dado que la creación de almacenes de datos suele producirse de forma interactiva en un cuaderno o mediante el Estudio de Azure Machine Learning, se usa el token de Azure Active Directory para la autenticación del acceso a datos.

> [!NOTE]
> El nombre del almacén de datos solo puede contener letras minúsculas, dígitos y caracteres de subrayado. 

### <a name="azure-blob-container"></a>Contenedor de blobs de Azure

Para registrar un almacén de datos de un contenedor de blobs de Azure, use [`register_azure_blob_container()`](/python/api/azureml-core/azureml.core.datastore%28class%29#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-).

El código siguiente crea y registra el almacén de datos `credentialless_blob` en el área de trabajo `ws` y lo asigna a la variable `blob_datastore`. Este almacén de datos accede al contenedor de blobs `my_container_name` de la cuenta de almacenamiento `my-account-name`.

```Python
# create blob datastore without credentials
blob_datastore = Datastore.register_azure_blob_container(workspace=ws,
                                                      datastore_name='credentialless_blob',
                                                      container_name='my_container_name',
                                                      account_name='my_account_name')
```

### <a name="azure-data-lake-storage-generation-1"></a>Azure Data Lake Storage Generation 1

Para un almacén de datos de Azure Data Lake Storage Generation 1 (ADLS Gen 1), use [register_azure_data_lake()](/python/api/azureml-core/azureml.core.datastore.datastore#register-azure-data-lake-workspace--datastore-name--store-name--tenant-id-none--client-id-none--client-secret-none--resource-url-none--authority-url-none--subscription-id-none--resource-group-none--overwrite-false--grant-workspace-access-false-) para registrar un almacén de datos que se conecte a un almacenamiento de Azure Data Lake Generation 1.

El código siguiente crea y registra el almacén de datos `credentialless_adls1` en el área de trabajo `workspace` y lo asigna a la variable `adls_dstore`. Este almacén de datos accede a la cuenta de almacenamiento `adls_storage` de Azure Data Lake Store.

```Python
# create adls gen1 without credentials
adls_dstore = Datastore.register_azure_data_lake(workspace = workspace,
                                                 datastore_name='credentialless_adls1',
                                                 store_name='adls_storage')

```

### <a name="azure-data-lake-storage-generation-2"></a>Azure Data Lake Storage Generation 2

Para un almacén de datos de Azure Data Lake Storage Generation 2 (ADLS Gen 2), utilice [register_azure_data_lake_gen2()](/python/api/azureml-core/azureml.core.datastore.datastore#register-azure-data-lake-gen2-workspace--datastore-name--filesystem--account-name--tenant-id--client-id--client-secret--resource-url-none--authority-url-none--protocol-none--endpoint-none--overwrite-false-) para registrar un almacén de datos que se conecte a un almacenamiento de Azure DataLake Gen 2.

El código siguiente crea y registra el almacén de datos `credentialless_adls2` en el área de trabajo `ws` y lo asigna a la variable `adls2_dstore`. Este almacén de datos accede al sistema de archivos `tabular` de la cuenta de almacenamiento `myadls2`.  

```python
# createn adls2 datastore without credentials
adls2_dstore = Datastore.register_azure_data_lake_gen2(workspace=ws, 
                                                       datastore_name='credentialless_adls2', 
                                                       filesystem='tabular', 
                                                       account_name='myadls2')
```

## <a name="use-data-in-storage"></a>Uso de los datos del almacenamiento

Los [conjuntos de datos de Azure Machine Learning](how-to-create-register-datasets.md) son el método recomendado para interactuar con los datos del almacenamiento con Azure Machine Learning. 

Los conjuntos de datos empaquetan sus datos en un objeto consumible evaluado de forma diferida para tareas de aprendizaje automático, como un curso. Además, con los conjuntos de datos, puede [descargar o montar](how-to-train-with-datasets.md#mount-vs-download) archivos de cualquier formato en un destino de proceso desde servicios de Azure Storage como Azure Blob Storage y Azure Data Lake.


**Para crear conjuntos de datos con acceso a datos basado en identidad**, tiene las siguientes opciones. Este tipo de creación de un conjunto de datos emplea el token de Azure Active Directory para la autenticación del acceso a datos. 

*  Hacer referencia a rutas de acceso de los almacenes de datos que también usan el acceso a datos basado en identidad. 
<br>En el ejemplo siguiente, `blob_datastore` se creó previamente mediante el acceso a datos basado en identidad.   

    ```python
    blob_dataset = Dataset.Tabular.from_delimited_files(blob_datastore,'test.csv') 
    ```

* Omitir la creación del almacén de datos y crear conjuntos de datos directamente desde las direcciones URL de almacenamiento. Actualmente, esta funcionalidad solo admite blobs de Azure y Azure Data Lake Storage Generation 1 y Generation 2.

    ```python
    blob_dset = Dataset.File.from_files('https://myblob.blob.core.windows.net/may/keras-mnist-fashion/')
    ```

**Sin embargo, cuando se envía un trabajo de entrenamiento que consume un conjunto de datos creado con acceso a datos basado en identidad**, se utiliza la identidad administrada de la instancia de proceso de entrenamiento para la autenticación del acceso a datos, en lugar de su token de Azure Active Directory. En este escenario, asegúrese de que se conceda a la identidad administrada de la instancia de proceso al menos el rol **Lector de datos de Storage Blob** del servicio de almacenamiento. Obtenga información sobre cómo [configurar la identidad administrada en una instancia de proceso](how-to-create-attach-compute-cluster.md#managed-identity). 

## <a name="next-steps"></a>Pasos siguientes

* [Creación de un conjunto de datos de Azure Machine Learning](how-to-create-register-datasets.md).
* [Entrenamiento con conjuntos de datos](how-to-train-with-datasets.md).
* [Conexión a los servicios de almacenamiento en Azure](how-to-access-data.md).
