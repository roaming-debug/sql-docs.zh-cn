---
description: SQL Server Native Client 消息结果
title: 'SQL Server 消息结果 (Native Client OLE DB 提供程序) '
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, errors
- errors [OLE DB], SQL Server message results
- OLE DB error handling, SQL Server message results
ms.assetid: 6663c6f9-def1-4d9e-845b-2085e5efc401
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c9bfcc73520ddcaf7243099c04af414c8f5aff54
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738537"
---
# <a name="sql-server-native-client-message-results"></a>SQL Server Native Client 消息结果
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在执行时不会生成 Native Client OLE DB 提供程序行集或受影响的行的计数：  
  
-   PRINT  
  
-   严重性级别为 10 或更低的 RAISERROR  
  
-   DBCC  
  
-   SET SHOWPLAN  
  
-   SET STATISTICS  
  
 这些语句会返回一个或多个信息性消息，或者使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回信息性消息以替代行集或计数结果。 成功执行时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native client OLE DB 提供程序将返回 S_OK，并且消息可供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] native client OLE DB 提供程序使用者使用。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native client OLE DB 提供程序返回 S_OK 并在执行多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或使用者执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序成员函数时提供一条或多条信息性消息。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 每次执行成员函数后，无论返回代码的值、是否存在返回的 IRowset 或 IMultipleResults 接口引用，或者是否缺少返回的或接口引用或受影响的行的计数，Native Client OLE DB 提供程序使用者允许动态指定查询文本。  
  
## <a name="see-also"></a>另请参阅  
 [错误](../../relational-databases/native-client-ole-db-errors/errors.md)  
  
  
