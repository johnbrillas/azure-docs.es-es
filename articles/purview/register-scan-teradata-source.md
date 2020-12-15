---
title: Registro de un origen de Teradata y configuración de los exámenes (versión preliminar)
description: En este artículo se describe cómo registrar un origen de Teradata en Azure Purview y cómo configurar un examen.
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: overview
ms.date: 12/06/2020
ms.openlocfilehash: 170ece293f8d24f3a29774c64c14f9334fe0930c
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96841688"
---
# <a name="register-and-scan-teradata-source-preview"></a>Registro y examen de un origen de Teradata (versión preliminar)

En este artículo se describe cómo registrar un origen de Teradata en Purview y cómo configurar un examen.

> [!IMPORTANT]
> Este origen de datos está actualmente en versión preliminar. Puede probarlo y enviarnos sus comentarios.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

El origen de Teradata admite **Examen completo** para extraer metadatos de una base de datos de Teradata y captura el **linaje** entre los recursos de datos.

## <a name="prerequisites"></a>Requisitos previos

- El conector admite un almacén de datos ubicado dentro de una red local, una red virtual de Azure o Amazon Virtual Private Cloud. Por consiguiente, necesita configurar un [entorno de ejecución de integración autohospedado](manage-integration-runtimes.md) para conectarse a él.

- Asegúrese de que Java Runtime Environment (JRE) está instalado en la máquina virtual en que está instalado el entorno de ejecución de integración autohospedado.
 
