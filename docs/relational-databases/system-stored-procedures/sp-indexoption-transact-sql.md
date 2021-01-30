---
description: sp_indexoption (Transact-SQL)
title: sp_indexoption (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_indexoption
- sp_indexoption_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_indexoption
ms.assetid: 75f836be-d322-4a53-a45d-25bee6b42a52
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d8f7011bc4ce2120164683009676953ec44d58c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205529"
---
# <a name="sp_indexoption-transact-sql"></a>sp_indexoption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为用户定义的聚集索引和非聚集索引或没有聚集索引的表设置锁选项值。  
  
 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 自动从页级锁、行级锁或表级锁中进行选择。 您不必手动设置这些选项。 为知道特定类型的锁定始终合适的专家用户提供 **sp_indexoption** 。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)] 请改用 [ALTER INDEX &#40;transact-sql&#41;](../../t-sql/statements/alter-index-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_indexoption [ @IndexNamePattern = ] 'table_or_index_name'   
    , [ @OptionName = ] 'option_name'   
    , [ @OptionValue = ] 'value'  
```  
  
## <a name="arguments"></a>参数  
`[ @IndexNamePattern = ] 'table_or_index_name'` 用户定义的表或索引的限定或非限定名称。 *table_or_index_name* 为 **nvarchar (1035)**，无默认值。 仅当指定限定索引或表名时，才需要使用引号。 如果提供了包含数据库名称的完全限定表名，则数据库名称必须为当前数据库的名称。 如果指定表名时未使用索引，则将为该表中的所有索引和表本身设置指定的选项（前提是不存在聚集索引）。  
  
`[ @OptionName = ] 'option_name'` 是索引选项名称。 *option_name* 是 **varchar (35)**，无默认值。 *option_name* 可以具有以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**AllowRowLocks**|如果为 TRUE，则访问索引时允许使用行锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用行锁。 如果为 FALSE，则不使用行锁。 默认值为 TRUE。|  
|**AllowPageLocks**|如果为 TRUE，则访问索引时允许使用页锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用页锁。 如果为 FALSE，则不使用页锁。 默认值为 TRUE。|  
|**DisAllowRowLocks**|如果为 TRUE，则不使用行锁。 如果为 FALSE，则访问索引时允许使用行锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用行锁。|  
|**DisAllowPageLocks**|如果为 TRUE，则不使用页锁。 如果为 FALSE，则访问索引时允许使用页锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用页锁。|  
  
`[ @OptionValue = ] 'value'` 指定是否启用 *option_name* 设置 (TRUE、ON、yes 或 1) 或 DISABLED (FALSE、OFF、no 或 0) 。 *值* 为 **varchar (12)**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或大于 0（失败）  
  
## <a name="remarks"></a>备注  
 不支持 XML 索引。 如果指定 XML 索引，或指定了不包含索引名的表名，且该表包含 XML 索引，则该语句将失败。 若要设置这些选项，请改用 [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) 。  
  
 若要显示当前行和页面锁定属性，请使用 [INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md) 或 [sys.databases](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目录视图。  
  
-   当 **AllowRowLocks** = True 或 **DisAllowRowLocks** = false，且 **AllowPageLocks** = true 或 **DisAllowPageLocks** = false 时，访问索引时允许使用行、页和表级锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]将选择相应的锁，并且可以将锁从行锁或页锁升级到表锁。  
  
 当 **AllowRowLocks** = False 或 **DisAllowRowLocks** = true，且 **AllowPageLocks** = false 或 **DisAllowPageLocks** = true 时，在访问索引时只允许使用表级别的锁。  
  
 如果指定表名时不包含索引，则设置将应用于该表的所有索引。 如果基础表没有聚集索引（即它是一个堆），则设置按如下方式应用：  
  
-   如果将 **AllowRowLocks** 或 **DISALLOWROWLOCKS** 设置为 TRUE 或 FALSE，则该设置将应用于堆以及任何关联的非聚集索引。  
  
-   当 **AllowPageLocks** 选项设置为 TRUE 或 **DISALLOWPAGELOCKS** 设置为 FALSE 时，该设置将应用于堆以及任何关联的非聚集索引。  
  
-   如果将 **AllowPageLocks** 选项设置为 FALSE 或将 **DISALLOWPAGELOCKS** 设置为 TRUE，则该设置将完全应用于非聚集索引。 也就是说，对非聚集索引禁用所有页锁。 在堆上，只允许对页禁用共享 (S) 锁、更新 (U) 锁和排他 (X) 锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]仍然可以获取意向页锁（IS、IU 或 IX），供内部使用。  
  
## <a name="permissions"></a>权限  
 需要对表的 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-setting-an-option-on-a-specific-index"></a>A. 对特定索引设置选项  
 下面的示例对表的索引不允许页锁 `IX_Customer_TerritoryID` `Customer` 。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_indexoption N'Sales.Customer.IX_Customer_TerritoryID',  
    N'disallowpagelocks', TRUE;  
```  
  
### <a name="b-setting-an-option-on-all-indexes-on-a-table"></a>B. 对表的所有索引设置选项  
 以下示例对与 `Product` 表关联的所有索引禁用行锁。 在执行 `sys.indexes` 过程的前后，通过查询 `sp_indexoption` 目录视图来显示语句的结果。  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the current row and page lock options for all indexes on the table.  
SELECT name, type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE object_id = OBJECT_ID(N'Production.Product');  
GO  
-- Set the disallowrowlocks option on the Product table.   
EXEC sp_indexoption N'Production.Product',  
    N'disallowrowlocks', TRUE;  
GO  
--Verify the row and page lock options for all indexes on the table.  
SELECT name, type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE object_id = OBJECT_ID(N'Production.Product');  
GO  
```  
  
### <a name="c-setting-an-option-on-a-table-with-no-clustered-index"></a>C. 对不包含聚集索引的表设置选项  
 以下示例对不包含聚集索引的表（堆）禁用页锁。 在 `sys.indexes` 执行过程之前和之后，将查询目录视图 `sp_indexoption` 以显示语句的结果。  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the current row and page lock options of the table.   
SELECT OBJECT_NAME (object_id) AS [Table], type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE OBJECT_NAME (object_id) = N'DatabaseLog';  
GO  
-- Set the disallowpagelocks option on the table.   
EXEC sp_indexoption DatabaseLog,  
    N'disallowpagelocks', TRUE;  
GO  
--Verify the row and page lock settings of the table.  
SELECT OBJECT_NAME (object_id) AS [Table], allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE OBJECT_NAME (object_id) = N'DatabaseLog';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [INDEXPROPERTY (Transact-SQL)](../../t-sql/functions/indexproperty-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  
