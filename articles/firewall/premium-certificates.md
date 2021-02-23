---
title: Certificados de la versión preliminar de Azure Firewall Prémium
description: Para configurar correctamente la inspección de TLS en la versión preliminar de Azure Firewall Prémium, debe configurar e instalar certificados de la entidad de certificación intermedia.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 02/16/2021
ms.author: victorh
ms.openlocfilehash: 3914a82903c293cf1a8306b5ecc1f542fef83e72
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549514"
---
# <a name="azure-firewall-premium-preview-certificates"></a>Certificados de la versión preliminar de Azure Firewall Prémium 

> [!IMPORTANT]
> Azure Firewall Prémium se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

 Para configurar correctamente la inspección de TLS de la versión preliminar de Azure Firewall Prémium, debe proporcionar un certificado de la entidad de certificación intermedia válido y depositarlo en Azure Key Vault.

## <a name="certificates-used-by-azure-firewall-premium-preview"></a>Certificados usados por la versión preliminar de Azure Firewall Prémium

Hay tres tipos de certificados que se usan en una implementación típica:

- **Certificado de la entidad de certificación intermedia (certificado de entidad de certificación)**

   Una entidad de certificación (CA) es una organización de confianza para firmar certificados digitales. Una entidad de certificación solicita un certificado para comprobar la identidad y la legitimidad de una empresa o individuo. Si la comprobación se realiza correctamente, la entidad de certificación emite un certificado firmado. Cuando el servidor presenta el certificado al cliente (por ejemplo, el explorador web) durante un protocolo de enlace SSL/TLS, el cliente intenta comparar la firma con una lista de firmantes *válidos conocidos*. Los exploradores web incluyen normalmente listas de entidades de certificación en las que confían implícitamente para identificar los hosts. Si la entidad no está en la lista, como sucede con algunos sitios que firman sus propios certificados, el explorador avisa al usuario de que el certificado no está firmado por una entidad reconocida y le pregunta si quiere continuar con la comunicación con el sitio no comprobado.

- **Certificado de servidor (certificado de sitio web)**

   Un certificado asociado con el nombre de dominio específico. Si un sitio web tiene un certificado válido, significa que una entidad de certificación ha realizado los pasos para comprobar que la dirección web pertenece realmente a esa organización. Cuando escribe una dirección URL o sigue un vínculo a un sitio web seguro, el explorador comprueba que el certificado tenga las siguientes características:
   - La dirección del sitio web coincide con la dirección del certificado.
   - El certificado está firmado por una entidad de certificación que el explorador reconoce como una entidad de *confianza*.
   
   En ocasiones, los usuarios pueden conectarse a un servidor con un certificado que no sea de confianza. Azure Firewall descartará la conexión como si el servidor la hubiera terminado.

- **Certificado de entidad de certificación raíz (certificado raíz)**

   Una entidad de certificación puede emitir varios certificados como una estructura de árbol. Un certificado raíz es el certificado de nivel superior del árbol.

La versión preliminar de Azure Firewall Prémium puede interceptar el tráfico HTTP/S saliente y generar automáticamente un certificado de servidor para `www.website.com`. Este certificado se genera mediante el certificado de la entidad de certificación intermedia que proporcione. Para que este procedimiento funcione, el explorador de usuario final y las aplicaciones cliente deben confiar en el certificado de la entidad de certificación raíz de la organización o en el certificado de la entidad de certificación intermedia. 

:::image type="content" source="media/premium-certificates/certificate-process.png" alt-text="Proceso de certificado":::

## <a name="intermediate-ca-certificate-requirements"></a>Requisitos de los certificados de la entidad de certificación intermedia

Asegúrese de que el certificado de la entidad de certificación cumpla los requisitos siguientes:

- Cuando se implementa como un secreto de Key Vault, debe usar PFX sin contraseña (Pkcs12) con un certificado y una clave privada.

- Debe ser un único certificado y no debe incluir toda la cadena de certificados.  

