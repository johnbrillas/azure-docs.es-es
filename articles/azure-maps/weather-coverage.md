---
title: Cobertura de los servicios de Weather de Azure Maps (versión preliminar)
description: Más información sobre la cobertura de los servicios de Weather (versión preliminar) de Microsoft Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
ms.custom: references_regions
manager: philmea
ms.openlocfilehash: 6c4e9eb765a72b7a0b495f81a954b484ef6aa2b7
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96905492"
---
# <a name="azure-maps-weather-services-preview-coverage"></a>Cobertura de los servicios de Weather de Azure Maps

> [!IMPORTANT]
> Los servicios de Weather de Azure Maps se encuentran actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


En este artículo se proporciona información sobre la cobertura de Azure Maps [Weather Services](/rest/api/maps/weather). Los servicios de datos meteorológicos de Azure Maps devuelven detalles como mosaicos de radar, condiciones meteorológicas actuales, previsiones meteorológicas y el tiempo a lo largo de una ruta.

Azure Maps no tiene el mismo nivel de información y precisión en todos los países y regiones.

La tabla siguiente proporciona información acerca de qué tipo de información meteorológica puede solicitar en cada país o región.

| Símbolo | Significado |
|--------|---------|
|*       |Cubre las condiciones actuales, la previsión por horas, por cuartos de día y por días, el tiempo durante la ruta y los índices diarios. |


## <a name="americas"></a>América

| País/región              |  Mosaicos de satélite | Previsión por minutos, mosaicos de radar | Alertas meteorológicas graves | Otros* |
|-----------------------------|:----------------:|:-----------------:|:-----------------:|:--------:|  
| Anguila                                 | ✓ |   | |  ✓| 
| Antártida                               | ✓ |   | |✓|
| Antigua y Barbuda                      | ✓ |   | |✓| 
| Argentina                                | ✓ |   | |✓| 
| Aruba                                    | ✓ |   | |✓| 
| Bahamas                                  | ✓ |   | |✓| 
| Barbados                                 | ✓ |   | |✓| 
| Belice                                   | ✓ |   | |✓| 
| Bermudas                                  | ✓ |   | |✓| 
| Bolivia                                  | ✓ |   | |✓| 
| Bonaire                                  | ✓ |   | |✓| 
| Brasil                                   | ✓ |   | ✓ |✓| 
| Islas Vírgenes Británicas                   | ✓ |   | |✓| 
| Canadá                                   | ✓ | ✓ | ✓ | ✓| 
| Islas Caimán                           | ✓ |   | |✓| 
| Chile                                    | ✓ |   | |✓| 
| Colombia                                 | ✓ |   | |✓| 
| Costa Rica                               | ✓ |   | |✓| 
| Cuba                                     | ✓ |   | |✓| 
| Curazao                                  | ✓ |   | |✓| 
| Dominica                                 | ✓ |   | |✓| 
| República Dominicana                       | ✓ |   | |✓| 
| Ecuador                                  | ✓ |   | |✓| 
| El Salvador                              | ✓ |   | |✓| 
| Islas Malvinas                         | ✓ |   | |✓| 
| Guayana Francesa                            | ✓ |   | |✓| 
| Groenlandia                                | ✓ |   | |✓| 
| Granada                                  | ✓ |   | |✓| 
| Guadalupe                               | ✓ |   | |✓| 
| Guatemala                                | ✓ |   | |✓| 
| Guayana                                   | ✓ |   | |✓| 
| Haití                                    | ✓ |   | |✓| 
| Honduras                                 | ✓ |   | |✓| 
| Jamaica                                  | ✓ |   | |✓| 
| Martinica                               | ✓ |   | |✓| 
| México                                   | ✓ |   | |✓| 
| Montserrat                               | ✓ |   | |✓| 
| Nicaragua                                | ✓ |   | |✓| 
| Panamá                                   | ✓ |   | |✓| 
| Paraguay                                 | ✓ |   | |✓| 
| Perú                                     | ✓ |   | |✓| 
| Puerto Rico                              | ✓ |   | ✓ |✓| 
| San Bartolomé                         | ✓ |   | |✓| 
| San Cristóbal y Nieves                    | ✓ |   | |✓| 
| Santa Lucía                              | ✓ |   | |✓| 
| San Martín                             | ✓ |   | |✓| 
| San Pedro y Miquelón                | ✓ |   | |✓| 
| San Vicente y las Granadinas         | ✓ |   | |✓| 
| San Eustaquio                           | ✓ |   | |✓|  
| Sint Maarten                             | ✓ |   | |✓| 
| Georgia del Sur e Islas Sandwich del Sur | ✓ |   | |✓| 
| Surinam                                 | ✓ |   | |✓| 
| Trinidad y Tobago                      | ✓ |   | |✓| 
| Islas Turcas y Caicos                 | ✓ |   | |✓| 
| EE. UU. alejadas de los EE.UU.                    | ✓ |   | |✓| 
| EE. UU. Vírgenes de EE. UU.                      | ✓ |   | ✓|✓| 
| Estados Unidos                            | ✓ | ✓ | ✓| ✓| 
| Uruguay                                  | ✓ |   | |✓| 
| Venezuela                                | ✓ |   | |✓| 


