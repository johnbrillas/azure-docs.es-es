---
title: Arquitectura de Azure Defender para IoT
description: Obtenga información sobre la arquitectura de Azure Defender para IoT, así como el flujo de información.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/13/2021
ms.author: shhazam
ms.openlocfilehash: 66b960bf874cc46985230e488c749663eff0b835
ms.sourcegitcommit: a0c1d0d0906585f5fdb2aaabe6f202acf2e22cfc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98621104"
---
# <a name="azure-defender-for-iot-architecture"></a>Arquitectura de Azure Defender para IoT

En este artículo se describe la arquitectura del sistema funcional de la solución Azure Defender para IoT. Azure Defender para IoT ofrece dos conjuntos de funcionalidades que se adaptan a las necesidades de su entorno: la solución sin agente para organizaciones y la solución basada en agente para los generadores de dispositivos.

## <a name="agentless-solution-for-organizations"></a>Solución sin agente para organizaciones
### <a name="defender-for-iot-components"></a>Componentes de Azure Defender para IoT

Defender para IoT se conecta a la nube de Azure y a los componentes locales. Esta solución está diseñada para ofrecer escalabilidad en entornos de gran tamaño y distribuidos geográficamente con varias ubicaciones remotas. Habilita una arquitectura distribuida multicapa por país, región, unidad de negocio o zona. 

Azure Defender para IoT incluye los siguientes componentes: 

**Implementaciones conectadas a la nube**

- Dispositivo o máquina virtual de sensor de Azure Defender para IoT
- Azure Portal para la administración en la nube y la integración con Azure Sentinel
- Consola de administración local para la administración del sitio local
- Un agente de seguridad incrustado (opcional)

**Implementaciones aisladas (sin conexión)**

- Dispositivo o máquina virtual de sensor de Azure Defender para IoT
- Consola de administración local para la administración del sitio local

:::image type="content" source="./media/architecture/defender-iot-security-architecture-v3.png" alt-text="Arquitectura de Defender para IoT":::

### <a name="azure-defender-for-iot-sensors"></a>Sensores de Azure Defender para IoT

Los sensores de Defender para IoT detectan y supervisan continuamente los dispositivos de red. Los sensores recopilan el tráfico de ICS mediante la supervisión pasiva (sin agente) en los dispositivos IoT y OT. 
 
Diseñado específicamente para las redes IoT y OT, la tecnología sin agente ofrece una visibilidad exhaustiva de los riesgos de IoT y OT a los pocos minutos de conectarse a la red. No afecta de ninguna manera el rendimiento de la red y los dispositivos de red debido a su enfoque no invasivo de análisis de tráfico (NTA). 
 
Al aprovechar el análisis de comportamiento patentado que toma en cuenta IoT y OT y la inspección de paquetes en profundidad (PPP) de nivel 7, le permite analizar más que las soluciones tradicionales basadas en firmas para detectar de inmediato las amenazas avanzadas de IoT y OT (por ejemplo, malware sin archivos) en función de actividades anómalas o no sin autorización. 
  
Los sensores de Defender para IoT se conectan a un puerto SPAN o a un punto de acceso de terminal de red e inmediatamente comienzan a realizar una inspección exhaustiva de paquetes en el tráfico de IoT y OT. 
 
La recopilación, procesamiento, análisis y alertas de datos tienen lugar directamente en el sensor. Esto lo hace idóneo para las ubicaciones con un ancho de banda reducido o una conectividad de alta latencia, ya que solo los metadatos se transfieren a la consola de administración.

El sensor incluye cinco motores de detección de análisis. Los motores desencadenan alertas basadas en el análisis del tráfico en tiempo real y registrado previamente. Están disponibles los motores siguientes: 

#### <a name="protocol-violation-detection-engine"></a>Motor de detección de infracciones del protocolo
El motor de detección de infracciones del protocolo identifica el uso de estructuras de paquetes y valores de campo que infringen las especificaciones del protocolo ICS, por ejemplo: Excepción Modbus y alertas de iniciación de un código de función obsoleta.

