---
title: 'Configuración de un servicio QnA Maker: QnA Maker'
description: Antes de crear alguna base de conocimiento de QnA Maker, primero debe configurar un servicio QnA Maker en Azure. Cualquiera que tenga autorización para crear recursos en una suscripción puede configurar un servicio QnA Maker.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: 7137b26dcf951f98473f0fcc139f563438ce8878
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102203477"
---
# <a name="manage-qna-maker-resources"></a>Administración de recursos de QnA Maker

Antes de crear alguna base de conocimiento de QnA Maker, primero debe configurar un servicio QnA Maker en Azure. Cualquiera que tenga autorización para crear recursos en una suscripción puede configurar un servicio QnA Maker.

Antes de crear el recurso, es útil comprender perfectamente los conceptos siguientes:

* [Recursos de QnA Maker](../Concepts/azure-resources.md)
* [Creación y publicación de claves](../Concepts/azure-resources.md#keys-in-qna-maker)

## <a name="create-a-new-qna-maker-service"></a>Creación de un servicio QnA Maker

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

En este procedimiento se crean los recursos de Azure necesarios para administrar el contenido de la base de conocimiento. Después de completar estos pasos, encontrará las claves de _suscripción_ en la página **Claves** del recurso en Azure Portal.

1. Inicie sesión en Azure Portal y [cree un recurso de QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker).

1. Después de leer los términos y condiciones, seleccione **Crear**:

    ![Creación de un servicio QnA Maker](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. En **QnA Maker**, seleccione las regiones y los niveles apropiados:

    ![Creación de un nuevo servicio QnA Maker: plan de tarifa y regiones](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * En el campo **Nombre**, escriba un nombre único que identifique este servicio QnA Maker. Este nombre también identifica el punto de conexión de QnA Maker con el que se asociarán las bases de conocimiento.
    * En **Suscripción**, elija la suscripción en la que se implementará el recurso de QnA Maker.
    * Seleccione el **plan de tarifa** para los servicios de administración de QnA Maker (API de portal y administración). Vea [más detalles sobre los precios de las SKU](https://aka.ms/qnamaker-pricing).
    * Cree un **grupo de recursos** (se recomienda) para implementar este recurso de QnA Maker, o use uno existente. QnA Maker crea varios recursos de Azure. Cuando se crea un grupo de recursos que contenga estos recursos, puede encontrar, administrar y eliminar fácilmente estos recursos por su nombre.
    * Seleccione una **ubicación del grupo de recursos**.
    * Elija el **Plan de tarifa de búsqueda** del servicio Azure Cognitive Search. Si la opción de nivel Gratis no está disponible (aparece atenuada), significa que ya tiene un servicio gratuito implementado a través de la suscripción. En ese caso, tendrá que comenzar con el nivel Básico. Consulte los [detalles de precios de Azure Cognitive Search](https://azure.microsoft.com/pricing/details/search/).
    * Elija la **Ubicación de búsqueda** en la que desea que se implementen los índices de Azure Cognitive Search. Las restricciones sobre dónde deben almacenarse los datos de los clientes ayudarán a determinar la ubicación elegida para Azure Cognitive Search.
    * En el campo **Nombre de aplicación**, escriba el nombre de la instancia de Azure App Service.
    * De forma predeterminada, el nivel predeterminado de App Service es Estándar (S1). Puede cambiar el plan después de la creación. Más información sobre los [precios de App Service](https://azure.microsoft.com/pricing/details/app-service/).
    * Elija la **ubicación de sitio web** en la que se implementará App Service.

        > [!NOTE]
        > La **ubicación de la búsqueda** puede ser diferente de la **ubicación del sitio web**.

    * Elija si desea habilitar **Application Insights**. Si **Application Insights** está habilitado, QnA Maker recopila telemetría de tráfico, registros de chat y errores.
    * Elija la **ubicación de App Insights** en la que se implementará el recurso de Application Insights.
    * Como medidas de ahorro de los costos, puede [compartir](#configure-qna-maker-to-use-different-cognitive-search-resource) algunos pero no todos los recursos de Azure creados para QnA Maker.

1. Una vez que se hayan validado todos los campos, seleccione **Crear**. El proceso puede tardar unos minutos en completarse.

1. Después de que se complete la implementación, verá que los siguientes recursos se crean en la suscripción:

   ![Recurso creado de un nuevo servicio QnA Maker](../media/qnamaker-how-to-setup-service/resources-created.png)

    El recurso con el tipo _Cognitive Services_ tiene las claves de la _suscripción_.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

En este procedimiento se crean los recursos de Azure necesarios para administrar el contenido de la base de conocimiento. Después de completar estos pasos, encontrará las claves de *suscripción* en la página **Claves** del recurso en Azure Portal.

1. Inicie sesión en Azure Portal y [cree un recurso de QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker).

1. Después de leer los términos y condiciones, seleccione **Crear**:

    ![Creación de un servicio QnA Maker](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. En **QnA Maker**, active la casilla Administrado (versión preliminar) y seleccione los niveles y regiones apropiados:

    ![Creación de un servicio QnA Maker administrado: plan de tarifa y regiones](../media/qnamaker-how-to-setup-service/enter-qnamaker-v2-info.png)

    * En **Suscripción**, elija la suscripción en la que se implementará el recurso de QnA Maker.
    * Cree un **grupo de recursos** (se recomienda) o use uno existente en el que implementar este recurso de QnA Maker administrado (versión preliminar). QnA Maker administrado (versión preliminar) crea pocos recursos de Azure. Cuando se crea un grupo de recursos que contenga estos recursos, puede encontrar, administrar y eliminar fácilmente estos recursos por su nombre.
    * En el campo **Nombre**, escriba un nombre único que identifique este servicio QnA Maker administrado (versión preliminar). 
    * Elija la **ubicación** en la que desea que se implemente el servicio QnA Maker administrado (versión preliminar). Tanto las API de administración como el punto de conexión de servicio se hospedarán en esta ubicación. 
    * Seleccione el **plan de tarifa** del servicio QnA Maker administrado (versión preliminar) (gratis para la versión preliminar). Vea [más detalles sobre los precios de las SKU](https://aka.ms/qnamaker-pricing).
    * Elija la **Ubicación de búsqueda** en la que desea que se implementen los índices de Azure Cognitive Search. Las restricciones sobre dónde deben almacenarse los datos de los clientes ayudarán a determinar la ubicación elegida para Azure Cognitive Search.
    * Elija el **Plan de tarifa de búsqueda** del servicio Azure Cognitive Search. Si la opción de nivel Gratis no está disponible (aparece atenuada), significa que ya tiene un servicio gratuito implementado a través de la suscripción. En ese caso, tendrá que comenzar con el nivel Básico. Consulte los [detalles de precios de Azure Cognitive Search](https://azure.microsoft.com/pricing/details/search/).

1. Una vez que se hayan validado todos los campos, seleccione **Revisar y crear**. El proceso puede tardar unos minutos en completarse.

1. Después de que se complete la implementación, verá que los siguientes recursos se crean en la suscripción:

    ![El recurso ha creado un servicio QnA Maker administrado (versión preliminar)](../media/qnamaker-how-to-setup-service/resources-created-v2.png)

    El recurso con el tipo _Cognitive Services_ tiene las claves de la _suscripción_.

---

## <a name="upgrade-azure-resources"></a>Actualización de recursos de Azure

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

### <a name="upgrade-qna-maker-sku"></a>Actualización de la SKU de QnA Maker

Cuando desee tener más preguntas y respuestas en su base de conocimiento, más allá de su plan actual, actualice el plan de tarifa del servicio de QnA Maker.

Para actualizar la SKU de administración de QnA Maker:

1. Vaya al recurso QnA Maker en Azure Portal y seleccione **Plan de tarifa**.

    ![Recurso QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Elija la SKU adecuada y presione **Seleccionar**.

    ![Precios de QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)
    
### <a name="upgrade-app-service"></a>Actualización de App Service

Cuando la base de conocimiento necesite atender más solicitudes de la aplicación cliente, actualice el plan de tarifa de App Service.

Puede [escalar verticalmente](../../../app-service/manage-scale-up.md) o escalar horizontalmente App Service.

Vaya al recurso App Service en Azure Portal y seleccione la opción **Escalar verticalmente** o **Escalar horizontalmente** según proceda.

![Escalado de App Service en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Actualización del servicio Azure Cognitive Search

Si planea tener muchas bases de conocimiento, actualice el plan de tarifa del servicio Azure Cognitive Search.

Actualmente, no puede realizar una actualización local de la SKU de Azure Search. Sin embargo, puede crear un recurso de Azure Search con la SKU deseada, restaurar los datos al nuevo recurso y luego vincularlo a la pila de QnA Maker. Para ello, siga estos pasos.

1. Cree un recurso de Azure Search en Azure Portal y elija la SKU que desee.

    ![Recurso Azure Search en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Restaure los índices a partir del recurso Azure Search original en uno nuevo. Vea el [código de ejemplo de restauración de copia de seguridad](https://github.com/pchoudhari/QnAMakerBackupRestore).

1. Después de que se hayan restaurado los datos, vaya al nuevo recurso de Azure Search, seleccione **Keys** (Claves) y anote el **nombre** y la **clave de administrador**:

    ![Claves de Azure Search en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. Para vincular el nuevo recurso de Azure Search a la pila de QnA Maker, vaya a la instancia de App Service en QnA Maker.

    ![Instancia de App Service en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. Seleccione **Configuración de la aplicación**  y modifique la configuración en los campos **AzureSearchName** y **AzureSearchAdminKey** del paso 3.

    ![Valor de App Service en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. Reinicie la instancia de App Service.

    ![Reinicie la instancia de App Service en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)
    
### <a name="inactivity-policy-for-free-search-resources"></a>Directiva de inactividad para recursos de búsqueda gratuitos

Si no va a usar un recurso de QnA Maker, debe quitar todos los recursos. Si no quita los recursos no utilizados, la base de conocimiento dejará de funcionar si ha creado un recurso de búsqueda gratuito.

Los recursos de búsqueda gratuitos se eliminan al cabo de 90 días sin recibir una llamada API.
    
# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Actualización del servicio Azure Cognitive Search

Si planea tener muchas bases de conocimiento, actualice el plan de tarifa del servicio Azure Cognitive Search.

Actualmente, no puede realizar una actualización local de la SKU de Azure Search. Sin embargo, puede crear un recurso de Azure Search con la SKU deseada, restaurar los datos al nuevo recurso y luego vincularlo a la pila de QnA Maker. Para ello, siga estos pasos.

1. Cree un recurso de Azure Search en Azure Portal y elija la SKU que desee.

    ![Recurso Azure Search en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Restaure los índices a partir del recurso Azure Search original en uno nuevo. Vea el [código de ejemplo de restauración de copia de seguridad](https://github.com/pchoudhari/QnAMakerBackupRestore).

1. Para vincular el nuevo recurso de búsqueda de Azure al servicio QnA Maker administrado (versión preliminar), consulte el tema siguiente.

### <a name="inactivity-policy-for-free-search-resources"></a>Directiva de inactividad para recursos de búsqueda gratuitos

Si no va a usar un recurso de QnA Maker, debe quitar todos los recursos. Si no quita los recursos no utilizados, la base de conocimiento dejará de funcionar si ha creado un recurso de búsqueda gratuito.

Los recursos de búsqueda gratuitos se eliminan al cabo de 90 días sin recibir una llamada API.

---

## <a name="configure-azure-resources"></a>Configuración de recursos de Azure

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

### <a name="configure-qna-maker-to-use-different-cognitive-search-resource"></a>Configuración de QnA Maker para usar distintos recursos de Cognitive Search

Si crea un servicio QnA y sus dependencias (como, Search) en el portal, se creará el servicio Search de inmediato y se vinculará al servicio QnA Maker. Después de crear estos recursos puede actualizar el valor de App Service para usar un servicio Search existente y eliminar el que acaba de crear.

El recurso **App Service** de QnA Maker utiliza el recurso Cognitive Search. Para cambiar el recurso de Cognitive Search que usa QnA Maker, debe cambiar la configuración en Azure Portal.

1. Obtenga la **clave de administración** y el **nombre** del recurso de Cognitive Search que desea que utilice QnA Maker.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y busque el recurso de **App Service** asociado al recurso de QnA Maker. Ambos tienen el mismo nombre.

1. Seleccione **Configuración** y, después, **Configuración**. Se mostrarán todos los valores existentes de la instancia de App Service de QnA Maker.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de Azure Portal que muestra los valores de configuración de App Service](../media/qnamaker-how-to-upgrade-qnamaker/change-search-service-app-service-configuration.png)

1. Cambie los valores de las siguientes claves:

    * **AzureSearchAdminKey**
    * **AzureSearchName**

1. Para usar la nueva configuración, debe reiniciar App Service. Seleccione **Información general** y, después, **Restaurar**.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de Azure Portal con el reinicio de App Service después de cambiar la configuración](../media/qnamaker-how-to-upgrade-qnamaker/screenshot-azure-portal-restart-app-service.png)

Si crea un servicio QnA mediante plantillas de Azure Resource Manager, puede crear todos los recursos y controlar la creación de App Service para usar un servicio Search existente.

Aprenda más sobre cómo realizar la [configuración de la aplicación](../../../app-service/configure-common.md#configure-app-settings) de App Service.

### <a name="get-the-latest-runtime-updates"></a>Obtención de las últimas actualizaciones del runtime

El runtime de QnAMaker forma parte de la instancia de Azure App Service que se implementa al [crear un servicio QnAMaker](./set-up-qnamaker-service-azure.md) en Azure Portal. El sistema de tiempo de ejecución se actualiza periódicamente. La instancia de App Service en QnA Maker está en modo de actualización automática tras la versión de extensión de sitio de abril de 2019 (versión 5+). Esta actualización está diseñada para administrar el tiempo de inactividad CERO durante las actualizaciones.

Puede consultar su versión actual en https://www.qnamaker.ai/UserSettings. Si su versión es anterior a la 5.x, debe reiniciar App Service para aplicar las actualizaciones más recientes:

1. Vaya a su servicio QnAMaker (grupo de recursos) en [Azure Portal](https://portal.azure.com).

    > [!div class="mx-imgBorder"]
    > ![Grupo de recursos de QnAMaker en Azure](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Seleccione la instancia de App Service y abra la sección **Información general**.

    > [!div class="mx-imgBorder"]
    > ![Instancia de App Service en QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)


1. Reinicie App Service. El proceso de actualización debería finalizar en un par de segundos. Todas las aplicaciones o bots dependientes que usen este servicio QnAMaker dejarán de estar disponibles para los usuarios finales durante el periodo de reinicio.

    ![Reinicie la instancia de App Service en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

### <a name="configure-app-service-idle-setting-to-avoid-timeout"></a>Configuración del valor de inactividad de App Service para evitar el tiempo de espera

El servicio de aplicaciones, que proporciona el entorno de ejecución de predicción de QnA Maker para una base de conocimiento publicada, tiene una configuración de tiempo de espera de inactividad que acepta automáticamente como predeterminado el tiempo de espera si el servicio está inactivo. En QnA Maker, esto significa que la API generateAnswer del entorno de ejecución de predicción agota a veces el tiempo de espera después de períodos sin tráfico.

Para mantener cargada la aplicación del punto de conexión de predicción incluso cuando no hay tráfico, establezca el tiempo de inactividad en siempre activo.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Busque y seleccione la instancia de App Service del recurso de QnA Maker. Tendrá el mismo nombre que el recurso de QnA Maker pero un **tipo** diferente de App Service.
1. Busque **Configuración** y seleccione **Configuración**.
1. En el panel Configuración, seleccione **Configuración general**, busque **Siempre activado** y elija **Activado** como valor.

    > [!div class="mx-imgBorder"]
    > ![En el panel Configuración, seleccionar **Configuración general** luego buscar **Siempre activado** y elegir **Activado** como valor.](../media/qnamaker-how-to-upgrade-qnamaker/configure-app-service-idle-timeout.png)

1. Para guardar la configuración, seleccione **Guardar**.
1. Se le preguntará si desea reiniciar la aplicación para usar la nueva configuración. Seleccione **Continuar**.

Aprenda más sobre cómo realizar la [configuración general](../../../app-service/configure-common.md#configure-general-settings) de App Service.

### <a name="business-continuity-with-traffic-manager"></a>Continuidad del negocio con Traffic Manager

El objetivo principal del plan de continuidad empresarial consiste en crear un punto de conexión de base de conocimiento resistente, que garantizaría que el bot o la aplicación que lo consume no tengan tiempos de inactividad.

> [!div class="mx-imgBorder"]
> ![Plan bcp de QnA Maker](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

La idea de alto nivel como se representa anteriormente es la siguiente:

1. Configure dos [servicios QnA Maker](set-up-qnamaker-service-azure.md) paralelos en [Regiones emparejadas de Azure](../../../best-practices-availability-paired-regions.md).

1. [Realice una copia de seguridad](../../../app-service/manage-backup.md) del servicio de aplicaciones de QnA Maker principal y [restáurela](../../../app-service/web-sites-restore.md) en la configuración secundaria. De este modo, se asegurará de que ambas configuraciones funcionan con el mismo nombre de host y las mismas claves.

1. Mantenga sincronizados los índices principal y secundario de Azure Search. Use el ejemplo de GitHub [aquí](https://github.com/pchoudhari/QnAMakerBackupRestore) para ver cómo realizar una copia de seguridad de los índices de Azure y cómo restaurarlos.

1. Realice una copia de seguridad de Application Insights con la [exportación continua](../../../azure-monitor/app/export-telemetry.md).

1. Una vez configuradas las pilas principal y secundaria, use [Traffic Manager](../../../traffic-manager/traffic-manager-overview.md) para configurar los dos puntos de conexión y establecer un método de enrutamiento.

1. Debe crear un certificado de Seguridad de la capa de transporte (TLS), conocido anteriormente como Capa de sockets seguros (SSL), para el punto de conexión de Traffic Manager. [Enlace el certificado TLS/SSL](../../../app-service/configure-ssl-bindings.md) en los servicios de aplicaciones.

1. Por último, use el punto de conexión de Traffic Manager del bot o de la aplicación.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

### <a name="configure-qna-maker-managed-preview-service-to-use-different-cognitive-search-resource"></a>Configuración de QnA Maker administrado (versión preliminar) para usar distintos recursos de Cognitive Search

Si crea un servicio QnA administrado (versión preliminar) y sus dependencias (como Search) en el portal, se creará el servicio Search de inmediato y se vinculará al servicio QnA Maker administrado (versión preliminar). Después de crear estos recursos, puede actualizar el servicio Search en la pestaña **Configuración**.

1. Vaya al servicio QnA Maker administrado (versión preliminar) en Azure Portal.

1. Seleccione **Configuración** y, después, el servicio de Azure Cognitive Search que desea vincular con el servicio QnA Maker administrado (versión preliminar).

    ![Captura de pantalla de la página de configuración de QnA Maker administrado (versión preliminar)](../media/qnamaker-how-to-upgrade-qnamaker/change-search-service-configuration.png)

1. Haga clic en **Guardar**.

> [!NOTE]
> Si cambia el servicio de Azure Search asociado a QnA Maker, perderá el acceso a todas las knowledge bases que existen en él. Asegúrese de exportar las knowledge bases existentes antes de cambiar el servicio de Azure Search.

---

## <a name="delete-azure-resources"></a>Eliminación de recursos de Azure

Si elimina cualquiera de los recursos de Azure usados para las bases de conocimiento de QnA Maker, estas dejarán de funcionar. Antes de eliminar todos los recursos, asegúrese de exportar las bases de conocimiento desde la página **Settings** (Configuración).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [App Service](../../../app-service/index.yml) y el [servicio Azure Search](../../../search/index.yml).

> [!div class="nextstepaction"]
> [Aprenda a crear en colaboración](../index.yml)
