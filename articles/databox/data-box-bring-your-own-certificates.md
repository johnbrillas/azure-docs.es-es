---
title: Uso de sus propios certificados con dispositivos Azure Data Box o Azure Data Box Heavy
description: Cómo usar sus propios certificados para obtener acceso a la interfaz de usuario web local y a la instancia de Blob Storage en el dispositivo Data Box o Data Box Heavy.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 12/11/2020
ms.author: alkohli
ms.openlocfilehash: 1836ed57305fd7e168961eb81670b56d4ce296cd
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100545180"
---
# <a name="use-your-own-certificates-with-data-box-and-data-box-heavy-devices"></a>Uso de sus propios certificados con los dispositivos Data Box y Data Box Heavy

Durante el procesamiento de pedidos, se generan certificados autofirmados para obtener acceso a la interfaz de usuario web local y a la instancia de Blob Storage de un dispositivo Data Box o Data Box Heavy. Si prefiere comunicarse con el dispositivo a través de un canal de confianza, puede utilizar sus propios certificados.

En este artículo se describe cómo instalar sus propios certificados y cómo revertir a los certificados predeterminados antes de devolver el dispositivo al centro de datos. Asimismo, también se proporciona un resumen de los requisitos de certificado.

## <a name="about-certificates"></a>Acerca de los certificados

Los certificados proporcionan un vínculo entre una **clave pública** y una entidad (como una nombre de dominio) que ha sido **firmada** (verificada) por un tercero de confianza como, por ejemplo, una **entidad de certificación**.  Proporcionan una manera cómoda de distribuir claves de cifrado públicas de confianza. De esta manera, los certificados aseguran que la comunicación es de confianza y que envía información cifrada al servidor adecuado.

Cuando el dispositivo Data Box está configurado desde el principio, se generan automáticamente certificados autofirmados. Como alternativa, puede aportar sus propios certificados. Hay instrucciones que debe seguir si tiene previsto traer sus propios certificados.

En un dispositivo Data Box o Data Box Heavy, se usan dos tipos de certificados de punto de conexión:

- Certificado de Blob Storage
- Certificado de UI local

### <a name="certificate-requirements"></a>Requisitos de certificados

Los certificados deben cumplir los siguientes requisitos:

- El certificado del punto de conexión debe tener un formato `.pfx` y una clave privada que se pueda exportar.
- Puede usar un certificado individual para cada punto de conexión, un certificado de varios dominios para varios puntos de conexión o un certificado de punto de conexión de comodín.
- Las propiedades de los certificados de punto de conexión son similares a las de un certificado SSL típico.
- En el equipo cliente será necesario un certificado correspondiente en formato DER (extensión de nombre de archivo `.cer`).
- Después de cargar este certificado de interfaz de usuario local, deberá reiniciar el explorador y borrar la memoria caché. Consulte las instrucciones específicas para el explorador.
- Estos certificados deben cambiarse si cambian el nombre del dispositivo o los nombres de dominio DNS.
- Al crear un certificado de punto de conexión, use la tabla siguiente:

  |Tipo |Nombre del firmante (SN)  |Nombre alternativo del firmante (SAN)  |Ejemplo de nombre de firmante |
  |---------|---------|---------|---------|
  |Interfaz de usuario local| `<DeviceName>.<DNSdomain>`|`<DeviceName>.<DNSdomain>`| `mydevice1.microsoftdatabox.com` |
  |Blob Storage|`*.blob.<DeviceName>.<DNSdomain>`|`*.blob.< DeviceName>.<DNSdomain>`|`*.blob.mydevice1.microsoftdatabox.com` |
  |Certificado único de varios SAN|`<DeviceName>.<DNSdomain>`|`<DeviceName>.<DNSdomain>`<br>`*.blob.<DeviceName>.<DNSdomain>`|`mydevice1.microsoftdatabox.com` |

Para obtener más información, consulte los [Requisitos de certificado](../../articles/databox-online/azure-stack-edge-gpu-certificate-requirements.md).

## <a name="add-certificates-to-device"></a>Adición de certificados al dispositivo

Puede usar sus propios certificados para obtener acceso a la interfaz de usuario web local y a la instancia de Blob Storage.

> [!IMPORTANT]
> Si el nombre del dispositivo o el dominio DNS cambia, debe crear certificados nuevos. Los certificados de cliente y los certificados de dispositivo deben actualizarse con el nombre de dispositivo y dominio DNS nuevos.

Siga estos pasos para agregar sus propios certificados al dispositivo:

1. Vaya a **Administrar** > **Certificados** .

   La opción **Nombre** muestra el nombre del dispositivo. El **dominio DNS** muestra el nombre de dominio del servidor DNS.

   Asimismo, en la parte inferior de la pantalla se muestran los certificados que se están usando. Si tiene un dispositivo nuevo, verá los certificados autofirmados que se generaron durante el procesamiento del pedido.

   ![Página de certificados para un dispositivo Data Box](media/data-box-bring-your-own-certificates/certificates-manage-certs.png)

2. Si tiene que cambiar el **nombre** (nombre del dispositivo) o el **dominio DNS** (el dominio del servidor DNS del dispositivo), hágalo ahora antes de agregar el certificado. Luego, seleccione **Aplicar**.

   Tenga en cuenta que el certificado debe cambiarse si cambian el nombre del dispositivo o los nombres de dominio DNS.

   ![Aplicación de un nuevo nombre de dispositivo y dominio DNS para un dispositivo Data Box](media/data-box-bring-your-own-certificates/certificates-device-name-dns.png)

