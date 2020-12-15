---
title: 处理存储过程结果 |Microsoft Docs
description: 了解 SQL Server 存储过程将数据返回到应用程序时所使用的机制。 应用程序必须能够处理所有这些类型。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC, stored procedures
- SQL Server Native Client ODBC driver, stored procedures
- stored procedures [ODBC], results
ms.assetid: 788ef2a4-17de-4526-960b-46bf29aafc9f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e1e01b61f3eb19b06b9f61653fe839818e52702d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97437415"
---
# <a name="processing-stored-procedure-results"></a>处理存储过程结果
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程具有四种用于返回数据的机制：  
  
-   过程中的每一条 SELECT 语句都生成一个结果集。  
  
-   过程可以通过输出参数返回数据。  
  
-   游标输出参数可以回传 [!INCLUDE[tsql](../../includes/tsql-md.md)] 服务器游标。  
  
-   过程可以具有整数返回代码。  
  
 应用程序必须能够处理来自存储过程的所有这些输出。 CALL 或 EXECUTE 语句应当包含返回代码和输出参数的参数标记。 使用 [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) 将它们全部绑定为输出参数， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序会将输出值传输到绑定变量。 输出参数和返回代码是返回给客户端的最后一项 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; 如果 [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) 返回 SQL_NO_DATA，则它们不会返回到应用程序。  
  
 ODBC 不支持绑定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 游标参数。 由于必须在执行过程之前绑定所有输出参数，因此，ODBC 应用程序无法调用包含输出游标参数的任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程。  
  
## <a name="see-also"></a>另请参阅  
 [运行存储过程](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)  
  
  
