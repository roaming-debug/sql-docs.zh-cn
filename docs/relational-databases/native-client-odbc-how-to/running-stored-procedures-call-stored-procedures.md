---
title: " (ODBC) 调用存储过程 |Microsoft Docs"
description: 在将 ODBC 应用程序用于 SQL Server 2005 或更高版本之前，了解如何使用 ODBC 管理器、以编程方式或使用文件来添加数据源。
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- stored procedures [ODBC], calling
ms.assetid: 31176be8-d40e-4f93-8d44-a46e804a3e2d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 152ecb6304a4049418e82dee42d9dd236fc38f4b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751837"
---
# <a name="running-stored-procedures---call-stored-procedures"></a>运行存储过程 - 调用存储过程
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ODBC 驱动程序支持将存储过程作为远程存储过程执行。 通过将存储过程作为远程存储过程执行，可使驱动程序和服务器能够优化存储过程的执行性能。  
  
  当 SQL 语句使用 ODBC CALL 转义子句调用存储过程时，Microsoft® SQL Server™ 驱动程序使用远程存储过程调用 (RPC) 机制将该过程发送到 SQL Server。 RPC 请求在 SQL Server 中跳过大多数语句分析和参数处理，因此，其速度快于使用 Transact-SQL EXECUTE 语句。  
  
 有关演示此功能的示例应用程序，请参阅使用 [ODBC&#41;&#40;处理返回代码和输出参数 ](../../relational-databases/native-client-odbc-how-to/running-stored-procedures-process-return-codes-and-output-parameters.md)。  
  
### <a name="to-run-a-procedure-as-an-rpc"></a>将过程作为 RPC 运行  
  
1.  构造使用 ODBC CALL 转义序列的 SQL 语句。 该语句对每个输入、输入/输出和输出参数以及过程返回值（如果有）使用参数标记：  
  
    ```  
    {? = CALL procname (?,?)}  
    ```  
  
2.  为每个输入、输入/输出和输出参数调用 [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) ，并为过程返回值调用，如果有任何)  (。  
  
3.  用 [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md)执行语句。  
  
> [!NOTE]  
>  如果应用程序使用 Transact-SQL EXECUTE 语法提交过程（这与 ODBC CALL 转义序列相反），SQL Server ODBC 驱动程序会将过程调用作为 SQL 语句（而非 RPC）传递给 SQL Server。 此外，如果未使用 Transact-SQL EXECUTE 语句，则不会返回输出参数。  
  
## <a name="see-also"></a>另请参阅  
  [批处理存储过程调用](../../relational-databases/native-client-odbc-stored-procedures/batching-stored-procedure-calls.md)   
 [运行存储过程](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [调用存储过程](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)   
 [过程](../../relational-databases/native-client-odbc-queries/executing-statements/procedures.md)  
  
