---
title: 'Inicio rápido: Creación de un servicio Private Link mediante Azure Portal'
titlesuffix: Azure Private Link
description: En este inicio rápido, aprenderá a crear un servicio Private Link desde Azure Portal
services: private-link
author: asudbring
ms.service: private-link
ms.topic: quickstart
ms.date: 01/18/2021
ms.author: allensu
ms.openlocfilehash: d394a475c5121607f70c03437382e104a5d0cbee
ms.sourcegitcommit: 4d48a54d0a3f772c01171719a9b80ee9c41c0c5d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2021
ms.locfileid: "98746414"
---
# <a name="quickstart-create-a-private-link-service-by-using-the-azure-portal"></a>Inicio rápido: Creación de un servicio Private Link mediante Azure Portal

Introducción a la creación de un servicio Private Link que hace referencia a su servicio.  Proporcione acceso de Private Link a su servicio o recurso implementado detrás de una instancia de Standard Load Balancer de Azure.  Los usuarios del servicio tienen acceso privado desde su red virtual.

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-an-internal-load-balancer"></a>Creación de un conjunto de equilibrador de carga interno

En esta sección, va a crear una red virtual y una instancia de Azure Load Balancer interna.

### <a name="virtual-network"></a>Virtual network

En esta sección, va a crear una red virtual y una subred para hospedar el equilibrador de carga que accede al servicio Private Link.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso > Redes > Red virtual** o busque **Red virtual** en el cuadro de búsqueda.

2. En **Crear red virtual**, escriba o seleccione esta información en la pestaña **Conceptos básicos**:

    | **Configuración**          | **Valor**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Detalles del proyecto**  |                                                                 |
    | Suscripción     | Selección de su suscripción a Azure                                  |
    | Grupo de recursos   | Seleccione **CreatePrivLinkService-rg**. |
    | **Detalles de instancia** |                                                                 |
    | Nombre             | Escriba **myVNet**.                                    |
    | Region           | Seleccione **Este de EE. UU. 2**. |

3. Seleccione la pestaña **Direcciones IP** o el botón **Siguiente: Direcciones IP** situado en la parte inferior de la página.

4. En la pestaña **Direcciones IP**, especifique esta información:

    | Configuración            | Value                      |
    |--------------------|----------------------------|
    | Espacio de direcciones IPv4 | Escriba **10.1.0.0/16**. |

5. En **Nombre de subred**, seleccione la palabra **predeterminada**.

6. En **Editar subred**, especifique esta información:

    | Configuración            | Value                      |
    |--------------------|----------------------------|
    | Nombre de subred | Escriba **mySubnet**. |
    | Intervalo de direcciones de subred | Escriba **10.1.0.0/24**. |

7. Seleccione **Guardar**.

8. Seleccione la pestaña **Revisar y crear** o el botón **Revisar y crear**.

9. Seleccione **Crear**.

### <a name="create-a-standard-load-balancer"></a>Creación de un equilibrador de carga estándar

Use el portal para crear un equilibrador de carga interno estándar. 

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Load Balancer**.

2. En la pestaña **Conceptos básicos** de la página **Crear equilibrador de carga**, escriba o seleccione la siguiente información: 

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Subscription               | Seleccione su suscripción.    |    
    | Resource group         | Seleccione el grupo de recursos **CreatePrivLinkService-RG** que ha creado en el paso anterior.|
    | Nombre                   | Escriba **myLoadBalancer**.                                   |
    | Region         | Seleccione **Este de EE. UU. 2**.                                        |
    | Tipo          | seleccione **Interno**.                                        |
    | SKU           | Seleccione **Estándar**. |
    | Virtual network | Seleccione **myVNet**, que creó en el paso anterior. |
    | Subnet  | Seleccione la subred **mySubnet**, que creó en el paso anterior. |
    | Asignación de dirección IP | seleccione **Dinámico**. |
    | Zona de disponibilidad | Seleccione **Con redundancia de zona**. |

3. Acepte los valores predeterminados en los demás valores y seleccione **Revisar y crear**.

