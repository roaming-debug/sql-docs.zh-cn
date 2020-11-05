---
description: 设置游标选项 (ODBC)
title: " (ODBC) 设置游标选项 |Microsoft Docs"
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- cursors [ODBC], options
ms.assetid: 0e72b48a-fc5a-4656-8cf5-39f57d8c1565
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d1cf4610efc2ea021b8cd9260f463af079d28dc9
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364816"
---
# <a name="set-cursor-options-odbc"></a>设置游标选项 (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  若要设置游标选项，请调用 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 设置或 [SQLGetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md) 以获取控制游标行为的语句选项。  
  
|*Attribute*|指定|  
|-----------------|---------------|  
|SQL_ATTR_CURSOR_TYPE|只进、静态、动态或由键集驱动的游标类型|  
|SQL_ATTR_CONCURRENCY|只读、锁定、乐观使用时间戳或乐观使用值的并发控制选项|  
|SQL_ATTR_ROW_ARRAY_SIZE|在每个提取中检索的行数|  
|SQL_ATTR_CURSOR_SENSITIVITY|显示或不显示由其他连接对游标行进行的更新的游标|  
|SQL_ATTR_CURSOR_SCROLLABLE|可以向前和向后滚动的游标|  
  
 这些属性（只进、只读、行集大小为 1）的默认值不使用服务器游标。 若要使用服务器游标，必须将这些属性中的至少一个属性设置为非默认值，并且所执行的语句必须是包含单个 SELECT 语句的单个 SELECT 语句或存储过程。 使用服务器游标时，SELECT 语句无法使用服务器游标不支持的子句：COMPUTE、COMPUTE BY、FOR BROWSE 和 INTO。  
  
 您可以通过设置 SQL_ATTR_CURSOR_TYPE 和 SQL_ATTR_CONCURRENCY 或设置 SQL_ATTR_CURSOR_SENSITIVITY 和 SQL_ATTR_CURSOR_SCROLLABLE 来控制使用的游标类型。 不应将指定游标行为的两种方法混用。  
  
## <a name="examples"></a>示例  

### <a name="a-set-a-dynamic-cursor"></a>A. 设置动态游标

 以下示例分配一个语句句柄，之后以行版本控制乐观并发方式设置动态游标类型，然后执行 SELECT。  
  
```  
retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
retcode = SQLSetStmtAttr(hstmt1, SQL_ATTR_CURSOR_TYPE, (SQLPOINTER)SQL_CURSOR_DYNAMIC, SQL_IS_INTEGER);  
retcode = SQLSetStmtAttr(hstmt1, SQL_ATTR_CONCURRENCY, SQLPOINTER)SQL_CONCUR_ROWVER, SQL_IS_INTEGER);  
retcode = SQLExecDirect(hstmt1, SELECT au_lname FROM authors", SQL_NTS);  
```  
  
### <a name="b-set-a-scrollable-sensitive-cursor"></a>B. 设置可滚动的敏感游标
 以下示例分配一个语句句柄，之后设置可滚动、敏感游标，然后执行 SELECT  
  
```  
retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
  
// Set the cursor options and execute the statement.  
retcode = SQLSetStmtAttr(hstmt1, SQL_ATTR_CURSOR_SCROLLABLE, SQLPOINTER)SQL_SCROLLABLE, SQL_IS_INTEGER);  
retcode = SQLSetStmtAttr(hstmt1, SQL_ATTR_CURSOR_SENSITIVITY, SQLPOINTER)SQL_INSENSITIVE, SQL_IS_INTEGER);  
retcode = SQLExecDirect(hstmt1, select au_lname from authors", SQL_NTS);  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行查询操作指南主题 ](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
  
