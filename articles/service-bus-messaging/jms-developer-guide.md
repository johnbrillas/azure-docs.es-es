---
title: Guía del desarrollador de JMS 2.0 para Azure Service Bus
description: Aprenda a usar la API de Java Message Service (JMS) 2.0 para comunicarse con Azure Service Bus.
ms.topic: article
ms.date: 01/17/2021
ms.openlocfilehash: 492da46a5ce4f5955a72972d91a8c56d54c385bb
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101726961"
---
# <a name="azure-service-bus-jms-20-developer-guide"></a>Guía del desarrollador de JMS 2.0 para Azure Service Bus

Esta guía contiene información detallada para ayudarle a comunicarse correctamente con Azure Service Bus mediante la API de Java Message Service (JMS) 2.0.

Como desarrollador de Java, si no está familiarizado con Azure Service Bus, considere la posibilidad de leer los artículos siguientes.

| Introducción | Conceptos |
|----------------|-------|
| <ul> <li> [Qué es Azure Service Bus](service-bus-messaging-overview.md) </li> <li> [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md) </li> </ul> | <ul> <li> [Niveles de mensajería Premium y Estándar de Service Bus](service-bus-premium-messaging.md) </li> </ul> |

## <a name="java-message-service-jms-programming-model"></a>Modelo de programación de Java Message Service (JMS)

El modelo de programación de la API de Java Message Service es como se muestra a continuación: 

> [!NOTE]
>
>El **nivel Premium de Azure Service Bus** admite JMS 1.1 y JMS 2.0.
> <br> <br>
> El **nivel estándar de Azure Service Bus** admite la funcionalidad de JMS 1.1 limitada. Para más información, consulte esta [documentación](service-bus-java-how-to-use-jms-api-amqp.md).
>

# <a name="jms-20-programming-model"></a>[Modelo de programación de JMS 2.0](#tab/JMS-20)

:::image type="content" source="./media/jms-developer-guide/java-message-service-20-programming-model.png "alt-text="Diagrama que muestra el modelo de programación de JMS 2.0" border="false":::

# <a name="jms-11-programming-model"></a>[Modelo de programación de JMS 1.1](#tab/JMS-11)

:::image type="content" source="./media/jms-developer-guide/java-message-service-11-programming-model.png "alt-text="Diagrama que muestra el modelo de programación de JMS 1.1" border="false":::

---

## <a name="jms---building-blocks"></a>JMS: bloques de creación

Los siguientes bloques de creación están disponibles para comunicarse con la aplicación JMS.