3. Para agregar un certificado, seleccione **Agregar certificado** para abrir el panel **Agregar certificado**. A continuación, seleccione el **Tipo de certificado**, ya sea **Blob Storage** o **Interfaz de usuario web local**.

   ![Panel para agregar certificados en la página Certificados de un dispositivo Data Box](media/data-box-bring-your-own-certificates/certificates-add-certificate-cert-type.png)

4. Elija el archivo de certificado (en formato `.pfx`) y escriba la contraseña que se estableció cuando se exportó el certificado. A continuación, seleccione **Validar y agregar**.

   ![Configuración para agregar un certificado de punto de conexión de blob a un dispositivo Data Box](media/data-box-bring-your-own-certificates/certificates-add-blob-cert.png)

   Una vez que el certificado se ha agregado correctamente, la pantalla **Certificados** muestra la huella digital del nuevo certificado. Así pues, el estado del certificado será **Válido**.

   ![Un nuevo certificado válido se ha agregado correctamente](media/data-box-bring-your-own-certificates/certificates-view-new-certificate.png)

5. Para ver los detalles del certificado, selecciónelo y haga clic en el nombre del mismo. Recuerde que el certificado expirará en un año.

   ![Ver los detalles del certificado de un dispositivo Data Box](media/data-box-bring-your-own-certificates/certificates-cert-details.png)

   <!--If you changed the local web UI certificate, you'll see the following error. This error will go away when you install the new certificate on the client computer.

   ![Error after a new Local web UI certificate is added to a Data Box device](media/data-box-bring-your-own-certificates/certificates-unable-to-communicate-error.png) TEST. RESTORE IF ERROR IS REPRODUCED.-->

6. Si ha cambiado el certificado de la interfaz de usuario web local, deberá reiniciar el explorador y, a continuación, la interfaz de usuario web local. Tenga en cuenta que este paso es necesario para evitar problemas de caché SSL.

  <!-- TESTING THIS - The communication error should be gone from the **Certificates** screen.-->

7. Instale el nuevo certificado en el equipo cliente que está usando para obtener acceso a la interfaz de usuario web local. Para obtener instrucciones, consulte más adelante [Importar certificados en el cliente](#import-certificates-to-client).


## <a name="import-certificates-to-client"></a>Importar certificados en el cliente

Después de agregar un certificado al dispositivo Data Box, debe importar el certificado en el equipo cliente que use para obtener acceso al dispositivo. El certificado se importará en el almacén de la entidad de certificación raíz de confianza del equipo local.

Para importar un certificado en un cliente de Windows, siga estos pasos:

1. En el Explorador de archivos, haga clic con el botón derecho en el archivo del certificado (con formato `.cer`) y seleccione **Instalar certificado**. Esta acción inicia el Asistente para importar certificados.

    ![Importación del certificado 1](media/data-box-bring-your-own-certificates/import-cert-01.png)

2. Para **Ubicación de almacén**, seleccione **Máquina local** y, después, **Siguiente**.

    ![En el Asistente para importación de certificados, seleccione Equipo local como ubicación del almacén.](media/data-box-bring-your-own-certificates/import-cert-02.png)

3. Seleccione **Colocar todos los certificados en el siguiente almacén**, haga clic en **Entidad de certificación raíz de confianza** y, a continuación, en **Siguiente**.

   ![Seleccionar el almacén de entidades de certificación raíz de confianza en el Asistente para la importación de certificados](media/data-box-bring-your-own-certificates/import-cert-03.png)

4. Compruebe la configuración y seleccione **Finalizar**. Un mensaje le indicará que la importación se realizó correctamente.

   ![Revise la configuración del certificado y finalice el Asistente para la importación de certificados](media/data-box-bring-your-own-certificates/import-cert-04.png)

## <a name="revert-to-default-certificates"></a>Revertir a los certificados predeterminados

Antes de devolver el dispositivo al centro de datos de Azure, debe revertir a los certificados originales que se generaron durante el procesamiento del pedido.

Para revertir a los certificados generados durante el procesamiento del pedido, siga estos pasos:

1. Vaya a **Administrar** > **Certificados** y, a continuación, seleccione **Revertir certificados**.

   Tenga en cuenta que la operación para la reversión de los certificados debe volver a usar los certificados autofirmados que se generaron durante el procesamiento del pedido. Asimismo, sus propios certificados se quitan del dispositivo.

   ![Opción para revertir certificados en la sección Administrar certificados de un dispositivo Data Box](media/data-box-bring-your-own-certificates/certificates-revert-certificates.png)

2. Una vez completada correctamente la reversión del certificado, vaya a **Apagar o reiniciar** y seleccione **Reiniciar**. Tenga en cuenta que este paso es necesario para evitar problemas de caché SSL.

   ![Apague o reinicie la interfaz de usuario web local después de revertir los certificados en un dispositivo Data Box](media/data-box-bring-your-own-certificates/certificates-restart-ui.png)

   Espere unos minutos y vuelva a iniciar sesión en la interfaz de usuario web local.