- Asegúrese de que "Visual C++ Redistributable 2012 Update 4" está instalado en la máquina del entorno de ejecución de integración autohospedado. Si aún no está instalada, descárguela de [aquí](https://www.microsoft.com/download/details.aspx?id=30679).

- Tendrá que instalar manualmente un controlador denominado Teradata JDBC Driver en la máquina virtual local. El archivo JAR ejecutable se puede descargar desde el sitio web de [Teradata](https://downloads.teradata.com/).

    > [!Note] 
    > Todas las cuentas de la máquina virtual deben poder acceder al controlador. No lo instale en una cuenta de usuario.

- Las versiones de la base de datos de Teradata compatibles son **12.x a 16.x**. Asegúrese de tener acceso de lectura al origen de Teradata que se está examinando.

### <a name="feature-flag"></a>Marca de características

El registro y examen de un origen de Teradata están disponibles detrás de una marca de características. Anexe lo siguiente a la dirección URL: *?feature.ext.datasource=%7b"teradata":"true"%7d* 

Por ejemplo, la dirección URL completa [https://web.purview.azure.com/?feature.ext.datasource=%7b"metadata":"true"%7d](https://web.purview.azure.com/?feature.ext.datasource=%7b"teradata":"true"%7d)

## <a name="setting-up-authentication-for-a-scan"></a>Configuración de la autenticación para un examen
La única forma admitida para configurar la autenticación de un origen de Teradata es la autenticación de **base de datos básica**

## <a name="register-a-teradata-source"></a>Registro de un origen de Teradata

Para registrar un nuevo origen de Teradata en el catálogo de datos, siga estos pasos:

1. Vaya a la cuenta de Purview.
2. Seleccione **Sources** (Orígenes) en el panel de navegación izquierdo.
3. Seleccione **Registrar**.
4. En **Register sources** (Registrar orígenes), seleccione **Teradata**.
5. Seleccione **Continuar**

En la pantalla **Registrar orígenes** [Registrar orígenes (Teradata)], haga lo siguiente:

1. Escriba un **nombre** con el que se mostrará el origen de datos en el catálogo.
2. Escriba el nombre del **host** para conectarse a un origen de Teradata. También puede ser una dirección IP o una cadena de conexión completa al servidor.
3. Seleccione una colección o cree una nueva (opcional).
4. **Finalice** el proceso para registrar el origen de datos.

    :::image type="content" source="media/register-scan-teradata-source/register-sources.png" alt-text="Opciones de registro de orígenes" border="true":::

## <a name="creating-and-running-a-scan"></a>Creación y ejecución de un examen

Para crear y ejecutar un nuevo examen, siga estos pasos:
1. En el centro de administración, haga clic en *Integration runtimes* (Entornos de ejecución de integración). Asegúrese de que está configurado un entorno de ejecución de integración autohospedado. Si no lo está, use los pasos que se mencionan [aquí](manage-integration-runtimes.md) para crear un entorno de ejecución de integración autohospedado para realizar exámenes en una máquina virtual o una instancia local de Azure que tenga acceso a la red local.

2. Luego, vaya a **Sources** (Orígenes).

3. Seleccione el origen de Teradata registrado.

4. Seleccione + New scan (+ Nuevo examen)
 
5. Especifique los detalles siguientes:

    - Nombre: nombre del examen.

    - Conectar mediante el entorno de ejecución de integración: seleccione el entorno de ejecución de integración autohospedado.

    - Método de autenticación: La autenticación de la base de datos es la única opción que se admite por ahora. Estará seleccionada de forma predeterminada.

    - Nombre de usuario: nombre de usuario para conectarse al servidor de la base de datos. Este nombre de usuario debería tener acceso de lectura al servidor.

    - Contraseña: la contraseña de usuario para conectarse al servidor de bases de datos.

    - Esquema: enumere el subconjunto de esquemas que se van a importar; se expresa como una lista separada por punto y coma. Por ejemplo, schema1; schema2. Si la lista está vacía, se importan todos los esquemas de usuario. De forma predeterminada, se ignoran todos los esquemas del sistema (por ejemplo, SysAdmin) y los objetos.

        Los patrones de nombres de esquemas aceptables que usan la sintaxis de expresiones SQL LIKE incluyen el uso de %, por ejemplo, A%; %B; %C%; D
        - empieza por A o    
        - termina en B o    
        - contiene C o    
        - igual a D

        No se permite usar NOT ni caracteres especiales.

    - Ubicación del controlador: ruta de acceso completa a la ubicación del controlador de Teradata en la máquina virtual del cliente. El nombre del controlador JDBC de Teradata debe ser: com.teradata.jdbc.TeraDriver

    - Memoria máxima disponible: Memoria máxima (en GB) disponible en la máquina virtual del cliente que se va a usar mediante el examen de procesos. Depende del tamaño del origen de Teradata que se va a examinar.

    > [!Note] 
    > Como regla general, especifique 2 GB de memoria por cada 1000 tablas

    :::image type="content" source="media/register-scan-teradata-source/setup-scan.png" alt-text="examen de configuración" border="true":::

6. Haga clic en *Continuar*.

7. Elija el desencadenador del examen. Puede configurar una programación o ejecutar el examen una vez.

    :::image type="content" source="media/register-scan-teradata-source/scan-trigger.png" alt-text="desencadenador del examen" border="true":::

8. Revise el examen y haga clic en *Save and run* (Guardar y ejecutar).

## <a name="viewing-your-scans-and-scan-runs"></a>Visualización de los exámenes y las ejecuciones de exámenes

1. Vaya al centro de administración. Seleccione **Data sources** (Orígenes de datos) en la sección **Sources and scanning** (Orígenes y examen).

2. Seleccione el origen de datos que desee. Verá una lista de los exámenes existentes en ese origen de datos.

3. Seleccione el examen cuyos resultados le interesa ver.

4. En esta página se muestran todas las ejecuciones de exámenes anteriores, junto con las métricas y el estado de cada ejecución del examen. También mostrará si el análisis se ha programado o es manual, a cuántos recursos se han aplicado clasificaciones, cuántos recursos totales se han detectado, la hora de inicio y finalización del examen y la duración total del examen.

## <a name="manage-your-scans"></a>Administración de exámenes

Para administrar o eliminar un examen, haga lo siguiente:

1. Vaya al centro de administración. Seleccione **Data sources** (Orígenes de datos) en la sección **Sources and scanning** (Orígenes y examen) y, a continuación, seleccione el origen de datos deseado.

2. Seleccione el examen que desea administrar. Seleccione **Edit** (Editar) para editar el examen.

3. Seleccione **Delete** (Eliminar) para eliminar el examen.

## <a name="next-steps"></a>Pasos siguientes

- [Examen del catálogo de datos de Azure Purview](how-to-browse-catalog.md)
- [Búsqueda en el catálogo de datos de Azure Purview](how-to-search-catalog.md)
