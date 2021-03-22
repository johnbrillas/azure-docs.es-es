---
title: Exportación de los datos de costos con una clave de SAS de la cuenta de Azure Storage
description: Este artículo ayuda a los asociados a crear una clave de SAS y configurar las exportaciones de Cost Management.
author: bandersmsft
ms.author: banders
ms.date: 03/08/2021
ms.topic: how-to
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.openlocfilehash: 16302a6bcc3bf41432500d2fdaa4ec27c28dd5b3
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102509638"
---
# <a name="export-cost-data-with-an-azure-storage-account-sas-key"></a>Exportación de los datos de costos con una clave de SAS de la cuenta de Azure Storage

La siguiente información solo se aplica a los asociados de Microsoft.

A menudo, los asociados no tienen sus propias suscripciones de Azure en el inquilino asociado a su propio Microsoft Partner Agreement. Los asociados con un plan de Microsoft Partner Agreement que son administradores globales de su cuenta de facturación pueden exportar y copiar los datos de costos en una cuenta de almacenamiento de otro inquilino mediante una clave de firma de acceso compartido (SAS). En otras palabras, una cuenta de almacenamiento con una clave de SAS permite que el asociado use una cuenta de almacenamiento que esté fuera de su acuerdo de socio comercial para recibir la información exportada. Este artículo ayuda a los asociados a crear una clave de SAS y configurar las exportaciones de Cost Management.

## <a name="requirements"></a>Requisitos

- Debe ser un asociado con un acuerdo Microsoft Partner Agreement y tener clientes en el plan de Azure.
- Debe ser administrador global de la cuenta de facturación de la organización del asociado.
- Debe tener acceso para configurar una cuenta de almacenamiento que se encuentre en otro inquilino de la organización del asociado. Es responsable de mantener los permisos y el acceso a los datos cuando exporta los datos a su cuenta de almacenamiento.

## <a name="configure-azure-storage-with-a-sas-key"></a>Configuración de Azure Storage con una clave de SAS

Obtenga un token de SAS de la cuenta de almacenamiento o cree uno mediante Azure Portal. Para crear uno en Azure Portal, use los pasos siguientes. Para más información sobre las claves de SAS, consulte [Otorgar acceso limitado a recursos de Azure Storage con firmas de acceso compartido (SAS)](../../storage/common/storage-sas-overview.md).

1. Vaya a la cuenta de almacenamiento en Azure Portal.
    - Si la cuenta tiene acceso a varios inquilinos, cambie los directorios para acceder a la cuenta de almacenamiento. Seleccione la cuenta en la esquina superior derecha de Azure Portal y, a continuación, seleccione **Cambiar directorios**.
    - Es posible que tenga que iniciar sesión en Azure Portal con la cuenta de inquilino correspondiente para acceder a la cuenta de almacenamiento.
1. En el menú izquierdo, seleccione **Firma de acceso compartido**.  
    :::image type="content" source="./media/export-cost-data-storage-account-sas-key/storage-shared-access-signature.png" alt-text="Captura de pantalla que muestra una firma de acceso compartido de Azure Storage configurada." lightbox="./media/export-cost-data-storage-account-sas-key/storage-shared-access-signature.png" :::
1. Configure el token con la misma configuración que se identifica en la imagen anterior.
    1. Seleccione **Blob** en _Servicios permitidos_.
    1. Seleccione **Servicio**, **Contenedor** y **Objeto** en _Tipos de recursos permitidos_.
    1. Seleccione **Lectura**, **Escritura**, **Eliminación**, **Lista**, **Adición** y **Creación** en _Permisos permitidos_.
    1. Elija las fechas de expiración. Asegúrese de actualizar el token de SAS de exportación antes de que expire. Cuanto mayor sea el período de tiempo que configure antes de la expiración, más tiempo se ejecutará la exportación antes de necesitar un nuevo token de SAS.
