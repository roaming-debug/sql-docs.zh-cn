---
title: 绑定参数 |Microsoft Docs
description: 了解如何在语句可以运行之前将 SQL 语句中的每个参数标记绑定到应用程序中的变量。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, parameters
- parameters [SQL Server Native Client], ODBC
- statements [ODBC], parameters
- binding parameters [SQL Server Native Client]
- parameter markers [ODBC]
- unbound parameter markers
- SQLBindParameter function
- ODBC applications, parameters
- bound parameter markers [SQL Server Native Client]
ms.assetid: d6c69739-8f89-475f-a60a-b2f6c06576e2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a1ddbcb0f19cb62310e748da4f710b196357acbc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478068"
---
# <a name="using-statement-parameters---binding-parameters"></a>使用语句参数 - 绑定参数
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在执行 SQL 语句前，该语句中的每个参数标记都必须与应用程序中的某个变量关联或绑定到某个变量。 这是通过调用 [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) 函数来完成的。 **SQLBindParameter** 介绍了程序变量 (地址、C 数据类型等) 驱动程序。 它还通过指示其序数值来标识参数标记，然后描述它所表示的 SQL 对象的特点（SQL 数据类型、精度等）。  
  
 在执行语句前，可以随时绑定或重新绑定参数标记。 直到发生下列事件之一时，参数绑定才会失效：  
  
-   如果调用 [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) 并将 *Option* 参数设置为 SQL_RESET_PARAMS 将释放所有绑定到语句句柄的参数。  
  
-   如果调用 **SQLBindParameter** ，并将 *ParameterNumber* 设置为绑定参数标记的序号，将自动释放以前的绑定。  
  
 应用程序还可以将参数绑定到程序变量数组以成批处理 SQL 语句。 数组绑定有两种不同的类型：  
  
-   当每个参数绑定到自身的变量数组时，将完成按列绑定。  
  
     通过调用 SQL_ATTR_PARAM_BIND_TYPE [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) *，并将**将 valueptr* 设置为 SQL_PARAM_BIND_BY_COLUMN 来指定按列绑定。  
  
-   当 SQL 语句中的所有参数作为一个单元绑定到包含这些参数的各个变量的结构数组时，将完成按行绑定。  
  
     通过调用 **SQLSetStmtAttr** 并将 *属性* 设置为 SQL_ATTR_PARAM_BIND_TYPE，并将 *将 valueptr* 设置为包含程序变量的结构的大小，可以指定按行绑定。  
  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序将字符或二进制字符串参数发送到服务器时，它会将值填充到在 **SQLBindParameter** *ColumnSize* 参数中指定的长度。 如果 ODBC 2.x 应用程序为 *ColumnSize* 指定0，则驱动程序将参数值填充到数据类型的精度。 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务器时，精度为 8000；连接到早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，精度为 255。 变体列的 *ColumnSize* 以字节为单位。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持为存储过程参数定义名称。 ODBC 3.5 还支持在调用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程时使用的命名参数。 此支持可用于：  
  
-   调用存储过程，并向为该存储过程定义的参数的子集提供值。  
  
-   以不同于创建存储过程时指定的顺序指定应用程序中的参数。  
  
 仅当使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] **execute** 语句或 ODBC 调用转义序列执行存储过程时，才支持命名参数。  
  
 如果为存储过程参数设置 **SQL_DESC_NAME** ，则查询中的所有存储过程参数也应该设置 **SQL_DESC_NAME**。  如果在存储过程调用中使用了文本（其中参数 **SQL_DESC_NAME** 设置），则文本应使用格式 *"name* = *value*"，其中 *NAME* 是存储过程参数名称 (例如， @p1) 。 有关详细信息，请参阅 [按名称绑定参数 (命名参数) ](../../odbc/reference/develop-app/binding-parameters-by-name-named-parameters.md)。  
  
## <a name="see-also"></a>另请参阅  
 [使用语句参数](../../relational-databases/native-client-odbc-queries/using-statement-parameters.md)  
  
