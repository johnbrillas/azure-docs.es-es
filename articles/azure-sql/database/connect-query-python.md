---
title: Uso de Python para consultar una base de datos
description: En este tema se muestra cómo usar Python para crear un programa que se conecta a una base de datos de Azure SQL Database y realiza consultas mediante instrucciones Transact-SQL.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: seo-python-october2019, sqldbrb=2, devx-track-python
ms.devlang: python
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/19/2020
ms.openlocfilehash: 8fb6d319cacf85630b2c400cd18d14487725f925
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/20/2020
ms.locfileid: "97703984"
---
# <a name="quickstart-use-python-to-query-a-database"></a>Inicio rápido: Uso de Python para consultar una base de datos
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi-asa.md)]

En este inicio rápido se utiliza Python para conectarse a Azure SQL Database, Azure SQL Managed Instance o a una base de datos de Synapse SQL y se usan instrucciones T-SQL para consultar los datos.

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido necesita instalar:

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- Base de datos en la que ejecutará una consulta.

  [!INCLUDE[create-configure-database](../includes/create-configure-database.md)]

- [Python](https://python.org/downloads) 3 y software relacionado

  # <a name="macos"></a>[macOS](#tab/macos)

  Para instalar Homebrew y Python, el controlador ODBC y SQLCMD, y el controlador de Python para SQL Server, use los pasos **1.2**, **1.3** y **2.1** en [Creación de aplicaciones de Python con SQL Server en macOS](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).

  Para más información, consulte [Microsoft ODBC Driver en macOS](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Para instalar Python y otros paquetes necesarios, use `sudo apt-get install python python-pip gcc g++ build-essential`.

  Para instalar el controlador ODBC, SQLCMD y el controlador de Python para SQL Server, consulte el artículo para [configurar un entorno para el desarrollo de pyodbc Python](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).

  Para más información, consulte [Microsoft ODBC Driver en Linux](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="windows"></a>[Windows](#tab/windows)

  Para instalar Python, el controlador ODBC, SQLCMD y el controlador de Python para SQL Server, consulte el artículo para [configurar un entorno para el desarrollo de pyodbc Python](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

  Para más información, consulte [Microsoft ODBC Driver](/sql/connect/odbc/microsoft-odbc-driver-for-sql-server).

---
Para explorar más Python y la base de datos de Azure SQL Database, consulte [Bibliotecas Azure SQL Database para Python](/python/api/overview/azure/sql), el [repositorio de pyodbc](https://github.com/mkleehammer/pyodbc/wiki/) y un [ejemplo de pyodbc](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="create-code-to-query-your-database"></a>Creación de código para consultar la base de datos 

1. En el editor de texto, cree un archivo llamado *sqltest.py*.  
   
1. Agregue el código siguiente: Obtenga la información de conexión de la sección de requisitos previos y sustituya sus propios valores por \<server>, \<database>, \<username> y \<password>.
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'   
   driver= '{ODBC Driver 17 for SQL Server}'
   
   with pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password) as conn:
       with conn.cursor() as cursor:
           cursor.execute("SELECT TOP 3 name, collation_name FROM sys.databases")
           row = cursor.fetchone()
           while row:
               print (str(row[0]) + " " + str(row[1]))
               row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute el siguiente comando:

   ```cmd
   python sqltest.py
   ```

1. Compruebe que se devuelven las bases de datos y sus intercalaciones y, después, cierre la ventana de comandos.

## <a name="next-steps"></a>Pasos siguientes

- [Diseño de la primera base de datos en Azure SQL Database](design-first-database-tutorial.md)
- [Controladores de Microsoft para Python para SQL Server](/sql/connect/python/python-driver-for-sql-server/)
- [Centro para desarrolladores de Python](https://azure.microsoft.com/develop/python/?v=17.23h)
