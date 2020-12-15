---
description: SQLFreeStmt
title: SQLFreeStmt |Microsoft Docs
ms.custom: ''
ms.date: 11/23/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLFreeStmt function
ms.assetid: d9666d0b-3446-480e-bf1a-10b01213e411
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 956ce78ae72c39e1986955fcc562c4a2472eb4a1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465188"
---
# <a name="sqlfreestmt"></a>SQLFreeStmt
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  通用   
      不建议在 ODBC 3.0 和更高版本中使用 **SQLFreeStmt** 。 但是，如果应用程序需要重用该语句，则仍应将 **SQLFreeStmt** 与 **SQL_RESET_PARAMS** 和 **SQL_UNBIND** 选项) 一起使用。 你还可以使用 [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md)、 [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md)、 [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md)、 [SQLSetDescField](../../relational-databases/native-client-odbc-api/sqlsetdescfield.md)和 [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) 来替换或复制 **SQLFreeStmt** 的功能，并应改为使用它们。  
  
 通常，重复使用语句比丢弃它们并分配新语句更有效。 但在某些情况下，如重用语句，仍然必须使用 SQLFreeStmt。  
  
## <a name="see-also"></a>另请参阅  
 [SQLFreeStmt 函数](../../odbc/reference/syntax/sqlfreestmt-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
