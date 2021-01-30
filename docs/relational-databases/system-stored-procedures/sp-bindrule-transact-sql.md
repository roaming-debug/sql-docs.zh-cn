---
description: sp_bindrule (Transact-SQL)
title: sp_bindrule (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_bindrule_TSQL
- sp_bindrule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindrule
ms.assetid: 2606073e-c52f-498d-a923-5026b9d97e67
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 266707eca11f6664d816a958e9a3051d3648339b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197702"
---
# <a name="sp_bindrule-transact-sql"></a>sp_bindrule (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  将规则绑定到列或别名数据类型。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 请改用[Unique 约束和 Check 约束](../../relational-databases/tables/unique-constraints-and-check-constraints.md) 。 CHECK 约束是使用 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 或 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 语句的 check 关键字创建的。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_bindrule [ @rulename = ] 'rule' ,   
     [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @rulename = ] 'rule'` 由 CREATE RULE 语句创建的规则的名称。 *规则* 为 **nvarchar (776)**，无默认值。  
  
`[ @objname = ] 'object_name'` 是要将规则绑定到的表和列或别名数据类型。 无法将规则绑定到 text、ntext、image、varchar(max)、nvarchar(max)、varbinary(max)、xml、CLR 用户定义类型或 timestamp 列。 无法将规则绑定到计算列。  
  
 *object_name* 为 **nvarchar (776)** ，无默认值。 如果 *object_name* 是由一个部分组成的名称，则将其解析为别名数据类型。 如果是由两部分或三部分组成的名称，则首先按表和列进行解析；如果解析失败，则按别名数据类型进行解析。 默认情况下，除非规则直接绑定到列，否则别名数据类型的现有列将继承 *规则* 。  
  
> [!NOTE]  
>  *object_name* 可以将方括号 **[** 和 **]** 字符包含为带分隔符的标识符字符。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。  
  
> [!NOTE]  
>  可以将针对使用别名数据类型的表达式创建的规则绑定到列或别名数据类型，但在引用这些规则时无法编译它们。 避免使用对别名数据类型创建的规则。  
  
`[ @futureonly = ] 'futureonly_flag'` 仅在将规则绑定到别名数据类型时使用。 *future_only_flag* 是 **varchar (15)** ，默认值为 NULL。 此参数设置为 **futureonly** 时，会阻止别名数据类型的现有列继承新规则。 如果 *futureonly_flag* 为 NULL，则会将新规则绑定到当前没有规则或使用别名数据类型的现有规则的别名数据类型的任何列。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 您可以将新规则绑定到列 (不过，使用 CHECK 约束优先) 或使用 **sp_bindrule** 而不解除现有规则绑定的别名数据类型。 覆盖原有规则。 如果使用现有 CHECK 约束将规则绑定到列，那么将评估所有限制。 不能将规则绑定到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。  
  
 当尝试执行 INSERT 语句时（而非绑定时），将强行执行规则。 可以将字符规则绑定到 **数值** 数据类型的列，但此类插入操作无效。  
  
 除非将 *futureonly_flag* 指定为 **futureonly**，否则别名数据类型的现有列将继承新规则。 使用别名数据类型定义的新列始终继承规则。 但是，如果 ALTER TABLE 语句的 ALTER COLUMN 子句将列的数据类型更改为绑定规则的别名数据类型，那么列不会继承与数据类型绑定的规则。 必须使用 **sp_bindrule** 专门将规则绑定到列。  
  
 将规则绑定到列时，相关信息将添加到 **sys.databases** 表中。 将规则绑定到别名数据类型时，相关信息将添加到 **sys.databases** 表中。  
  
## <a name="permissions"></a>权限  
 若要将规则绑定到表列，您必须具有表的 ALTER 权限。 若要将规则绑定到别名数据类型，您需要具有别名数据类型的 CONTROL 权限或该类型所属架构的 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-binding-a-rule-to-a-column"></a>A. 将规则绑定到列  
 假设已经用 CREATE RULE 语句在当前数据库中创建了名为 `today` 的规则，以下示例将规则绑定到 `HireDate` 表的 `Employee` 列。 将行添加到 `Employee` 时，按照 `HireDate` 规则检查 `today` 列的数据。  
  
```  
USE master;  
GO  
EXEC sp_bindrule 'today', 'HumanResources.Employee.HireDate';  
```  
  
### <a name="b-binding-a-rule-to-an-alias-data-type"></a>B. 将规则绑定到别名数据类型  
 假设存在名为 `rule_ssn` 的规则和名为 `ssn` 的别名数据类型，以下示例将 `rule_ssn` 绑定到 `ssn`。 在 CREATE TABLE 语句中，类型为 `ssn` 的列会继承 `rule_ssn` 规则。 类型为的现有列 `ssn` 还继承此 `rule_ssn` 规则，除非为 *futureonly_flag* 指定 **futureonly** 或 `ssn` 直接绑定规则。 绑定到列的规则始终优先于绑定到数据类型的规则。  
  
```  
USE master;  
GO  
EXEC sp_bindrule 'rule_ssn', 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. 使用 futureonly_flag  
 以下示例将 `rule_ssn` 规则绑定到别名数据类型 `ssn`。 由于已指定 `futureonly`，因此不影响类型为 `ssn` 的现有列。  
  
```  
USE master;  
GO  
EXEC sp_bindrule rule_ssn, 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. 使用分隔标识符  
 下面的示例演示如何在 *object_name* 参数中使用分隔标识符。  
  
```  
USE master;  
GO  
CREATE TABLE [t.2] (c1 int) ;  
-- Notice the period as part of the table name.  
EXEC sp_bindrule rule1, '[t.2].c1' ;  
-- The object contains two periods;   
-- the first is part of the table name   
-- and the second distinguishes the table name from the column name.  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE RULE (Transact-SQL)](../../t-sql/statements/create-rule-transact-sql.md)   
 [DROP RULE (Transact-SQL)](../../t-sql/statements/drop-rule-transact-sql.md)   
 [sp_unbindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
