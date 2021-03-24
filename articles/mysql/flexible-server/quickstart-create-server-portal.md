---
title: 'Inicio rápido: Creación de un servidor flexible de Azure Database for MySQL: Azure Portal'
description: Este artículo le ayudará a usar Azure Portal para crear un servidor flexible de Azure Database for MySQL en cuestión de minutos.
author: savjani
ms.author: pariks
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 10/22/2020
ms.openlocfilehash: 074b799a4f0e83c47aac0b2b3fca5386bd45429f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100521975"
---
# <a name="quickstart-use-the-azure-portal-to-create-an-azure-database-for-mysql-flexible-server"></a>Inicio rápido: Uso de Azure Portal para crear un servidor flexible de Azure Database for MySQL

Servidor flexible de Azure Database for MySQL es un servicio administrado que se usa para ejecutar, administrar y escalar servidores MySQL de alta disponibilidad en la nube. En este inicio rápido se muestra cómo crear un servidor flexible mediante Azure Portal.

> [!IMPORTANT] 
> Actualmente, la opción de implementación Servidor flexible de Azure Database for MySQL se encuentra en versión preliminar pública.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal
Vaya a [Azure Portal](https://portal.azure.com/). Introduzca sus credenciales para iniciar sesión en el portal. La vista predeterminada es el panel del servicio.

## <a name="create-an-azure-database-for-mysql-flexible-server"></a>Creación de un servidor flexible de Azure Database for MySQL

Puede crear un servidor flexible mediante un conjunto definido de [recursos de proceso y almacenamiento](./concepts-compute-storage.md). Cree el servidor dentro en un [grupo de recursos de Azure](../../azure-resource-manager/management/overview.md).

Siga estos pasos para crear un servidor flexible:

1. Busque y seleccione **Azure Database for MySQL** en el portal:
    
    > :::image type="content" source="./media/quickstart-create-server-portal/find-mysql-portal.png" alt-text="Captura de pantalla que muestra una búsqueda de servidores de Azure Database for MySQL.":::

2. Seleccione **Agregar**. 

3. En la página **Seleccionar opción de implementación de Azure Database for MySQL**, seleccione **Servidor flexible** como opción de implementación:
     
    > :::image type="content" source="./media/quickstart-create-server-portal/deployment-option.png" alt-text="Captura de pantalla que muestra la opción Servidor flexible.":::    

4. En la pestaña **Datos básicos**, especifique la siguiente información: 

    > :::image type="content" source="./media/quickstart-create-server-portal/create-form.png" alt-text="Captura de pantalla que muestra la pestaña Conceptos básicos de la página Servidor flexible."::: 
                                    
    |**Configuración**|**Valor sugerido**|**Descripción**|
    |---|---|---|
    Subscription|Nombre de la suscripción|La suscripción de Azure que desea usar para el servidor. Si tiene varias suscripciones, elija aquella en la que quiere que se le facture el recurso.|
    Resource group|**myresourcegroup**| Un nuevo nombre de grupo de recursos o uno existente de la suscripción.|
    Nombre de servidor |**mydemoserver**|Escriba un nombre único que identifique su servidor flexible. El nombre de dominio `mysql.database.azure.com` se anexa al nombre del servidor proporcionado. El nombre del servidor solo puede contener letras minúsculas, números y el carácter de guion (-). Debe contener entre 3 y 63 caracteres.|
    Nombre de usuario administrador |**mydemouser**| Su propia cuenta de inicio de sesión para usar al conectarse al servidor. El nombre del usuario administrador no puede ser **azure_superuser**, **admin**, **administrator**, **root**, **guest** ni **public**.|
    Contraseña |La contraseña| Una contraseña nueva para la cuenta de administrador del servidor. Debe tener entre 8 y 128 caracteres. Debe contener también caracteres de tres de las siguientes categorías: Letras del alfabeto inglés mayúsculas y minúsculas, números (0 a 9) y caracteres no alfanuméricos (!, $, #, %, etc.).|
    Region|Región más cercana a los usuarios| Ubicación más cercana a los usuarios.|
    Versión|**5.7**| Una versión principal de MySQL.|
    Proceso y almacenamiento | **Flexible**, **Standard_B1ms**, **10 GiB** y **7 días** | Configuración de los recursos de proceso, almacenamiento y copia de seguridad para el nuevo servidor. Seleccione **Configurar servidor**. **Flexible**, **Standard_B1ms**, **10 GiB** y **7 días** son los valores predeterminados para el **nivel de proceso**, el **tamaño de proceso**, el **tamaño de almacenamiento** y el **período de retención de copia de seguridad**. Puede dejar esos valores tal como están o ajustarlos. Para guardar este proceso y la selección de almacenamiento, seleccione **Guardar** a fin de continuar con la configuración. En la captura de pantalla siguiente se muestran las opciones de proceso y almacenamiento.|
    
    > :::image type="content" source="./media/quickstart-create-server-portal/compute-storage.png" alt-text="Captura de pantalla que muestra las opciones de proceso y almacenamiento.":::

5. Configure las opciones de red.

    En la pestaña **Redes**, puede elegir cómo se puede tener acceso al servidor. Servidor flexible de Azure Database for MySQL proporciona dos formas de conectarse al servidor: 
   - Acceso público (direcciones IP permitidas)
   - Acceso privado (integración con red virtual) 
   
   Si usa un acceso público, el acceso a su servidor se limita a las direcciones IP permitidas que agrega a una regla de firewall. Este método evita que herramientas y aplicaciones externas se conecten al servidor o a las bases de datos de este, a menos que cree una regla para abrir el firewall para una dirección IP o un intervalo específico. Si usa un acceso privado (integración con red virtual), el acceso a su servidor se limita a la red virtual. En este inicio rápido, aprenderá a habilitar el acceso público para conectarse al servidor. [Obtenga más información sobre los métodos de conectividad en el artículo de conceptos.](./concepts-networking.md)

    > [!NOTE]
    > No puede cambiar el método de conectividad después de crear el servidor. Por ejemplo, si seleccionó la opción **Acceso público (direcciones IP permitidas)** al crear el servidor, no podrá cambiar a la opción **Acceso privado (integración con red virtual)** después de la creación. Es muy recomendable que cree el servidor con acceso privado para ayudar a proteger el acceso al servidor mediante la integración de red virtual. [Obtenga más información sobre el acceso privado en el artículo de conceptos.](./concepts-networking.md)

    > :::image type="content" source="./media/quickstart-create-server-portal/networking.png" alt-text="Captura de pantalla que muestra la pestaña Redes.":::  

6. Seleccione **Revisar y crear** para revisar la configuración del servidor flexible.

7. Seleccione **Crear** para realizar el aprovisionamiento del servidor. El aprovisionamiento puede tardar unos minutos.

8. Seleccione **Notificaciones** en la barra de herramientas (botón de campana) para supervisar el proceso de implementación. Una vez realizada la implementación, puede seleccionar **Anclar al panel** para crear un icono para el servidor flexible en el panel de Azure Portal. Este icono es un acceso directo a la página de **información general** del servidor. Al seleccionar **Ir al recurso**, se abre la página **Información general** del servidor.

De forma predeterminada, se crean estas bases de datos en el servidor: information_schema, mysql, performance_schema y sys.

> [!NOTE]
> Para evitar problemas de conectividad, compruebe si la red permite el tráfico de salida por el puerto 3306 que usa el servidor flexible de Azure Database for MySQL.  

## <a name="connect-to-the-server-by-using-mysqlexe"></a>Conexión al servidor con mysql.exe

Si ha creado el servidor flexible con acceso privado (integración con red virtual), deberá conectarse a su servidor desde un recurso en la misma red virtual que el servidor. Puede crear una máquina virtual y agregarla a la red virtual creada con el servidor flexible. Para más información, consulte cómo se configura la [documentación de acceso privado](how-to-manage-virtual-network-portal.md).

Si ha creado el servidor flexible con acceso público (direcciones IP permitidas), puede agregar la dirección IP local a la lista de reglas de firewall del servidor. En la [documentación para la creación o administración de reglas de firewall](how-to-manage-firewall-portal.md) encontrará instrucciones paso a paso.

Puede usar [mysql.exe](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) o [MySQL Workbench](./connect-workbench.md) para conectarse al servidor desde su entorno local. 

```bash
wget --no-check-certificate https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem
mysql -h mydemoserver.mysql.database.azure.com -u mydemouser -p --ssl=true --ssl-ca=DigiCertGlobalRootCA.crt.pem
```

Si ha aprovisionado un servidor flexible mediante el **acceso público**, también puede usar [Azure Cloud Shell](https://shell.azure.com/bash) para conectarse a dicho servidor flexible mediante el cliente MySQL preinstalado, como se muestra a continuación:

Para usar Azure Cloud Shell para conectarse a un servidor flexible, tendrá que permitir el acceso de red desde Azure Cloud Shell al servidor flexible. Para ello, en Azure Portal puede ir a la hoja **Redes** del servidor flexible de MySQL, activar la casilla de la sección **Firewall** "Allow public access from any Azure service within Azure to this server" (Permitir el acceso público desde cualquier servicio de Azure a este servidor) y hacer clic en Guardar para conservar la configuración.

> [!NOTE]
> La comprobación de **Allow public access from any Azure service within Azure to this server** (Permitir el acceso público desde cualquier servicio de Azure a este servidor) solo debe usarse solo para desarrollo o pruebas. Configura el firewall para permitir conexiones desde las direcciones IP asignadas a cualquier servicio o recurso de Azure, incluidas las conexiones de las suscripciones de otros clientes.

Haga clic en **Probar** para iniciar el Azure Cloud Shell y use los siguientes comandos para conectarse a su servidor flexible. Use el nombre del servidor, el nombre de usuario y la contraseña en el comando. 

```azurecli-interactive
wget --no-check-certificate https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem
mysql -h mydemoserver.mysql.database.azure.com -u mydemouser -p --ssl=true --ssl-ca=DigiCertGlobalRootCA.crt.pem
```

Si ve el siguiente mensaje de error al conectarse a su servidor flexible después del comando anterior, significa que no ha configurado la regla de firewall mediante la casilla Allow public access from any Azure service within Azure to this server (Permitir el acceso público desde cualquier servicio de Azure a este servidor) mencionada antes o que la opción no se ha guardado. Vuelva a intentar configurar el firewall y reintente la operación.

ERROR 2002 (HY000): No es posible conectarse a un servidor MySQL en <servername> (115)

## <a name="clean-up-resources"></a>Limpieza de recursos
Ha creado un servidor flexible de Azure Database for MySQL en un grupo de recursos. Si no cree que vaya a necesitar estos recursos en el futuro, puede eliminarlos mediante la eliminación del grupo de recursos o bien puede eliminar simplemente el servidor de MySQL. Para eliminar el grupo de recursos, siga estos pasos:

1. En Azure Portal, busque y seleccione **Grupos de recursos**.
1. En la lista, seleccione el nombre de su grupo de recursos.
1. En la página de **información general** del grupo de recursos, seleccione **Eliminar grupo de recursos**.
1. En el cuadro de diálogo de confirmación, escriba el nombre del grupo de recursos y seleccione **Eliminar**.

Para eliminar el servidor, puede seleccionar **Eliminar** en la página de **información general** del servidor, como se muestra aquí:

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-server-portal/delete-server.png" alt-text="Captura de pantalla que muestra cómo eliminar un servidor.":::

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Compilación de una aplicación web PHP (Laravel) con MySQL](tutorial-php-database-app.md)
