---
title: Filtros de seguridad para limitar los resultados mediante Active Directory
titleSuffix: Azure Cognitive Search
description: Aprenda a implementar privilegios de seguridad en el nivel de documento de los resultados de búsqueda de Azure Cognitive Search mediante filtros de seguridad y las identidades de Azure Active Directory (AD).
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/16/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 5788585b2365b12a90a508e5a972b61f73e48c15
ms.sourcegitcommit: 8c3a656f82aa6f9c2792a27b02bbaa634786f42d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97629517"
---
# <a name="security-filters-for-trimming-azure-cognitive-search-results-using-active-directory-identities"></a>Filtros de seguridad para limitar los resultados de Azure Cognitive Search mediante las identidades de Active Directory

En este artículo se muestra cómo usar las identidades de seguridad de Azure Active Directory (AD) junto con los filtros de Azure Cognitive Search para limitar los resultados de búsqueda en función de la pertenencia a grupos de usuarios.

En este artículo se tratan las tareas siguientes:
> [!div class="checklist"]
> - Creación de usuarios y grupos de Azure AD
> - Asociación del usuario con el grupo que ha creado
> - Almacenamiento en caché de nuevos grupos
> - Indexación de documentos con grupos asociados
> - Emisión de una solicitud de búsqueda con filtro de identificadores de grupo
> 
> [!NOTE]
> Los fragmentos de código de ejemplo de este artículo están escritos en C#. El código fuente completo se puede encontrar [en GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started). 

## <a name="prerequisites"></a>Requisitos previos

El índice de Azure Cognitive Search debe tener un [campo de seguridad](search-security-trimming-for-azure-search.md) para almacenar la lista de identidades de grupo que tengan acceso de lectura al documento. Este caso de uso supone una correspondencia uno a uno entre un elemento protegible (por ejemplo, la solicitud de la universidad de un individuo) y un campo de seguridad que especifica quién tiene acceso a ese elemento (personal de admisión).

Debe tener permisos de administrador de Azure AD, que se necesitan en este tutorial para crear usuarios, grupos y asociaciones. 

La solicitud debe registrarse en Azure AD, como aplicación multiinquilino, como se describe en el siguiente procedimiento.

### <a name="register-your-application-with-azure-active-directory"></a>Registro de la aplicación en Azure Active Directory

Este paso integra la aplicación con Azure AD con el fin de aceptar inicios de sesión de cuentas de usuario y grupo. Si no es administrador de inquilinos en su organización, es posible que tenga que [crear un inquilino](../active-directory/develop/quickstart-create-new-tenant.md) para realizar los pasos siguientes.

