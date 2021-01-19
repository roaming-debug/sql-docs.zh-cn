---
description: DBCC CHECKCONSTRAINTS (Transact-SQL)
title: DBCC CHECKCONSTRAINTS (Transact-SQL)
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC CHECKCONSTRAINTS
- DBCC_CHECKCONSTRAINTS_TSQL
- CHECKCONSTRAINTS
- CHECKCONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC CHECKCONSTRAINTS statement
- consistency [SQL Server], constraints
- checking constraint consistency
- constraints [SQL Server], consistency checks
- integrity [SQL Server], constraints
ms.assetid: da6c9cee-6687-46e8-b504-738551f9068b
author: pmasl
ms.author: umajay
ms.openlocfilehash: a0257d403980bcb869a7ac1e06471cd1699dd16d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170599"
---
# <a name="dbcc-checkconstraints-transact-sql"></a>DBCC CHECKCONSTRAINTS (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

检查当前数据库中指定表上的指定约束或所有约束的完整性。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC CHECKCONSTRAINTS  
[   
    (   
    table_name | table_id | constraint_name | constraint_id   
    )  
]  
    [ WITH   
    [ { ALL_CONSTRAINTS | ALL_ERRORMSGS } ]  
    [ , ] [ NO_INFOMSGS ]   
    ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 table_name \| table_id \| constraint_name \| constraint_id     
 要检查的表或约束。 如果指定了 table_name 或 table_id，将对该表中所有启用的约束进行检查 。 如果指定了 constraint_name 或 constraint_id，则仅检查该约束 。 如果表标识符或约束标识符都未指定，则对当前数据库中所有表上的已启用约束进行检查。  
 约束名称唯一地标识其所属于的表。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。  
  
 WITH  
 启用要指定的选项。  
  
 ALL_CONSTRAINTS  
 如果指定了表名或检查所有的表，则对表上所有启用及禁用的约束进行检查；否则，仅对启用的约束进行检查。 如果指定了约束名，则 ALL_CONSTRAINTS 无效。  
  
 ALL_ERRORMSGS  
 返回所检查的表中违反约束的所有行。 默认为前 200 行。  
  
 NO_INFOMSGS  
 取消显示所有信息性消息。  
  
## <a name="remarks"></a>备注  
DBCC CHECKCONSTRAINTS 构造并执行一个对表的所有 FOREIGN KEY 和 CHECK 约束的查询。
  
例如，外键查询可具有如下形式：
  
```sql
SELECT <columns>  
FROM <table_being_checked> LEFT JOIN <referenced_table>  
    ON <table_being_checked.fkey1> = <referenced_table.pkey1>   
    AND <table_being_checked.fkey2> = <referenced_table.pkey2>  
WHERE <table_being_checked.fkey1> IS NOT NULL   
    AND <referenced_table.pkey1> IS NULL  
    AND <table_being_checked.fkey2> IS NOT NULL  
    AND <referenced_table.pkey2> IS NULL  
```  
  
查询数据存储在临时表中。 检查完所有请求的表和约束后，将返回结果集。
DBCC CHECKCONSTRAINTS 检查 FOREIGN KEY 和 CHECK 约束的完整性，但并不检查表的磁盘上数据结构的完整性。 可使用 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 和 [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md) 执行这些数据结构检查。
  
**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本
  
如果已指定 table_name 或 table_id 并启用它用于系统版本控制，DBCC CHECKCONSTRAINTS 还会对指定表执行临时数据一致性检查 。 如果未指定 NO_INFOMSGS，此命令将在单独一行上在输出中返回每项一致性冲突。 输出格式将为 ([pkcol1], [pkcol2]..) = (\<pkcol1_value>, \<pkcol2_value>...)AND \<what is wrong with temporal table record>。
  
|勾选标记|检查失败时输出中的其他信息|  
|-----------|-----------------------------------------------|  
|PeriodEndColumn ≥ PeriodStartColumn（当前）|[sys_end] = '{0}' AND MAX(DATETIME2) = '9999-12-31 23:59:59.99999'|  
|PeriodEndColumn ≥ PeriodStartColumn（当前、历史记录）|[sys_start] = '{0}' AND [sys_end] = '{1}'|  
|PeriodStartColumn < current_utc_time（当前）|[sys_start] = '{0}' AND SYSUTCTIME|  
|PeriodEndColumn < current_utc_time（历史记录）|[sys_end] = '{0}' AND SYSUTCTIME|  
|重叠|对于两个重叠记录，则为(sys_start1, sys_end1)、(sys_start2, sys_end2)。<br /><br /> 如果有 2 个以上的重叠记录，输出也将具有多行，每行显示一对重叠。|  
  
没有方法指定 constraint_name 或 constraint_id 以仅运行临时一致性检查。
  
## <a name="result-sets"></a>结果集  
DBCC CHECKCONSTRAINTS 返回带有以下列的行集。
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|表名称|**varchar**|表的名称。|  
|Constraint Name|**varchar**|违反的约束名。|  
|其中|**varchar**|标识违反约束的行的列值分配。<br /><br /> 该列中的值可以用于 SELECT 语句（用于查询违反约束的行）的 WHERE 子句中。|  
  
## <a name="permissions"></a>权限  
要求具有 **sysadmin** 固定服务器角色或 **db_owner** 固定数据库角色的成员身份。
  
## <a name="examples"></a>示例  
  
### <a name="a-checking-a-table"></a>A. 检查表  
以下示例检查 `Table1` 数据库中的 `AdventureWorks` 表的约束完整性。
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TABLE Table1 (Col1 INT, Col2 CHAR(30));  
GO  
INSERT INTO Table1 VALUES (100, 'Hello');  
GO  
ALTER TABLE Table1 WITH NOCHECK ADD CONSTRAINT chkTab1 CHECK (Col1 > 100);  
GO  
DBCC CHECKCONSTRAINTS(Table1);  
GO  
```  
  
### <a name="b-checking-a-specific-constraint"></a>B. 检查特定约束  
以下示例检查 `CK_ProductCostHistory_EndDate` 约束的完整性。
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC CHECKCONSTRAINTS ('Production.CK_ProductCostHistory_EndDate');  
GO  
```  
  
### <a name="c-checking-all-enabled-and-disabled-constraints-on-all-tables"></a>C. 检查所有表的所有启用和禁用的约束  
 以下示例检查当前数据库中所有表上的所有启用和禁用约束的完整性。  
  
```sql  
DBCC CHECKCONSTRAINTS WITH ALL_CONSTRAINTS;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[DBCC CHECKDB (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[DBCC CHECKTABLE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)
  
  