#### <a name="policy-violation-detection-engine"></a>Motor de detección de infracciones de directivas
Con el aprendizaje automático, el motor de detección de infracciones de directivas alerta a los usuarios de cualquier desviación respecto al comportamiento de línea de base, como el uso no autorizado de códigos de función específicos, el acceso a objetos específicos o los cambios en la configuración del dispositivo. Por ejemplo: Cambió la versión de software de DeltaV y alertas de programación de PLC no autorizadas. En concreto, el motor de infracciones de directivas modela las redes ICS como secuencias deterministas de estados y transiciones, mediante una técnica patentada denominada modelado de estado finito industrial (IFSM). El motor de detección de infracciones de directivas establece una línea de base de las redes ICS, de modo que la plataforma requiere un período de aprendizaje más corto para crear una línea de base de la red que los enfoques matemáticos o los análisis genéricos, que se desarrollaron originalmente para TI, en lugar de redes.

#### <a name="industrial-malware-detection-engine"></a>Motor de detección de malware industrial
El motor de detección de malware industrial identifica comportamientos que indican la presencia de malware conocido, como Conficker, Black Energy, Havex, WannaCry, NotPetya y Triton. 

#### <a name="anomaly-detection-engine"></a>Motor de detección de anomalías
El motor de detección de anomalías detecta las comunicaciones de máquina a máquina (M2M) y los comportamientos inusuales. Al modelar redes ICS como secuencias deterministas de estados y transiciones, la plataforma requiere un período de aprendizaje más corto que los enfoques o análisis matemáticos que se desarrollaron originalmente para TI, en lugar de OT. También detecta anomalías más rápido, con un número mínimo de falsos positivos. Las alertas del motor de detección de anomalías incluyen intentos excesivos de inicio de sesión de SMB y alertas de detección de exámenes de PLC.

#### <a name="operational-incident-detection"></a>Detección de incidentes operativos
La detección de incidentes operativos detecta problemas operativos, como la conectividad intermitente, que pueden se indicadores de los primeros síntomas de errores en el equipo. Por ejemplo, se considera que el dispositivo está desconectado (no responde) y se han enviado alertas al comando de detención del PLC de Siemens S7.

### <a name="management-consoles"></a>Consolas de administración
La administración de Azure Defender para IoT en entornos híbridos se realiza a través de dos portales de administración: 
- La consola del sensor
- La consola de administración local
- Azure Portal

### <a name="sensor-console"></a>La consola del sensor
Las detecciones del sensor se muestran en la consola del sensor, donde puede verlas, investigarlas y analizarlas en un mapa de red, un inventario de dispositivos y una amplia variedad de informes, como informes de evaluación de riesgos, consultas de minería de datos y vectores de ataque. También puede usar la consola para ver y administrar las amenazas detectadas por los motores del sensor, reenviar información a sistemas asociados, administrar usuarios, etc.

:::image type="content" source="./media/architecture/sensor-console-v2.png" alt-text="Consola de sensores de Defender para IoT":::

### <a name="on-premises-management-console"></a>Consola de administración local
La consola de administración local permite a los operadores del centro de operaciones de seguridad (SOC) administrar y analizar las alertas agregadas de varios sensores en un solo panel y proporciona una visión general del estado de las redes OT.

Esta arquitectura proporciona una vista unificada completa de la red en el nivel de SOC, un control de alertas optimizado y el control de la seguridad de la red operativa, lo que garantiza que la toma de decisiones y la administración de riesgos se realizan sin errores.

Además de varios inquilinos, supervisión, análisis de datos y control remoto centralizado de sensores, la consola de administración proporciona herramientas de mantenimiento del sistema adicionales (como la exclusión de alertas) y características de presentación de informes totalmente personalizadas para cada uno de los dispositivos remotos. Esta arquitectura escalable es compatible con la administración local en el nivel de sitio, zona y global dentro del SOC.

La consola de administración se puede implementar para la configuración de alta disponibilidad, que proporciona una consola de copia de seguridad que recibe periódicamente copias de seguridad de todos los archivos de configuración necesarios para una recuperación. Si se produce un error en la consola principal, los dispositivos de administración del sitio local se conmutarán por error automáticamente para sincronizarse con la consola de copia de seguridad a fin de mantener la disponibilidad sin interrupciones.

Al estar integrada estrechamente con los runbooks y flujos de trabajo de SOC, facilita la priorización de las actividades de mitigación y la correlación entre sitios de las amenazas.

