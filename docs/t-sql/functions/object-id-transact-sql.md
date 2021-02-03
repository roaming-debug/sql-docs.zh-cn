---
description: OBJECT_ID (Transact-SQL)
title: OBJECT_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECT_ID
- OBJECT_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], IDs
- identification numbers [SQL Server], database objects
- checking object exists
- IDs [SQL Server], database objects
- OBJECT_ID function
- database objects [SQL Server], IDs
- displaying object IDs
- viewing object IDs
- verifying object exists
ms.assetid: f89286db-440f-4218-a828-30881ce3077a
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0d3e0cac16d1a3d7f180117dc29b20fea8e53276
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189917"
---
# <a name="object_id-transact-sql"></a>OBJECT_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回架构范围内对象的数据库对象标识号。  
  
> [!IMPORTANT]  
>  使用 OBJECT_ID 不能查询非架构范围内的对象（如 DDL 触发器）。 对于在 [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) 目录视图中找不到的对象，需要通过查询适当的目录视图来获取该对象的标识号。 例如，若要返回 DDL 触发器的对象标识号，请使用 `SELECT OBJECT_ID FROM sys.triggers WHERE name = 'DatabaseTriggerLog``'`。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
OBJECT_ID ( '[ database_name . [ schema_name ] . | schema_name . ]   
  object_name' [ ,'object_type' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **'** *object_name* **'**  
 要使用的对象。 object_name 为 varchar 或 nvarchar。 如果 object_name 为 varchar，则它会隐式转换为 nvarchar。 可以选择是否指定数据库和架构名称。  
  
 **'** *object_type* **'**  
 架构范围的对象类型。 object_type 为 varchar 或 nvarchar。 如果 object_type 为 varchar，则它会隐式转换为 nvarchar。 有关对象类型的列表，请参阅 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) 中的 type 列。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="exceptions"></a>例外  
 对于空间索引，OBJECT_ID 返回 NULL。  
  
 出现错误时，返回 NULL。  
  
 用户只能查看符合如下条件的安全对象的元数据：该安全对象为该用户所有，或已授予该用户对该安全对象的权限。 也就是说，如果用户对该对象没有任何权限，则那些会生成元数据的内置函数（如 OBJECT_ID）可能返回 NULL。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="remarks"></a>注解  
 当该参数对系统函数可选时，则采用当前数据库、主机、服务器用户或数据库用户。 内置函数后面必须跟括号。  
  
 当指定临时表名时，除非当前数据库为 tempdb，否则必须在该临时表名之前加上数据库名称。 例如：`SELECT OBJECT_ID('tempdb..#mytemptable')`。  
  
 系统函数可以在选择列表、WHERE 子句和任何允许使用表达式的地方使用。 有关详细信息，请参阅[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md) 和 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-object-id-for-a-specified-object"></a>A. 返回指定对象的对象 ID  
 以下示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `Production.WorkOrder` 表的对象 ID。  
  
```sql  
USE master;  
GO  
SELECT OBJECT_ID(N'AdventureWorks2012.Production.WorkOrder') AS 'Object ID';  
GO  
```  
  
### <a name="b-verifying-that-an-object-exists"></a>B. 验证对象是否存在  
 以下示例通过验证表是否具有对象 ID 来检查指定表的存在性。 如果该表存在，则将其删除。 如果该表不存在，则不执行 `DROP TABLE` 语句。  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'dbo.AWBuildVersion', N'U') IS NOT NULL  
DROP TABLE dbo.AWBuildVersion;  
GO  
```  
  
### <a name="c-using-object_id-to-specify-the-value-of-a-system-function-parameter"></a>C. 使用 OBJECT_ID 指定系统函数的参数值  
 以下示例使用 [sys.dm_db_index_operational_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md) 函数返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `Person.Address` 表的所有索引和分区信息。  
 
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
> [!IMPORTANT]  
>  在使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数 DB_ID 和 OBJECT_ID 返回参数值时，请始终确保返回有效的 ID。 如果找不到数据库或对象的名称，例如相应名称不存在或拼写不正确，则两个函数都会返回 NULL。 **sys.dm_db_index_operational_stats** 函数将 NULL 解释为指定所有数据库或所有对象的通配符值。 由于这可能是无心之举，所以此部分中的示例说明了确定数据库 ID 和对象 ID 的安全方法。
  
```sql  
DECLARE @db_id INT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');  
IF @db_id IS NULL   
  BEGIN;  
    PRINT N'Invalid database';  
  END;  
ELSE IF @object_id IS NULL  
  BEGIN;  
    PRINT N'Invalid object';  
  END;  
ELSE  
  BEGIN;  
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);  
  END;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-returning-the-object-id-for-a-specified-object"></a>D：返回指定对象的对象 ID  
 以下示例返回 [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)] 数据库中 `FactFinance` 表的对象 ID。  
  
```sql  
SELECT OBJECT_ID('AdventureWorksPDW2012.dbo.FactFinance') AS 'Object ID';  
```  
  
## <a name="see-also"></a>另请参阅  
 [元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.dm_db_index_operational_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_NAME (Transact-SQL)](../../t-sql/functions/object-name-transact-sql.md)  
  
  

