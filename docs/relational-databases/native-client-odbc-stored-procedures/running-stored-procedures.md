---
title: 运行存储过程 |Microsoft Docs
description: 存储过程是存储在数据库中的可执行对象。 SQL Server 支持存储过程和扩展存储过程。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC, stored procedures
- stored procedures [ODBC], running
- SQL Server Native Client ODBC driver, stored procedures
- stored procedures [ODBC], executing
ms.assetid: 866b6dd3-2acd-4dfb-aeca-a0352b2d4c6a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2d28d9647c974dc4607ebd3498f0add4eede487a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97437471"
---
# <a name="running-stored-procedures"></a>运行存储过程
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  存储过程是存储在数据库中的可执行对象。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持：  
  
-   存储过程：  
  
     预编译为单个可执行过程的一条或多条 SQL 语句。  
  
-   扩展存储过程：  
  
     编写为扩展存储过程的 SQL Server 开放式数据服务 API 的 C 或 C++ 动态链接库 (DLL)。 开放式数据服务 API 扩展了存储过程的功能，以包括 C 或 C++ 代码。  
  
 执行语句时，对数据源调用存储过程（而不是直接在客户端应用程序中执行或准备语句）可以：  
  
-   提高性能  
  
     SQL 语句在创建过程时进行分析和编译。 这样，在执行过程时便可节省此开销。  
  
-   降低网络开销  
  
     执行过程而不是通过网络发送复杂的查询，从而可降低网络的流量。 如果 ODBC 应用程序使用 ODBC { CALL } 语法执行存储过程，ODBC 还可以进行额外的优化，使得无需对参数数据进行转换。  
  
-   提供更好的一致性  
  
     如果组织的规则是在一个中央资源（如存储过程）中实现的，则只需对它们进行一次编码、测试和调试即可。 然后，各个编程人员可以使用经过测试的存储过程，而不是开发他们自己的实现。  
  
-   提高准确性  
  
     由于存储过程通常由有经验的编程人员开发，因此与由不同技术水平的编程人员多次开发而成的代码相比，这些存储过程通常具有更高的效率和更少的错误。  
  
-   增加功能  
  
     扩展存储过程可使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句所不具备的 C 和 C++ 功能。  
  
     有关如何调用存储过程的示例，请参阅使用 [ODBC&#41;&#40;处理返回代码和输出参数 ](../../relational-databases/native-client-odbc-how-to/running-stored-procedures-process-return-codes-and-output-parameters.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [调用存储过程](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)  
  
-   [批处理存储过程调用](../../relational-databases/native-client-odbc-stored-procedures/batching-stored-procedure-calls.md)  
  
-   [处理存储过程结果](../../relational-databases/native-client-odbc-stored-procedures/processing-stored-procedure-results.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [运行存储过程操作指南主题 &#40;ODBC&#41;](../native-client-odbc-how-to/running-stored-procedures-call-stored-procedures.md)  
  
