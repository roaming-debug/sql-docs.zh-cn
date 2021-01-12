---
title: SET IDENTITY_INSERT (Transact-SQL) | Microsoft Docs
description: SET IDENTITY_INSERT 语句的 Transact-SQL 参考。 设置为 ON 时，允许将显式值插入到表的标识列中。
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET IDENTITY_INSERT
- SET_IDENTITY_INSERT_TSQL
- IDENTITY_INSERT_TSQL
- IDENTITY_INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY_INSERT option
- SET IDENTITY_INSERT statement
- identity values [SQL Server], explicit values
- identity columns [SQL Server], explicit values
ms.assetid: a5dd49f2-45c7-44a8-b182-e0a5e5c373ee
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: 0a2d8e7cb4625d4b1ddcd35473e16f9bcad1d810
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093360"
---
# <a name="set-identity_insert-transact-sql"></a>SET IDENTITY_INSERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

允许将显式值插入到表的标识列中。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SET IDENTITY_INSERT [ [ database_name . ] schema_name . ] table_name { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *database_name*  
 指定的表所在的数据库的名称。  
  
 *schema_name*  
 表所属架构的名称。  
  
 *table_name*  
 包含标识列的表的名称。  
  
## <a name="remarks"></a>备注  
 任何时候，一个会话中只有一个表的 IDENTITY_INSERT 属性可以设置为 ON。 如果某个表已将此属性设置为 ON，则对另一个表发出 SET IDENTITY_INSERT ON 语句时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回一个错误信息，指出 SET IDENTITY_INSERT 已设置为 ON，并报告已将其属性设置为 ON 的表。  
  
 如果插入值大于表的当前标识值，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动将新插入值作为当前标识值使用。  
  
 SET IDENTITY_INSERT 的设置是在执行或运行时设置的，而不是在分析时设置的。  
  
## <a name="permissions"></a>权限  
 用户必须拥有表，或对表具有 ALTER 权限。  
  
## <a name="examples"></a>示例  
 下面的示例将创建一个包含标识列的表，并说明如何使用 `SET IDENTITY_INSERT` 设置来填充由 `DELETE` 语句导致的标识值中的空隙。  
  
```sql
USE AdventureWorks2012;  
GO  
-- Create tool table.  
CREATE TABLE dbo.Tool(  
   ID INT IDENTITY NOT NULL PRIMARY KEY,   
   Name VARCHAR(40) NOT NULL  
);  
GO  
-- Inserting values into products table.  
INSERT INTO dbo.Tool(Name)   
VALUES ('Screwdriver')  
        , ('Hammer')  
        , ('Saw')  
        , ('Shovel');  
GO  
  
-- Create a gap in the identity values.  
DELETE dbo.Tool  
WHERE Name = 'Saw';  
GO  
  
SELECT *   
FROM dbo.Tool;  
GO  
  
-- Try to insert an explicit ID value of 3;  
-- should return an error:
-- An explicit value for the identity column in table 'AdventureWorks2012.dbo.Tool' can only be specified when a column list is used and IDENTITY_INSERT is ON.
INSERT INTO dbo.Tool (ID, Name) VALUES (3, 'Garden shovel');  
GO  
-- SET IDENTITY_INSERT to ON.  
SET IDENTITY_INSERT dbo.Tool ON;  
GO  
  
-- Try to insert an explicit ID value of 3.  
INSERT INTO dbo.Tool (ID, Name) VALUES (3, 'Garden shovel');  
GO  
  
SELECT *   
FROM dbo.Tool;  
GO  
-- Drop products table.  
DROP TABLE dbo.Tool;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [IDENTITY（属性）&#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [SCOPE_IDENTITY (Transact-SQL)](../../t-sql/functions/scope-identity-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