4. En la pestaña **Revisar + crear**, seleccione **Crear**.   

## <a name="create-load-balancer-resources"></a>Creación de recursos del equilibrador de carga

En esta sección, va a configurar:

* Las opciones del equilibrador de carga para un grupo de direcciones de back-end.
* Un sondeo de estado.
* Una regla de equilibrador de carga.

### <a name="create-a-backend-pool"></a>Creación de un grupo de back-end

Un grupo de direcciones de back-end contiene las direcciones IP de las tarjetas de interfaz de red virtuales conectadas al equilibrador de carga. 

Cree el grupo de direcciones de back-end **myBackendPool** para incluir máquinas virtuales para el tráfico de Internet de equilibrio de carga.

1. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, en la lista de recursos, **myLoadBalancer**.

2. En **Configuración**, seleccione **Grupos de back-end** y, a continuación, seleccione **Agregar**.

3. En la página **Agregar un grupo back-end**, en nombre, escriba **myBackEndPool**, como el nombre del grupo de back-end y, a continuación, seleccione **Aceptar**.

### <a name="create-a-health-probe"></a>Creación de un sondeo de estado

El equilibrador de carga supervisa el estado de la aplicación con un sondeo de estado. 

El sondeo de estado agrega o quita las máquinas virtuales del equilibrador de carga a partir de su respuesta a las comprobaciones de estado. 

Cree un sondeo de mantenimiento llamado **myHealthProbe** para supervisar el mantenimiento de las máquinas virtuales.

1. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, en la lista de recursos, **myLoadBalancer**.

2. En **Configuración**, seleccione **Sondeos de estado** y, a continuación, seleccione **Agregar**.
    
    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba **myHealthProbe**. |
    | Protocolo | seleccione **TCP**. |
    | Port | Escriba **80**.|
    | Intervalo | Escriba **15** como número de **Intervalo**, en segundos, entre los intentos de sondeo. |
    | Umbral incorrecto | Seleccione **2** como número de **Umbral incorrecto** o errores de sondeo consecutivos que deben producirse para que una máquina virtual se considere que no funciona de manera correcta.|
    | | |

3. Deje el resto de valores predeterminados y seleccione **Aceptar**.

### <a name="create-a-load-balancer-rule"></a>Creación de una regla de equilibrador de carga

Las reglas de equilibrador de carga se utilizan para definir cómo se distribuye el tráfico a las máquinas virtuales. Defina la configuración IP del front-end para el tráfico entrante y el grupo de direcciones IP de back-end para recibir el tráfico. Los puertos de origen y de destino se definen en la regla. 

En esta sección va a crear una regla de equilibrador de carga:

* Llamada **myHTTPRule**.
* En el front-end llamado **LoadBalancerFrontEnd**.
* A la escucha en el **puerto 80**.
* Dirige el tráfico con equilibrio de carga al back-end llamado **myBackendPool** en el **puerto 80**.

1. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, en la lista de recursos, **myLoadBalancer**.

2. En **Configuración**, seleccione **Reglas de equilibrio de carga** y, a continuación, seleccione **Agregar**.

3. Use estos valores para configurar la regla de equilibrio de carga:
    
    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba **myHTTPRule**. |
    | Versión de la dirección IP | Seleccione **IPv4**. |
    | Dirección IP del front-end | Seleccione **LoadBalancerFrontEnd**. |
    | Protocolo | seleccione **TCP**. |
    | Port | Escriba **80**.|
    | Puerto back-end | Escriba **80**. |
    | Grupo back-end | Seleccione **MyBackendPool**.|
    | Sondeo de mantenimiento | Seleccione **myHealthProbe**. |
    | Tiempo de espera de inactividad (minutos) | Mueva el control deslizante a **15** minutos. |
    | Restablecimiento de TCP | Seleccione **Habilitado**. |

4. Deje el resto de valores predeterminados y después seleccione **Aceptar**.

## <a name="create-a-private-link-service"></a>Creación de un servicio Private Link

En esta sección, va a crear un servicio Private Link detrás de un equilibrador de carga estándar.

