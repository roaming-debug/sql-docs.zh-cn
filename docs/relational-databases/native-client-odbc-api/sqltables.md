---
description: SQLTables
title: SQLTables |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLTables function
ms.assetid: 77b6c15c-9cf7-4019-b3f0-3d27d23ef656
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f5881dcbd92b136f50547655a02c749002c43acf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754787"
---
# <a name="sqltables"></a>SQLTables
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  可以对静态服务器游标执行 SQLTables。 尝试对可更新的 (动态或键集) 游标执行 SQLTables 将返回 SQL_SUCCESS_WITH_INFO，指示游标类型已更改。  
  
 SQLTables 在 *CatalogName* 参数 SQL_ALL_CATALOGS 并且所有其他参数都包含 (NULL 指针) 的默认值时，报告所有数据库中的表。  
  
 为了报告可用的目录、架构和表类型，SQLTables)  (长度为零的字节指针的空字符串。 空字符串不是默认值（NULL 指针）。  
  
 通过接受由两部分组成的 *CatalogName* 参数的名称： *Linked_Server_Name*，> Native Client ODBC 驱动程序支持链接服务器上的表的报告信息。  
  
 SQLTables 返回其名称与 *TableName* 匹配并且由当前用户拥有的所有表的相关信息。  
  
## <a name="sqltables-and-table-valued-parameters"></a>SQLTables 和表值参数  
 如果语句特性 SQL_SOPT_SS_NAME_SCOPE 的值 SQL_SS_NAME_SCOPE_TABLE_TYPE，而不是其默认值 SQL_SS_NAME_SCOPE_TABLE，则 SQLTables 将返回有关表类型的信息。 SQLTables 返回的结果集的第4列中表类型返回的 TABLE_TYPE 值为表类型。 有关 SQL_SOPT_SS_NAME_SCOPE 的详细信息，请参阅 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)。  
  
 表、视图和同义词共享公用的命名空间，该命名空间不同于表类型使用的命名空间。 虽然表和视图不能具有相同的名称，但是在相同目录和架构中的表和表类型可以具有相同的名称。  
  
 有关表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="example"></a>示例  
  
```  
// Get a list of all tables in the current database.  
SQLTables(hstmt, NULL, 0, NULL, 0, NULL, 0, NULL,0);  
  
// Get a list of all tables in all databases.  
SQLTables(hstmt, (SQLCHAR*) "%", SQL_NTS, NULL, 0, NULL, 0, NULL,0);  
  
// Get a list of databases on the current connection's server.  
SQLTables(hstmt, (SQLCHAR*) "%", SQL_NTS, (SQLCHAR*)"", 0, (SQLCHAR*)"",  
    0, NULL, 0);  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQLTables 函数](../../odbc/reference/syntax/sqltables-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
