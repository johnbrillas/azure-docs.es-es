---
title: 'Procedimiento para configurar un punto de conexión privado: QnA Maker'
description: Descripción de la creación de puntos de conexión privados disponible en QnA Maker administrado.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 01/12/2021
ms.openlocfilehash: 5a7ddcf25993ab4ce87bdc47c5e0ae32fa24b07f
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98987192"
---
# <a name="private-endpoints"></a>Puntos de conexión privados

Un punto de conexión privado de Azure es una interfaz de red que le conecta de forma privada y segura a un servicio con la tecnología de Azure Private Link. Ahora, QnA Maker proporciona soporte para crear puntos de conexión privados para el servicio Azure Search. Esta funcionalidad está disponible en QnA Maker administrado. 

Los puntos de conexión privados se proporcionan mediante [Azure Private Link](https://docs.microsoft.com/azure/private-link/private-link-overview), como un servicio independiente. Para obtener más información sobre los costos, vea la [página de precios](https://azure.microsoft.com/pricing/details/private-link/). 

## <a name="prerequisites"></a>Requisitos previos
> [!div class="checklist"]
> * Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/) antes de empezar.
> * Un [recurso administrado](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) de QnA Maker creado en Azure Portal. Recuerde el identificador de Azure Active Directory, la suscripción y el nombre de recurso de QnA que seleccionó al crear el recurso.

## <a name="steps-to-enable-private-endpoint"></a>Pasos para habilitar el punto de conexión privado
1. Asigne el rol *Colaborador* del servicio administrado QnA Maker en la instancia del servicio Azure Search. Para esta operación se necesita acceso de *Propietario* a la suscripción. Vaya a la pestaña Identidad del recurso del servicio para obtener la identidad.
![Identidad de servicio administrado](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-identity.png)

2. Agregue la identidad anterior como *Colaborador*; para ello, vaya a la pestaña IAM del servicio Azure Search. ![IAM de servicio administrado](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-access-control.png)

3. Haga clic en *Agregar asignaciones de roles*, agregue la identidad y haga clic en *Guardar*.
![Asignación de roles administrados](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-role-assignment.png)

4. Ahora, vaya a la pestaña *Redes* de la instancia del servicio Azure Search y cambie los datos de conectividad de punto de conexión de *Públicos* a *Privados*. Esta operación es un proceso de larga duración y puede tardar hasta 30 minutos en completarse. 
![Redes de Azure Search administradas](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-networking.png)

5. Vaya a la pestaña *Redes* del servicio administrado QnA Maker y, en *Permitir acceso desde*, seleccione la opción *Redes y puntos de conexión privados seleccionados* y haga clic en *Guardar*. 
![Redes de QnA Maker administrado](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-networking-2.png)

Esto establecerá una conexión de punto de conexión privado entre el servicio QnA Maker y la instancia del servicio de búsqueda cognitiva de Azure. Puede comprobar la conexión del punto de conexión privado en la pestaña *Redes* de la instancia del servicio Azure Search. Una vez que se haya completado toda la operación, ya podrá usar el servicio QnA Maker. 
![Servicio de redes administrado](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-networking-3.png)


## <a name="support-details"></a>Detalles sobre compatibilidad
 * No se admite el cambio del servicio Azure Search una vez que se habilita el acceso privado al servicio QnA Maker. Si cambia el servicio Azure Search a través de la pestaña "Configuración" después de haber habilitado el acceso privado, el servicio QnA Maker se volverá inutilizable.
 * Después de establecer la conexión de punto de conexión privado, si cambia las redes del servicio Azure Search a "Públicas", no podrá usar el servicio QnA Maker. Para que la conexión del punto de conexión privado funcione, las redes del servicio Azure Search deben ser "Privadas".
