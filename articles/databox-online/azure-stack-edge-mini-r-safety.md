---
title: Guía de seguridad de Azure Stack Edge Mini R | Microsoft Docs
description: En este artículo se describen las consideraciones, directrices y convenciones de seguridad y se explica cómo instalar y usar el dispositivo de Azure Stack Edge Mini R de manera segura.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: alkohli
ms.openlocfilehash: eb42a9a77927d8577dfec3c9167294eb8f809fec
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100382638"
---
# <a name="azure-stack-edge-mini-r-safety-instructions"></a>Instrucciones de seguridad de Azure Stack Edge Mini R

![Icono de advertencia 1](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
![Icono Leer el aviso de seguridad](./media/azure-stack-edge-mini-r-safety/icon-safety-read-all-instructions.png)
 **LEA LA INFORMACIÓN DE SEGURIDAD Y SALUD**

Lea toda la información de seguridad de este artículo antes de usar su dispositivo de Azure Stack Edge Mini R, que consta de una batería, una fuente de alimentación con conexión de CA/CC, un adaptador de alimentación y un módulo de servidor. Si no sigue las instrucciones, podría provocar un incendio, descargas eléctricas, daños o perjuicios en las propiedades. Lea toda la información de seguridad a continuación antes de usar el Azure Stack Edge Mini R.

## <a name="safety-icon-conventions"></a>Convenciones de iconos de seguridad

Las siguientes palabras de advertencia para los símbolos de notificación de peligro son:

| Icono | Descripción |
|:--- |:--- |
| ![Símbolo de peligro](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)| **PELIGRO:** Indica una situación peligrosa que, si no se evita, causará la muerte o lesiones graves. <br> **ADVERTENCIA:** Indica una situación peligrosa que, si no se evita, podría causar la muerte o lesiones graves. <br> **PRECAUCIÓN:** Indica una situación peligrosa que, si no se evita, podría causar lesiones leves o graves.|
|

Los siguientes iconos de riesgo se deben tener en mente al configurar y poner en marcha el dispositivo de Azure Stack Edge Mini R:

| Icono | Descripción |
|:--- |:--- |
| ![Lea todas las instrucciones primero](./media/azure-stack-edge-mini-r-safety/icon-safety-read-all-instructions.png) | Lea todas las instrucciones primero |
| ![Icono Aviso](./media/azure-stack-edge-mini-r-safety/icon-safety-notice.png) **AVISO:** | Indica información considerada importante, pero no relacionada con la peligrosidad. |
| ![Símbolo de peligro](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) | Símbolo de peligro |
| ![Icono de descarga eléctrica](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) | Peligro de descarga eléctrica |
| ![Solo para uso en interiores](./media/azure-stack-edge-mini-r-safety/icon-safety-indoor-use-only.png) | Solo para uso en interiores |
| ![Icono de que no contiene piezas reparables por el usuario](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) | No contiene piezas reparables por el usuario. No acceder sin la formación adecuada. |
|

## <a name="handling-precautions-and-site-selection"></a>Precauciones de manejo y selección de ubicación

El dispositivo de Azure Stack Edge Mini R cuenta con las siguientes precauciones de manejo y criterios de selección de ubicación:

![Icono de advertencia 2](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
![Icono de descarga eléctrica](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png)
![Icono No contiene piezas reparables por el usuario](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) **PRECAUCIÓN:**

* Inspeccione el dispositivo *recibido* en busca de daños. Si el contenedor del dispositivo está dañado, [póngase en contacto con el Soporte técnico de Microsoft](azure-stack-edge-placeholder.md) para obtener un reemplazo. No intente hacer funcionar el dispositivo.
* Si sospecha que el dispositivo funciona mal, [póngase en contacto con el Soporte técnico de Microsoft](azure-stack-edge-placeholder.md) para obtener un reemplazo. No intente reparar el dispositivo.
* El dispositivo contiene piezas que el usuario no puede reparar. Dentro, hay elementos con niveles de energía, corriente y voltaje peligrosos. No lo abra. Devuelva el dispositivo a Microsoft para que lo repare.

![Icono de advertencia 3](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

Se recomienda usar el sistema de la siguiente manera:

* Alejado de fuentes de calor, incluidos radiadores y luz solar directa.
* En lugares que no estén expuestos a humedad o lluvia.
* En un espacio que reduzca la vibración y los impactos físicos.  El sistema está diseñado para soportar impactos y vibraciones de conformidad con MIL-STD-810G.
* Aislado de campos electromagnéticos potentes generados por dispositivos eléctricos.
* No permita que ningún líquido u objeto extraño entre en el sistema. No coloque bebidas ni otros contenedores de líquidos sobre el sistema o cerca de él.

![Icono de advertencia 4](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
![Icono No contiene piezas reparables por el usuario](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) **PRECAUCIÓN:**

* Este equipo contiene una batería de litio. No intente reparar la batería. El usuario no puede reparar las baterías de este equipo. Hay riesgo de explosión si la batería se sustituye por una de un tipo incorrecto.

![Icono de advertencia 5](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

Solo debe cargar la batería cuando esté conectada al dispositivo de Azure Stack Edge Mini R, no debe cargarla como un dispositivo independiente.

![Icono de advertencia 6](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

* El interruptor de encendido y apagado de la batería solo es para descargar la batería en el módulo de servidor. Si el adaptador de alimentación está conectado a la batería, la alimentación se pasará al módulo del servidor incluso si el interruptor está en la posición de apagado.

![Icono de advertencia 7](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

* No queme ni provoque cortocircuitos a la batería. Se debe reciclar o desechar correctamente.

![Icono de advertencia 8](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

* En lugar de usar la fuente de alimentación de CA/CC proporcionada, este sistema también tiene la opción de usar una batería de tipo 2590 comprobada en el terreno. En este caso, el usuario final debe comprobar que cumple todas las normativas relativas a la seguridad, el transporte, el medio ambiente y cualquier otra norma nacional o local.
* Cuando use el sistema con una batería de tipo 2590, use dicha batería según las condiciones de uso especificadas por el fabricante de la misma.

![Icono de advertencia 9](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

Este dispositivo tiene dos puertos SFP+, que se pueden usar con transceptores ópticos. Para evitar radiación láser peligrosa, use solo los transceptores de clase 1.

## <a name="electrical-precautions"></a>Precauciones eléctricas

El dispositivo de Azure Stack Edge Mini R tiene las siguientes precauciones eléctricas:

![Icono de advertencia 10](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) ![Icono de descarga eléctrica](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) **ADVERTENCIA:**

Cuando se use con el adaptador de fuente de alimentación:

* Proporcione una conexión eléctrica a tierra segura para el cable de la fuente de alimentación. El cable de corriente alterna (CA) tiene una clavija de tres cables (un enchufe con un pin de toma de tierra). Esta clavija solo encaja en un enchufe con toma de tierra. No anule el propósito de la clavija de toma de tierra.
* Dado que la clavija del cable de la fuente de alimentación es el dispositivo de desconexión principal, asegúrese de que los enchufes están ubicados cerca del dispositivo y que se pueda acceder a ellos con facilidad.
* Desconecte los cables de alimentación (tirando de la clavija, no del cable) y desconecte todos los cables, si se diera alguna de las condiciones siguientes:

  * El cable de alimentación o la clavija se deshilacha o se daña de alguna otra forma.
  * El dispositivo está expuesto a la lluvia, a exceso de humedad o a otros líquidos.
  * El dispositivo se cayó y se dañó la carcasa del dispositivo.
  * Sospecha que el dispositivo necesita ser reparado.
* Desenchufe de forma definitiva la unidad antes de moverla o si piensa que se ha dañado de algún modo.

* Proporcione una fuente de alimentación adecuada con protección frente a sobrecargas eléctricas para cumplir con las especificaciones de energía siguientes:

* Voltaje: 100 a 240 voltios CA
* Corriente: 1,7 amperios
* Frecuencia: de 50 Hz a 60 Hz

![Icono de advertencia 11](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
![Icono de descarga eléctrica](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) **ADVERTENCIA:**

* No intente modificar ni usar cables de alimentación de CA distintos a los que se proporcionan con el equipamiento.

![Icono de advertencia 12](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
![Icono de descarga eléctrica](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png)
![Solo para uso en interiores](./media/azure-stack-edge-mini-r-safety/icon-safety-indoor-use-only.png) **ADVERTENCIA:**

* Las fuentes de alimentación que tengan con este símbolo solo se pueden usar en interiores.

## <a name="regulatory-information"></a>Información de disposiciones legales

A continuación se incluye información reglamentaria del dispositivo de Azure Stack Edge Mini R con el número de modelo normativo: TMA01.

El dispositivo de Azure Stack Edge Mini R está diseñado para usarse con equipos de tecnologías de información conforme con IEC/EN 60950-1 o IEC/EN 62368 1 (marca CE), y parte de la lista de NRTL (UL, CSA, ETL, etc.).

En los países distintos de EE. UU. y Canadá, los cables de red (no incluidos con el equipo) no se deben instalar con este equipo si su longitud supera los tres metros.

El equipo está diseñado para funcionar en los siguientes entornos:

| Entorno | Especificaciones |
|:---  |:--- |
| Especificaciones de temperatura del sistema | <ul><li>Temperatura de almacenamiento: -20 &deg;C a 50 &deg;C (-4 &deg;F a 122 &deg;F)</li><li>Operación continua: 0 &deg;C a 40 &deg;C (32 &deg;F a 104 &deg;F)</li></ul> |
| Especificaciones de humedad relativa | <ul><li>Almacenamiento: del 5 a 95 % de humedad relativa</li><li>Operativo: del 10 a 90 % de humedad relativa</li></ul>|
| Especificaciones de altitud máxima | <ul><li>Operativo: 15 000 pies (4572 metros)</li><li>No operativo: 40 000 pies (12 192 metros)</li></ul>|

> ![Icono de aviso - 2](./media/azure-stack-edge-mini-r-safety/icon-safety-notice.png) **AVISO:** &nbsp;Las modificaciones o cambios realizados en el equipo que no hayan sido aprobados expresamente por Microsoft podrían anular la autorización del usuario para operar con el equipo.

#### <a name="canada-and-usa"></a>CANADÁ y EE. UU.:

> ![Icono de aviso - 3](./media/azure-stack-edge-mini-r-safety/icon-safety-notice.png) **AVISO:** &nbsp; Este equipo ha sido probado y se ajusta a los límites de los dispositivos digitales de clase A, en virtud de la sección 15 de las Normas de FCC. Estos límites están diseñados para proporcionar una protección razonable contra interferencias perjudiciales cuando el equipo se hace funcionar en un entorno comercial. Este equipo genera, utiliza y puede irradiar energía de radiofrecuencia y, si no se instala y se usa de acuerdo con el manual de instrucciones, puede causar interferencias perjudiciales para las comunicaciones de radio. El uso de este equipo en una zona residencial puede provocar interferencias perjudiciales, en cuyo caso, se requerirá que el usuario corrija la interferencia a su costa.

El adaptador WiFi USB Netgear A6150 que se proporciona con este equipo está preparado para funcionar cerca del cuerpo y en las pruebas que se han realizado ha quedado acreditado que cumple la tasa de absorción específica (SAR) cuando se lleva en el cuerpo. El límite de SAR que establece la FCC es 1,6 W/kg cuando se calcula el promedio en 1 g de tejido. No transporte ni use el producto a menos de 10 mm del cuerpo para garantizar el cumplimiento de los requisitos de exposición de radiofrecuencia.

El adaptador WiFi USB Netgear A6150 cumple la norma ANSI/IEEE C 95.1-1999 y se han usado los métodos de medición y procedimientos especificados en el suplemento C del boletín 65 de OET.

Tasa de absorción específica de Netgear A6150 (SAR): 1,18 W/kg de promedio en 1 g de tejido

El adaptador WiFi USB de Netgear A6150 solo se debe usar con antenas aprobadas. Este dispositivo y sus antenas no deben estar ubicados en el mismo lugar ni funcionar junto con otra antena o transmisor, salvo que se sigan los procedimientos para producto con varios transmisores FCC. En los productos disponibles en el mercado de Estados Unidos, solo se puede usar el canal 1~11. No se pueden seleccionar otros canales.

La operación en la banda de 5150 – 5250 MHz solo es para el uso en interiores, con el fin de reducir la posibilidad de interferencias dañinas a los sistemas satelitales móviles que compartan el canal.

![Advertencia de información sobre normativa: uso en interiores](./media/azure-stack-edge-mini-r-safety/regulatory-information-indoor-use-only.png)

Se recomienda a los usuarios que los radares de alta potencia se asignen como usuarios principales (usuarios prioritarios) de las bandas de 5250 – 5350 MHz y 5650 – 5850 MHz, y estos radares pueden causar interferencias o daños en los dispositivos LE-LAN.

Este equipo genera, utiliza y puede irradiar energía de radiofrecuencia y, si no se instala y se usa de acuerdo con las instrucciones, puede causar interferencias perjudiciales para las comunicaciones de radio. Sin embargo, no es posible garantizar que no se vayan a producir interferencias en una instalación determinada.

Si este equipo produce interferencias en la recepción de radio o televisión, lo que se puede determinar encendiendo y apagando el equipo, se recomienda al usuario que tome una o varias de las medidas siguientes para intentar eliminarlas:

- Reorientar o cambiar la ubicación de la antena receptora.
- Aumentar la separación entre el equipo y el receptor.
- Conectar el equipo a una toma de un circuito que no sea al que está conectado el receptor.
- Ponerse en contacto con el distribuidor o con un técnico de radio o televisión para solicitarle ayuda.

Para más información sobre los problemas de interferencia, visite el sitio web de FCC, [fcc.gov/cgb/consumerfacts/interference.html](https://www.fcc.gov/consumers/guides/interference-radio-tv-and-telephone-signals). También puede llamar a FCC para solicitar hojas informativas sobre las interferencias y las interferencias telefónicas.

Puede encontrar más información sobre la seguridad de la radiofrecuencia en el sitio web de FCC, [https://www.fcc.gov/general/radio-frequency-safety-0](https://www.fcc.gov/general/radio-frequency-safety-0), y en el sitio web de Industry Canada, [http://www.ic.gc.ca/eic/site/smt-gst.nsf/eng/sf01904.html](http://www.ic.gc.ca/eic/site/smt-gst.nsf/eng/sf01904.html).

Este producto ha demostrado el cumplimiento de EMC en condiciones que incluían el uso de dispositivos periféricos compatibles y cables apantallados entre componentes del sistema. Es importante usar dispositivos periféricos compatibles y cables apantallados entre los componentes del sistema para reducir la posibilidad de que provocar interferencias en radios, televisores y otros dispositivos electrónicos.

Este dispositivo cumple con la sección 15 de las Normas de FCC y los estándares RSS exentos de licencia de Industry Canada. El uso está sujeto a las dos condiciones siguientes: (1) este dispositivo no puede causar interferencias perjudiciales y (2) este dispositivo debe aceptar cualquier interferencia recibida, incluida una que pueda ocasionar el mal funcionamiento del dispositivo.

![Advertencia de información sobre normativa 1](./media/azure-stack-edge-mini-r-safety/regulatory-information-1.png)

CAN ICES-3(A)/NMB-3(A)

Microsoft Corporation, One Microsoft Way, Redmond, WA 98052, USA

Estados Unidos: (800) 426-9400

Canadá: (800) 933-4750

Identificador de FCC de adaptador WiFi USB Netgear A6150: PY318300429
 
Identificador de IC de adaptador WiFi USB Netgear A6150: 4054A-18300429

El adaptador WiFi USB Netgear A6150 que se proporciona con este equipo es compatible con SAR en cuanto a los límites de población general o exposición incontrolada en IC RSS-102 y se ha probado según los procedimientos y métodos de medición especificados en IEEE 1528. Mantenga al menos una distancia de 10 mm cuando se lleve en el cuerpo.

El adaptador WiFi USB Netgear A6150 cumple con el límite de exposición de RF portátil de Canadá establecido para un entorno sin control y es seguro para el funcionamiento previsto, tal como se describe en su manual. Para lograr una mayor reducción de la exposición a la radiofrecuencia, mantenga el producto lo más lejos posible del cuerpo o configure el dispositivo para que tenga una menor potencia de salida inferior, si dicha función está disponible.

En la sección de Estados Unidos, se puede ver una tabla con el promedio de la tasa de absorción específica (SAR) en 1 g para cada producto.

![Advertencia de información sobre normativa 2](./media/azure-stack-edge-mini-r-safety/regulatory-information-2.png)

#### <a name="european-union"></a>UNIÓN EUROPEA:

Solicite una copia de la declaración de conformidad de la UE para este equipo. Envíe un correo electrónico a [CSI_Compliance@microsoft.com](mailto:CSI_Compliance@microsoft.com).

El adaptador USB WiFi de Netgear A6150 que se proporciona con este equipo cumple con la directiva 2014/53/EU y también se puede proporcionar a petición.

![Icono de advertencia 13](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)  **ADVERTENCIA:**

Se trata de un producto de clase A. En un entorno doméstico, este producto puede causar interferencias de radio, en cuyo caso es posible que se pida al usuario que tome las medidas adecuadas.

Eliminación de residuos de baterías y aparatos eléctricos y electrónicos:

![Icono de advertencia 14](./media/azure-stack-edge-mini-r-safety/icon-ewaste-disposal.png)

Este símbolo en el producto o en su embalaje o baterías significa que este producto y las baterías que contiene no se deben eliminar con los residuos domésticos. En su lugar, es responsabilidad suya entregarlas a un punto de recolección aplicable para el reciclaje de baterías y aparatos eléctricos y electrónicos. Esta recolección y reciclaje independiente ayudará a conservar los recursos naturales y a evitar posibles consecuencias desfavorables para la salud y el entorno debido a la posible presencia de sustancias peligrosas en las baterías y en los equipos eléctricos y electrónicos, que podrían ser ocasionados por una eliminación inapropiada. Para más información acerca de dónde dejar las baterías y los residuos eléctricos y electrónicos, póngase en contacto con la oficina local de su ciudad o municipio, el servicio doméstico de eliminación de deshechos o la tienda donde compró este producto. Póngase en contacto con erecycle@microsoft.com para más información de WEEE.

Este producto contiene baterías de celda de moneda.

El adaptador WiFi USB Netgear A6150 que se proporciona con este equipo está preparado para funcionar cerca del cuerpo y en las pruebas que se han realizado ha quedado acreditado que cumple la tasa de absorción específica (SAR) cuando se lleva en el cuerpo. No transporte ni use el producto a menos de 10 mm del cuerpo para garantizar el cumplimiento de los requisitos de exposición de radiofrecuencia.

**Tasa de absorción específica de Netgear A6150 (SAR):** 0,54 W/kg de promedio en 10 g de tejido

 
Este dispositivo se puede usar en todos los Estados miembros de la UE. El dispositivo deberá utilizarse de acuerdo con la normativa nacional y local. En los siguientes países, este dispositivo está restringido a un uso en interiores si se utiliza en el rango de frecuencia de 5150-5350 MHz:  

![Países de la UE que requieren un uso solo en interiores](./media/azure-stack-edge-mini-r-safety/mini-r-safety-eu-indoor-use-only.png)

De acuerdo con los artículos 10.8 (a) y 10.8 (b) de RED, en la tabla siguiente se proporciona información sobre las bandas de frecuencia que se usan y la potencia de transmisión de radiofrecuencia máxima de los productos inalámbricos de Netgear para su venta en la Unión Europea:

**WiFi**

| Intervalo de frecuencia (MHz) | Canales usados | Potencia de transmisión máxima (dBm/mW) |
| --------------------- | ------------- | --------------------------- |
| 2400-2483,5 | 1-13    | ODFM: 19,9 dBm (97,7 mW) <br> CCK: 17,9 dBm (61,7 mW) |
| 5150-5320   | 36-48   | 22,9 dBm (195 mW) |
| 5250-5350   | 52-64   | 22,9 dBm (195 mW) con TPC <br> 19,9 dBm (97,7 mW) no TPC |
| 5470-5725   | 100-140 | 29,9 dBm (977 mW) con TPC <br> 29,6 dBm (490 mW) no TPC |

Microsoft Irlanda Sandyford Ind Est Dublín D18 KX32 IRL

Número de teléfono: +353 1 295 3826

Número de faz: +353 1 706 4110

#### <a name="singapore"></a>SINGAPUR:

El adaptador WiFi USB Netgear A6150 que se proporciona con este equipo cumple con las normas de IMDA.


## <a name="next-steps"></a>Pasos siguientes

- [Preparación de la implementación de Azure Stack Edge Mini R](azure-stack-edge-mini-r-deploy-prep.md)
