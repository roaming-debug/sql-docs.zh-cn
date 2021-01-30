---
description: ALTER TABLE - SQL 命令
title: ALTER TABLE-SQL 命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- alter table [ODBC]
ms.assetid: 3a01a291-f4d9-43bc-a725-5a95546ff364
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 186647c40517c5ecf77a55e6bbc08155c98ffed9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205755"
---
# <a name="alter-table---sql-command"></a>ALTER TABLE - SQL 命令
以编程方式修改表的结构。  
  
## <a name="syntax"></a>语法  
  
```  
  
ALTER TABLE TableName1  
   ADD | ALTER [COLUMN] FieldName1  
      FieldType [(nFieldWidth [, nPrecision])]  
      [NULL | NOT NULL]  
      [CHECK lExpression1 [ERROR cMessageText1]]  
      [DEFAULT eExpression1]  
      [PRIMARY KEY | UNIQUE]  
      [REFERENCES TableName2 [TAG TagName1]]  
      [NOCPTRANS]  
 - Or -  
ALTER TABLE TableName1  
   ALTER [COLUMN] FieldName2  
      [NULL | NOT NULL]  
      [SET DEFAULT eExpression2]  
      [SET CHECK lExpression2 [ERROR cMessageText2]]  
      [DROP DEFAULT]  
      [DROP CHECK]  
 - Or -  
ALTER TABLE TableName1  
   [DROP [COLUMN] FieldName3]  
   [SET CHECK lExpression3 [ERROR cMessageText3]]  
   [DROP CHECK]  
   [ADD PRIMARY KEY eExpression3 TAG TagName2]  
   [DROP PRIMARY KEY]  
   [ADD UNIQUE eExpression4 [TAG TagName3]]  
   [DROP UNIQUE TAG TagName4]  
   [ADD FOREIGN KEY [eExpression5] TAG TagName4  
      REFERENCES TableName2 [TAG TagName5]]  
   [DROP FOREIGN KEY TAG TagName6 [SAVE]]  
   [RENAME COLUMN FieldName4 TO FieldName5]  
   [NOVALIDATE]  
```  
  
## <a name="arguments"></a>参数  
 *TableName1*  
 指定其结构已修改的表的名称。  
  
 添加 [COLUMN] *FieldName1*  
 指定要添加的字段的名称。  
  
 ALTER [COLUMN] *FieldName1*  
 指定要修改的现有字段的名称。  
  
 *FieldType* [ ( *NFieldWidth* [， *nPrecision*]] )   
 为新的或修改的字段指定字段类型、字段宽度和字段精度 (小数点位数) 。  
  
 *FieldType* 是一个表示字段的 [数据类型](../../odbc/microsoft/visual-foxpro-field-data-types.md)的单字母。 某些字段数据类型要求指定 *nFieldWidth* 或 *nPrecision* ，或同时指定两者。  
  
 对于 D、G、I、L、M、P、T 和 Y 类型，将忽略 *nFieldWidth* 和 *nPrecision* 。 默认情况下， *nPrecision* 为零 (没有小数位数) B、F 或 N 类型不包含 *nPrecision* 。  
  
 NULL &#124; NOT NULL  
 允许或禁止字段中出现 null 值。  
  
 如果省略 NULL 而不是 NULL，则设置为 NULL 的当前设置将确定是否允许在字段中使用 null 值。 但是，如果省略 NULL 而不是 NULL，并且包含 PRIMARY KEY 或 UNIQUE 子句，则默认情况下，将忽略 SET NULL 的当前设置，并且该字段不为空。  
  
 检查 *lExpression1*  
 指定字段的验证规则。 *lExpression1* 的计算结果必须为逻辑表达式，可以是用户定义函数或存储过程。 每当追加空白记录时，都会检查验证规则。 如果验证规则不允许在追加的记录中使用空白字段值，则会生成错误。  
  
 错误 *cMessageText1*  
 指定在字段验证规则生成错误时所显示的错误消息。  
  
 默认 *eExpression1*  
 指定字段的默认值。 *EExpression1* 的数据类型必须与字段的数据类型相同。  
  
 PRIMARY KEY  
 创建主索引标记。 索引标记与该字段具有相同的名称。  
  
 UNIQUE  
 创建与该字段具有相同名称的候选索引标记。  
  
