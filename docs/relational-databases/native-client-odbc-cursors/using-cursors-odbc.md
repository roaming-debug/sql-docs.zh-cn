---
title: 使用游标 (ODBC) |Microsoft Docs
description: ODBC 支持游标模型，允许使用多种类型的游标、游标内的滚动/定位、多个并发选项和定位更新。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, cursors
- ODBC cursors, about ODBC cursors
- ODBC applications, cursors
- cursors [ODBC]
- ODBC cursors
ms.assetid: 51322f92-0d76-44c9-9c33-9223676cf1d3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: beb1de9d4bfa8507ab24a66ec9f7a306cfccc15e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756187"
---
# <a name="using-cursors-odbc"></a>使用游标 (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 支持允许以下项的游标模型：  
  
-   多种类型的游标。  
  
-   在游标中滚动和定位。  
  
-   多个并发选项。  
  
-   定位更新。  
  
 ODBC 应用程序很少声明和打开游标，或使用与游标相关的任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 ODBC 自动为从 SQL 语句返回的每个结果集打开游标。 在执行 SQL 语句之前，游标的特征由 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 设置的语句属性控制。 用于处理结果集的 ODBC API 函数支持完整范围的游标功能，包括提取、滚动和定位更新。  
  
 下面是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本和 ODBC 应用程序如何使用游标的比较。  
  
|操作|[!INCLUDE[tsql](../../includes/tsql-md.md)]|ODBC|  
|------------|------------------------|----------|  
|定义游标行为|通过 DECLARE CURSOR 参数进行指定|使用[SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)设置 cursor 特性|  
|打开游标|声明游标打开 *cursor_name*|**SQLExecDirect** 或 **SQLExecute**|  
|提取行|FETCH|**SQLFetch** 或 [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)|  
|定位更新|UPDATE 或 DELETE 中的 WHERE CURRENT OF 子句|**SQLSetPos**|  
|关闭游标|关闭 *cursor_name* 解除分配|[SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md)|  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中实现的服务器游标支持 ODBC 游标模型的功能。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 驱动程序使用服务器游标以支持 ODBC API 的游标功能。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [如何实现游标](../../relational-databases/native-client-odbc-cursors/implementation/how-cursors-are-implemented.md)  
  
-   [游标类型](../../relational-databases/native-client-odbc-cursors/cursor-types.md)  
  
-   [游标行为](../../relational-databases/native-client-odbc-cursors/cursor-behaviors.md)  
  
-   [游标属性](../../relational-databases/native-client-odbc-cursors/properties/cursor-properties.md)  
  
-   [ODBC&#41;的游标编程详细信息 &#40;](../../relational-databases/native-client-odbc-cursors/programming/cursor-programming-details-odbc.md)  
  
-   [滚动和提取行](../../relational-databases/native-client-odbc-cursors/scrolling-and-fetching-rows.md)  
  
-   [ODBC&#41;的定位更新 &#40;](../../relational-databases/native-client-odbc-cursors/positioned-updates-odbc.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [CLOSE (Transact-SQL)](../../t-sql/language-elements/close-transact-sql.md)   
 [游标](../../relational-databases/cursors.md)   
 [DEALLOCATE (Transact-SQL)](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [DECLARE CURSOR (Transact-SQL)](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [FETCH (Transact-SQL)](../../t-sql/language-elements/fetch-transact-sql.md)   
 [OPEN (Transact-SQL)](../../t-sql/language-elements/open-transact-sql.md)  
  
  
