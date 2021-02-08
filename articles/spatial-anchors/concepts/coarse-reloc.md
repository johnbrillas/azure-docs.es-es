---
title: Relocalización general
description: Aprenda a usar la relocalización general para buscar delimitadores cercanos.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 01/28/2021
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.custom: devx-track-csharp
ms.openlocfilehash: fc04242e61c748d7ae52e61e40206ba338a1b6aa
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99071150"
---
# <a name="coarse-relocalization"></a>Relocalización general

La relocalización general es una característica que permite la localización a gran escala proporcionando una respuesta aproximada pero rápida a la pregunta: *¿Dónde está mi dispositivo ahora/qué contenido debo observar?* La respuesta no es precisa, pero tiene el formato: *Está cerca de estos delimitadores, intente localizar uno de ellos*.

La relocalización general funciona mediante el etiquetado de los delimitadores con varias lecturas de sensor en el dispositivo que se usan posteriormente para realizar consultas rápidas. En el caso de los escenarios exteriores, los datos del sensor suelen ser la posición GPS (sistema de posición global) del dispositivo. Cuando el GPS no está disponible o no es confiable (por ejemplo, en interiores), los datos del sensor están formados por los puntos de acceso Wi-Fi y las balizas Bluetooth al alcance. Los datos recopilados del sensor contribuyen al mantenimiento de un índice espacial que Azure Spatial Anchors usa para determinar rápidamente los delimitadores que se encuentran cerca del dispositivo.

## <a name="when-to-use-coarse-relocalization"></a>Cuándo usar la relocalización general

Si tiene previsto administrar más de 35 delimitadores espaciales en un espacio mayor que una pista de tenis, es probable que se beneficie de la indexación espacial de relocalización general.

La búsqueda rápida de los delimitadores habilitados por la relocalización general está diseñada para simplificar el desarrollo de aplicaciones respaldadas por colecciones a escala mundial de, por ejemplo, millones de delimitadores distribuidos geográficamente. La complejidad de los índices espaciales está oculta, lo que le permite centrarse en la lógica de aplicación. Azure Spatial Anchors se encarga de que todo el trabajo pesado de delimitador se realice en segundo plano.

## <a name="using-coarse-relocalization"></a>Uso de la relocalización general

El flujo de trabajo típico para crear y consultar Azure Spatial Anchors con la relocalización general es el siguiente:
1.  Cree y configure un proveedor de huellas digitales de sensor para recopilar los datos de sensor que elija.
2.  Inicie una sesión de Azure Spatial Anchors y cree delimitadores. Como la huella digital de sensores está habilitada, los delimitadores se indexan espacialmente mediante la relocalización general.
3.  Consulte los delimitadores cercanos mediante la relocalización general, con los criterios de búsqueda dedicados de la sesión de Azure Spatial Anchors.

Puede consultar el siguiente tutorial para configurar la relocalización general en la aplicación:
* [Relocalización general en Unity](../how-tos/set-up-coarse-reloc-unity.md)
* [Relocalización general en Objective-C](../how-tos/set-up-coarse-reloc-objc.md)
* [Relocalización general en Swift](../how-tos/set-up-coarse-reloc-swift.md)
* [Relocalización general en Java](../how-tos/set-up-coarse-reloc-java.md)
* [Relocalización general en C++ y NDK](../how-tos/set-up-coarse-reloc-cpp-ndk.md)
* [Relocalización general en C++ y WinRT](../how-tos/set-up-coarse-reloc-cpp-winrt.md)

## <a name="sensors-and-platforms"></a>Sensores y plataformas

### <a name="platform-availability"></a>Disponibilidad de la plataforma

Los tipos de datos de sensor que puede enviar al servicio de delimitador son los siguientes:

* Posición del GPS: latitud, longitud, altitud.
* Intensidad de la señal de puntos de acceso WiFi en el rango.
* Intensidad de la señal de las balizas Bluetooth en el rango.

En la tabla siguiente se resume la disponibilidad de los datos de sensor en las plataformas admitidas, junto con las advertencias específicas de la plataforma:

|                 | HoloLens | Android | iOS |
|-----------------|----------|---------|-----|
| **GPS**         | NO<sup>1</sup>  | SÍ<sup>2</sup> | SÍ<sup>3</sup> |
| **WiFi**        | SÍ<sup>4</sup> | SÍ<sup>5</sup> | No  |
| **Balizas de BLE** | SÍ<sup>6</sup> | SÍ<sup>6</sup> | SÍ<sup>6</sup>|


