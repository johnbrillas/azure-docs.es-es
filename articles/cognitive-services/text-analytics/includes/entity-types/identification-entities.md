---
title: Entidades de identificación
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/17/2021
ms.author: aahi
ms.openlocfilehash: a376b050d79709885e3542d330bb6b1eea48d046
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750836"
---
### <a name="financial-account-identification"></a>Identificación de cuenta financiera

Esta categoría de entidad incluye información financiera y formas oficiales de identificación.

#### <a name="category-aba-routing-number"></a>Categoría: número de ruta de ABA

Este categoría contiene la entidad siguiente:

:::row:::
    :::column span="":::
        **Entidad**

        Número de ruta de ABA

    :::column-end:::
    :::column span="2":::
        **Detalles**

        Números de enrutamiento de tránsito de American Banker Association (ABA).
      
    :::column-end:::
:::row-end:::

#### <a name="category-swift-code"></a>Categoría: código SWIFT

Este categoría contiene la entidad siguiente:

:::row:::
    :::column span="":::
        **Entidad**

        Código SWIFT

    :::column-end:::
    :::column span="2":::
        **Detalles**

        Códigos SWIFT de información de instrucciones de pago.
      
    :::column-end:::
:::row-end:::

#### <a name="category-credit-card"></a>Categoría: tarjeta de crédito

Este categoría contiene la entidad siguiente:

:::row:::
    :::column span="":::
        **Entidad**

        Tarjeta de crédito

    :::column-end:::
    :::column span="2":::
        **Detalles**

        Números de tarjeta de crédito. 
      
    :::column-end:::
:::row-end:::

#### <a name="category-international-banking-account-number-iban"></a>Categoría: número de cuenta bancaria internacional (IBAN) 

Este categoría contiene la entidad siguiente:

:::row:::
    :::column span="":::
        **Entidad**

        Tarjeta de crédito

    :::column-end:::
    :::column span="2":::
        **Detalles**

        Códigos IBAN de información de instrucciones de pago.
      
    :::column-end:::
:::row-end:::

### <a name="government-and-countryregion-specific-identification"></a>Identificación específica del país o región y del gobierno

> [!NOTE]
> Las entidades financieras y específicas del país siguientes no se devuelven con el parámetro `domain=phi`:
> * Números de pasaporte
> * Id. de impuesto

Las entidades siguientes se agrupan y enumeran por país:

#### <a name="argentina"></a>Argentina

:::row:::
    :::column span="":::
        **Entidad**

        Número de identidad nacional de Argentina (DNI)

    :::column-end:::
:::row-end:::


#### <a name="austria"></a>Austria

:::row:::
    :::column span="":::
        **Entidad**

        Tarjeta de identidad de Austria

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal de Austria

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal (NIF) de Austria

    :::column-end:::
:::row-end:::



#### <a name="australia"></a>Australia

:::row:::
    :::column span="":::
        **Entidad**

        Número de cuenta bancaria de Australia

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Número de empresa de Australia

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Número de sociedad de Australia

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Permiso de conducir de Australia  

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de cuenta del seguro médico de Australia

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de pasaporte de Australia

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Número de pasaporte de Australia

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Número de expediente fiscal de Australia

    :::column-end:::

:::row-end:::


#### <a name="belgium"></a>Bélgica

:::row:::
    :::column span="":::
        **Entidad**

        Número nacional de Bélgica

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal (NIF) de Bélgica

    :::column-end:::

:::row-end:::


#### <a name="brazil"></a>Brasil 

:::row:::
    :::column span="":::
        **Entidad**

        Número de entidad jurídica de Brasil (CNPJ)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de CPF de Brasil

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Tarjeta de identificación nacional de Brasil (RG)

    :::column-end:::
:::row-end:::

#### <a name="canada"></a>Canadá

:::row:::
    :::column span="":::
        **Entidad**

        Número de cuenta bancaria de Canadá

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Número de permiso de conducir de Canadá

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número del servicio de salud de Canadá

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de pasaporte de Canadá

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación sanitaria personal de Canadá (PHIN)

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Número de la seguridad social de Canadá

    :::column-end:::
:::row-end:::

#### <a name="chile"></a>Chile 

:::row:::
    :::column span="":::
        **Entidad**

        Número del carné de identidad de Chile

    :::column-end:::
:::row-end:::

#### <a name="china"></a>China

:::row:::
    :::column span="":::
        **Entidad**

        Número del carné de identidad de residente de China (PRC)

    :::column-end:::
:::row-end:::


#### <a name="european-union-eu"></a>Unión Europea (UE)

