---
description: 过程
title: 过程 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- stored procedures [ODBC]
- stored procedures [ODBC], about ODBC stored procedures
- ODBC applications, statements
- statements [ODBC], stored procedures
- ODBC applications, stored procedures
ms.assetid: c64d5f3a-376b-48ef-84f3-b6148ac8600a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e9d5de4527aab29daf7a755837362f24cda5033f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752827"
---
# <a name="procedures"></a>过程
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  存储过程是包含一个或多个 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句的预先编译的可执行对象。 存储过程可具有输入和输出参数，并且还可以生成整数返回代码。 应用程序可以通过使用目录函数枚举可用的存储过程。  
  
 以 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 为目标的 ODBC 应用程序只应使用直接执行以调用某一存储过程。 当连接到的早期版本时 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] NATIVE Client ODBC 驱动程序通过创建一个临时存储过程来实现 [SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md) ，然后在 **SQLExecute** 上调用该存储过程。 它增加了 **SQLPrepare** 创建临时存储过程的开销，该过程仅调用目标存储过程，而不是直接执行目标存储过程。 即使在连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的某一实例时，准备调用都要求额外的网络上的往返，并且生成只调用存储过程执行计划的执行计划。  
  
 在执行某一存储过程时，ODBC 应用程序应使用 ODBC CALL 语法。 驱动程序将优化，以便在使用 ODBC CALL 语法时，使用远程过程调用机制来调用该过程。 这比用于将 [!INCLUDE[tsql](../../../includes/tsql-md.md)] EXECUTE 语句发送到服务器的机制效率更高。  
  
 有关详细信息，请参阅 [运行存储过程](../../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行语句 ](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  