<sup>1</sup> Se puede asociar un dispositivo GPS externo a HoloLens. Póngase en contacto con [nuestro soporte técnico](../spatial-anchor-support.md) si quiere usar HoloLens con un dispositivo GPS.<br/>
<sup>2</sup> Compatible con las API [LocationManager][3] (tanto GPS como NETWORK)<br/>
<sup>3</sup> Compatible con las API [CLLocationManager][4]<br/>
<sup>4</sup> Compatible con una frecuencia de aproximadamente un examen cada 3 segundos <br/>
<sup>5</sup> A partir del nivel 28 de API, los exámenes WiFi se limitar a 4 llamadas cada 2 minutos. Desde Android 10, la limitación se puede deshabilitar desde el menú de configuración del Desarrollador. Para más información, consulte la [documentación de Android][5].<br/>
<sup>6</sup> Limitado a [Eddystone][1] e [iBeacon][2]

### <a name="which-sensor-to-enable"></a>Qué sensor habilitar

La elección del sensor es específica de la aplicación que se desarrolla y de la plataforma.
En el diagrama siguiente se proporciona un punto de partida sobre la combinación de sensores que se puede habilitar según el escenario de localización:

![Diagrama de la selección de los sensores habilitados](media/coarse-relocalization-enabling-sensors.png)

En las secciones siguientes se proporciona más información sobre las ventajas y limitaciones de cada tipo de sensor.

### <a name="gps"></a>GPS

GPS es la opción para escenarios de exteriores.
Al usar el GPS en la aplicación, tenga en cuenta que las lecturas proporcionadas por el hardware suelen ser:

* frecuencia asincrónica y baja (menos de 1 Hz).
* no confiable/ruidoso (en promedio de la desviación estándar de 7-m).

En general, tanto el sistema operativo del dispositivo como los Spatial Anchors de Azure realizarán algún filtrado y extrapolación en la señal GPS sin procesar en un intento de mitigar estos problemas. Este procesamiento adicional necesita tiempo para la convergencia, por lo que para obtener mejores resultados debe intentar:

* Crear el proveedor de huellas digitales del sensor lo antes posible en su aplicación.
* Mantener el proveedor de huellas digitales del sensor activo entre varias sesiones.
* Compartir el proveedor de huellas digitales del sensor entre varias sesiones.

Los dispositivos GPS de nivel de consumidor suelen ser imprecisos. Un estudio de [Zandenbergen y Barbeau (2011)][6] informa que la precisión media de los teléfonos móviles con GPS asistido (A-GPS) es de alrededor de 7 metros, ¡un valor bastante grande para ser ignorado! Para tener en cuenta estos errores de medición, el servicio trata los delimitadores como distribuciones de probabilidad en el espacio GPS. Como tal, un delimitador es ahora la región del espacio que más probablemente (es decir, con más del 95% de confianza) contiene su posición GPS verdadera y desconocida.

La misma razón se aplica cuando se consulta con GPS. El dispositivo se representa como otra región de confianza espacial en torno a su verdadera posición GPS desconocida. Descubrir los delimitadores cercanos se traduce en simplemente encontrar los delimitadores con regiones de confianza *lo suficientemente cercanas* a la región de confianza del dispositivo, como se ilustra en la imagen siguiente:

![Selección de candidatos de delimitador con GPS](media/coarse-reloc-gps-separation-distance.png)

### <a name="wifi"></a>Wi-Fi

En HoloLens y Android, la intensidad de la señal WiFi puede ser una opción correcta para habilitar la relocalización general de interior.
Su ventaja es la posible disponibilidad inmediata de los puntos de acceso WiFi (habituales, por ejemplo, en oficinas o centros comerciales) sin necesidad de configuración adicional.

> [!NOTE]
> iOS no proporciona ninguna API para leer la intensidad de la señal WiFi y, por tanto, no se puede usar para la relocalización general habilitada para WiFi.

Al usar el WiFi en la aplicación, tenga en cuenta que las lecturas proporcionadas por el hardware suelen ser:

* frecuencia asincrónica y baja (menos de 0.1 Hz).
* posible limitación al nivel de sistema operativo.
* no confiable/ruidoso (en promedio de la desviación estándar de 3-m).

Los Spatial Anchors de Azure intentarán crear un mapa de intensidad de señal WiFi filtrado durante una sesión para intentar mitigar estos problemas. Para obtener mejores resultados, debe intentar:

* crear la sesión antes de colocar el primer delimitador.
* mantener la sesión activa lo máximo posible (es decir, cree todos los delimitadores y la consulta en una sesión).

