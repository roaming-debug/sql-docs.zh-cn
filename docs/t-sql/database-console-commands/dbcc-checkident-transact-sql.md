---
description: DBCC CHECKIDENT (Transact-SQL)
title: DBCC CHECKIDENT (Transact-SQL)
ms.custom: ''
ms.date: 03/07/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKIDENT
- DBCC CHECKIDENT
- CHECKIDENT_TSQL
- DBCC_CHECKIDENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- checking identity values
- reseeding identity values
- resetting identity values
- identity values [SQL Server]
- identity values [SQL Server], checking
- modifying identity values
- current identity values
- DBCC CHECKIDENT statement
- identity values [SQL Server], reseeding
- reporting current identity values
ms.assetid: 2c00ee51-2062-4e47-8b19-d90f524c6427
author: pmasl
ms.author: umajay
monikerRange: = azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017 || = azure-sqldw-latest
ms.openlocfilehash: bb5a426514c8ddd9607114f444119429deb58aeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346066"
---
# <a name="dbcc-checkident-transact-sql"></a>DBCC CHECKIDENT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  检查 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中指定表的当前标识值，如有必要，则更改标识值。 还可以使用 DBCC CHECKIDENT 为标识列手动设置新的当前标识值。  
  
 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  

DBCC CHECKIDENT
 (
    table_name  
        [, { NORESEED | { RESEED [, new_reseed_value ] } } ]  
)  
[ WITH NO_INFOMSGS ]  
```  

```syntaxsql
-- Syntax for Azure Synapse Analytics
DBCC CHECKIDENT   
 (   
    table_name  
        [RESEED, new_reseed_value ]   
)  
[ WITH NO_INFOMSGS ]  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

 *table_name*  
 是要对其当前标识值进行检查的表名。 指定的表必须包含标识列。 表名必须遵循有关[标识符](../../relational-databases/databases/database-identifiers.md)的规则。 两个或三个部分的名称必须进行分隔，例如 'Person.AddressType' 或 [Person.AddressType]。
  
 NORESEED  
 指定不应更改当前标识值。  
  
 RESEED  
 指定应该更改当前标识值。  
  
 *new_reseed_value*  
 用作标识列的当前值的新值。  
  
 WITH NO_INFOMSGS  
 取消显示所有信息性消息。  
  
## <a name="remarks"></a>备注

 对当前标识值所做的具体更正取决于参数规范。  
  
|DBCC CHECKIDENT 命令|标识更正或所做的更正|  
|-----------------------------|---------------------------------------------|  
|DBCC CHECKIDENT ( *table_name*, NORESEED )|不重置当前标识值。 DBCC CHECKIDENT 将返回标识列的当前标识值和当前最大值。 如果这两个值不相同，则应重置标识值，以避免值序列中的潜在错误或空白。|  
|DBCC CHECKIDENT ( *table_name* )<br /><br /> 或<br /><br /> DBCC CHECKIDENT ( *table_name*, RESEED )|如果表的当前标识值小于标识列中存储的最大标识值，则使用标识列中的最大值对其进行重置。 请参阅后面的“例外”一节。|  
|DBCC CHECKIDENT ( *table_name*, RESEED, *new_reseed_value* )|将当前标识值设置为 *new_reseed_value*。 如果自从创建表以来未在表中插入任何行，或者已使用 TRUNCATE TABLE 语句删除所有行，则在运行 DBCC CHECKIDENT 之后插入的第一行将使用 *new_reseed_value* 作为标识。 如果行位于表中，或如果已通过使用 DELETE 语句删除了所有行，则下一个插入行将使用 new_reseed_value  + [当前增量](../../t-sql/functions/ident-incr-transact-sql.md)值。 如果事务插入一个行并且稍后回滚，则下一个插入行使用 new_reseed_value + [当前增量](../../t-sql/functions/ident-incr-transact-sql.md)值，就像已删除该行一样。 如果该表不为空，那么将标识值设置为小于标识列中的最大值的数字时，将会出现下列情况之一：<br /><br /> -如果标识列中存在 PRIMARY KEY 或 UNIQUE 约束，则随后在表中执行插入操作时将生成错误消息 2627，原因是生成的标识值将与现有值冲突。<br /><br /> -如果不存在 PRIMARY KEY 或 UNIQUE 约束，则随后的插入操作将产生重复的标识值。|  
  
## <a name="exceptions"></a>例外

 下表列出了 DBCC CHECKIDENT 不自动重置当前标识值时的条件，并提供了重置该值的方法。  
  
