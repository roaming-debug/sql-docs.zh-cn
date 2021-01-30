---
description: CREATE TABLE - SQL 命令
title: CREATE TABLE-SQL 命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CREATE TABLE [ODBC]
ms.assetid: be2143ba-fc16-42c9-84f7-8985cd924860
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0fb1e91c03f7a1a1fee98ce887be991370450999
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183207"
---
# <a name="create-table---sql-command"></a>CREATE TABLE - SQL 命令
创建具有指定字段的表。  
  
 Visual FoxPro ODBC 驱动程序支持此命令的本机 Visual FoxPro 语言语法。 有关特定于驱动程序的信息，请参阅 **驱动程序备注**。  
  
## <a name="syntax"></a>语法  
  
```  
  
CREATE TABLE | DBF TableName1 [NAME LongTableName] [FREE]  
   (FieldName1FieldType [(nFieldWidth [, nPrecision])]  
      [NULL | NOT NULL]   
      [CHECK lExpression1 [ERROR cMessageText1]]  
      [DEFAULT eExpression1]  
      [PRIMARY KEY | UNIQUE]  
      [REFERENCES TableName2 [TAG TagName1]]  
      [NOCPTRANS]  
   [, FieldName2 ...]  
      [, PRIMARY KEY eExpression2 TAG TagName2  
      |, UNIQUE eExpression3 TAG TagName3]  
      [, FOREIGN KEY eExpression4 TAG TagName4 [NODUP]  
            REFERENCES TableName3 [TAG TagName5]]  
      [, CHECK lExpression2 [ERROR cMessageText2]])  
| FROM ARRAY ArrayName  
```  
  