1. En la parte superior izquierda de Azure Portal, seleccione **Crear un recurso**.

2. Busque **Private Link** en el cuadro de **Buscar en Marketplace**.

3. Seleccione **Crear**.

4. En **Información general** en **Private Link Center**, seleccione el botón azul **Create private link service** (Crear servicio de vínculo privado).

5. En la pestaña **Aspectos básicos** en **Create private link service** (Crear servicio de vínculo privado), escriba o seleccione la información siguiente.

    | Configuración | Value |
    | ------- | ----- |
    | **Detalles del proyecto** |  |
    | Subscription | Seleccione su suscripción. |
    | Grupo de recursos | Seleccione **CreatePrivLinkService-rg**. |
    | **Detalles de instancia** |  |
    | Nombre | Escriba **myPrivateLinkService**. |
    | Region | Seleccione **Este de EE. UU. 2**. |

6. Seleccione la pestaña **Configuración de salida** o seleccione **Siguiente: Configuración de salida** en la parte inferior de la página.

7. En la pestaña **Configuración de salida**, escriba o seleccione la siguiente información:

    | Configuración | Value |
    | ------- | ----- |
    | Equilibrador de carga | Seleccione **myLoadBalancer**. |
    | Dirección IP de front-end del equilibrador de carga | Seleccione **LoadBalancerFrontEnd (10.1.0.4)** . |
    | Subred NAT de origen | Seleccione **mySubnet (10.1.0.0/24)** . |
    | Habilitación del proxy TCP V2 | Deje el valor predeterminado de **No**. </br> Si la aplicación espera un encabezado de proxy TCP V2, seleccione **Sí**. |
    | **Configuración de dirección IP privada** |  |
    | Deje los valores predeterminados. |  |

8. Seleccione la pestaña **Seguridad de acceso** o seleccione **Siguiente: Seguridad de acceso** en la parte inferior de la página.

9. Deje el valor predeterminado de **Solo control de acceso basado en rol** en la pestaña **Seguridad de acceso**.

10. Seleccione la pestaña **Etiquetas** o seleccione **Siguiente: Etiquetas** en la parte inferior de la página.

11. Seleccione la pestaña **Revisar y crear** o seleccione **Siguiente: Revisar y crear** en la parte inferior de la página.

12. En la pestaña **Revisar y crear**, seleccione **Crear**.

Se crea el servicio Private Link, que puede recibir tráfico. Si desea ver los flujos de tráfico, configure la aplicación detrás de la instancia de Standard Load Balancer.


## <a name="create-private-endpoint"></a>Creación de un punto de conexión privado

En esta sección, asignará el servicio Private Link a un punto de conexión privado. Una red virtual contiene el punto de conexión privado para el servicio Private Link. Esta red virtual contiene los recursos que accederán al servicio Private Link.

### <a name="create-private-endpoint-virtual-network"></a>Creación de una red virtual de punto de conexión privado

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso > Redes > Red virtual** o busque **Red virtual** en el cuadro de búsqueda.

2. En **Crear red virtual**, escriba o seleccione esta información en la pestaña **Conceptos básicos**:

    | **Configuración**          | **Valor**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Detalles del proyecto**  |                                                                 |
    | Suscripción     | Selección de su suscripción a Azure                                  |
    | Grupo de recursos   | Seleccione **CreatePrivLinkService-rg**. |
    | **Detalles de instancia** |                                                                 |
    | Nombre             | Escriba **myVNetPE**                                    |
    | Region           | Seleccione **Este de EE. UU. 2**. |

3. Seleccione la pestaña **Direcciones IP** o el botón **Siguiente: Direcciones IP** situado en la parte inferior de la página.

4. En la pestaña **Direcciones IP**, especifique esta información:

    | Configuración            | Value                      |
    |--------------------|----------------------------|
    | Espacio de direcciones IPv4 | Escriba **11.1.0.0/16**. |

5. En **Nombre de subred**, seleccione la palabra **predeterminada**.

