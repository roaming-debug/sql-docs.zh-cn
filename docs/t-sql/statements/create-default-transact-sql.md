---
description: CREATE DEFAULT (Transact-SQL)
title: CREATE DEFAULT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_DEFAULT_TSQL
- CREATE DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], default
- default objects
- CREATE DEFAULT statement
- objects [SQL Server], creating
- DEFAULT definition
ms.assetid: 08475db4-7d90-486a-814c-01a99d783d41
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c1ebdb3e7282fba1f22eedcda9278e44a22de424
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186899"
---
# <a name="create-default-transact-sql"></a>CREATE DEFAULT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

创建名为默认值的对象。 如果绑定到列或别名数据类型，且在插入期间没有显式提供值，那么 default 会指定插入对象绑定到的列（或所有列，如果是别名数据类型的话）中的值。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]而应使用通过 ALTER TABLE 或 CREATE TABLE 的 DEFAULT 关键字创建的默认定义。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
CREATE DEFAULT [ schema_name . ] default_name   
AS constant_expression [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*schema_name*  
 default 所属架构的名称。  
  
default_name  
 default 的名称。 默认值名称必须遵守[标识符](../../relational-databases/databases/database-identifiers.md)规则。 可以选择是否指定默认值所有者名称。  
  
constant_expression  
只包含常量值的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)（不得包含任何列或其他数据库对象的名称）。 可以使用任何常量、内置函数或数学表达式，除非它们包含别名数据类型。 不得使用用户定义的函数。 将字符和日期常量放在单引号 (') 中；不需要将货币、整数和浮点常量放在引号内。 二进制数据必须以 0x 开头，货币数据必须以美元符号 ($) 开头。 默认值必须与列数据类型兼容。  
  
## <a name="remarks"></a>备注  
 只能在当前数据库中创建 default 名称。 按照架构，在数据库内默认值名称必须是唯一的。 创建 default 时，使用 sp_bindefault 将它绑定到列或别名数据类型。  
  
 如果 default 与其绑定到的列不兼容，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 便会在尝试插入 default 值时生成错误消息。 例如，N/A 无法用作数值列的 default。  
  
 如果 default 值对于它绑定到的列而言太长，值便会遭截断。  
  
 无法在单个批处理中结合使用 CREATE DEFAULT 语句与其他 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
 必须先删除 default，然后才能创建同名的新 default。 此外，必须先通过执行 sp_unbindefault 来解除绑定 default，然后才能删除它。  
  
 如果列同时有默认值以及与之关联的规则，则默认值不能违反该规则。 永远不会插入与规则冲突的默认值，每次试图插入这样的默认值时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 都会生成错误消息。  
  
 当绑定到列以后，在以下情况下将插入默认值：  
  
-   值不是显式插入。  
  
-   在 INSERT 中使用 DEFAULT VALUES 或 DEFAULT 关键字来插入默认值。  
  
 如果在创建列时指定 NOT NULL，且没有为它创建 default，便会在用户无法在此列中输入内容时生成错误消息。 下表说明默认值的存在与将列定义为 NULL 或 NOT NULL 之间的关系。 表中的项显示了结果。  
  
|列定义|没有输入项，没有默认值|没有输入项，有默认值|输入 NULL，没有默认值|输入 NULL，有默认值|  
|-----------------------|--------------------------|-----------------------|----------------------------|-------------------------|  
|**NULL**|Null|默认值|Null|Null|  
|**NOT NULL**|错误|默认值|error|error|  
  
 若要重命名默认值，请使用 sp_rename。 若要获得默认值的报告，请使用 sp_help。  
  
## <a name="permissions"></a>权限  
 若要使用 CREATE DEFAULT，用户至少必须在当前数据库中拥有 CREATE DEFAULT 权限，并对在其中创建 default 的架构拥有 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-simple-character-default"></a>A. 创建简单的字符默认值  
 以下示例创建名为 `unknown` 的字符默认值。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DEFAULT phonedflt AS 'unknown';  
```  
  
### <a name="b-binding-a-default"></a>B. 绑定默认值  
 以下示例绑定在示例 A 中创建的默认值。只有当没有为 `Phone` 表的 `Contact` 列指定项时，默认值才会生效。 
 
 > [!Note] 
 >  省略任意项与在 INSERT 语句中显式声明 NULL 不同。  
  
 由于不存在名为 `phonedflt` 的 default，因此无法执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 本例只用于演示。  
  
```sql  
USE AdventureWorks2012;  
GO  
sp_bindefault 'phonedflt', 'Person.PersonPhone.PhoneNumber';  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE RULE (Transact-SQL)](../../t-sql/statements/create-rule-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT (Transact-SQL)](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE (Transact-SQL)](../../t-sql/statements/drop-rule-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [sp_bindefault (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindefault (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
  
  
