---
description: 连接数据源 (ODBC)
title: " (ODBC) 连接到数据源 |Microsoft Docs"
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- checking connection states
- ODBC data sources, connections
- data sources [SQL Server Native Client]
- SQLBrowseConnect function
- ODBC applications, connections
- ODBC applications, data sources
- connections [SQL Server Native Client]
- SQLConnect function
- SQLDriveConnect function
- verifying connection states
- SQL Server Native Client ODBC driver, data sources
- SQL Server Native Client ODBC driver, connections
ms.assetid: ae30dd1d-06ae-452b-9618-8fd8cd7ba074
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 97bcd6e94d77f6c4fe76426ac25d6d21c60282b3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756357"
---
# <a name="connecting-to-a-data-source-odbc"></a>连接数据源 (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在分配环境和连接句柄并设置连接属性后，应用程序将连接到数据源或驱动程序。 有三种可用于连接的函数：  
  
-   **SQLConnect**  
  
-   **SQLDriverConnect**  
  
-   **SQLBrowseConnect**  
  
 有关建立与数据源的连接的详细信息，包括可用的各种连接字符串选项，请参阅将 [连接字符串关键字用于 SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
## <a name="sqlconnect"></a>SQLConnect  
 **SQLConnect** 是最简单的连接函数。 它接受三个参数：数据源名称、用户 ID 和密码。 当这三个参数包含连接到数据库所需的所有信息时，请使用 **SQLConnect** 。 为此，请使用 **SQLDataSources** 生成数据源列表;提示用户输入数据源、用户 ID 和密码;然后调用 **SQLConnect**。  
  
 **SQLConnect** 假定数据源名称、用户 ID 和密码足以连接到数据源，并且 odbc 数据源包含 odbc 驱动程序建立连接所需的所有其他信息。 与 [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md) 和 [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)不同， **SQLConnect** 不使用连接字符串。  
  
## <a name="sqldriverconnect"></a>SQLDriverConnect  
 当需要超过数据源名称、用户 ID 和密码的信息时，将使用 **SQLDriverConnect** 。 **SQLDriverConnect** 的参数之一是包含驱动程序特定信息的连接字符串。 由于以下原因，你可以使用 **SQLDriverConnect** 而不是 **SQLConnect** ：  
  
-   在连接时指定特定于驱动程序的信息。  
  
-   请求驱动程序提示用户输入连接信息。  
  
-   不使用 ODBC 数据源进行连接。  
  
 **SQLDriverConnect** 连接字符串包含一系列关键字/值对，这些关键字值对指定 ODBC 驱动程序支持的所有连接信息。 每个驱动程序都支持标准 ODBC 关键字（DSN、FILEDSN、DRIVER、UID、PWD 和 SAVEFILE），以及用于驱动程序支持的所有连接信息的特定于驱动程序的关键字。 **SQLDriverConnect** 可用于在不使用数据源的情况下进行连接。 例如，设计为与实例建立 "无 DSN" 连接的应用程序 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以使用连接字符串调用 **SQLDriverConnect** ，该连接字符串定义登录 ID、密码、网络库、要连接到的服务器名称以及要使用的默认数据库。  
  
 当使用 **SQLDriverConnect** 时，有两个选项可提示用户输入所需的任何连接信息：  
  
-   应用程序对话框  
  
     你可以创建一个应用程序对话框，以提示输入连接信息，然后使用空窗口句柄和 *DriverCompletion* 设置为 SQL_DRIVER_NOPROMPT 来调用 **SQLDriverConnect** 。 这些参数设置将禁止 ODBC 驱动程序打开自己的对话框。 在对应用程序的用户界面进行控制非常重要时，使用此方法。  
  
-   驱动程序对话框  
  
     可以编写应用程序代码，将有效的窗口句柄传递到 **SQLDriverConnect** ，并将 *DriverCompletion* 参数设置为 SQL_DRIVER_COMPLETE、SQL_DRIVER_PROMPT 或 SQL_DRIVER_COMPLETE_REQUIRED。 驱动程序然后生成一个对话框，以便提示用户输入连接信息。 此方法可以简化应用程序代码。  
  
## <a name="sqlbrowseconnect"></a>SQLBrowseConnect  
 **SQLBrowseConnect**（如 **SQLDriverConnect**）使用连接字符串。 但是，通过使用 **SQLBrowseConnect**，应用程序可以在运行时以迭代方式构造完整的连接字符串与数据源。 这允许应用程序执行以下两种操作：  
  
-   生成自己的对话框以提示输入此信息，因此保留对其用户界面的控制。  
  
-   浏览系统以找到可由特定的驱动程序使用的数据源，这可能要执行若干步骤。  
  
     例如，用户可能要首先浏览网络以找到服务器，然后在选择某一服务器后，浏览该服务器以找到驱动程序可访问的数据库。  
  
 当 **SQLBrowseConnect** 完成成功连接时，它将返回一个连接字符串，该字符串可用于对 **SQLDriverConnect** 的后续调用。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序始终在成功的 **SQLConnect**、 **SQLDriverConnect** 或 **SQLBrowseConnect** 上返回 SQL_SUCCESS_WITH_INFO。 如果 ODBC 应用程序在获取 SQL_SUCCESS_WITH_INFO 后调用 **SQLGetDiagRec** ，则它可能会收到以下消息：  
  
 5701  
 指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将用户的上下文置于在数据源中定义的默认数据库中，或者置于为在连接中使用的登录 ID 定义的默认数据库中（如果数据源没有默认数据库）。  
  
 5703  
 指示要在服务器上使用的语言。  
  
 以下示例显示系统管理员对成功的连接返回的消息：  
  
```  
szSqlState = "01000", *pfNativeError = 5701,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]  
       Changed database context to 'pubs'."  
szSqlState = "01000", *pfNativeError = 5703,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]  
       Changed language setting to 'us_english'."  
```  
  
 您可以忽略消息 5701 和 5703；它们仅供参考。 但是，不应忽视 SQL_SUCCESS_WITH_INFO 返回代码，因为可能返回并非 5701 或 5703 的其他消息。 例如，如果驱动程序连接到运行实例的服务器 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，并且该服务器具有过时的目录存储过程，则 SQL_SUCCESS_WITH_INFO 之后通过 **SQLGetDiagRec** 返回的错误之一如下：  
  
```  
SqlState:   01000  
pfNative:   0  
szErrorMsg: "[Microsoft][SQL Server Native Client]The ODBC  
            catalog stored procedures installed on server  
            my65server are version 06.50.0193; version 07.00.0205  
            or later is required to ensure proper operation.  
            Please contact your system administrator."  
```  
  
 连接应用程序的错误处理函数 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应调用 **SQLGetDiagRec** ，直到它返回 SQL_NO_DATA。 然后，它应作用于 *pfNative* 代码不是5701或5703的任何消息。  
  
## <a name="see-also"></a>另请参阅  
 [与 SQL Server &#40;ODBC&#41;通信 ](../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
  
