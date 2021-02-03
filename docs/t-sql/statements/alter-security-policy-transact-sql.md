---
description: ALTER SECURITY POLICY (Transact-SQL)
title: ALTER SECURITY POLICY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SECURITY_POLICY_TSQL
- ALTER SECURITY POLICY
- ALTER_SECURITY_TSQL
- ALTER SECURITY
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER SECURITY POLICY statement
ms.assetid: a8efc37e-113d-489c-babc-b914fea2c316
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e7c0976f6fe428b21817a2577cced14dda66852e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182199"
---
# <a name="alter-security-policy-transact-sql"></a>ALTER SECURITY POLICY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

更改安全策略。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER SECURITY POLICY schema_name.security_policy_name   
    (  
        { ADD { FILTER | BLOCK } PREDICATE tvf_schema_name.security_predicate_function_name   
           ( { column_name | arguments } [ , ...n ] ) ON table_schema_name.table_name   
           [ <block_dml_operation> ]  }   
        | { ALTER { FILTER | BLOCK } PREDICATE tvf_schema_name.new_security_predicate_function_name   
             ( { column_name | arguments } [ , ...n ] ) ON table_schema_name.table_name   
           [ <block_dml_operation> ] }  
        | { DROP { FILTER | BLOCK } PREDICATE ON table_schema_name.table_name }   
        | [ <additional_add_alter_drop_predicate_statements> [ , ...n ] ]  
    )    [ WITH ( STATE = { ON | OFF } ) ]  
    [ NOT FOR REPLICATION ]  
[;]  
  
<block_dml_operation>  
    [ { AFTER { INSERT | UPDATE } }   
    | { BEFORE { UPDATE | DELETE } } ]  
```  

## <a name="arguments"></a>参数
security_policy_name  
安全策略的名称。 安全策略名称必须符合有关标识符的规则，并且在数据库中以及对其架构均必须是唯一的。  
  
schema_name  
是安全策略所属架构的名称。 由于架构绑定，必须使用 schema_name。  
  
[ FILTER | BLOCK ]  
绑定到目标表的函数的安全谓词类型。 FILTER 谓词以静默方式筛选可用于读取操作的行。 BLOCK 谓词显式阻止违反谓词函数的写入操作。  
  
tvf_schema_name.security_predicate_function_name  
是用作谓词的内联表值函数，并且将强制用于对目标表的查询。 最多能为针对特定表的特定 DML 操作定义一个安全谓词。 使用 SCHEMABINDING 选项创建内联表值函数。  
  
{ column_name | arguments }  
用作安全谓词函数的参数的列名和表达式。 目标表上的任意列都能用作谓词函数的参数。 可以使用包含文字、内置的表达式和使用算术运算符的表达式。  
  
table_schema_name.table_name  
是安全谓词的目标表。 对于特定 DML 操作，多个禁用的安全策略能以单个表为目标，但是在任何给定时间只能启用一个。  
  
*\<block_dml_operation>*  
应用的 block 谓词的特定 DML 操作。 AFTER 指定将针对 DML 操作（INSERT 或 UPDATE）执行后的行值计算谓词。 BEFORE 指定将针对 DML 操作（UPDATE 或 DELETE）执行前的行值计算谓词。 如果不指定任何操作，则谓词将应用到所有操作。  
  
你无法对应用的 block 谓词的操作执行 ALTER，因为该操作用于唯一标识该谓词。 相反，必须删除该谓词，并为新操作添加一个新的谓词。  
  
WITH ( STATE = { ON | OFF } )  
使安全策略能够或禁止其对目标表强制执行其安全谓词。 如果未指定，则将启用正在创建的安全性策略。  
  
NOT FOR REPLICATION  
指示当复制代理修改目标对象时不应执行安全策略。 有关详细信息，请参阅[控制同步期间触发器和约束的行为（复制 Transact-SQL 编程）](../../relational-databases/replication/control-behavior-of-triggers-and-constraints-in-synchronization.md)。  
  
table_schema_name.table_name  
是应用的安全谓词的目标表。 多个禁用的安全策略能以单个表为目标，但是在任何给定时间只能启用一个。  
  
## <a name="remarks"></a>备注
ALTER SECURITY POLICY 语句位于事务范围内。 如果对事务进行回滚，也将对该语句进行回滚。  
  
将谓词函数用于内存优化表时，安全策略中必须包含 SCHEMABINDING 并使用 WITH NATIVE_COMPILATION 编译提示 。 不能使用 ALTER 语句更改 SCHEMABINDING 参数，因为该参数应用于所有谓词。 若要更改架构绑定，必须先删除安全策略，然后再重新创建。  
  
在执行相应 DML 操作后计算阻止谓词。 因此，READ UNCOMMITTED 查询可看到要回滚的临时值。  
  
## <a name="permissions"></a>权限  
需要 ALTER ANY SECURITY POLICY 权限。  
  
另外，每个添加的谓词都需要以下权限：  
  
-   针对被用作谓词的函数的 SELECT 和 REFERENCES 权限。  
-   针对绑定到策略的目标表的 REFERENCES 权限。  
-   针对目标表上被用作参数的每一列的 REFERENCES 权限。  
  
## <a name="examples"></a>示例  
以下示例演示了 ALTER SECURITY POLICY 语法的使用。 有关完整安全策略方案的示例，请参阅[行级安全性](../../relational-databases/security/row-level-security.md)。  
  
### <a name="a-adding-an-additional-predicate-to-a-policy"></a>A. 向策略添加其他谓词  
以下语法更改安全策略，同时添加一个筛选器谓词到 `mytable` 表。  
  
```sql  
ALTER SECURITY POLICY pol1   
    ADD FILTER PREDICATE schema_preds.SecPredicate(column1)   
    ON myschema.mytable;  