- Debe ser válido durante un año.  

- Debe tener una clave privada RSA con un tamaño mínimo de 4096 bytes.  

- Debe tener la extensión `KeyUsage` marcada como Crítica con `KeyCertSign` (RFC 5280; 4.2.1.3 Uso de claves).

- Debe tener la extensión `BasicContraints` marcada como Crítica (RFC 5280; 4.2.1.9 Restricciones básicas).  

- La marca `CA` debe estar establecida en TRUE.

- La longitud de la ruta de acceso debe ser mayor o igual que uno.

## <a name="azure-key-vault"></a>Azure Key Vault

[Azure Key Vault](../key-vault/general/overview.md) es un almacén de secretos administrado por la plataforma que puede usar para proteger los secretos, las claves y los certificados TLS/SSL. Azure Firewall Prémium admite la integración con Key Vault en el caso de los certificados de servidor que están conectados a una directiva de firewall.
 
Para configurar el almacén de claves:

- Debe importar un certificado existente con su par de claves en el almacén de claves. 
- De manera alternativa, también puede usar un secreto de almacén de claves que se almacene como un archivo PFX codificado en Base 64 sin contraseña.  Un archivo PFX es un certificado digital que contiene la clave privada y la clave pública.
- Se recomienda usar una importación de certificados de entidad de certificación, ya que le permite configurar una alerta basada en la fecha de expiración del certificado.
- Después de haber importado un certificado o un secreto, debe definir directivas de acceso en el almacén de claves para permitir que la identidad que se va a conceder obtenga acceso al certificado o al secreto.
- El certificado de entidad de certificación proporcionado debe ser de confianza para la carga de trabajo de Azure. Asegúrese de que se ha implementado correctamente.

Puede crear o volver a usar una identidad administrada asignada por el usuario existente, que Azure Firewall usará para recuperar los certificados de Key Vault en su nombre. Para más información, consulte [¿Qué son las identidades administradas en los recursos de Azure?](../active-directory/managed-identities-azure-resources/overview.md). 

## <a name="configure-a-certificate-in-your-policy"></a>Configuración de un certificado en la directiva

Para configurar un certificado de entidad de certificación en la directiva de firewall prémium, seleccione la directiva y, luego, elija **TLS inspection (preview)** (Inspección de TLS [versión preliminar]). En la página **TLS inspection**(Inspección de TLS), seleccione **Habilitado**. Después, seleccione el certificado de entidad de certificación en Azure Key Vault, como se muestra en la ilustración siguiente:

:::image type="content" source="media/premium-certificates/tls-inspection.png" alt-text="Diagrama de información general de Azure Firewall Prémium":::
 
> [!IMPORTANT]
> Para ver y configurar un certificado desde Azure Portal, debe agregar su cuenta de usuario de Azure a la directiva de acceso de Key Vault. Proporcione a su cuenta de usuario los permisos **Obtener** y **Enumerar** en **Permisos de secretos**.
   :::image type="content" source="media/premium-certificates/secret-permissions.png" alt-text="Directiva de acceso de Azure Key Vault":::


## <a name="troubleshooting"></a>Solución de problemas

Si el certificado de entidad de certificación es válido, pero no puede acceder a los nombres de dominio completos o a las direcciones URL bajo la inspección de TLS, compruebe los siguientes elementos:

- El certificado de servidor web debe ser válido.  

- El certificado de entidad de certificación raíz debe estar instalado en el sistema operativo cliente.  

- El explorador o el cliente HTTPS deben contener un certificado raíz válido. Firefox y otros exploradores pueden tener directivas de certificación especiales.  

- El tipo de destino de la dirección URL en la regla de aplicación debe cubrir la ruta de acceso correcta y cualquier otro hipervínculo insertado en la página HTML de destino. Puede usar caracteres comodín para facilitar la cobertura de la ruta de acceso necesaria de la dirección URL completa.  


## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre las características de Azure Firewall Prémium](premium-features.md)
