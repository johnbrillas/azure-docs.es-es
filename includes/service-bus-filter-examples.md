---
title: archivo de inclusión
description: archivo de inclusión
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 01/22/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7ddc0234accd9f434aad850d2404e2f3001c4bae
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98742701"
---
## <a name="examples"></a>Ejemplos

### <a name="filter-on-system-properties"></a>Filtrado por propiedades del sistema
Para hacer referencia a una propiedad del sistema en un filtro, utilice el siguiente formato: `sys.<system-property-name>`. 

```csharp
sys.Label LIKE '%bus%'`
sys.messageid = 'xxxx'
sys.correlationid like 'abc-%'
```

### <a name="filter-on-message-properties"></a>Filtrado por propiedades de mensaje
Estos son ejemplos de cómo usar las propiedades del mensaje en un filtro. Puede acceder a las propiedades del mensaje mediante `user.property-name` o simplemente `property-name`.

```csharp
MessageProperty = 'A'
SuperHero like 'SuperMan%'
```

### <a name="filter-on-message-properties-with-special-characters"></a>Filtrado por propiedades del mensaje con caracteres especiales
Si el nombre de la propiedad del mensaje tiene caracteres especiales, use comillas dobles (`"`) para incluir el nombre de la propiedad. Por ejemplo, si el nombre de la propiedad es `"http://schemas.microsoft.com/xrm/2011/Claims/EntityLogicalName"`, use la sintaxis siguiente en el filtro. 

```csharp
"http://schemas.microsoft.com/xrm/2011/Claims/EntityLogicalName" = 'account'
```

### <a name="filter-on-message-properties-with-numeric-values"></a>Filtrado por propiedades del mensaje con valores numéricos
En los siguientes ejemplos se muestra cómo se pueden usar las propiedades con valores numéricos en los filtros. 

```csharp
MessageProperty = 1
MessageProperty > 1
MessageProperty > 2.08
MessageProperty = 1 AND MessageProperty2 = 3
MessageProperty = 1 OR MessageProperty2 = 3
```

### <a name="parameter-based-filters"></a>Filtros basados en parámetros
Estos son algunos ejemplos de cómo usar filtros basados en parámetros. En estos ejemplos, `DataTimeMp` es una propiedad del mensaje de tipo `DateTime` y `@dtParam` es un parámetro que se pasa al filtro como un objeto `DateTime`.

```csharp
DateTimeMp < @dtParam
DateTimeMp > @dtParam

(DateTimeMp2-DateTimeMp1) <= @timespan //@timespan is a parameter of type TimeSpan
DateTimeMp2-DateTimeMp1 <= @timespan
```

### <a name="using-in-and-not-in"></a>Usar IN y NOT IN

```csharp
StoreId IN('Store1', 'Store2', 'Store3')"

sys.To IN ('Store5','Store6','Store7') OR StoreId = 'Store8'

sys.To NOT IN ('Store1','Store2','Store3','Store4','Store5','Store6','Store7','Store8') OR StoreId NOT IN ('Store1','Store2','Store3','Store4','Store5','Store6','Store7','Store8')
```

Para ver un ejemplo de C#, consulte el [ejemplo de filtros de temas en GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Azure.Messaging.ServiceBus/BasicSendReceiveTutorialwithFilters).


### <a name="set-rule-action-for-a-sql-filter"></a>Establecimiento de una acción de regla para un filtro SQL

```csharp
// instantiate the ManagementClient
this.mgmtClient = new ManagementClient(connectionString);

// create the SQL filter
var sqlFilter = new SqlFilter("source = @stringParam");

// assign value for the parameter
sqlFilter.Parameters.Add("@stringParam", "orders");

// instantiate the Rule = Filter + Action
var filterActionRule = new RuleDescription
{
    Name = "filterActionRule",
    Filter = sqlFilter,
    Action = new SqlRuleAction("SET source='routedOrders'")
};

// create the rule on Service Bus
await this.mgmtClient.CreateRuleAsync(topicName, subscriptionName, filterActionRule);
```

### <a name="correlation-filter-using-correlationid"></a>Filtro de correlación con CorrelationID

```csharp
new CorrelationFilter("Contoso");
```

Filtra los mensajes con `CorrelationID` establecido en `Contoso`. 

### <a name="correlation-filter-using-system-and-user-properties"></a>Filtro de correlación con propiedades del sistema y del usuario

```csharp
var filter = new CorrelationFilter();
filter.Label = "Important";
filter.ReplyTo = "johndoe@contoso.com";
filter.Properties["color"] = "Red";
```

Es equivalente a: `sys.ReplyTo = 'johndoe@contoso.com' AND sys.Label = 'Important' AND color = 'Red'`.