```  
  
### <a name="b-enabling-an-existing-policy"></a>B. 启用一个现有策略  
以下示例使用了 ALTER 语法来启用安全策略。  
  
```sql  
ALTER SECURITY POLICY pol1 WITH ( STATE = ON );  
```  
  
### <a name="c-adding-and-dropping-multiple-predicates"></a>C. 添加和删除多个谓词  
以下语法更改安全策略，同时添加筛选器谓词到 `mytable1` 和 `mytable3` 表，并删除 `mytable2` 表上的筛选器谓词。  
  
```sql  
ALTER SECURITY POLICY pol1  
ADD FILTER PREDICATE schema_preds.SecPredicate1(column1)   
    ON myschema.mytable1,  
DROP FILTER PREDICATE   
    ON myschema.mytable2,  
ADD FILTER PREDICATE schema_preds.SecPredicate2(column2, 1)   
    ON myschema.mytable3;  
```  
  
### <a name="d-changing-the-predicate-on-a-table"></a>D. 更改表中谓词  
下面的语法可将 mytable 表中的现有筛选器谓词更改为 SecPredicate2 函数。  
  
```sql  
ALTER SECURITY POLICY pol1  
    ALTER FILTER PREDICATE schema_preds.SecPredicate2(column1)  
        ON myschema.mytable;  
```  
  
### <a name="e-changing-a-block-predicate"></a>E. 更改 block 谓词  
更改 block 谓词函数以便对表进行操作。  
  
```sql 
ALTER SECURITY POLICY rls.SecPol  
    ALTER BLOCK PREDICATE rls.tenantAccessPredicate_v2(TenantId) 
    ON dbo.Sales AFTER INSERT;  
```  
  
## <a name="see-also"></a>另请参阅  
[行级安全性](../../relational-databases/security/row-level-security.md)   
[CREATE SECURITY POLICY (Transact-SQL)](../../t-sql/statements/create-security-policy-transact-sql.md)   
[DROP SECURITY POLICY (Transact-SQL)](../../t-sql/statements/drop-security-policy-transact-sql.md)   
[sys.security_policies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
[sys.security_predicates (Transact-SQL)](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)  
  
  