6. En **Editar subred**, especifique esta información:

    | Configuración            | Value                      |
    |--------------------|----------------------------|
    | Nombre de subred | Escriba **mySubnetPE** |
    | Intervalo de direcciones de subred | Escriba **11.1.0.0/24**. |

7. Seleccione **Guardar**.

8. Seleccione la pestaña **Revisar y crear** o el botón **Revisar y crear**.

9. Seleccione **Crear**.

### <a name="create-private-endpoint"></a>Creación de un punto de conexión privado

1. En la parte superior izquierda de la pantalla en el portal, seleccione **Crear un recurso** > **Redes** > **Private Link**, o bien, en el cuadro de búsqueda, escriba **Private Link**.

2. Seleccione **Crear**.

3. En **Private Link Center**, seleccione **Private endpoints** (Puntos de conexión privados) en el menú izquierdo.

4. En **Private endpoints** (Puntos de conexión privados), select **+ Add** (+ Agregar).

5. En la pestaña **Aspectos básicos** de **Crear un punto de conexión privado**, escriba o seleccione esta información:

    | Parámetro | Value |
    | ------- | ----- |
    | **Detalles del proyecto** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **CreatePrivLinkService-rg**. Ha creado este grupo de recursos en la sección anterior.|
    | **Detalles de instancia** |  |
    | Nombre  | Escriba **myPrivateEndpoint**. |
    | Region | Seleccione **Este de EE. UU. 2**. |

6. Seleccione la pestaña **Recurso** o el botón **Siguiente: Recurso** en la parte inferior de la página.
    
7. En **Recurso**, escriba o seleccione esta información:

    | Parámetro | Value |
    | ------- | ----- |
    | Método de conexión | Seleccione **Conectarse a un recurso de Azure en mi directorio**. |
    | Subscription | Seleccione su suscripción. |
    | Tipo de recurso | Seleccione **Microsoft.Network/privateLinkServices**. |
    | Resource | Seleccione **myPrivateLinkService**. |

8. Seleccione la pestaña **Configuración** o el botón **Siguiente: Configuración** situado en la parte inferior de la pantalla.

9. En **Configuración**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | **Redes** |  |
    | Virtual Network | Seleccione **myVNetPE**. |
    | Subred | Seleccione **mySubnetPE**. |

10. Seleccione la pestaña **Revisar y crear** o el botón **Revisar y crear** en la parte inferior de la pantalla.

11. Seleccione **Crear**.

### <a name="ip-address-of-private-endpoint"></a>Dirección IP del punto de conexión privado

En esta sección, encontrará la dirección IP del punto de conexión privado que se corresponde con el equilibrador de carga y el servicio Private Link.

1. En la columna izquierda de Azure Portal, seleccione **Grupos de recursos**.

2. Seleccione el grupo de recursos **CreatePrivLinkService-RG**.

3. En el grupo de recursos **CreatePrivLinkService-RG**, seleccione **myPrivateEndpoint**.

4. En la página **Información general** de **myPrivateEndpoint**, seleccione el nombre de la interfaz de red asociada al punto de conexión privado.  El nombre de la interfaz de red comienza por **myPrivateEndpoint.nic**.

5. En la página **Información general** del punto de conexión privado, la dirección IP del punto de conexión se muestra en **Dirección IP privada**.
    

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de usar el servicio Private Link, elimine el grupo de recursos para limpiar los recursos que se han usado en este inicio rápido.

1. Escriba **CreatePrivLinkService-rg** en el cuadro de búsqueda de la parte superior del portal y seleccione **CreatePrivLinkService-rg** en los resultados de la búsqueda.
1. Seleccione **Eliminar grupo de recursos**.
1. En **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS**, escriba **CreatePrivLinkService-rg**.
1. Seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido:

* Ha creado una red virtual y una instancia interna de Azure Load Balancer.
* Ha creado un servicio Private Link.
* Ha creado una red virtual y un punto de conexión privado para el servicio Private Link.

Para más información sobre el punto de conexión privado de Azure, continúe a
> [!div class="nextstepaction"]
> [Inicio rápido: Creación de un punto de conexión privado mediante Azure Portal](create-private-endpoint-portal.md)