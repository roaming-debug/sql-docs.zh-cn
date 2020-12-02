---
description: CREATE RULE (Transact-SQL)
title: CREATE RULE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RULE_TSQL
- CREATE RULE
- CREATE_RULE_TSQL
- RULE
dev_langs:
- TSQL
helpviewer_keywords:
- list rules [SQL Server]
- unbinding rules
- pattern rules [SQL Server]
- range rules [SQL Server]
- alias data types [SQL Server], rules
- CREATE RULE statement
- reports [SQL Server], rules
- status information [SQL Server], rules
- rules [SQL Server], precedence
- binding rules [SQL Server]
- rules [SQL Server], creating
ms.assetid: b016a289-3a74-46b1-befc-a13183be51e4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e0b6c3ae3d269b8617d3d032c8aa2d947c211834
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124017"
---
# <a name="create-rule-transact-sql"></a>CREATE RULE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建名为规则的对象。 当绑定到列或别名数据类型时，使用规则指定可以插入到列中的可接受的值。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]建议您改用检查约束。 检查约束是使用 CREATE TABLE 或 ALTER TABLE 的 CHECK 关键字创建的。 有关详细信息，请参阅 [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md)。  
  
 列或别名数据类型只能被绑定一个规则。 不过，列可以同时有一个规则以及一个或多个检查约束与其相关联。 在这种情况下，将评估所有限制。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE RULE [ schema_name . ] rule_name   
AS condition_expression  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *schema_name*  
 规则所属架构的名称。  
  
 rule_name  
 新规则的名称。 规则名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 根据需要，指定规则所有者名称。  
  
 condition_expression  
 定义规则的条件。 规则可以是 WHERE 子句中任何有效的表达式，并且可以包括诸如算术运算符、关系运算符和谓词（如 IN、LIKE、BETWEEN）这样的元素。 规则不能引用列或其他数据库对象。 可以包括不引用数据库对象的内置函数。 不能使用用户定义函数。  
  
 condition_expression 包括一个变量。 每个局部变量前面都有一个 at 符号 (@)。 该表达式引用通过 UPDATE 或 INSERT 语句输入的值。 在创建规则时，可以使用任何名称或符号表示值，但第一个字符必须是 at 符号 (@)。  
  
> [!NOTE]  
>  请避免对使用别名数据类型的表达式创建规则。 虽然可以对使用别名数据类型的表达式创建规则，但在将规则绑定到列或别名数据类型后，表达式被引用时将无法对其进行编译。  
  
## <a name="remarks"></a>备注  
 不能在单个批处理中将 CREATE RULE 语句与其他 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句组合在一起。 规则不适用于在创建规则时已存在于数据库中的数据，而且规则不能绑定到系统数据类型。  
  
 规则只能在当前的数据库中创建。 创建规则后，执行 sp_bindrule 可将规则绑定到列或别名数据类型。 规则必须与列数据类型兼容。 例如，不能将 "\@value LIKE A%" 用作数值列的规则。 无法将规则绑定到 text、ntext、image、varchar(max)、nvarchar(max)、varbinary(max)、xml、CLR 用户定义类型或 timestamp 列。 无法将规则绑定到计算列。  
  
 用单引号 (') 将字符和日期常量括起来，并在二进制常量前加 0x。 如果规则与其绑定到的列不兼容，则 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]在插入值时（但不是在绑定规则时）会返回一个错误消息。  
  
 只有当尝试在别名数据类型的数据库列中插入值或进行更新时，绑定到别名数据类型的规则才会激活。 因为规则不检验变量，所以不要为别名数据类型变量赋予会被绑定到同一数据类型的列的规则所拒绝的值。  
  
 若要获取关于规则的报告，请使用 sp_help。 若要显示某条规则的文本，请以该规则的名称作为参数来执行 sp_helptext。 若要重命名规则，请使用 sp_rename。  
  
 在创建同名的新规则之前，必须使用 DROP RULE 删除原有规则，而在此之前，必须先使用 sp_unbindrule 取消绑定。 若要取消规则与列的绑定，请使用 sp_unbindrule。  
  
 可以在不取消绑定原有规则的情况下将新规则绑定到列或数据类型；新规则将覆盖原有规则。 绑定到列的规则始终优先于绑定到别名数据类型的规则。 将规则绑定到列时，将替换已经绑定到该列的别名数据类型的规则。 但是，将规则绑定到数据类型时，不会替换绑定到该别名数据类型的列的规则。 下表显示了当规则绑定到列以及绑定到已存在规则的别名数据类型时有效的优先级。  
  
|新规则绑定到|旧规则绑定到<br /><br /> 别名数据类型|旧规则绑定到<br /><br /> 列|  
|-----------------------|-------------------------------------------|----------------------------------|  
|别名数据类型|旧规则被替换|没有变化|  
|列|旧规则被替换|旧规则被替换|  
  
 如果列同时有与之相关联的默认值和规则，则默认值必须在规则定义的范围内。 与规则冲突的默认值永远不能被插入。 每次试图插入这样的默认值时，SQL Server 数据库引擎都会生成错误消息。  
  
## <a name="permissions"></a>权限  
 若要执行 CREATE RULE，用户必须至少在当前数据库中具有 CREATE RULE 权限，并且对要在其中创建规则的架构具有 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-rule-with-a-range"></a>A. 创建具有范围的规则  
 以下示例创建一个规则，用以限制插入该规则被绑定到的列中的整数的范围。  
  
```sql  
CREATE RULE range_rule  
AS   
@range>= $1000 AND @range <$20000;  
```  
  
### <a name="b-creating-a-rule-with-a-list"></a>B. 创建具有列表的规则  
 以下示例创建一个规则，用于将输入到该规则被绑定到的列中的实际值限制为只能是该规则中列出的值。  
  
```sql  
CREATE RULE list_rule  
AS   
@list IN ('1389', '0736', '0877');  
```  
  
### <a name="c-creating-a-rule-with-a-pattern"></a>C. 创建具有模式的规则  
 以下示例创建一个遵循这种模式的规则：任意两个字符的后面跟一个连字符 (`-`) 和任意多个字符（或没有字符），并以 `0` 到 `9` 之间的整数结尾。  
  
```sql  
CREATE RULE pattern_rule   
AS  
@value LIKE '__-%[0-9]'  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT (Transact-SQL)](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE (Transact-SQL)](../../t-sql/statements/drop-rule-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [sp_bindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
  
