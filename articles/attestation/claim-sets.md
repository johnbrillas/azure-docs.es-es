---
title: Conjuntos de notificaciones de Azure Attestation
description: Los conjuntos de notificaciones de Azure Attestation.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: eb08bb262806cb662822a75898196546a5c1058e
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98762549"
---
# <a name="claim-sets"></a>Conjuntos de notificaciones

Las notificaciones generadas en el proceso de atestación de enclaves mediante Microsoft Azure Attestation se pueden dividir en las siguientes categorías:

- **Notificaciones entrantes**: las notificaciones generadas por Microsoft Azure Attestation después de analizar la evidencia de atestación; las pueden usar los autores de directivas para definir las reglas de autorización en una directiva personalizada.

- **Notificaciones salientes**: notificaciones generadas por Azure Attestation; contiene todas las notificaciones que terminan en el token de atestación.

- **Notificaciones de propiedad**: notificaciones creadas como salida por Azure Attestation. Contiene todas las notificaciones que representan las propiedades del token de atestación, como la codificación del informe, la duración de la validez del informe, etc.

### <a name="common-incoming-claims-across-all-attestation-types"></a>Notificaciones entrantes comunes en todos los tipos de atestación

Las siguientes notificaciones se generan en Azure Attestation y se pueden usar para definir reglas de autorización en una directiva personalizada:
- **x-ms-ver**: versión de esquema de JWT (se espera que sea "1.0").
- **x-ms-attestation-type**: valor de cadena que representa el tipo de atestación. 
- **x-ms-policy-hash**: hash de la directiva de evaluación de Azure Attestation calculada como BASE64URL (SHA256 (UTF8 (BASE64URL (UTF8 (texto de directiva))))
- **x-ms-policy-signer**: objeto JSON con un miembro "jwk" que representa la clave que un cliente usó para firmar su directiva, cuando el cliente carga una directiva firmada.

Las siguientes notificaciones se consideran en desuso, pero se admiten por completo. Se recomienda usar los nombres de notificaciones que no estén en desuso.

Notificación en desuso | Notificación recomendada 
--- | --- 
ver | x-ms-ver
tee | x-ms-attestation-type
maa-policyHash | x-ms-policy-hash
policy_hash | x-ms-policy-hash
policy_signer | x-ms-policy-signer

### <a name="common-outgoing-claims-across-all-attestation-types"></a>Notificaciones salientes comunes en todos los tipos de atestación

Las siguientes notificaciones se definen en [JWT de IETF](https://tools.ietf.org/html/rfc7519) y se usan en Azure Attestation en el objeto de respuesta:

- **Notificación "jti" (identificador de JWT)** :
- **Notificación "iss" (emisor)** :
- **Notificación "iat" (emitido a las)** :
- **Notificación "exp" (fecha de expiración)** :
- **Notificación "nbf" (no antes de)** :

Las siguientes notificaciones se definen en [EAT de IETF](https://tools.ietf.org/html/draft-ietf-rats-eat-03#page-9) y se usan en Azure Attestation en el objeto de respuesta:
- **"Notificación de nonce" (nonce)**

Las notificaciones siguientes se generan de forma predeterminada en función de las notificaciones entrantes.
- **x-ms-ver**: versión de esquema de JWT (se espera que sea "1.0").
- **x-ms-attestation-type**: valor de cadena que representa el tipo de atestación. 
- **x-ms-policy-hash**: valor de cadena que contiene el hash SHA256 del texto de la directiva calculado por BASE64URL(SHA256(UTF8(BASE64URL(UTF8(texto de directiva)))).
- **x-ms-policy-signer**: contiene un JWK con la clave pública o la cadena de certificados presente en el encabezado de la directiva firmada. x-ms-policy-signer solo se agrega si la directiva está firmada

## <a name="claims-specific-to-sgx-enclaves"></a>Notificaciones específicas de enclaves SGX

### <a name="incoming-claims-specific-to-sgx-attestation"></a>Notificaciones entrantes específicas de la atestación de SGX

Las siguientes notificaciones se generan en el servicio para la atestación de SGX y se pueden usar para definir reglas de autorización en una directiva personalizada:
- **x-ms-sgx-is-debuggable**: valor booleano que indica si el enclave tiene habilitada la depuración o no
- **x-ms-sgx-product-id**
- **sgx-mrsigner**: valor hexadecimal codificado del campo "mrsigner" de la oferta.
- **x-ms-sgx-mrenclave**: valor hexadecimal codificado del campo "mrenclave" de la oferta.
- **x-ms-sgx-svn**: número de versión de seguridad codificado en la oferta. 

### <a name="outgoing-claims-specific-to-sgx-attestation"></a>Notificaciones salientes específicas de la atestación de SGX

Las siguientes notificaciones se generan en el servicio y se incluyen en el objeto de respuesta para la atestación de SGX:
- **x-ms-sgx-is-debuggable**: valor booleano que indica si el enclave tiene habilitada la depuración o no
- **x-ms-sgx-product-id**
- **sgx-mrsigner**: valor hexadecimal codificado del campo "mrsigner" de la oferta.
- **x-ms-sgx-mrenclave**: valor hexadecimal codificado del campo "mrenclave" de la oferta.
- **x-ms-sgx-svn**: número de versión de seguridad codificado en la oferta. 
- **x-MS-SGX-EHD**: datos contenidos en el enclave con el formato BASE64URL (datos contenidos en el encave).
- **x-ms-sgx-collateral**: objeto JSON que describe el colateral que se usa para realizar la atestación. El valor de la notificación colateral x-ms-sgx es un objeto JSON anidado con los siguientes pares clave-valor:
    - **qeidcertshash**: valor SHA256 de los certificados de emisión de identidad de QE.
    - **qeidcrlhash**: valor SHA256 de la lista CRL de certificados de emisión de identidades de QE.
    - **qeidhash**: valor SHA256 del colateral de identidad de QE.
    - **quotehash**: valor SHA256 del presupuesto evaluado.
    - **tcbinfocertshash**: valor SHA256 de los certificados de emisión de información de TCB.
    - **tcbinfocrlhash**: valor SHA256 de la lista CRL de certificados de emisión de información de TCB.
    - **tcbinfohash**: objeto JSON que describe el colateral que se usa para realizar la atestación.

Las siguientes notificaciones se consideran en desuso, pero se admiten por completo y seguirán estando incluidas en el futuro. Se recomienda usar los nombres de notificaciones que no estén en desuso.

Notificación en desuso | Notificación recomendada
--- | --- 
$is-debuggable | x-ms-sgx-is-debuggable
$sgx-mrsigner | x-ms-sgx-mrsigner
$sgx-mrenclave | x-ms-sgx-mrenclave
$product-id | x-ms-sgx-product-id
$svn | x-ms-sgx-svn
$tee | x-ms-attestation-type
maa-ehd | x-ms-sgx-ehd
aas-ehd | x-ms-sgx-ehd
maa-attestationcollateral | x-ms-sgx-collateral

## <a name="claims-specific-to-trusted-platform-module-tpm-vbs-attestation"></a>Notificaciones específicas de la atestación del Módulo de plataforma segura (TPM)/VBS

### <a name="incoming-claims-for-tpm-attestation"></a>Notificaciones entrantes para la atestación de TPM

Notificaciones emitidas por Azure Attestation para la atestación de TPM. La disponibilidad de las notificaciones depende de la evidencia proporcionada para la atestación.

- **aikValidated**: valor booleano que indica si se ha validado o no el certificado de la clave de identidad de la atestación (AIK).
- **aikPubHash**:  cadena que contiene el hash SHA256 en base64 (clave pública de identidad de atestación en formato DER).
- **tpmVersion**:   valor entero que contiene la versión principal del Módulo de plataforma segura (TPM).
- **secureBootEnabled**: valor booleano que indica si está habilitado el arranque seguro.
- **iommuEnabled**:  valor booleano que indica si está habilitada la unidad de administración de memoria de entrada y salida (IOMMU).
- **bootDebuggingDisabled**: valor booleano que indica si está deshabilitada la depuración de arranque.
- **notSafeMode**:  valor booleano que indica si Windows no se ejecuta en modo seguro.
- **notWinPE**:  valor booleano que indica si Windows no se ejecuta en modo WinPE.
- **vbsEnabled**:  valor booleano que indica si VBS está habilitado.
- **vbsReportPresent**:  valor booleano que indica si está disponible el informe de enclaves de VBS.

### <a name="incoming-claims-for-vbs-attestation"></a>Notificaciones entrantes para la atestación de VBS

Las notificaciones que emite Azure Attestation para la atestación de VBS se suman a las disponibles para la atestación de TPM. La disponibilidad de las notificaciones depende de la evidencia proporcionada para la atestación.

- **enclaveAuthorId**:  valor de cadena que contiene el valor codificado en Base64Url del identificador de autor del enclave; es decir, el identificador de autor del módulo principal del enclave.
- **enclaveImageId**:  valor de cadena que contiene el valor codificado en Base64Url del identificador de imagen del enclave, es decir, el identificador de imagen del módulo principal del enclave.
- **enclaveOwnerId**:  valor de cadena que contiene el valor codificado en Base64Url del identificador de propietario del enclave.
- **enclaveFamilyId**:  valor de cadena que contiene el valor codificado en Base64Url del identificador de familia del enclave; es decir, el identificador de familia del módulo principal del enclave.
- **enclaveSvn**:  valor entero que contiene el número de versión de seguridad del módulo principal del enclave.
- **enclavePlatformSvn**:  valor entero que contiene el número de versión de seguridad de la plataforma que hospeda el enclave.
- **enclaveFlags**:  la notificación enclaveFlags es un valor entero que contiene marcas que describen la directiva de tiempo de ejecución del enclave.

### <a name="outgoing-claims-specific-to-tpm-and-vbs-attestation"></a>Notificaciones salientes específicas de la atestación de TPM y de VBS

- **cnf (confirmación)** : esta notificación se usa para identificar la clave de la prueba de posesión. La notificación de confirmación, tal como se define en RFC 7800, contiene la parte pública de la clave del enclave atestado que se representa como un objeto de clave web JSON (JWK) (RFC 7517).
- **rp_data (datos de usuario de confianza)** : datos de usuario de confianza, si los hay, especificados en la solicitud, que utiliza este usuario como nonce para garantizar que el informe está actualizado. rp_data solo se agrega si hay rp_data

### <a name="property-claims"></a>Notificaciones de propiedad

- **report_validity_in_minutes**: notificación de entero que indica cuánto tiempo es válido el token.
  - **Valor predeterminado (tiempo)** : un día, expresado en minutos.
  - **Valor máximo (tiempo)** : un año, expresado en minutos.
- **omit_x5c**: notificación booleana que indica si Azure Attestation debe omitir el certificado usado para proporcionar la autenticidad de la prueba de servicio. Si es True, se agrega x5t al token de atestación. Si es False (valor predeterminado), se agregar x5c al token de atestación.

## <a name="next-steps"></a>Pasos siguientes
- [Creación y firma de una directiva de atestación](author-sign-policy.md)
- [Configuración de Azure Attestation con PowerShell](quickstart-powershell.md)
