---
title: Configuración de Azure Attestation con Azure Portal
description: Instalación y configuración de un proveedor de atestación mediante Azure Portal.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 3ab1e6011a1c127c9ac5a2c7652a4bf458372e1e
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98733944"
---
# <a name="quickstart-set-up-azure-attestation-with-azure-portal"></a>Inicio rápido: Configuración de Azure Attestation con Azure Portal

Siga los pasos que se indican a continuación para administrar un proveedor de atestación mediante Azure Portal.

## <a name="attestation-provider"></a>Proveedor de atestación

### <a name="create-an-attestation-provider"></a>Creación de un proveedor de atestación

#### <a name="to-configure-the-provider-with-unsigned-policies"></a>Para configurar el proveedor con directivas sin firmar

1.  En el menú de Azure Portal o en la página principal, seleccione **Crear un recurso**.
2.  En el cuadro de búsqueda, escriba **attestation**.
3.  En la lista de resultados, elija **Microsoft Azure Attestation**.
4.  En la página Microsoft Azure Attestation, elija **Create** (Crear).
5.  En la página Create attestation provider (Crear proveedor de atestación), proporcione las siguientes entradas:
    
    **Suscripción**: Elija una suscripción
    
    **Resource Group** (Grupo de recursos): seleccione un grupo de recursos existente o elija **Create new** (Crear nuevo) y escriba un nombre de grupo de recursos.
    
    **Nombre**: se requiere un nombre único.

    **Location** (Ubicación): elija una ubicación. 
    
    **Policy signer certificates file** (Archivo de firmantes de directivas de atestación): no cargue el archivo de firmantes de directivas de atestación para configurar el proveedor con directivas sin firmar. 
6.  Después de proporcionar las entradas necesarias, haga clic en **Review+Create** (Revisar y crear).
7.  Corrija los problemas de validación, si los hay, y haga clic en **Create** (Crear).

#### <a name="to-configure-the-provider-with-signed-policies"></a>Para configurar el proveedor con directivas firmadas

1.  En el menú de Azure Portal o en la página principal, seleccione **Crear un recurso**.
2.  En el cuadro de búsqueda, escriba **attestation**.
3.  En la lista de resultados, elija **Microsoft Azure Attestation**.
4.  En la página Microsoft Azure Attestation, elija **Create** (Crear).
5.  En la página Create attestation provider (Crear proveedor de atestación), proporcione la siguiente información:
    
    a. **Suscripción**: Elija una suscripción
    
    b. **Resource Group** (Grupo de recursos): seleccione un grupo de recursos existente o elija **Create new** (Crear nuevo) y escriba un nombre de grupo de recursos.
    
    c. **Nombre**: se requiere un nombre único.

    d. **Location** (Ubicación): elija una ubicación. 
    
    e. **Policy signer certificates file** (Archivo de firmantes de directivas de atestación): Para configurar el proveedor de atestación con certificados de firma de directivas, cargue el archivo de certificados. Puede consultar ejemplos [aquí](./policy-signer-examples.md). 
6.  Después de proporcionar las entradas necesarias, haga clic en **Review+Create** (Revisar y crear).
7.  Corrija los problemas de validación, si los hay, y haga clic en **Create** (Crear).

### <a name="view-attestation-provider"></a>Visualización del proveedor de atestación

1.  En el menú de Azure Portal o en la página principal, seleccione **Todos los recursos**.
2.  En el cuadro filtro, escriba el nombre del proveedor de atestación y selecciónelo.

### <a name="delete-attestation-provider"></a>Eliminación del proveedor de atestación

1.  En el menú de Azure Portal o en la página principal, seleccione **Todos los recursos**.
2.  En el cuadro filtro, escriba el nombre del proveedor de atestación.
3.  Seleccione la casilla y haga clic en **Eliminar**.
4.  Escriba Sí y haga clic en **Eliminar** [o]
1.  En el menú de Azure Portal o en la página principal, seleccione **Todos los recursos**.
2.  En el cuadro filtro, escriba el nombre del proveedor de atestación.
3.  Seleccione el proveedor de atestación y vaya a la página de información general.
4.  Haga clic en **Delete** (Eliminar) en el menú superior y haga clic en **Yes** (Sí).


## <a name="attestation-policy-signers"></a>Firmantes de directivas de atestación

### <a name="view-policy-signer-certificates"></a>Visualización de los firmantes de directivas de atestación

1.  En el menú de Azure Portal o en la página principal, seleccione **Todos los recursos**.
2.  En el cuadro filtro, escriba el nombre del proveedor de atestación.
3.  Seleccione el proveedor de atestación y vaya a la página de información general.
4.  Haga clic en **Policy signer certificates** (Firmantes de directivas de atestación) en el menú de recursos del lado izquierdo o en el panel inferior.
5.  Haga clic en **Download policy signer certificates** (Descargar firmantes de directivas de atestación) (el botón estará deshabilitado para los proveedores de atestación creados sin el requisito de firma de directivas).
6.  El archivo de texto descargado tendrá todos los certificados en formato JWS.
a.  Compruebe el recuento de certificados y los certificados descargados.

### <a name="add-policy-signer-certificate"></a>Adición de un firmante de directivas de atestación

1.  En el menú de Azure Portal o en la página principal, seleccione **Todos los recursos**.
2.  En el cuadro filtro, escriba el nombre del proveedor de atestación.
3.  Seleccione el proveedor de atestación y vaya a la página de información general.
4.  Haga clic en **Policy signer certificates** (Firmantes de directivas de atestación) en el menú de recursos del lado izquierdo o en el panel inferior.
5.  Haga clic en **Add** (Agregar) en el menú superior (el botón estará deshabilitado para los proveedores de atestación creados sin el requisito de firma de directivas).
6.  Cargue el archivo del firmante de directivas de atestación y haga clic en **Add** (Agregar). Puede consultar ejemplos [aquí](./policy-signer-examples.md).