## <a name="middle-east-and-africa"></a>Oriente Medio y África

| País/región              |  Mosaicos de satélite | Previsión por minutos, mosaicos de radar | Alertas meteorológicas graves | Otros* | 
|-----------------------------|:----------------:|:-----------------:|:--------:|:--------:| 
| Argelia                     | ✓               |                              |     |  ✓| 
| Angola                      | ✓               |                              |     |  ✓| 
| Bahréin                     | ✓               |                              |     |  ✓| 
| Benín                       | ✓               |                              |     |  ✓| 
| Botsuana                    | ✓               |                              |     |  ✓| 
| Isla Bouvet               | ✓               |                              |     |  ✓| 
| Burkina Faso                | ✓               |                              |     |  ✓| 
| Burundi                     | ✓               |                              |     |  ✓| 
| Camerún                    | ✓               |                              |     |  ✓| 
| Cabo Verde                  | ✓               |                              |     |  ✓| 
| República Centroafricana    | ✓               |                              |     |  ✓| 
| Chad                        | ✓               |                              |     |  ✓| 
| Comores                     | ✓               |                              |     |  ✓| 
| Congo (RDC)                 | ✓               |                              |     |  ✓|
| Costa de Marfil               | ✓               |                              |     |  ✓| 
| Yibuti                    | ✓               |                              |     |  ✓| 
| Egipto                       | ✓               |                              |     |  ✓| 
| Guinea Ecuatorial           | ✓               |                              |     |  ✓| 
| Eritrea                     | ✓               |                              |     |  ✓| 
| Suazilandia                    | ✓               |                              |     |  ✓| 
| Etiopía                    | ✓               |                              |     |  ✓| 
| Territorios Australes Franceses | ✓               |                              |     |  ✓| 
| Gabón                       | ✓               |                              |     |  ✓| 
| Gambia                      | ✓               |                              |     |  ✓| 
| Ghana                       | ✓               |                              |     |  ✓| 
| Guinea                      | ✓               |                              |     |  ✓| 
| Guinea-Bisáu               | ✓               |                              |     |  ✓| 
| Irán                        | ✓               |                              |     |  ✓| 
| Iraq                        | ✓               |                              |     |  ✓| 
| Israel                      | ✓               |                              |   ✓   |  ✓| 
| Jordania                      | ✓               |                              |     |  ✓| 
| Kenia                       | ✓               |                              |     |  ✓| 
| Kuwait                      | ✓               |                              |     |  ✓| 
| Líbano                     | ✓               |                              |     |  ✓| 
| Lesoto                     | ✓               |                              |     |  ✓| 
| Liberia                     | ✓               |                              |     |  ✓| 
| Libia                       | ✓               |                              |     |  ✓| 
| Madagascar                  | ✓               |                              |     |  ✓| 
| Malaui                      | ✓               |                              |     |  ✓| 
| Malí                        | ✓               |                              |     |  ✓| 
| Mauritania                  | ✓               |                              |     |  ✓| 
| Mauricio                   | ✓               |                              |     |  ✓| 
| Mayotte                     | ✓               |                              |     |  ✓| 
| Marruecos                     | ✓               |                              |     |  ✓| 
| Mozambique                  | ✓               |                              |     |  ✓| 
| Namibia                     | ✓               |                              |     |  ✓| 
| Níger                       | ✓               |                              |     |  ✓| 
| Nigeria                     | ✓               |                              |     |  ✓| 
| Omán                        | ✓               |                              |     |  ✓| 
| Autoridad palestina       | ✓               |                              |     |  ✓| 
| Qatar                       | ✓               |                              |     |  ✓| 
| Reunión                     | ✓               |                              |     |  ✓| 
| Ruanda                      | ✓               |                              |     |  ✓| 
| Santa Elena, Ascensión y Tristán da Cunha        | ✓               |            |     |  ✓| 
| Santo Tomé y Príncipe       | ✓               |                              |     |  ✓| 
| Arabia Saudí                | ✓               |                              |     |  ✓| 
| Senegal                     | ✓               |                              |     |  ✓| 
| Seychelles                  | ✓               |                              |     |  ✓| 
| Sierra Leona                | ✓               |                              |     |  ✓| 
| Somalia                     | ✓               |                              |     |  ✓| 
| Sudáfrica                | ✓               |                              |     |  ✓| 
| Sudán del Sur                 | ✓               |                              |     |  ✓| 
| Sudán                       | ✓               |                              |     |  ✓| 
| Siria                       | ✓               |                              |     |  ✓| 
| Tanzania                    | ✓               |                              |     |  ✓| 
| Togo                        | ✓               |                              |     |  ✓| 
| Túnez                     | ✓               |                              |     |  ✓| 
| Uganda                      | ✓               |                              |     |  ✓| 
| Emiratos Árabes Unidos        | ✓               |                              |     |  ✓| 
| Yemen                       | ✓               |                              |     |  ✓| 
| Zambia                      | ✓               |                              |     |  ✓| 
| Zimbabue                    | ✓               |                              |     |  ✓| 