### <a name="bluetooth-beacons"></a>Balizas Bluetooth
<a name="beaconsDetails"></a>

La implementación adecuada de balizas Bluetooth es una buena solución para escenarios de relocalización general de gran escala en interiores, donde no hay GPS o es inexacto. También es el único método para interiores que se admite en las tres plataformas.

Las balizas suelen ser dispositivos versátiles en los que se puede configurar todo, incluidos los UUID y las direcciones MAC. Azure Spatial Anchors espera que las balizas se identifiquen de forma única por su UUID. Si no se garantiza esta singularidad, lo más probable es que se produzcan resultados incorrectos. Para obtener mejores resultados, debe intentar:

* Asignar UUID únicos a las balizas.
* Implementarlas de forma que abarquen el espacio de manera uniforme y, de este modo, se pueda acceder al menos a tres balizas desde cualquier punto del espacio.
* Pasar la lista de UUID de baliza únicos al proveedor de huellas digitales del sensor

Las señales de radio, como Bluetooth, se ven afectadas por los obstáculos y pueden interferir con otras. Por estas razones, puede ser difícil adivinar si el espacio se abarca de forma uniforme. Para garantizar una mejor experiencia del cliente, se recomienda probar manualmente la cobertura de las balizas. Para ello, se puede recorrer el espacio con los dispositivos candidatos y una aplicación que muestre la disponibilidad de Bluetooth. Mientras prueba la cobertura, asegúrese de que puede alcanzar al menos tres balizas desde cualquier posición estratégica del espacio. La configuración de demasiadas balizas puede producir más interferencias entre ellas y no mejorará necesariamente la precisión de la relocalización general.

Las balizas Bluetooth suelen tener una cobertura de 80 metros si no hay obstáculos en el espacio.
Esto significa que, en el caso de un espacio sin grandes obstáculos, puede implementar balizas en un patrón de cuadrícula cada 40 metros.

Una baliza que se quede sin batería afectará negativamente a los resultados, por lo que debe asegurarse de supervisar la implementación periódicamente en busca de baterías bajas o agotadas.

Azure Spatial Anchors solo realizará un seguimiento de las balizas Bluetooth que se encuentran en la lista de UUID conocidos de proximidad de las balizas. Las balizas maliciosas programadas para tener UUID en la lista de permitidos pueden afectar negativamente a la calidad del servicio. Por ese motivo, obtendrá los mejores resultados en espacios seleccionados en los que pueda controlar su implementación.

### <a name="sensors-accuracy"></a>Precisión de los sensores

La precisión de la señal del GPS, tanto en la creación del delimitador como en las consultas, tiene una gran influencia sobre el conjunto de delimitadores devueltos. Por el contrario, las consultas basadas en WiFi o balizas considerarán todos los delimitadores que tienen al menos un punto de acceso o señalización en común con la consulta. En ese sentido, el resultado de una consulta basada en WiFi o balizas está determinada principalmente por el intervalo físico de los puntos de acceso, las señalizaciones y los obstáculos ambientales.
En la siguiente tabla se calcula el espacio de búsqueda esperado para cada tipo de sensor:

| Sensor      | Radio del espacio de búsqueda (aproximado) | Detalles |
|-------------|:-------:|---------|
| GPS         | 20 m - 30 m | Determinado por la incertidumbre del GPS entre otros factores. Los números informados se estiman para la precisión media de GPS de los teléfonos móviles con A-GPS, que es de 7 metros. |
| Wi-Fi        | 50 m - 100 m | Determinado por el intervalo de puntos de acceso inalámbrico. Depende de la frecuencia, la intensidad del transmisor, los obstáculos físicos, las interferencias, etc. |
| Las balizas de BLE |  70 m | Determinado por el intervalo de la señalización. Depende de la frecuencia, la intensidad de transmisión, los obstáculos físicos, las interferencias, etc. |

<!-- Reference links in article -->
[1]: https://developers.google.com/beacons/eddystone
[2]: https://developer.apple.com/ibeacon/
[3]: https://developer.android.com/reference/android/location/LocationManager
[4]: https://developer.apple.com/documentation/corelocation/cllocationmanager?language=objc
[5]: https://developer.android.com/guide/topics/connectivity/wifi-scan
[6]: https://www.cambridge.org/core/journals/journal-of-navigation/article/positional-accuracy-of-assisted-gps-data-from-highsensitivity-gpsenabled-mobile-phones/E1EE20CD1A301C537BEE8EC66766B0A9
