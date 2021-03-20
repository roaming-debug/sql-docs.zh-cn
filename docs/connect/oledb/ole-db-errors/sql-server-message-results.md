---
title: SQL Server 消息结果（OLE DB 驱动程序）
description: 了解不会生成 OLE DB Driver for SQL Server 行集或计数的 Transact-SQL 语句及其预期返回值。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, errors
- errors [OLE DB], SQL Server message results
- OLE DB error handling, SQL Server message results
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dabbcef5f1d894f4d9a9c08230a13f60a269ce2f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104741717"
---
# <a name="sql-server-message-results"></a>SQL Server 消息结果
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

执行下列 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句时不会生成 OLE DB Driver for SQL Server 行集或受影响的行数：  
  
-   PRINT  
  
-   严重性级别为 10 或更低的 RAISERROR  
  
-   DBCC  
  
-   SET SHOWPLAN  
  
-   SET STATISTICS  
  
 这些语句会返回一个或多个信息性消息，或者使 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 返回信息性消息以替代行集或计数结果。 在成功执行之后，OLE DB Driver for SQL Server 返回 S_OK，并向 OLE DB Driver for SQL Server 使用者提供消息。  
  
 在执行多个 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句，或者在使用者执行 OLE DB Driver for SQL Server 成员函数之后，OLE DB Driver for SQL Server 返回 S_OK，并提供一条或多条信息性消息。  
  
允许 OLE DB Driver for SQL Server 使用者动态指定查询文本。 在每次执行成员函数后使用者应检查错误接口。 无论返回代码的值是什么、是否返回对 `IRowset` 或 `IMultipleResults` 接口的引用、受影响的行数是多少，始终应执行这些检查。
  
## <a name="see-also"></a>另请参阅  
 [错误](../../oledb/ole-db-errors/errors.md)  
  
  