> [!NOTE]  
>  通过包括 UNIQUE 选项创建的候选索引 (，它是为 ALTER TABLE 或 CREATE TABLE 中的 ANSI 兼容性提供的) 不同于使用索引命令中的 UNIQUE 选项创建的索引。 使用 UNIQUE in INDEX 命令创建的索引允许使用重复的索引键;候选索引不允许有重复的索引键。  
  
 对于用于主索引或候选索引的字段，不允许使用 Null 值和重复记录。  
  
 如果要使用 "添加列" 来创建新字段，如果为支持 null 值的字段创建主索引或候选索引，则 Visual FoxPro 不会生成错误。 但是，如果您尝试将 null 值或重复值输入到用于主索引或候选索引的字段中，则 Visual FoxPro 将生成错误。  
  
 如果要修改现有字段，并且主索引表达式或候选索引表达式包含表中的字段，则 Visual FoxPro 会检查这些字段，以确定它们是否包含 null 值或重复记录。 如果是这样，Visual FoxPro 将生成一个错误，并且表不会更改。  
  
 引用 *TableName2* 标记 *TagName1*  
 指定要建立持久关系的父表。 标记 *TagName1* 指定了关系所基于的父表的索引标记。 索引标记名称最多可包含10个字符。  
  
 NOCPTRANS  
 阻止为 "字符" 和 "备注" 字段转换为其他代码页。 如果将表转换为另一代码页，则不会转换为其指定了 NOCPTRANS 的字段。 只能为 "字符" 和 "备注" 字段指定 NOCPTRANS。  
  
 下面的示例创建一个名为 mytable 的表，该表包含两个字符字段和两个备注字段。 第二个字符字段 "char2" 和第二个 "备注" 字段（memo2）包含用于防止转换的 NOCPTRANS。  
  
