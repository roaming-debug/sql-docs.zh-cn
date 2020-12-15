---
description: SQLProcedures
title: SQLProcedures |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLProcedures function
ms.assetid: ec41f017-f5e0-40ef-913a-65d206068631
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5fc316c908b5de2e2fab906bbe80c1ed52caaec6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484999"
---
# <a name="sqlprocedures"></a>SQLProcedures
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程都返回值。 **SQLProcedures** 报告结果集列 SQL_PT_FUNCTION PROCEDURE_TYPE。  
  
 **SQLProcedures** 返回 SQL_SUCCESS *CatalogName、SchemaName* 或 *ProcName* 参数是否存在值。 当在这些参数中使用了无效值时， **SQLFetch** 将返回 SQL_NO_DATA。  
  
 可以对静态服务器游标执行 **SQLProcedures** 。 尝试对可更新的 (动态或键集) 游标执行 **SQLProcedures** 将返回 SQL_SUCCESS_WITH_INFO，指示游标类型已更改。  
  
 **SQLProcedures** 返回有关其名称与 *ProcName* 匹配并且可以由当前用户执行的任何过程的信息，或者当前用户已被授予 VIEW DEFINITION 权限的任何过程的相关信息。  
  
## <a name="see-also"></a>另请参阅  
 [SQLProcedures 函数](../../odbc/reference/syntax/sqlprocedures-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
