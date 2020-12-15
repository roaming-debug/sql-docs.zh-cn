---
description: SQLSetEnvAttr
title: SQLSetEnvAttr |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLSetEnvAttr function
ms.assetid: d4114571-feca-4330-b2e4-7bfd1050b812
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba2b49fa3e5fbc8be5ed21a42604993ce0be6755
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438690"
---
# <a name="sqlsetenvattr"></a>SQLSetEnvAttr
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [Odbc 程序员参考](../../odbc/reference/odbc-programmer-s-reference.md)定义了 odbc 驱动程序应如何从写入 odbc 2 的应用程序解释 **SQLSetEnvAttr** 属性规范。*x* 或 ODBC 3。*x* API。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序符合这些规则。  
  
 **SQLSetEnvAttr** 控制的属性之一是是否要使用连接池。 如果连接池与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序一起使用，则在使用 [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md)或 **SQLConnect** 连接时， *DriverCompletion* 参数必须设置为 SQL_DRIVER_NOPROMPT。  
  
## <a name="see-also"></a>另请参阅  
 [SQLSetEnvAttr 函数](../../odbc/reference/syntax/sqlsetenvattr-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