### <a name="delete-policy-signer-certificate"></a>Eliminación de un firmante de directivas de atestación

1.  En el menú de Azure Portal o en la página principal, seleccione **Todos los recursos**.
2.  En el cuadro filtro, escriba el nombre del proveedor de atestación.
3.  Seleccione el proveedor de atestación y vaya a la página de información general.
4.  Haga clic en **Policy signer certificates** (Firmantes de directivas de atestación) en el menú de recursos del lado izquierdo o en el panel inferior.
5.  Haga clic en **Delete** (Eliminar) en el menú superior (el botón estará deshabilitado para los proveedores de atestación creados sin el requisito de firma de directivas).
6.  Cargue el archivo del firmante de directivas de atestación y haga clic en **Delete** (Eliminar). Puede consultar ejemplos [aquí](./policy-signer-examples.md). 

## <a name="attestation-policy"></a>Directiva de atestación

### <a name="view-attestation-policy"></a>Visualización de la directiva de atestación

1.  En el menú de Azure Portal o en la página principal, seleccione **Todos los recursos**.
2.  En el cuadro filtro, escriba el nombre del proveedor de atestación.
3.  Seleccione el proveedor de atestación y vaya a la página de información general.
4.  Haga clic en **Policy** (Directiva) en el menú de recursos del lado izquierdo o en el panel inferior.
5.  Seleccione el valor que prefiera para **Attestation Type** (Tipo de atestación) y visualice el campo **Current policy** (Directiva actual).

### <a name="configure-attestation-policy"></a>Configuración de la directiva de atestación

#### <a name="when-attestation-provider-is-created-without-policy-signing-requirement"></a>Cuando el proveedor de atestación se crea sin requisito de firma de directivas

##### <a name="upload-policy-in-jwt-format"></a>Carga de la directiva en formato JWT

1.  En el menú de Azure Portal o en la página principal, seleccione **Todos los recursos**.
2.  En el cuadro filtro, escriba el nombre del proveedor de atestación.
3.  Seleccione el proveedor de atestación y vaya a la página de información general.
4.  Haga clic en **Policy** (Directiva) en el menú de recursos del lado izquierdo o en el panel inferior.
5.  Haga clic en **Configure** (Configurar) en el menú de la parte superior.
6.  Cuando el proveedor de atestación se crea sin requisito de firma de directivas, el usuario puede cargar una directiva en formato **JWT** o formato de **Texto**.
7.  Para **Policy Format** (Formato de la directiva), seleccione **JWT**.
8.  Cargue el archivo de directiva con el contenido de la directiva en formato **JWT sin firmar o firmado** y haga clic en **Save** (Guardar). Puede consultar ejemplos [aquí](./policy-examples.md).
    
    En la opción de carga de archivo, la vista previa de la directiva se mostrará en formato de texto y la vista previa de la directiva no se puede modificar.

7.  Haga clic en **Refresh** (Actualizar) en el menú superior para ver la directiva configurada.

##### <a name="upload-policy-in-text-format"></a>Carga de la directiva en formato de texto

1.  En el menú de Azure Portal o en la página principal, seleccione **Todos los recursos**.
2.  En el cuadro filtro, escriba el nombre del proveedor de atestación.
3.  Seleccione el proveedor de atestación y vaya a la página de información general.
4.  Haga clic en **Policy** (Directiva) en el menú de recursos del lado izquierdo o en el panel inferior.
5.  Haga clic en **Configure** (Configurar) en el menú de la parte superior.
6.  Cuando el proveedor de atestación se crea sin requisito de firma de directivas, el usuario puede cargar una directiva en formato **JWT** o formato de **Texto**.
7.  Para **Policy Format** (Formato de la directiva), seleccione **Text** (Texto).
8.  Cargue el archivo de directiva con el contenido en formato de **texto** o escriba el contenido de la directiva en el área de texto y haga clic en **Save** (Guardar). Puede consultar ejemplos [aquí](./policy-examples.md).

    En la opción de carga de archivo, la vista previa de la directiva se mostrará en formato de texto y la vista previa de la directiva no se puede modificar.

8.  Haga clic en **Refresh** (Actualizar) para ver la directiva configurada.

#### <a name="when-attestation-provider-is-created-with-policy-signing-requirement"></a>Cuando el proveedor de atestación se crea con requisito de firma de directivas

##### <a name="upload-policy-in-jwt-format"></a>Carga de la directiva en formato JWT

1.  En el menú de Azure Portal o en la página principal, seleccione **Todos los recursos**.
2.  En el cuadro filtro, escriba el nombre del proveedor de atestación.
3.  Seleccione el proveedor de atestación y vaya a la página de información general.
4.  Haga clic en **Policy** (Directiva) en el menú de recursos del lado izquierdo o en el panel inferior.
5.  Haga clic en **Configure** (Configurar) en el menú de la parte superior.
6.  Cuando el proveedor de atestación se crea con el requisito de firma de directivas, el usuario solo puede cargar la directiva en **formato JWT firmado**.
7.  Cargue el archivo de directiva en **formato JWT firmado** y haga clic en **Save** (Guardar). Puede consultar ejemplos [aquí](./policy-examples.md).

    En la opción de carga de archivo, la vista previa de la directiva se mostrará en formato de texto y la vista previa de la directiva no se puede modificar.
    
8.  Haga clic en **Refresh** (Actualizar) para ver la directiva configurada.