:::row:::
    :::column span="":::
        **Entidad**

        Número de tarjeta de débito de la UE

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de permiso de conducir de la UE

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación nacional de la UE

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de pasaporte de la UE

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número del seguro social (SSN) o identificador equivalente de la UE

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal de la UE (TIN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Coordenadas GPS de la UE

    :::column-end:::
:::row-end:::

#### <a name="france"></a>Francia

:::row:::
    :::column span="":::
        **Entidad**

        Número de permiso de conducir de Francia

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de seguro médico de Francia

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Carné de identidad nacional de Francia (CNI)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de pasaporte de Francia

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número del seguro social de Francia (INSEE)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal de Francia (número SPI)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal (NIF) de Francia

    :::column-end:::
:::row-end:::

#### <a name="germany"></a>Alemania

:::row:::
    :::column span="":::
        **Entidad**

        Número de permiso de conducir de Alemania

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número del carné de identidad de Alemania

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de pasaporte de Alemania

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal de Alemania

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal de Alemania

    :::column-end:::
:::row-end:::

#### <a name="hong-kong"></a>RAE de Hong Kong

:::row:::
    :::column span="":::
        **Entidad**

        Número del carné de identidad de Hong Kong (HKID)

    :::column-end:::
:::row-end:::

#### <a name="hungary"></a>Hungría

:::row:::
    :::column span="":::
        **Entidad**

        Número de identificación personal de Hungría

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal de Hungría

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal de Hungría

    :::column-end:::
:::row-end:::

#### <a name="india"></a>India

:::row:::
    :::column span="":::
        **Entidad**

        Número de cuenta permanente de India (PAN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación única de India (Aadhaar)

    :::column-end:::
:::row-end:::


#### <a name="indonesia"></a>Indonesia

:::row:::
    :::column span="":::
        **Entidad**

        Número del carné de identidad de Indonesia (KTP)

    :::column-end:::
:::row-end:::

#### <a name="ireland"></a>Irlanda

:::row:::
    :::column span="":::
        **Entidad**

        Número de servicio público personal de Irlanda (PPS)

    :::column-end:::
:::row-end:::

#### <a name="israel"></a>Israel

:::row:::
    :::column span="":::
        **Entidad**

        Identificación nacional

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de cuenta bancaria de Israel

    :::column-end:::
:::row-end:::

#### <a name="italy"></a>Italia

:::row:::
    :::column span="":::
        **Entidad**

        Id. del permiso de conducir de Italia

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Código fiscal de Italia

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal de Italia

    :::column-end:::
:::row-end:::


#### <a name="japan"></a>Japón

:::row:::
    :::column span="":::
        **Entidad**

        Número de cuenta bancaria de Japón

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Número de permiso de conducir de Japón

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Mi número personal de Japón

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Mi número corporativo de Japón

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de registro de residente de Japón

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de la tarjeta de residencia de Japón

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número del seguro social (SIN) de Japón

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de pasaporte de Japón

    :::column-end:::
:::row-end:::

#### <a name="luxembourg"></a>Luxemburgo

:::row:::
    :::column span="":::
        **Entidad**

        Número de identificación nacional de Luxemburgo (personas físicas)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación nacional de Luxemburgo (personas jurídicas)

    :::column-end:::
:::row-end:::

#### <a name="malta"></a>Malta

:::row:::
    :::column span="":::
        **Entidad**

        Número del carné de identidad de Malta

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal de Malta

    :::column-end:::
:::row-end:::


#### <a name="new-zealand"></a>Nueva Zelanda

:::row:::
    :::column span="":::
        **Entidad**

        Número de cuenta bancaria de Nueva Zelanda

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número del permiso de conducir de Nueva Zelanda

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de Hacienda Pública de Nueva Zelanda

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número del Ministerio de Sanidad de Nueva Zelanda

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Número de bienestar social de Nueva Zelanda

    :::column-end:::
:::row-end:::


#### <a name="philippines"></a>Filipinas

:::row:::
    :::column span="":::
        **Entidad**

        Número de identificación multiuso unificado de Filipinas

    :::column-end:::
:::row-end:::

#### <a name="portugal"></a>Portugal 

:::row:::
    :::column span="":::
        **Entidad**

        Número de la tarjeta de ciudadano de Portugal

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Número de identificación fiscal de Portugal

    :::column-end:::
:::row-end:::

#### <a name="singapore"></a>Singapur

:::row:::
    :::column span="":::
        **Entidad**

        Número de la tarjeta de identificación del registro nacional de Singapur (NRIC)

    :::column-end:::
:::row-end:::


#### <a name="south-africa"></a>Sudáfrica

:::row:::
    :::column span="":::
        **Entidad**

        Número de identificación de Sudáfrica

    :::column-end:::
:::row-end:::


#### <a name="south-korea"></a>Corea del Sur

:::row:::
    :::column span="":::
        **Entidad**

        Número de registro de residente de Corea del Sur

    :::column-end:::
:::row-end:::

#### <a name="spain"></a>España

:::row:::
    :::column span="":::
        **Entidad**

        DNI de España

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de seguridad social de España (SSN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de identificación fiscal de España

    :::column-end:::
:::row-end:::
 
#### <a name="switzerland"></a>Suiza

:::row:::
    :::column span="":::
        **Entidad**

        Número del seguro social de Suiza (AHV)

    :::column-end:::
:::row-end:::


#### <a name="taiwan"></a>Taiwán 

:::row:::
    :::column span="":::
        **Entidad**

        Identificación nacional de Ta

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Certificado de residente de Taiwán (ARC/TARC)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Número de pasaporte de Taiwán

    :::column-end:::
:::row-end:::

#### <a name="united-kingdom"></a>Reino Unido

:::row:::
    :::column span="":::
        **Entidad**

        Reino Unido Número de permiso de conducir

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Reino Unido Número de lista electoral

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Reino Unido Número del servicio de salud nacional (NHS)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Reino Unido Número del seguro social (NINO)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Reino Unido o Estados Unidos Número de pasaporte

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

       Reino Unido Número único de referencia del contribuyente

    :::column-end:::

:::row-end:::


#### <a name="united-states"></a>Estados Unidos

:::row:::
    :::column span="":::
        **Entidad**

        EE. UU. Número de seguridad social

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       EE. UU. Número de permiso de conducir

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Estados Unidos o Reino Unido Número de pasaporte

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       EE. UU. Número de identificación fiscal (NIF) individual

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       EE. UU. Número de la agencia antidroga de Estados Unidos (DEA)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       EE. UU. Número de cuenta bancaria

    :::column-end:::
:::row-end:::