|条件|重置方法|  
|---------------|-------------------|  
|当前标识值大于表中的最大值。|执行 DBCC CHECKIDENT (table_name  , NORESEED) 以确定列中的当前最大值。 接下来，在 DBCC CHECKIDENT (table_name, RESEED,new_reseed_value) 命令中指定该值作为 new_reseed_value。<br /><br /> \- 或 -<br /><br /> 执行 DBCC CHECKIDENT (*table_name*, RESEED,*new_reseed_value*)，其中 *new_reseed_value* 设置为非常低的值，然后运行 DBCC CHECKIDENT (*table_name*, RESEED) 以更正值。|  
|删除表中的所有行。|执行 DBCC CHECKIDENT (table_name, RESEED,new_reseed_value)，其中 new_reseed_value 设置为新起始值    。|  
  
## <a name="changing-the-seed-value"></a>更改种子值

 种子值是针对加载到表中的第一行插入到标识列的值。 所有后续行都包含当前标识值和增量值，其中当前标识值是为当前表或视图生成的最新标识值。  
  
 不能将 DBCC CHECKIDENT 用于下列任务：  
  
- 更改创建表或视图时为标识列指定的原始种子值。  
  
- 重设表或视图中的现有行的种子值。  
  
 若要更改原始种子值并重设所有现有行的种子值，请删除并重新创建标识列，然后为标识列指定新的种子值。 当表包含数据时，还会将标识号添加到具有指定种子值和增量值的现有行中。 无法保证行的更新顺序。  
  
## <a name="result-sets"></a>结果集

 无论是否为包含标识列的表指定了任何选项，DBCC CHECKIDENT 都会为所有操作（除了一个操作）返回以下消息。 该操作指定新的种子值。  
  
`Checking identity information: current identity value '\<current identity value>', current column value '\<current column value>'. DBCC execution completed. If DBCC printed error messages, contact your system administrator.`
  
 在 DBCC CHECKIDENT 用于通过使用 RESEED *new_reseed_value* 来指定新的种子值时，将返回以下消息。  
  
`Checking identity information: current identity value '\<current identity value>'. DBCC execution completed. If DBCC printed error messages, contact your system administrator.`
  
## <a name="permissions"></a>权限

 调用方必须拥有包含此表的架构，或者是 **sysadmin** 固定服务器角色、**db_owner** 固定数据库角色或 **db_ddladmin** 固定数据库角色的成员。

[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 需要 db_owner 权限。
  
## <a name="examples"></a>示例  
  
### <a name="a-resetting-the-current-identity-value-if-its-needed"></a>A. 在需要时重置当前标识值  
 以下示例在需要时重置 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中指定表的当前标识值。  
  
```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKIDENT ('Person.AddressType');  
GO  
```  
  
### <a name="b-reporting-the-current-identity-value"></a>B. 报告当前标识值

 以下示例报告 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的指定表中的当前标识值，但如果该标识值不正确，不会进行更正。  
  
```sql
USE AdventureWorks2012;
GO  
DBCC CHECKIDENT ('Person.AddressType', NORESEED);
GO  
```  
  
### <a name="c-forcing-the-current-identity-value-to-a-new-value"></a>C. 强制将当前标识值设为新值  
 以下示例强制将 `AddressTypeID` 表中的 `AddressType` 列中的当前标识值设置为 10。 因为该表有现有行，因此下一个插入行将使用 11 作为值，即为该列定义的新的当前标识值加上 1（该列的当前增量值）。  

```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKIDENT ('Person.AddressType', RESEED, 10);  
GO  
```

### <a name="d-resetting-the-identity-value-on-an-empty-table"></a>D. 重置空表上的标识值

 以下示例在从表中删除所有记录后，强制将 `ErrorLog` 表中的 `ErrorLogID` 列中的当前标识值设置为 1。 因为该表没有现有行，因此下一个插入行将使用 1 作为值，即当前标识值（而不会加上为该列定义的增量值）。  
  
```sql
USE AdventureWorks2012;  
GO  
TRUNCATE TABLE dbo.ErrorLog
GO
DBCC CHECKIDENT ('dbo.ErrorLog', RESEED, 1);  
GO  
```  
  
## <a name="see-also"></a>另请参阅

[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[IDENTITY（属性）(Transact-SQL)](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
[复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)  
[USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md)  
[IDENT_SEED (Transact-SQL)](../../t-sql/functions/ident-seed-transact-sql.md)  
[IDENT_INCR (Transact-SQL)](../../t-sql/functions/ident-incr-transact-sql.md)  
