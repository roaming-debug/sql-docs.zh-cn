---
title: 调用存储过程 |Microsoft Docs
description: 了解 ODBC 调用转义序列，这是运行存储过程的首选方法。 Native Client ODBC 驱动程序还支持 SQLEXECUTE。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- calling stored procedures
- ODBC, stored procedures
- stored procedures [ODBC], calling
- SQL Server Native Client ODBC driver, stored procedures
- ODBC CALL escape sequence
- escape sequences [SQL Server]
- CALL statement
ms.assetid: d13737f4-f641-45bf-b56c-523e2ffc080f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 59ec1c5395a262920062802d5785575425c994a6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748129"
---
# <a name="calling-a-stored-procedure"></a>调用存储过程
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序同时支持 ODBC 调用转义序列和执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程的[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)语句; odbc call 转义序列是首选方法。 使用 ODBC 语法使应用程序能检索存储过程的返回代码，还可以对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序进行优化以使用最初为在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的两台计算机间发送远程过程调用 (RPC) 开发的协议。 此 RPC 协议通过避免在服务器上进行大量参数处理和语句分析来提高性能。  
  
> [!NOTE]  
>  当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用带有 ODBC (的命名参数调用存储过程时，请参阅 [按 (名称绑定参数) ](../../odbc/reference/develop-app/binding-parameters-by-name-named-parameters.md)) ，参数名称必须以 " \@ " 字符开头。 这是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 特有的限制。 与 Microsoft 数据访问组件 (MDAC) 相比，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序更加严格地遵守此限制。  
  
 调用过程的 ODBC CALL 转义序列是：  
  
 {[**？ =**]**调用**_procedure_name_[ ( [*参数*] [**，**[*参数*]] ... ) ]}  
  
 其中 *procedure_name* 指定过程的名称， *参数* 指定过程参数。 只有使用 ODBC CALL 转义序列的语句中才支持命名参数。  
  
 一个过程可以有零个或多个参数。 它还可以返回值（如语法开头的可选参数标记 ?= 所示）。 如果参数是输入或输入/输出参数，它可以是文字或参数标记。 如果参数是输出参数，它必须是参数标记，因为输出是未知的。 执行过程调用语句之前，必须与 [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) 绑定参数标记。  
  
 过程调用的输入和输入/输出参数可以省略。 如果使用括号但不带任何参数调用过程，驱动程序将指示数据源使用第一个参数的默认值。 例如：  
  
 {**调用** _procedure_name_ **( )**}  
  
 如果该过程不具有任何参数，则它可能失败。 如果不带括号调用过程，驱动程序将不发送任何参数值。 例如：  
  
 {**call** _procedure_name_}  
  
 可以为过程调用中的输入和输入/输出参数指定文字。 例如，过程 InsertOrder 具有五个输入参数。 以下对 InsertOrder 的调用省略了第一个参数，为第二个参数提供文字，为第三、第四、第五个参数使用参数标记。 （按顺序对参数编号，从值 1 开始。）  
  
```  
{call InsertOrder(, 10, ?, ?, ?)}  
```  
  
 请注意如果省略一个参数，分隔该参数与其他参数的逗号必须保留。 如果省略输入参数或输入/输出参数，过程会使用该参数的默认值。 指定输入参数或输入/输出参数默认值的其他方式包括：将绑定到该参数的长度/指示器缓冲区的值设置为 SQL_DEFAULT_PARAM，或使用 DEFAULT 关键字。  
  
 如果省略输入/输出参数，或者如果为该参数提供文字，驱动程序将放弃输出值。 同样，如果省略过程返回值的参数标记，驱动程序将放弃返回值。 最后一点，如果应用程序为不返回值的过程指定返回值参数，驱动程序将绑定到该参数的长度/指示器缓冲区的值设置为 SQL_NULL_DATA。  
  
## <a name="delimiters-in-call-statements"></a>CALL 语句中的分隔符  
 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序还支持 ODBC {CALL} 转义序列特有的兼容性选项。 该驱动程序接受仅用一对双引号分隔整个存储过程名称的 CALL 语句：  
  
```  
{ CALL "master.dbo.sp_who" }  
```  
  
 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序还接受遵循 ISO 规则并用双引号将每个标识符括起来的 CALL 语句：  
  
```  
{ CALL "master"."dbo"."sp_who" }  
```  
  
 不过，使用默认设置运行时，如果标识符包含不遵循 ISO 标准的字符，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序不支持上述任何一种带引号的标识符形式。 例如，驱动程序无法使用带有带引号的标识符的 CALL 语句访问名为 **"Proc"** 的存储过程：  
  
```  
{ CALL "MyDB"."MyOwner"."My.Proc" }  
```  
  
 此语句将被该驱动程序解释为：  
  
```  
{ CALL MyDB.MyOwner.My.Proc }  
```  
  
 服务器引发错误，指出名为 **MyDB** 的链接服务器不存在。  
  
 使用带方括号的标识符时则不存在此问题，此语句可以正确地进行解释：  
  
```  
{ CALL [MyDB].[MyOwner].[My.Table] }  
```  
  
## <a name="see-also"></a>另请参阅  
 [运行存储过程](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)  
  