1. Seleccione **Solo HTTPS** en _Protocolos permitidos_.
1. Seleccione **Basic** (Básico) en _Preferred routing tier_ (Nivel de enrutamiento preferido).
1. Seleccione **key1** en _Clave de firma_. Si rota o actualiza la clave que se usa para firmar el token de SAS, deberá volver a generar un nuevo token de SAS para la exportación.
1. Seleccione **Generar la cadena de conexión y SAS**.
    El valor del **token de SAS** que se muestra es el token que necesita al configurar las exportaciones.

## <a name="create-a-new-export-with-a-sas-token"></a>Creación de una nueva exportación con un token de SAS

Vaya a **Exportaciones** en el ámbito de la cuenta de facturación y cree una nueva exportación mediante los pasos siguientes.

1. Seleccione **Crear**.
1. Configure los detalles de la exportación como lo haría para una exportación normal. Puede configurar la exportación para que use un directorio o un contenedor existente, o puede especificar un nuevo directorio o contenedor y las exportaciones lo crearán automáticamente.
1. Al configurar el almacenamiento, seleccione **Usar token de SAS**.  
    :::image type="content" source="./media/export-cost-data-storage-account-sas-key/new-export.png" alt-text="Captura de pantalla que muestra la nueva exportación donde se selecciona el token de SAS." lightbox="./media/export-cost-data-storage-account-sas-key/new-export.png" :::
1. Escriba el nombre de la cuenta de almacenamiento y pegue el token de SAS.
1. Especifique un contenedor o un directorio existente o identifique los nuevos que se van a crear.
1. Seleccione **Crear**.

La exportación basada en tokens de SAS solo funciona mientras el token sigue siendo válido. Restablezca el token antes de que expire el actual o la exportación dejará de funcionar. Dado que el token proporciona acceso a la cuenta de almacenamiento, proteja el token con tanto cuidado como lo haría con cualquier otra información confidencial. Es responsable de mantener los permisos y el acceso a los datos cuando exporta los datos a su cuenta de almacenamiento.

## <a name="troubleshoot-exports-using-sas-tokens"></a>Solución de problemas de exportaciones mediante tokens de SAS

A continuación se muestran algunos problemas comunes que pueden producirse al configurar o usar exportaciones basadas en tokens de SAS.

- No se ve la opción de la clave de SAS en Azure Portal.
  - Compruebe que es un asociado que tiene un acuerdo Microsoft Partner Agreement y que tiene permisos de administrador global para la cuenta de facturación. Son las únicas personas que pueden exportar con una clave de SAS.

- Aparece el siguiente mensaje de error al intentar configurar la exportación:

    **Asegúrese de que el token de SAS es válido para Blob service, es válido para el contenedor y los tipos de recurso de objeto y tiene los permisos: adición, creación, lectura, escritura y eliminación. (Código de error del servicio de almacenamiento: AuthorizationResourceTypeMismatch)**

    - Asegúrese de que está configurando y generando la clave de SAS correctamente en Azure Storage.

- No se puede ver la clave de SAS completa después de crear una exportación.
  - No ver la clave es el comportamiento esperado. Una vez configurada la exportación mediante SAS, la clave se oculta por motivos de seguridad.

- No se puede acceder a la cuenta de almacenamiento desde el inquilino en el que está configurada la exportación.
  - Es el comportamiento esperado. Si la cuenta de almacenamiento se encuentra en otro inquilino, primero debe ir a ese inquilino en Azure Portal para encontrar la cuenta de almacenamiento.

- La exportación no se realiza debido a un error relacionado con el token de SAS.
  - La exportación solo funciona mientras el token de SAS sigue siendo válido. Cree una nueva clave y ejecute la exportación.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la exportación de datos de Cost Management, consulte [Tutorial: Creación y administración de datos exportados](tutorial-export-acm-data.md).
- Para obtener información sobre la exportación de grandes cantidades de datos de uso, consulte [Recuperación de conjuntos de datos de costos de gran tamaño con exportaciones](ingest-azure-usage-at-scale.md).
