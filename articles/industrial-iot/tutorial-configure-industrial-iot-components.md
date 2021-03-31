---
title: Configuración de componentes de Azure Industrial IoT
description: En este tutorial, aprenderá a cambiar los valores predeterminados de la configuración.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: tutorial
ms.date: 3/22/2021
ms.openlocfilehash: 348276a71b2227063374da6455445118fcb00fcf
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104787294"
---
# <a name="tutorial-configure-the-industrial-iot-components"></a>Tutorial: Configuración de componentes de Azure Industrial IoT

El script de implementación configura automáticamente todos los componentes para que funcionen entre sí mediante los valores predeterminados. Sin embargo, se puede cambiar la configuración de los valores predeterminados para satisfacer sus necesidades.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Personalizar la configuración de los componentes


Estos son algunos de los valores de personalización más significativos de los componentes:
* IoT Hub
    * Redes → Acceso público: configure el acceso a Internet, por ejemplo, los filtros IP.
    * Redes → Conexiones de punto de conexión privado: cree un punto de conexión que no sea accesible a través de Internet y que otros servicios de Azure o dispositivos locales puedan utilizar internamente (por ejemplo, mediante una conexión VPN).
    * IoT Edge: administre la configuración de los dispositivos perimetrales que están conectados a los servidores de OPC UA. 
* Cosmos DB
    * Replicación de datos globalmente: configure la redundancia de datos
    * Firewall y redes virtuales: configure Internet y el acceso de red virtual, y los filtros IP.
    * Conexiones de punto de conexión privado: cree un punto de conexión que no sea accesible a través de Internet 
* Key Vault
    * Secretos: administre la configuración de la plataforma.
    * Directivas de acceso: administre qué aplicaciones y usuarios pueden acceder a los datos de Key Vault y qué operaciones (por ejemplo, lectura, escritura, lista, eliminación) se pueden realizar en la red, el firewall, la red virtual y los puntos de conexión privados.
* Azure Active Directory > Registros de aplicaciones
    * <APP_NAME>-web → Autenticación: administre los URI de respuesta, que es la lista de URI que se pueden usar como páginas de aterrizaje después de que la autenticación se realice correctamente. Es posible que el script de implementación no pueda configurar esto automáticamente en ciertos escenarios como, por ejemplo, ante la falta de derechos de administrador de Azure Active Directory. Es posible que desee agregar o modificar los URI al cambiar el nombre de host de la aplicación web, por ejemplo, el número de puerto usado por el localhost para la depuración.
* App Service
    * Configuración: administre las variables de entorno que controlan los servicios o la interfaz de usuario
* Máquina virtual
    * Redes: configure las redes compatibles y las reglas de firewall
    * Consola serie: acceso SSH para obtener información o para depurar, obtener las credenciales de la salida del script de implementación o restablecer la contraseña
* IoT Hub → IoT Edge
    * Administre las identidades de los dispositivos IoT Edge que pueden acceder al centro de conectividad, configure los módulos que se instalan y la configuración que usan, por ejemplo, los parámetros de codificación de OPC Publisher.
* IoT Hub → IoT Edge → \<DEVICE> → Establecer módulos → OpcPublisher (solo para operaciones independientes de OPC Publisher)


## <a name="configuration-options"></a>Opciones de configuración

|Opción de configuración (abreviatura/nombre completo)    |    Descripción   |
|----------------------------------------------|------------------|
pf/publishfile |Nombre de archivo para configurar los nodos que se van a publicar. Si se especifica esta opción, se coloca OPC Publisher en modo independiente.
lf/logfile |El nombre de archivo del archivo de registro que se va a usar
ll/loglevel |Nivel de registro que se va a usar (valores permitidos: grave, error, advertencia, información, depuración, detalle).
me/messageencoding |La codificación de mensajería de los mensajes salientes. Valores permitidos: JSON, Uadp
mm/messagingmode |El modo de mensajería para los mensajes salientes. Valores permitidos: PubSub, Samples
fm/fullfeaturedmessage |El modo completo de los mensajes (todos los campos rellenos). El valor predeterminado es "true", en caso de compatibilidad heredada, use "false"
aa/autoaccept |El publicador es de confianza para todos los servidores a los que se ha conectado
bs/batchsize |El número de mensajes de cambio de datos de OPC UA que se almacenarán en caché para el procesamiento por lotes.
si/iothubsendinterval |El intervalo de procesamiento por lotes del desencadenador en segundos.
ms/iothubmessagesize |El tamaño máximo del mensaje (IoT D2C).
om/maxoutgressmessages |El tamaño máximo del búfer de salida de mensajes (IoT D2C).
di/diagnosticsinterval |Muestra información de diagnóstico del publicador en el intervalo especificado en segundos (se necesita información del nivel de registro). -1 deshabilita el registro de diagnóstico remoto y la salida del diagnóstico
lt/logflugtimespan |El intervalo de tiempo en segundos durante el que se debe vaciar el archivo de registro.
ih/iothubprotocol |Protocolo que se va a usar para la comunicación con el centro de conectividad. Valores permitidos: AmqpOverTcp, AmqpOverWebsocket, MqttOverTcp, MqttOverWebsocket, Amqp, Mqtt, Tcp, Websocket, Any
hb/heartbeatinterval |El publicador lo usa como valor predeterminado en segundos para la configuración del intervalo de latidos de los nodos que aún no tienen esta configuración.
ot/operationtimeout |Tiempo de espera de la operación del cliente OPC UA del publicador en milisegundos.
ol/opcmaxstringlen |La longitud máxima de una cadena que OPC puede transmitir o recibir.
oi/opcsamplinginterval |Valor predeterminado en milisegundos para solicitar a los servidores que muestreen los valores
op/opcpublishinginterval |Valor predeterminado en milisegundos de la configuración del intervalo de publicación de las suscripciones en el servidor de OPC UA.
ct/createsessiontimeout |El intervalo en segundos en que el publicador envía mensajes de mantenimiento de la conexión a los servidores de OPC en los puntos de conexión a los que está conectado.
kt/keepalivethresholt |Especifica el número de paquetes de mantenimiento de la conexión que un servidor puede omitir antes de que se desconecte la sesión.
tm/trustmyself |El certificado del publicador se coloca automáticamente en el almacén de confianza.
at/appcertstoretype |El tipo de almacén de certificados de aplicación propio (valores permitidos: Directory, X509Store).


## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido a cambiar los valores predeterminados de la configuración, puede 

> [!div class="nextstepaction"]
> [Extraer datos de IIoT en Azure Data Explorer](tutorial-industrial-iot-azure-data-explorer.md)

> [!div class="nextstepaction"]
> [Visualizar y analizar los datos mediante Time Series Insights](tutorial-visualize-data-time-series-insights.md)