## <a name="asia-pacific"></a>Asia Pacífico

| País/región              |  Mosaicos de satélite | Previsión por minutos, mosaicos de radar | Alertas meteorológicas graves | Otros* |
|-----------------------------|:----------------:|:-----------------:|:--------:|  :--------:|
| Afganistán                       | ✓ |   | | ✓| 
| Samoa Americana                    | ✓ |   |  ✓| ✓| 
| Australia                         | ✓ | ✓ |  ✓ | ✓| 
| Bangladés                        | ✓ |   | | ✓| 
| Bután                            | ✓ |   | | ✓| 
| Territorio Británico del Océano Índico    | ✓ |   | | ✓| 
| Brunéi                            | ✓ |   | | ✓| 
| Camboya                          | ✓ |   | | ✓| 
| China                             | ✓ | ✓ |  ✓ | ✓| 
| Isla de Navidad                  | ✓ |   | | ✓| 
| Islas Cocos (Keeling)           | ✓ |   | | ✓| 
| Islas Cook                      | ✓ |   | | ✓| 
| Islas Fiji                              | ✓ |   | | ✓| 
| Polinesia francesa                  | ✓ |   | | ✓| 
| Guam                              | ✓ |   |  ✓| ✓| 
| Isla Heard e Islas McDonald | ✓ |   | | ✓| 
| RAE de Hong Kong                     | ✓ |   | | ✓| 
| India                             | ✓ |   | | ✓| 
| Indonesia                         | ✓ |   | | ✓| 
| Japón                             | ✓ | ✓ |  ✓| ✓| 
| Kazajistán                        | ✓ |   | | ✓| 
| Kiribati                          | ✓ |   | | ✓| 
| Corea                             | ✓ | ✓ | ✓ |  ✓| 
| Kirguistán                        | ✓ |   | | ✓| 
| Laos                              | ✓ |   | | ✓| 
| RAE de Macao                         | ✓ |   | | ✓| 
| Malasia                          | ✓ |   | | ✓| 
| Maldivas                          | ✓ |   | | ✓| 
| Islas Marshall                  | ✓ |   | ✓ | ✓| 
| Micronesia                        | ✓ |   | ✓ | ✓| 
| Mongolia                          | ✓ |   | | ✓| 
| Myanmar                           | ✓ |   | | ✓| 
| Nauru                             | ✓ |   | | ✓| 
| Nepal                             | ✓ |   | | ✓| 
| Nueva Caledonia                     | ✓ |   | | ✓| 
| Nueva Zelanda                       | ✓ |   | ✓ | ✓| 
| Niue                              | ✓ |   | | ✓| 
| Isla Norfolk                    | ✓ |   | | ✓| 
| Corea del Norte                       | ✓ |   | | ✓| 
| Islas Marianas del Norte          | ✓ |   | ✓ | ✓| 
| Pakistán                          | ✓ |   | | ✓| 
| Palaos                             | ✓ |   | ✓ | ✓| 
| Papúa Nueva Guinea                  | ✓ |   | | ✓| 
| Filipinas                       | ✓ |   | ✓ | ✓| 
| Islas Pitcairn                  | ✓ |   | | ✓| 
| Samoa                             | ✓ |   | | ✓| 
| Singapur                         | ✓ |   | | ✓| 
| Islas Salomón                   | ✓ |   | | ✓| 
| Sri Lanka                         | ✓ |   | | ✓| 
| Taiwán                            | ✓ |   | | ✓| 
| Tayikistán                        | ✓ |   | | ✓| 
| Tailandia                          | ✓ |   | | ✓| 
| Timor-Leste                       | ✓ |   | | ✓| 
| Tokelau                           | ✓ |   | | ✓| 
| Tonga                             | ✓ |   | | ✓| 
| Turkmenistán                      | ✓ |   | | ✓| 
| Tuvalu                            | ✓ |   | | ✓| 
| Uzbekistán                        | ✓ |   | | ✓| 
| Vanuatu                           | ✓ |   | | ✓| 
| Vietnam                           | ✓ |   | | ✓| 
| Wallis y Futuna                 | ✓ |   | | ✓| 