1. En [Azure Portal](https://portal.azure.com), busque el recurso de Azure Active Directory de su suscripción.

1. A la izquierda, en **Administrar**, seleccione **Registros de aplicaciones** y, luego, elija **Nuevo registro**.

1. Asigne un nombre al registro; podría ser uno similar al de la aplicación de búsqueda. Seleccione **Registrar**.

1. Una vez creado el registro de aplicaciones, copie el identificador de la aplicación. Tendrá que proporcionar esta cadena a la aplicación.

   Si va a recorrer paso a paso [DotNetHowToSecurityTrimming](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToEncryptionUsingCMK), pegue este valor en el archivo **app.config**.

   Repítalo para el identificador de inquilino.

   :::image type="content" source="media/search-manage-encryption-keys/cmk-application-id.png" alt-text="Identificador de la aplicación en la sección Información esencial":::

1. En el lado izquierdo, seleccione **Permisos de API** y, después, **Agregar un permiso**. 

1. Seleccione **Microsoft Graph** y, luego, **Permisos delegados**.

1. Busque y agregue los siguientes permisos delegados:

   - **Directory.ReadWrite.All**
   - **Group.ReadWrite.All**
   - **User.ReadWrite.All**

Microsoft Graph proporciona una API que permite el acceso mediante programación a Azure AD a través de una API de REST. El ejemplo de código para este tutorial usa los permisos para llamar a Microsoft Graph API para la creación de grupos, usuarios y asociaciones. Las API también se usan para almacenar en caché identificadores de grupo de caché para filtrar con mayor rapidez.

## <a name="create-users-and-groups"></a>Creación de usuarios y grupos

Si va a agregar la búsqueda a una solicitud establecida, podría tener identificadores de grupo y usuarios existentes en Azure AD. En este caso, puede omitir los tres pasos siguientes. 

Sin embargo, si no tiene usuarios existentes, puede utilizar Microsoft Graph API para crear las entidades de seguridad. Los siguientes fragmentos de código muestran cómo generar identificadores, que se convierten en valores de datos para el campo de seguridad en el índice de Azure Cognitive Search. En nuestra hipotética solicitud de admisión de la universidad, esto serían los identificadores de seguridad para el personal de admisión.

La pertenencia de usuarios y grupos podría ser muy fluida, especialmente en grandes organizaciones. El código que crea identidades de usuario y grupo debe ejecutarse con la frecuencia suficiente como para recoger los cambios en la pertenencia de la organización. Del mismo modo, el índice de Azure Cognitive Search requiere una programación de actualización similar para reflejar el estado actual de recursos y usuarios permitidos.

### <a name="step-1-create-group"></a>Paso 1: [Crear grupo](/graph/api/group-post-groups) 

```csharp
private static Dictionary<Group, List<User>> CreateGroupsWithUsers(string tenant)
{
    Group group = new Group()
    {
        DisplayName = "My First Prog Group",
        SecurityEnabled = true,
        MailEnabled = false,
        MailNickname = "group1"
    };
```

### <a name="step-2-create-user"></a>Paso 2: [Crear usuario](/graph/api/user-post-users)

```csharp
User user1 = new User()
{
    GivenName = "First User",
    Surname = "User1",
    MailNickname = "User1",
    DisplayName = "First User",
    UserPrincipalName = String.Format("user1@{0}", tenant),
    PasswordProfile = new PasswordProfile() { Password = "********" },
    AccountEnabled = true
};
```

### <a name="step-3-associate-user-and-group"></a>Paso 3: Asociar el usuario y el grupo

```csharp
List<User> users = new List<User>() { user1, user2 };
Dictionary<Group, List<User>> groups = new Dictionary<Group, List<User>>() { { group, users } };
```

### <a name="step-4-cache-the-groups-identifiers"></a>Paso 4: Almacenar los identificadores de grupos en caché

Si lo desea, para reducir la latencia de red, puede almacenar en caché las asociaciones entre usuarios y grupos para que cuando se emita una solicitud de búsqueda, los grupos se devuelven desde la memoria caché, ahorrando un ida y vuelta a Azure AD. Puede usar la [Batch API de Azure AD](/graph/json-batching) para enviar una solicitud HTTP única con varios usuarios y crear la caché.

Microsoft Graph se ha diseñado para controlar un alto volumen de solicitudes. Si se produce un número excesivo de solicitudes, Microsoft Graph produce un error en la solicitud con el código de estado HTTP 429. Para más información, consulte [Guía de limitación de Microsoft Graph](/graph/throttling).

## <a name="index-document-with-their-permitted-groups"></a>Documento de índice con sus grupos permitidos

Las operaciones de consulta en Azure Cognitive Search se ejecutan sobre un índice de Azure Cognitive Search. En este paso, una operación de indexación importa datos que permiten realizar búsquedas en un índice, incluidos los identificadores usados como filtros de seguridad. 

Azure Cognitive Search no autentica identidades de usuario ni proporciona lógica para establecer el contenido que un usuario tiene permiso para ver. El caso de uso para el recorte de seguridad asume que se proporciona la asociación entre un documento confidencial y el identificador del grupo que tiene acceso a ese documento, que se ha importado intacto en un índice de búsqueda. 

En el ejemplo hipotético, el cuerpo de la solicitud PUT en un índice de Azure Cognitive Search incluiría el trabajo o la transcripción de la universidad de un solicitante junto con el identificador de grupo que tiene permiso para ver el contenido. 

En el ejemplo genérico utilizado en el ejemplo de código para este tutorial, la acción del índice podría ser la siguiente:

```csharp
private static void IndexDocuments(string indexName, List<string> groups)
{
    IndexDocumentsBatch<SecuredFiles> batch = IndexDocumentsBatch.Create(
        IndexDocumentsAction.Upload(
            new SecuredFiles()
            {
                FileId = "1",
                Name = "secured_file_a",
                GroupIds = new[] { groups[0] }
            }),
              ...
            };

IndexDocumentsResult result = searchClient.IndexDocuments(batch);
```

## <a name="issue-a-search-request"></a>Emisión de una solicitud de búsqueda

Por motivos de recorte de seguridad, los valores del campo de seguridad en el índice son valores estáticos que se usan para incluir o excluir documentos en los resultados de la búsqueda. Por ejemplo, si el identificador de grupo de admisiones es "A11B22C33D44-E55F66G77-H88I99JKK", todos los documentos de un índice de Azure Cognitive Search que tengan ese identificador en el campo de seguridad se incluyen (o excluyen) en los resultados de búsqueda devueltos al solicitante.

Para filtrar los documentos devueltos en los resultados de búsqueda en función de los grupos de usuario que emiten la solicitud, revise los siguientes pasos.

### <a name="step-1-retrieve-users-group-identifiers"></a>Paso 1: Recuperar identificadores de grupo del usuario

Si los grupos del usuario todavía no estaban almacenados en la caché o la memoria caché ha expirado, emita la solicitud de [grupos](/graph/api/directoryobject-getmembergroups).

```csharp
private static async void RefreshCache(IEnumerable<User> users)
{
    HttpClient client = new HttpClient();
    var userGroups = await _microsoftGraphHelper.GetGroupsForUsers(client, users);
    _groupsCache = new ConcurrentDictionary<string, List<string>>(userGroups);
}
```

### <a name="step-2-compose-the-search-request"></a>Paso 2: Crear la solicitud de búsqueda

Suponiendo que tiene pertenencia a grupos del usuario, puede emitir la solicitud de búsqueda con los valores de filtro apropiados.

```csharp
private static void SearchQueryWithFilter(string user)
{
    // Using the filter below, the search result will contain all documents that their GroupIds field   
    // contain any one of the Ids in the groups list
    string filter = String.Format("groupIds/any(p:search.in(p, '{0}'))", string.Join(",", String.Join(",", _groupsCache[user])));
    SearchOptions searchOptions =
        new SearchOptions()
        {
            Filter = filter
        };
    searchOptions.Select.Add("name");

    SearchResults<SecuredFiles> results = searchClient.Search<SecuredFiles>("*", searchOptions);

    Console.WriteLine("Results for groups '{0}' : {1}", _groupsCache[user], results.GetResults().Select(r => r.Document.Name));
}
```

### <a name="step-3-handle-the-results"></a>Paso 3: Controlar los resultados

La respuesta incluye una lista filtrada de documentos, que consta de los que el usuario tiene permiso para ver. Dependiendo de cómo se cree la página de resultados de búsqueda, puede que desee incluir indicaciones visuales para reflejar el conjunto de resultados filtrados.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido un patrón para usar inicios de sesión de Azure AD para filtrar documentos en los resultados de Azure Cognitive Search, recortando los resultados de los documentos que no coinciden con el filtro proporcionado en la solicitud. Para ver un patrón alternativo que es posible que sea más sencillo o para volver a consultar otras características de seguridad, consulte los vínculos siguientes.

- [Filtros de seguridad para restringir los resultados](search-security-trimming-for-azure-search.md)
- [Seguridad en Azure Cognitive Search](search-security-overview.md)