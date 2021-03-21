---
description: 释放语句句柄
title: 释放语句句柄 |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- reusing statement handles
- freeing statement handles
- statements [ODBC], statement handles
- SQLFreeStmt function
- ODBC applications, statements
- statement handles [ODBC]
ms.assetid: 96fdff84-0ca7-460a-a240-94ee826ea41c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d31428d5a7dbe00f3eaa7b5a3b5d7e6ca157ed74
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752817"
---
# <a name="freeing-a-statement-handle"></a>释放语句句柄
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  重用语句句柄比删除它们后再分配新句柄更高效。 对语句句柄执行新 SQL 语句之前，应用程序应当验证当前语句设置是否正确。 这些设置包括语句属性、参数绑定和结果集绑定。 通常，旧 SQL 语句的参数和结果集必须通过使用 SQL_RESET_PARAMS 和 SQL_UNBIND 选项调用 [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) 进行取消绑定，然后再为新的 sql 语句重新绑定。  
  
 当应用程序使用完该语句后，它将调用 [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) 来释放该语句。 请注意， **SQLDisconnect** 会自动释放连接上的所有语句。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行查询 ](../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
  