## <a name="europe"></a>Europa

| País/región              |  Mosaicos de satélite | Previsión por minutos, mosaicos de radar | Alertas meteorológicas graves | Otros* | 
|-----------------------------|:----------------:|:-----------------:|:--------:|:--------:|
| Albania                | ✓ |   | | ✓| 
| Andorra                | ✓ |   | ✓ | ✓| 
| Armenia                | ✓ |   | | ✓| 
| Austria                | ✓ | ✓ | ✓ | ✓|
| Azerbaiyán             | ✓ |   | | ✓| 
| Bielorrusia                | ✓ |   | | ✓| 
| Bélgica                | ✓ | ✓ |  ✓| ✓| 
| Bosnia y Herzegovina | ✓ | ✓ | ✓ | ✓| 
| Bulgaria               | ✓ |   |  ✓| ✓| 
| Croacia                | ✓ | ✓ |  ✓| ✓| 
| Chipre                 | ✓ |   | ✓ | ✓| 
| Chequia                | ✓ | ✓ | ✓ | ✓| 
| Dinamarca                | ✓ | ✓ | ✓ | ✓| 
| Estonia                | ✓ | ✓ |  ✓| ✓| 
| Islas Feroe          | ✓ |   | | ✓| 
| Finlandia                | ✓ | ✓ | ✓ | ✓| 
| Francia                 | ✓ | ✓ | ✓ | ✓| 
| Georgia                | ✓ |   | | ✓| 
| Alemania                | ✓ | ✓ | ✓ | ✓| 
| Gibraltar              | ✓ | ✓ | | ✓| 
| Grecia                 | ✓ |   |  ✓| ✓| 
| Guernsey               | ✓ |   | | ✓| 
| Hungría                | ✓ | ✓ |  ✓| ✓| 
| Islandia                | ✓ |   | ✓ | ✓| 
| Irlanda                | ✓ | ✓ |  ✓| ✓| 
| Italia                  | ✓ |   |  ✓| ✓|
| Isla de Man            | ✓ |   | | ✓| 
| Jan Mayen              | ✓ |   | | ✓| 
| Jersey                 | ✓ |   | | ✓| 
| Kosovo                 | ✓ |   |  ✓| ✓| 
| Letonia                 | ✓ |   | ✓ | ✓| 
| Liechtenstein          | ✓ | ✓ |  ✓| ✓| 
| Lituania              | ✓ |   | ✓ | ✓| 
| Luxemburgo             | ✓ | ✓ |  ✓| ✓| 
| Macedonia del Norte        | ✓ |   | ✓ | ✓| 
| Malta                  | ✓ |   | ✓ | ✓| 
| Moldova                | ✓ | ✓ | ✓ | ✓| 
| Mónaco                 | ✓ | ✓ |  ✓| ✓| 
| Montenegro             | ✓ | ✓ |  ✓| ✓| 
| Países Bajos            | ✓ | ✓ |  ✓| ✓| 
| Noruega                 | ✓ | ✓ |  ✓| ✓| 
| Polonia                 | ✓ | ✓ |  ✓| ✓| 
| Portugal               | ✓ | ✓ |  ✓| ✓| 
| Rumanía                | ✓ | ✓ |  ✓| ✓| 
| Rusia                 | ✓ |   |  ✓| ✓| 
| San Marino             | ✓ |   |  ✓| ✓| 
| Serbia                 | ✓ | ✓ |  ✓| ✓| 
| Eslovaquia               | ✓ | ✓ |  ✓| ✓| 
| Eslovenia               | ✓ | ✓ |  ✓| ✓| 
| España                  | ✓ | ✓ |  ✓| ✓| 
| Svalbard               | ✓ |   | | ✓|
| Suecia                 | ✓ | ✓ |  ✓| ✓| 
| Suiza            | ✓ | ✓ | ✓| ✓| 
| Turquía                 | ✓ |   | | ✓| 
| Ucrania                | ✓ |   | | ✓| 
| Reino Unido         | ✓ | ✓ | ✓| ✓| 
| Ciudad del Vaticano           | ✓ |   |✓ | ✓|