- Holístico: reduzca la complejidad con una única plataforma unificada para la administración de dispositivos, la administración de riesgos y vulnerabilidades, y la supervisión de amenazas con respuesta a incidentes.

- Agregación y correlación: muestra, agrega y analiza los datos y las alertas recopilados en todos los sitios.

- Control de todos los sensores: configura y supervisa todos los sensores desde una ubicación única.

   :::image type="content" source="media/updates/alerts-and-site-management-v2.png" alt-text="Administración de todas las alertas y la información":::.

### <a name="azure-portal"></a>Azure portal

El portal de Defender para IoT de Azure se usa para ayudarle a lo siguiente:

- Comprar dispositivos de soluciones

- Instalar y actualizar software
- Incorporar sensores a Azure
- Actualizar paquetes de inteligencia sobre amenazas

## <a name="agent-based-solution-for-device-builders"></a>Una solución basada en agente para los generadores de dispositivos

### <a name="embedded-security-agent-built-in-mode"></a>Agente de seguridad incrustado: modo integrado

En el modo **Integrado**, Azure Defender para IoT se habilita al elegir la activación de la opción **Seguridad** en IoT Hub. Con la oferta de alertas, recomendaciones y supervisión en tiempo real, el modo integrado proporciona una seguridad sin precedentes y una visibilidad de los dispositivos en un solo paso. El modo de integración no requiere la instalación del agente en ningún dispositivo y usa el análisis avanzado en las actividades registradas para analizar y proteger el dispositivo de campo y el centro de IoT.

### <a name="embedded-security-agent-enhanced-mode"></a>Agente de seguridad incrustado: modo mejorado

En el modo **Mejorado**, después de activar la opción **Seguridad** en IoT Hub e instalar los agentes de dispositivos de Defender para IoT en los dispositivos, los agentes recopilan, agregan y analizan eventos de seguridad sin procesar de estos. Los eventos de seguridad sin procesar pueden incluir las conexiones de IP, la creación de procesos, los inicios de sesión de los usuarios y otra información relacionada con la seguridad. Además, los agentes de dispositivo de Azure Defender para IoT controlan la agregación de eventos para ayudar a evitar un alta tasa de rendimiento de la red. Los agentes son altamente personalizables, lo que le permite usarlos para tareas específicas, como enviar solo información importante a los SLA más rápidos, o para agregar información de seguridad exhaustiva y contexto en segmentos más grandes, lo que evita mayores costos de servicio.

Los agentes de dispositivos y otra aplicaciones usan el **SDK de Send Security Message de Azure** para enviar información de seguridad a Azure IoT Hub. IoT Hub obtiene esta información y la reenvía al servicio de Defender para IoT.

Una vez que el servicio de Defender para IoT está habilitado, además de los datos reenviados, IoT Hub envía también todos los datos internos para el análisis por parte de Defender para IoT. Estos datos incluyen los registros de operaciones de dispositivos a la nube, las identidades de los dispositivos y la configuración del Hub. Toda esta información ayuda a crear la canalización de análisis de Defender para IoT.

La canalización de análisis de Defender para IoT también recibe flujos adicionales de inteligencia sobre amenazas desde varios orígenes dentro de Microsoft y asociados de Microsoft. La canalización de análisis completa de Defender para IoT funciona con cada configuración del cliente realizada en el servicio (como alertas personalizadas y uso del SDK de Send Security Message).

Con la canalización de análisis, Defender para IoT combina todos los flujos de información para generar alertas y recomendaciones viables. La canalización contiene reglas personalizadas creadas tanto por investigadores de seguridad como por expertos, así como modelos de Machine Learning en busca de desviaciones del comportamiento estándar del dispositivo y análisis de riesgos.

Las recomendaciones y alertas de Defender para IoT (salida de la canalización de análisis) se escriben en el área de trabajo de Log Analytics de cada cliente. Incluir los eventos sin procesar en el área de trabajo, así como las alertas y recomendaciones, permite investigaciones y consultas de profundización con los detalles exactos de las actividades sospechosas detectadas.

:::image type="content" source="media/architecture/micro-agent-architecture.png" alt-text="La arquitectura de micro agente.":::

## <a name="see-also"></a>Consulte también

[Preguntas más frecuentes de Defender para IoT](resources-frequently-asked-questions.md)

[Requisitos previos del sistema](quickstart-system-prerequisites.md)