> [!NOTE]
> La guía siguiente se ha adaptado del [tutorial de Oracle Java EE 6 para Java Message Service (JMS)](https://docs.oracle.com/javaee/6/tutorial/doc/bnceh.html).
>
> Se recomienda consultar este tutorial para comprender mejor el modelo Java Message Service (JMS).
>

### <a name="connection-factory"></a>Generador de conexiones
El cliente utiliza el objeto de generador de conexiones para conectar con el proveedor de JMS. El generador de conexiones encapsula un conjunto de parámetros de configuración de conexión definidos por el administrador.

Cada generador de conexiones es una instancia de la interfaz `ConnectionFactory`, `QueueConnectionFactory` o `TopicConnectionFactory`.

Para simplificar la conexión con Azure Service Bus, estas interfaces se implementan a través de `ServiceBusJmsConnectionFactory`, `ServiceBusJmsQueueConnectionFactory` y, `ServiceBusJmsTopicConnectionFactory`, respectivamente. Se puede crear una instancia del generador de conexiones con los parámetros siguientes: 
   * Cadena de conexión: la cadena de conexión para el espacio de nombres de nivel Premium de Azure Service Bus
   * Contenedor de propiedades ServiceBusJmsConnectionFactorySettings que contiene
      * connectionIdleTimeoutMS: tiempo de espera de conexión inactivo en milisegundos
      * traceFrames: marca booleana para recopilar fotogramas de seguimiento de AMQP para la depuración
      * *Otros parámetros de configuración*

El generador puede crearse como se indica a continuación. La cadena de conexión es un parámetro necesario, pero las propiedades adicionales son opcionales.

```java
ConnectionFactory factory = new ServiceBusJmsConnectionFactory(SERVICE_BUS_CONNECTION_STRING, null);
```

> [!IMPORTANT]
> Las aplicaciones Java que utilizan la API de JMS 2.0 deben conectarse a Azure Service Bus solo con la cadena de conexión. Actualmente, la autenticación para los clientes de JMS solo se admite mediante la cadena de conexión.
>
> No se admite de momento la autenticación respaldada de Azure Active Directory (AAD).
>

### <a name="jms-destination"></a>Destino de JMS

Un destino es el objeto que utiliza un cliente para especificar el destino de los mensajes que genera y el origen de los mensajes que consume.

Los destinos se asignan a entidades de Azure Service Bus: colas (en escenarios de punto a punto) y temas (en escenarios de publicación y suscripción).

### <a name="connections"></a>Conexiones

Una conexión encapsula una conexión virtual con un proveedor de JMS. Con Azure Service Bus, esto representa una conexión con estado entre la aplicación y Azure Service Bus mediante AMQP.

Se crea una conexión desde el generador de conexiones, tal y como se muestra a continuación.

```java
Connection connection = factory.createConnection();
```

### <a name="sessions"></a>Sesiones

Una sesión es un contexto de un solo subproceso para generar y consumir mensajes. Se puede utilizar para crear mensajes, productores de mensajes y consumidores, pero también proporciona un contexto transaccional para permitir la agrupación de envíos y recepciones en una unidad atómica de trabajo.

Se puede crear una sesión a partir del objeto de conexión, como se muestra a continuación.

```java
Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
```

#### <a name="session-modes"></a>Modos de sesión

Una sesión se puede crear con cualquiera de los siguientes modos.

| Modos de sesión | Comportamiento |
| ----------------- | -------- |
|**Session.AUTO_ACKNOWLEDGE** | La sesión confirma automáticamente la recepción de un mensaje de un cliente, ya sea cuando la sesión ha regresado correctamente de una llamada de recepción o cuando el cliente de escucha del mensaje al que la sesión ha llamado para procesar el mensaje regresa correctamente.|
|**Session.CLIENT_ACKNOWLEDGE** |El cliente confirma un mensaje consumido mediante la llamada al método de confirmación del mensaje.|
|**Session.DUPS_OK_ACKNOWLEDGE**|Este modo de confirmación indica a la sesión que confirme de forma diferida la entrega de mensajes.| 
|**Session.SESSION_TRANSACTED**|Este valor se puede pasar como argumento al método createSession(int sessionMode) en el objeto de conexión para especificar que la sesión debe utilizar una transacción local.| 

Cuando no se especifica el modo de sesión, se selecciona **Session.AUTO_ACKNOWLEDGE** de forma predeterminada.

### <a name="jmscontext"></a>JMSContext

> [!NOTE]
> JMSContext se define como parte de la especificación de JMS 2.0.
>

JMSContext combina la funcionalidad proporcionada por el objeto de conexión y de sesión. Se puede crear a partir del objeto generador de conexiones.

```java
JMSContext context = connectionFactory.createContext();
```

#### <a name="jmscontext-modes"></a>Modos de JMSContext

Al igual que el objeto de **sesión**, JMSContext se puede crear con los mismos modos de confirmación que se mencionan en los [modos de sesión](#session-modes).

```java
JMSContext context = connectionFactory.createContext(JMSContext.AUTO_ACKNOWLEDGE);
```

Cuando no se especifica el modo, se selecciona **JMSContext.AUTO_ACKNOWLEDGE** de forma predeterminada.

### <a name="jms-message-producers"></a>Productores de mensajes JMS

Un productor de mensajes es un objeto que se crea mediante JMSContext o una sesión y se utiliza para enviar mensajes a un destino.

Se puede crear como un objeto independiente, tal como se indica a continuación: 

```java
JMSProducer producer = context.createProducer();
```

o crearse en tiempo de ejecución cuando es necesario enviar un mensaje.

```java
context.createProducer().send(destination, message);
```

### <a name="jms-message-consumers"></a>Consumidores de mensajes de JMS

Un consumidor de mensajes es un objeto que se crea mediante JMSContext o una sesión y se usa para recibir los mensajes enviados a un destino. Se puede crear tal y como se muestra a continuación:

```java
JMSConsumer consumer = context.createConsumer(dest);
```

#### <a name="synchronous-receives-via-receive-method"></a>Recepciones sincrónicas mediante el método receive()

El consumidor de mensajes proporciona una manera sincrónica de recibir mensajes del destino mediante el método `receive()`.

Si no se especifica ningún argumento o tiempo de espera, o se especifica un tiempo de espera de "0", el consumidor se bloquea indefinidamente a menos que llegue el mensaje o se interrumpa la conexión (lo que sea anterior).

```java
Message m = consumer.receive();
Message m = consumer.receive(0);
```

Cuando se proporciona un argumento positivo distinto de cero, el consumidor se bloquea hasta que el temporizador expira.

```java
Message m = consumer.receive(1000); // time out after one second.
```

#### <a name="asynchronous-receives-with-jms-message-listeners"></a>Recepciones asincrónicas con clientes de escucha de mensajes JMS

Un cliente de escucha de mensajes es un objeto que se usa para el control asincrónico de los mensajes en un destino. Implementa la interfaz `MessageListener` que contiene el método `onMessage` en el que debe residir la lógica de negocios específica.

Se debe crear una instancia de un objeto del cliente de escucha de mensajes y registrarla en un consumidor de mensajes específico mediante el método `setMessageListener`.

```java
Listener myListener = new Listener();
consumer.setMessageListener(myListener);
```

### <a name="consuming-from-topics"></a>Consumo desde temas

Los [consumidores de mensajes JMS](#jms-message-consumers) se crean en un [destino](#jms-destination) que puede ser una cola o un tema.

Los consumidores en las colas son simplemente objetos del lado cliente que residen en el contexto de la sesión (y la conexión) entre la aplicación cliente y Azure Service Bus.

Sin embargo, los consumidores en los temas tienen dos partes: 
   * Un **objeto del lado cliente** que reside en el contexto de la sesión (o JMSContext) y
   * una **suscripción** que es una entidad en Azure Service Bus.

Las suscripciones se documentan [aquí](java-message-service-20-entities.md#java-message-service-jms-subscriptions) y pueden ser una de las siguientes: 
   * Suscripciones duraderas compartidas
   * Suscripciones no duraderas compartidas
   * Suscripciones duraderas no compartidas
   * Suscripciones no duraderas no compartidas

### <a name="jms-queue-browsers"></a>Exploradores de colas de JMS

La API de JMS proporciona un objeto `QueueBrowser` que permite a la aplicación examinar los mensajes de la cola y mostrar los valores de encabezado de cada mensaje.

Se puede crear un explorador de cola mediante JMSContext como se indica a continuación.

```java
QueueBrowser browser = context.createBrowser(queue);
```

> [!NOTE]
> La API de JMS no proporciona una API para examinar un tema.
>
> Esto se debe a que el propio tema no almacena los mensajes. En cuanto se envía el mensaje al tema, se reenvía a las suscripciones adecuadas.
>

### <a name="jms-message-selectors"></a>Selectores de mensajes de JMS

Se pueden usar los selectores de mensajes al recibir aplicaciones para filtrar los mensajes que se reciben. Con los selectores de mensajes, la aplicación receptora descarga el trabajo de filtrado de los mensajes al proveedor JMS (en este caso, Azure Service Bus) en lugar de asumir esa responsabilidad.

Los selectores se pueden usar al crear cualquiera de los siguientes consumidores: 
   * Suscripciones duraderas compartidas
   * Suscripciones duraderas no compartidas
   * Suscripciones no duraderas compartidas
   * Suscripciones no duraderas no compartidas
   * Explorador de colas


## <a name="summary"></a>Resumen

En esta guía para desarrolladores se muestra cómo las aplicaciones cliente de Java que usan Java Message Service (JMS) pueden interactuar con Azure Service Bus.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Service Bus y las entidades de Java Message Service (JMS), consulte los vínculos siguientes: 
* [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md)
* [Service Bus: entidades de Java Message Service](service-bus-queues-topics-subscriptions.md#java-message-service-jms-20-entities)
* [Compatibilidad de AMQP 1.0 en Azure Service Bus](service-bus-amqp-overview.md)
* [Guía para desarrolladores sobre AMQP 1.0 de Service Bus](service-bus-amqp-dotnet.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [API de Java Message Service (documento de Oracle externo)](https://docs.oracle.com/javaee/7/api/javax/jms/package-summary.html)
* [Migración de ActiveMQ a Service Bus](migrate-jms-activemq-to-servicebus.md)
