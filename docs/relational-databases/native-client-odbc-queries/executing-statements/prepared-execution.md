---
description: 准备好的执行
title: 已准备好的执行 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- deferred statement preparation
- prepared execution [ODBC]
- SQLPrepare function
- ODBC applications, statements
- SQLExecute function
- statements [ODBC], prepared execution
ms.assetid: f3a9d32b-6cd7-4f0c-b38d-c8ccc4ee40c3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8da07014b33b710a2036f5e411b3310180eaae02
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752887"
---
# <a name="prepared-execution"></a>准备好的执行
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC API 会定义准备好的执行，以此来减少反复执行 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句时所需的分析和编译开销。 该应用程序生成一个包含 SQL 语句的字符串，然后分两个阶段执行该语句。 它将调用 [SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md) 一次，以将语句分析并编译为执行计划 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 。 然后，它会在每次执行准备好的执行计划时调用 **SQLExecute** 。 这节省了每次执行的分析和编译开销。 应用程序通常使用准备好的执行来重复执行相同的参数化 SQL 语句。  
  
 对于多数数据库中的需要执行三次或四次以上的语句，准备好的执行要比直接执行更快速。这主要是因为准备好的执行仅需编译一次语句，而直接执行的语句在每次执行时都需要编译。 准备好的执行还可以减少网络流量，因为驱动程序在每次执行语句时都可以向数据源发送执行计划标识符及参数值，而不是整个 SQL 语句。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通过改进的算法来检测和重复使用 **SQLExecDirect** 的执行计划，从而减少了直接执行和准备好的执行之间的性能差异。 这使直接执行的语句也具备了准备好的执行的某些性能优势。 有关详细信息，请参阅 [直接执行](../../../relational-databases/native-client-odbc-queries/executing-statements/direct-execution.md)。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 还提供针对准备好的执行的本机支持。 执行计划建立在 **SQLPrepare** 上，并在调用 **SQLExecute** 时执行。 由于在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **SQLPrepare** 上生成临时存储过程不是必需的，因此 **tempdb** 中的系统表不会产生额外的开销。  
  
 出于性能方面的原因，语句准备会推迟到调用 **SQLExecute** 或元属性操作 (例如，在 ODBC) 中执行 [SQLDescribeCol](../../../relational-databases/native-client-odbc-api/sqldescribecol.md) 或 [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md) 。 此选项为默认行为。 正在准备的语句如有任何错误，需等到执行该语句或执行元属性操作后才会发现。 将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序特定的语句属性 SQL_SOPT_SS_DEFER_PREPARE 设置为 SQL_DP_OFF 可以关闭此默认行为。  
  
 在延迟准备的情况下，在调用 **SQLExecute** 之前调用 **SQLDescribeCol** 或 **SQLDescribeParam** 会生成到服务器的额外往返。 在 **SQLDescribeCol** 上，驱动程序从查询中删除 WHERE 子句，并将其发送到服务器，并将 FMTONLY 设置为 ON，以获取查询返回的第一个结果集中的列的说明。 在 **SQLDescribeParam** 上，驱动程序调用服务器来获取查询中任何参数标记所引用的表达式或列的说明。 此方法也存在一些限制，如无法解析子查询中的参数。  
  
 与 Native Client ODBC 驱动程序的过度使用 **SQLPrepare** 会 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 降低性能，尤其是在连接到早期版本的 SQL Server 时。 不应对执行一次的语句使用准备好的执行。 对于执行一次的语句，准备好的执行要比直接执行慢，因为它需要多进行一次从客户端到服务器的网络往返。 在早期版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中，它还生成临时存储过程。  
  
 预定义语句不能用于在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 上创建临时对象。  
  
 使用 [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md)时使用的一些早期 ODBC 应用程序 **SQLPrepare** 。 **SQLBindParameter** 不需要使用 **SQLPrepare**，它可与 **SQLExecDirect** 一起使用。 例如，使用 **SQLExecDirect** 和 **SQLBindParameter** 从仅执行一次的存储过程检索返回代码或输出参数。 不要将 **SQLPrepare** 与 **SQLBindParameter** 一起使用，除非将多次执行相同的语句。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行语句 ](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  
