---
description: SQLPrimaryKeys
title: SQLPrimaryKeys |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLPrimaryKeys function
ms.assetid: bc61cd5b-d2f4-4f87-abc7-743cf9ea772d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2d8d86b56b073f67a925e4822d88b228e9444e63
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485009"
---
# <a name="sqlprimarykeys"></a>SQLPrimaryKeys
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  表中可能有一列或多列可用作唯一的行标识符，而在没有 PRIMARY KEY 约束的情况下创建的表会将空结果集返回到 SQLPrimaryKeys。 ODBC 函数 [SQLSpecialColumns](../../relational-databases/native-client-odbc-api/sqlspecialcolumns.md) 报告没有主键的表的行标识符候选项。  
  
 SQLPrimaryKeys 返回 SQL_SUCCESS *CatalogName*、 *SchemaName* 或 *TableName* 参数是否存在值。 当这些参数中使用的值无效时，SQLFetch 返回 SQL_NO_DATA。  
  
 可以对静态服务器游标执行 SQLPrimaryKeys。 尝试对可更新的 (动态或键集) 游标执行 SQLPrimaryKeys 将返回 SQL_SUCCESS_WITH_INFO，指示游标类型已更改。  
  
 通过接受由两部分组成的 *CatalogName* 参数的名称： *Linked_Server_Name*，> Native Client ODBC 驱动程序支持链接服务器上的表的报告信息。  
  
## <a name="sqlprimarykeys-and-table-valued-parameters"></a>SQLPrimaryKeys 和表值参数  
 如果语句特性 SQL_SOPT_SS_NAME_SCOPE 的值 SQL_SS_NAME_SCOPE_TABLE_TYPE，而不是其默认值 SQL_SS_NAME_SCOPE_TABLE，则 SQLPrimaryKeys 将返回有关表类型的主键列的信息。 有关 SQL_SOPT_SS_NAME_SCOPE 的详细信息，请参阅 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)。  
  
 有关表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLPrimaryKeys 函数](../../odbc/reference/syntax/sqlprimarykeys-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
