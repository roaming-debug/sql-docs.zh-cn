---
title: 命令语法（OLE DB 驱动程序）| Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 识别的命令语法，以及如何运行 SQL Server 存储过程。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, commands
- commands [OLE DB]
- OLE DB Driver for SQL Server, stored procedures
- stored procedures [OLE DB], command syntax
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a8210c1131d928da03bb6b580a5f6218cbfd97af
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751917"
---
# <a name="command-syntax"></a>命令语法
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server 可以识别由 DBGUID_SQL 宏指定的命令语法。 对于 OLE DB Driver for SQL Server，说明符指示 ODBC SQL、ISO 和 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 组合使用有效的语法。 例如，以下 SQL 语句使用 ODBC SQL 转义序列指定 LCASE 字符串函数：  
  
```  
SELECT customerid={fn LCASE(CustomerID)} FROM Customers  
```  
  
 LCASE 返回一个字符串，将所有大写字母字符转换为相应的小写字母字符。 ISO 字符串函数 LOWER 执行相同的操作，因此以下 SQL 语句是与上述 ODBC 语句等效的 ISO 命令：  
  
```  
SELECT customerid=LOWER(CustomerID) FROM Customers  
```  
  
 在指定为命令的文本时，适用于 SQL Server 的 OLE DB 驱动程序成功处理这种语句的形式之一。  
  
## <a name="stored-procedures"></a>存储过程  
 使用适用于 SQL Server 的 OLE DB 驱动程序命令执行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 存储过程时，在命令文本中使用 ODBC CALL 转义序列。 适用于 SQL Server 的 OLE DB 驱动程序然后使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的远程过程调用机制优化命令处理。 例如，以下 ODBC SQL 语句是比 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 形式更常使用的命令文本：  
  
-   ODBC SQL  
  
    ```  
    {call SalesByCategory('Produce', '1995')}  
    ```  
  
-   Transact-SQL  
  
    ```  
    EXECUTE SalesByCategory 'Produce', '1995'  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [命令](../../oledb/ole-db-commands/commands.md)  
  
  
