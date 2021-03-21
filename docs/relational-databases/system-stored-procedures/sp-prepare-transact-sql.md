---
description: sp_prepare (Transact SQL)
title: sp_prepare (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 02/28/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepare_TSQL
- sp_cursor_prepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_prepare
ms.assetid: f328c9eb-8211-4863-bafa-347e1bf7bb3f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 017009d5dd3d22c8bba1eed67f5a8ebe01d87ed7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754977"
---
# <a name="sp_prepare-transact-sql"></a>sp_prepare (Transact SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

准备参数化 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句并返回执行的语句 *句柄* 。  `sp_prepare` 通过在表格格式数据流 (TDS) 包中指定 ID = 11 来调用。  
  
 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
sp_prepare handle OUTPUT, params, stmt, options  
```  
  
## <a name="arguments"></a>参数  
 *柄*  
 是 SQL Server 生成的已 *准备的句柄* 标识符。 *句柄* 是带有 **int** 返回值的必需参数。  
  
 *params*  
 标识参数化语句。 变量的参数定义将替换为语句 *中的参数* 标记。 *params* 是调用 **ntext**、 **nchar** 或 **nvarchar** 输入值的必需参数。 如果语句未参数化，则输入一个 NULL 值。  
  
 *stmt*  
 定义游标结果集。 *Stmt* 参数是必需的，并且调用了 **ntext**、 **nchar** 或 **nvarchar** 输入值。  
  
 *options*  
 一个可选参数，它返回游标结果集列的说明。 *选项* 需要以下 int 输入值：  
  
|值|说明|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
## <a name="examples"></a>示例  
A. 下面的示例准备并执行一个简单的语句。  
  
```sql  
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@P1 NVARCHAR(128), @P2 NVARCHAR(100)',  
    N'SELECT database_id, name FROM sys.databases WHERE name=@P1 AND state_desc = @P2';  
EXEC sp_execute @P1, N'tempdb', N'ONLINE';  
EXEC sp_unprepare @P1;  
```

B. 下面的示例在 AdventureWorks2016 数据库中准备一个语句，然后使用该句柄执行该语句。

```sql
-- Prepare query
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@Param INT',  
    N'SELECT *
FROM Sales.SalesOrderDetail AS sod
INNER JOIN Production.Product AS p ON sod.ProductID = p.ProductID
WHERE SalesOrderID = @Param
ORDER BY Style DESC;';  

-- Return handle for calling application
SELECT @P1;
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
-----------
1

(1 row affected)
```

然后，应用程序在放弃已准备的计划之前，使用句柄值1执行查询两次。

```sql
EXEC sp_execute 1, 49879;  
GO

EXEC sp_execute 1, 48766;
GO

EXEC sp_unprepare 1; 
GO
```
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  