## <a name="arguments"></a>参数  
 CREATE TABLE &#124; DBF *TableName1*  
 指定要创建的表的名称。 表和 DBF 选项完全相同。  
  
 名称 *LongTableName*  
 指定表的长名称。 仅当数据库处于打开状态时，才可以指定长的表名称，因为数据库中存储了长的表名。  
  
 长名称最多可以包含128个字符，并且可以用来代替数据库中的短文件名。  
  
 FREE  
 指定不将表添加到打开的数据库。 如果数据库未打开，则不需要使用 "免费"。  
  
 *(FieldName1 FieldType* [ ( *NFieldWidth* [， *nPrecision*] ) ]  
 分别指定字段名称、字段类型、字段宽度和字段精度 (小数点位数) 。  
  
 *FieldType* 是指示字段的 [数据类型](../../odbc/microsoft/visual-foxpro-field-data-types.md)的单字母。 某些字段数据类型要求指定 *nFieldWidth* 或 *nPrecision* ，或同时指定两者。  
  
 对于 D、G、I、L、M、P、T 和 Y 类型，将忽略 *nFieldWidth* 和 *nPrecision* 。 *nPrecision* 默认为零 (没有任何小数位) 如果 B、F 或 N 类型不包含 *nPrecision* 。  
  
 Null  
 允许在字段中为 null 值。  
  
 NOT NULL  
 禁止字段中出现 null 值。  
  
 如果省略 NULL 而不是 NULL，则设置为 NULL 的当前设置将确定是否允许在字段中使用 null 值。 但是，如果省略 NULL 而不是 NULL，并且包括 PRIMARY KEY 或 UNIQUE 子句，则将忽略 SET NULL 的当前设置，并且该字段默认为 NOT NULL。  
  
 检查 *lExpression1*  
 指定字段的验证规则。 *lExpression1* 可以是用户定义的函数。 每当追加空白记录时，都会检查验证规则。 如果验证规则不允许在追加的记录中使用空白字段值，则会生成错误。  
  
 错误 *cMessageText1*  
 指定当字段规则生成错误时视觉 FoxPro 显示的错误消息。 仅当在 "浏览" 窗口或 "编辑" 窗口中更改数据时，才显示该消息。  
  
 默认 *eExpression1*  
 指定字段的默认值。 *EExpression1* 的数据类型必须与字段的数据类型相同。  
  
 PRIMARY KEY  
 为字段创建主索引。 主索引标记的名称与字段相同。  
  
 UNIQUE  
 创建字段的候选索引。 候选索引标记与该字段具有相同的名称。  
  
> [!NOTE]  
>  通过在 CREATE TABLE 或 ALTER TABLE-SQL) 中包含 UNIQUE 选项创建的候选索引 (与索引命令中使用 UNIQUE 选项创建的索引不同。 用 INDEX 命令中的 UNIQUE 选项创建的索引允许使用重复的索引键;候选索引不允许有重复的索引键。 有关其唯一选项的其他信息，请参阅 [索引](../../odbc/microsoft/index-command.md) 。  
  
 对于用于主索引或候选索引的字段，不允许使用 Null 值和重复记录。 但是，如果为支持 null 值的字段创建主索引或候选索引，则 Visual FoxPro 不会生成错误。 如果尝试将 null 值或重复值输入到用于主索引或候选索引的字段中，Visual FoxPro 将生成错误。  
  
 引用 *TableName2*[标记 *TagName1*]  
 指定要建立持久关系的父表。 如果省略 TAG *TagName1*，则使用父表的主索引键建立关系。 如果父表没有主索引，Visual FoxPro 将生成错误。  
  
 包含标记 *TagName1* ，以便基于父表的现有索引标记建立关系。 索引标记名称最多可包含10个字符。  
  
 父表不能是可用的表。  
  
 NOCPTRANS  
 阻止为 "字符" 和 "备注" 字段转换为其他代码页。 如果将表转换为另一代码页，则不会转换为其指定了 NOCPTRANS 的字段。 只能为 "字符" 和 "备注" 字段指定 NOCPTRANS。  
  
 下面的示例创建一个名为 mytable 的表，其中包含两个字符字段和两个备注字段。 第二个字符字段 "char2" 和第二个 "备注" 字段（memo2）包含用于防止转换的 NOCPTRANS。  
  
```  
CREATE TABLE mytable (char1 C(10), char2 C(10) NOCPTRANS,;  
   memo1 M, memo2 M NOCPTRANS)  
```  
  
 PRIMARY KEY *eExpression2* 标记 *TagName2*  
 指定要创建的主索引。 *eExpression2* 指定表中的任何字段或字段的组合。 标记 *TagName2* 指定创建的主索引标记的名称。 索引标记名称最多可包含10个字符。  
  
 因为一个表只能有一个主索引，所以如果您已经为字段创建了主索引，则不能包含此子句。 如果在 CREATE TABLE 中包含多个 PRIMARY KEY 子句，则 Visual FoxPro 会生成错误。  
  
 UNIQUE *eExpression3* 标记 *TagName3*  
 创建候选索引。 *eExpression3* 指定表中的任何字段或字段的组合。 但是，如果已使用其中一个主键选项创建了主索引，则不能包含为主索引指定的字段。 标记 *TagName3* 指定创建的候选索引标记的标记名称。 索引标记名称最多可包含10个字符。  
  
 一个表可以有多个候选索引。  
  
 外键 *eExpression4* 标记 *TagName4*[NODUP]  
 创建外 (非键) 索引，并建立与父表之间的关系。 *eExpression4* 指定外部索引键表达式， *TagName4* 指定创建的外部索引键标记的名称。 索引标记名称最多可包含10个字符。 包含 NODUP 以创建候选外部索引。  
  
 您可以为表创建多个外部索引，但外部索引表达式必须指定表中的不同字段。  
  
 引用 *TableName3*[标记 *TagName5*]  
 指定要建立持久关系的父表。 包含标记 *TagName5* ，以便基于父表的索引标记建立关系。 索引标记名称最多可包含10个字符。 默认情况下，如果省略 TAG *TagName5，* 则使用父表的主索引键建立关系。  
  
 检查 *eExpression2*[ERROR *cMessageText2*]  
 指定表验证规则。 错误 *cMessageText2* 指定执行表验证规则时视觉 FoxPro 显示的错误消息。 仅当在 "浏览" 窗口或 "编辑" 窗口中更改数据时，才显示该消息。  
  
 FROM 数组 *ArrayName*  
 指定现有数组的名称，其内容为表中每个字段的名称、类型、精度和小数位数。 可以通过 **AFIELDS** ( ) 函数定义数组的内容。  
  
## <a name="remarks"></a>备注  
 新表在最小可用工作区域中打开，可以通过其别名进行访问。 新表以独占方式打开，而不考虑设置的当前设置。  
  
 如果数据库处于打开状态，并且不包含 FREE 子句，则新表将添加到数据库中。 不能创建与数据库中的表同名的新表。  
  
 如果数据库处于打开状态，CREATE TABLE SQL 需要独占使用数据库。 若要打开数据库以供独占使用，请在打开的数据库中包含 EXCLUSIVE。  
  
 如果在创建新表时数据库未打开，包括名称、CHECK、DEFAULT、FOREIGN KEY、PRIMARY KEY 或引用子句会生成错误。  
  
> [!NOTE]  
>  CREATE TABLE 语法使用逗号分隔某些 CREATE TABLE 选项。 此外，NULL、NOT NULL、CHECK、DEFAULT、PRIMARY KEY 和 UNIQUE 子句必须置于包含列定义的括号内。  
  
## <a name="driver-remarks"></a>驱动程序备注  
 当应用程序将 ODBC SQL 语句 CREATE TABLE 发送到数据源时，Visual FoxPro ODBC 驱动程序将使用下表中所示的语法将命令转换为 Visual FoxProCREATE TABLE 命令。  
  
|ODBC 语法|Visual FoxPro 语法|  
|-----------------|--------------------------|  
|CREATE TABLE *基准表名称*<br /><br />  (*列标识符数据类型*<br /><br /> [NOT NULL]<br /><br /> [，*列标识符数据类型*<br /><br /> [NOT NULL] ... ) |CREATE TABLE *TableName1* [NAME *LongTableName*]<br /><br />  (*FieldName1* *FieldType*<br /><br /> [ (*nFieldWidth* [， *nPrecision*] ) ]<br /><br /> [NOT NULL] ) |  
  
 使用驱动程序创建表时，驱动程序会在创建后立即关闭该表，以允许其他用户访问表。 这不同于 Visual FoxPro，后者使表在创建时独占打开。 但是，如果包含 CREATE TABLE 语句的数据源上的存储过程执行，则该表将保持打开状态。  
  
 如果数据源是数据库 ( dbc 文件) ，则 Visual FoxPro ODBC 驱动程序会创建一个名为 *LongTableName* 的表，其名称与 *基表名称* 相同。  
  
### <a name="using-data-definition-language-ddl"></a>使用数据定义语言 (DDL)   
 不能在以下位置包含 DDL：  
  
-   在需要事务的批处理 SQL 语句中  
  
-   在手动提交模式下，在需要事务的语句之后，除非应用程序首先调用 **SQLTransact**。  
  
 例如，如果要创建一个临时表，则应在开始需要事务的语句之前创建该表。 如果在需要事务的批处理 SQL 语句中包含 CREATE TABLE 语句，则驱动程序将返回一条错误消息。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE-SQL 命令](../../odbc/microsoft/alter-table-sql-command.md)   
 [ (Visual FoxPro ODBC 驱动程序支持的数据类型) ](../../odbc/microsoft/supported-data-types-visual-foxpro-odbc-driver.md)   
 [INSERT-SQL 命令](../../odbc/microsoft/insert-sql-command.md)   
 [SELECT - SQL 命令](../../odbc/microsoft/select-sql-command.md)