```  
CREATE TABLE mytable (char1 C(10), char2 C(10) NOCPTRANS,;  
   memo1 M, memo2 M NOCPTRANS)  
```  
  
 ALTER [COLUMN] *FieldName2*  
 指定要修改的现有字段的名称。  
  
 设置默认 *eExpression2*  
 为现有字段指定新的默认值。 *EExpression2* 的数据类型必须与字段的数据类型相同。  
  
 设置 CHECK *lExpression2*  
 为现有字段指定新的验证规则。 *lExpression2* 的计算结果必须是逻辑表达式，可能是用户定义函数或存储过程。  
  
 错误 *cMessageText2*  
 指定在字段验证规则生成错误时所显示的错误消息。 仅当在浏览或编辑窗口中更改数据时，才会显示该消息。  
  
 DROP DEFAULT  
 删除现有字段的默认值。  
  
 删除检查  
 删除现有字段的验证规则。  
  
 DROP [COLUMN] *FieldName3*  
 指定要从表中删除的字段。 从表中删除字段还会删除字段的默认值设置和字段验证规则。  
  
 如果索引键或触发器表达式引用此字段，则在删除该字段时表达式将变为无效。 在这种情况下，当删除字段时，不会生成错误，但无效的索引键或触发器表达式会在运行时生成错误。  
  
 设置 CHECK *lExpression3*  
 指定表验证规则。 *lExpression3* 的计算结果必须是逻辑表达式，可能是用户定义函数或存储过程。  
  
 错误 *cMessageText3*  
 指定当表验证规则生成错误时所显示的错误消息。 仅当在浏览或编辑窗口中更改数据时，才会显示该消息。  
  
 删除检查  
 删除表的验证规则。  
  
 ADD PRIMARY KEY *eExpression3* TAG *TagName2*  
 向表中添加主索引。 *eExpression3* 指定主索引键表达式， *TagName2* 指定主索引标记的名称。 索引标记名称最多可包含10个字符。 如果省略 TAG *TagName2* 并且 *eExpression3* 为单个字段，则主索引标记的名称与 *eExpression3* 中指定的字段的名称相同。  
  
 删除主密钥  
 删除主索引及其索引标记。 因为一个表只能有一个主键，所以不需要指定主键的名称。 删除主索引时还会根据主键删除任何持久关系。  
  
 ADD UNIQUE *eExpression4*[TAG *TagName3*]  
 将候选索引添加到表中。 *eExpression4* 指定候选索引键表达式， *TagName3* 指定候选索引标记的名称。 索引标记名称最多可包含10个字符。 如果省略标记 *TagName3* ，并且 *eExpression4* 是单个字段，则候选索引标记的名称与 *eExpression4* 中指定的字段的名称相同。  
  
 DROP UNIQUE 标记 *TagName4*  
 删除候选索引及其索引标记。 因为一个表可以有多个候选键，所以必须指定候选索引标记的名称。  
  
 添加外键 [ *eExpression5*] 标记 *TagName4*  
 向表中添加外 (非主要) 索引。 *eExpression5* 指定外部索引键表达式， *TagName4* 指定外部索引标记的名称。 索引标记名称最多可包含10个字符。  
  
 引用 *TableName2*[标记 *TagName5*]  
 指定要建立持久关系的父表。 包含标记 *TagName5* ，以便基于父表的现有索引标记建立关系。 索引标记名称最多可包含10个字符。 如果省略 TAG *TagName5*，则使用父表的主索引标记建立关系。  
  
 删除外键标记 *TagName6*[SAVE]  
 删除其索引标记为 *TagName6* 的外键。 如果省略 SAVE，则会从结构索引中删除索引标记。 包含 SAVE 禁止从结构索引删除索引标记。  
  
 将 *FIELDNAME4* 列重命名为 *FieldName5*  
 允许您更改表中字段的名称。 *FieldName4* 指定要重命名的字段的名称。 *FieldName5* 指定字段的新名称。  
  
> [!CAUTION]  
>  在重命名表字段时应小心，因为索引表达式、字段和表验证规则、命令和函数可以引用原始字段名称。  
  
 NOVALIDATE  
 指定 Visual FoxPro 允许对表的结构进行更改;这些更改可能会违反表中数据的完整性。 默认情况下，Visual FoxPro 会阻止 ALTER TABLE 进行更改，这会违反表中数据的完整性。 包含 NOVALIDATE 以重写此默认行为。  
  
## <a name="remarks"></a>备注  
 ALTER TABLE 可用于修改尚未添加到数据库中的表的结构。 但是，如果在修改可用表时包含 DEFAULT、FOREIGN KEY、PRIMARY KEY、引用或 SET 子句，则 Visual FoxPro 会生成错误。  
  
 ALTER TABLE 可以通过创建新的表头并将记录追加到表头来重建表。 例如，更改字段的类型或宽度可能会导致重新生成表。  
  
 重新生成表后，将对任何更改了类型或宽度的字段执行字段验证规则。 如果更改表中任何字段的类型或宽度，则会执行表规则。  
  
 如果修改包含记录的表的字段或表验证规则，则 Visual FoxPro 会根据现有数据测试新的字段或表验证规则，并在第一次出现的字段或表验证规则或触发器冲突时发出警告。  
  
 如果修改的表在数据库中，ALTER TABLE SQL 需要独占使用数据库。 若要打开数据库以供独占使用，请在打开的数据库中包含 EXCLUSIVE。  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TABLE-SQL 命令](../../odbc/microsoft/create-table-sql-command.md)   
 [INDEX 命令](../../odbc/microsoft/index-command.md)
