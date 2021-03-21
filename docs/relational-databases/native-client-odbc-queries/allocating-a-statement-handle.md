---
description: 分配语句句柄
title: 分配语句句柄 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLSetStmtAttr function
- statements [ODBC], statement handles
- ODBC applications, executing queries
- SQLGetStmtAttr function
- SQL Server Native Client ODBC driver, statements
- allocating statement handles
- ODBC applications, statements
- statement handles [ODBC]
- SQLAllocHandle function
ms.assetid: 9ee207f3-2667-45f5-87ca-e6efa1fd7a5c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1d935c85dca7eb17fe252839618d12a70691b152
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750747"
---
# <a name="allocating-a-statement-handle"></a>分配语句句柄
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在应用程序可以执行语句之前，它必须分配语句句柄。 为此，可调用 **SQLAllocHandle** ，并将 *HandleType* 参数设置为 SQL_HANDLE_STMT，并将 *将 inputhandle* 指向连接句柄。  
  
 语句属性是语句句柄的特征。 示例语句属性可以包括使用书签以及用于语句结果集的游标类型。 语句属性使用 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)进行设置，并且使用 [SQLGetStmtAttr](../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md)检索其当前设置。 不要求应用程序设置任何语句属性；所有语句属性都具有默认值，并且某些语句属性是驱动程序特定的。  
  
 使用多个 ODBC 语句和连接选项时，请务必小心。 如果调用 [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) 并将 *fOption* 设置为 SQL_ATTR_LOGIN_TIMEOUT 控制应用程序等待建立连接时等待连接超时的时间 (0 指定无限等待) 。 响应时间较长的站点可以将此值设置为较高的值，以确保具有充足的时间完成连接。 然而，间隔应始终足够短，这样，当驱动程序无法连接时，可以在合理的时间内向用户提供回复。  
  
 调用 **SQLSetStmtAttr** 并将 *fOption* 设置为 SQL_ATTR_QUERY_TIMEOUT 会设置查询超时间隔，以帮助保护服务器和用户的长时间运行的查询。  
  
 如果调用 **SQLSetStmtAttr** 并将 *fOption* 设置为 SQL_ATTR_MAX_LENGTH 会限制单个语句可以检索的 **文本** 和 **图像** 数据量。 如果调用 **SQLSetStmtAttr** ，并将 *fOption* 设置为 SQL_ATTR_MAX_ROWS 也会将行集限制为前 *n* 行（如果这是所有应用程序都需要）。 请注意，设置 SQL_ATTR_MAX_ROWS 会导致驱动程序向服务器发出 SET ROWCOUNT 语句。 这会影响所有 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句，包括触发器和更新。  
  
 当您设置这些选项时，请务必小心。 最好是连接句柄中的所有语句句柄对于 SQL_ATTR_MAX_LENGTH 和 SQL_ATTR_MAX_ROWS 都具有相同的设置。 如果驱动程序从一个语句句柄切换到另一个对于这些选项具有不同值的语句句柄，则驱动程序必须生成适当的 SET TEXTSIZE 和 SET ROWCOUNT 语句以更改这些设置。 驱动程序无法将这些语句与用户 SQL 语句放在相同的批中，因为用户 SQL 语句可能包含必须为批中第一条语句的语句。 驱动程序必须在单独的批中发送 SET TEXTSIZE 和 SET ROWCOUNT 语句，这会自动生成到服务器的一次附加往返。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行查询 ](../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
  
