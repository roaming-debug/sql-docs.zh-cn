---
description: PARSENAME (Transact-SQL)
title: PARSENAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- PARSENAME_TSQL
- PARSENAME
dev_langs:
- TSQL
helpviewer_keywords:
- PARSENAME function
- parsing [SQL Server], PARSENAME function
- names [SQL Server], objects
- objects [SQL Server], names
- part of object names [SQL Server]
ms.assetid: abf34f99-9ee9-460b-85b2-930ca5c4b5ae
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 02227b388b7c2a50d96aa329fa49c796434f8680
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472018"
---
# <a name="parsename-transact-sql"></a>PARSENAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回对象名称的指定部分。 可检索的对象部分包括对象名称、架构名称、数据库名称和服务器名称。 
  
> [!NOTE]  
>  PARSENAME 函数不指示指定名称的对象是否存在。 PARSENAME 仅返回指定对象名称的指定部分。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
PARSENAME ('object_name' , object_piece )
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

“object_name”参数具有要检索其指定部分的对象的名称。 此参数是可选的限定对象名称。 如果对象名称的所有部分都是限定的，则此名称可包含四部分：服务器名称、数据库名称、架构名称以及对象名称。  “object_name”字符串的每个部分都是 sysname 类型，它等效于 nvarchar(128) 或 256 个字节。 如果字符串的任何部分超过 256 个字节，PARSENAME 将对该部分返回 NULL，因为它不是有效的 sysname。
  
object_piece  
要返回的对象部分。 object_piece 的数据类型为 int，可以为下列值：  
    1 = 对象名称  
    2 = 架构名称  
    3 = 数据库名称  
    4 = 服务器名称  
  
## <a name="return-type"></a>返回类型

 **sysname**
  
## <a name="remarks"></a>备注

 如果存在下列条件之一，则 PARSENAME 返回 NULL：  
  
-   object_name 或 object_piece 为 NULL 。  
  
-   发生语法错误。  
  
 请求的对象部分长度为 0，且不是有效的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符。 长度为零的对象的名称将使整个限定名称无效。  
  
## <a name="examples"></a>示例

 以下示例使用 `PARSENAME` 返回有关 `Person` 数据库中 `AdventureWorks2012` 表的信息。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 1) AS 'Object Name';  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 2) AS 'Schema Name';  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 3) AS 'Database Name';  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 4) AS 'Server Name';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```
Object Name
------------------------------
DimCustomer

(1 row(s) affected)

Schema Name
------------------------------
dbo

(1 row(s) affected)

Database Name
------------------------------
AdventureWorksPDW2012

(1 row(s) affected)

Server Name
------------------------------
(null)

(1 row(s) affected)
```
  
## <a name="see-also"></a>另请参阅

- [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)  
- [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
- [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
- [系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  

