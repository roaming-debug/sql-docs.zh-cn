---
description: ALTER TABLE column_constraint (Transact-SQL)
title: column_constraint (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- column_constraint
- column_constraint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER TABLE statement
- constraints [SQL Server], properties
- constraints [SQL Server], definitions
- column_constraint
ms.assetid: 8119b7c7-e93b-4de5-8f71-c3b7c70b993c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 085ac66991ba4e6bbc5778bbcbfd36cb77a7fa4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172493"
---
# <a name="alter-table-column_constraint-transact-sql"></a>ALTER TABLE column_constraint (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  指定 PRIMARY KEY、FOREIGN KEY、UNIQUE 或 CHECK 约束的属性，约束是使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 添加到表中的新列定义的一部分。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
[ CONSTRAINT constraint_name ]   
{   
    [ NULL | NOT NULL ]   
    { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [ WITH FILLFACTOR = fillfactor ]   
        [ WITH ( index_option [, ...n ] ) ]  
        [ ON { partition_scheme_name (partition_column_name)   
            | filegroup | "default" } ]   
    | [ FOREIGN KEY ]   
        REFERENCES [ schema_name . ] referenced_table_name   
            [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 CONSTRAINT  
 指定 PRIMARY KEY、UNIQUE、FOREIGN KEY 或 CHECK 约束的定义的开始。  
  
 constraint_name  
 约束的名称。 除了不能以数字符号 (#) 开头以外，约束名称还必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 如果未提供 constraint_name，则会将系统生成的名称分配给约束。  
  
 NULL | NOT NULL  
 指定列是否可接受空值。 只有在指定了默认值的情况下，才能添加不允许 Null 值的列。 如果新列允许 Null 值，而且未指定默认值，则表中每一行的新列将包含 NULL。 如果新列允许 Null 值并且指定了新列的默认值，那么可以使用 WITH VALUES 选项在表中所有现有行的新列中存储默认值。  
  
 如果新列不允许 Null 值，则添加新列时必须添加 DEFAULT 定义。 自动加载新列时，每个现有行的新列将包含默认值。  
  
 如果添加列时要求对表中的数据行进行物理更改（如向每行添加默认值），则在运行 ALTER TABLE 时将锁定表。 表处于锁定状态时，会影响更改表内容的功能。 相反，添加允许空值和不指定默认值的列的操作只是一项元数据操作，不会用到锁。  
  
 使用 CREATE TABLE 或 ALTER TABLE 时，数据库和会话设置会影响且可能代替列定义中所用的数据类型的可为 Null 性。 建议始终将非计算列显式定义为 NULL 或 NOT NULL；如果使用用户定义数据类型，则建议允许该列使用此数据类型的默认可为 Null 性。 有关详细信息，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。  
  
 PRIMARY KEY  
 通过唯一索引对指定的一列或多列强制实体完整性的约束。 对每个表只能创建一个 PRIMARY KEY 约束。  
  
 UNIQUE  
 通过唯一索引为指定的一列或多列提供实体完整性的约束。  
  
 CLUSTERED | NONCLUSTERED  
 指定为 PRIMARY KEY 或 UNIQUE 约束创建聚集或非聚集索引。 PRIMARY KEY 约束默认为 CLUSTERED。 UNIQUE 约束默认为 NONCLUSTERED。  
  
 如果表中已存在聚集约束或聚集索引，则不能指定 CLUSTERED。 如果表中已存在聚集约束或索引，则 PRIMARY KEY 约束默认为 NONCLUSTERED。  
  
 无法将 ntext、text、varchar(max)、nvarchar(max)、varbinary(max)、xml 或 image 数据类型的列指定为索引的列      。  
  
 WITH FILLFACTOR =fillfactor  
 指定[!INCLUDE[ssDE](../../includes/ssde-md.md)]在存储索引数据时使用的每个索引页的填充程度。 用户指定的填充因子值的范围可以为 1 到 100。 如果未指定值，则默认值为 0。  
  
> [!IMPORTANT]  
>  将 WITH FILLFACTOR = fillfactor 记录为适用于 PRIMARY KEY 或 UNIQUE 约束的唯一索引选项是为了保持向后兼容，但在未来的版本中将不会以此方式进行记录。 可在 ALTER TABLE 的 [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md) 子句中指定其他索引选项。  
  
 ON { partition_scheme_name(partition_column_name) | filegroup | "default" } 适用于：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本  。  
  
 指定为约束创建的索引的存储位置。 如果指定了 partition_scheme_name，则将对该索引进行分区，并将分区映射到由 partition_scheme_name 指定的文件组 。 如果指定了 filegroup，则将在命名文件组内创建索引。 如果指定了 "default" 或者根本没有指定 ON，将在创建表的同一个文件组中创建索引 。 当为 PRIMARY KEY 约束或 UNIQUE 约束添加聚集索引时，如果指定了 ON，则创建聚集索引时将把整个表移动到指定的文件组中。  
  
 在此上下文中，默认值不是关键字。 而是默认文件组的标识符，并且必须进行分隔，如 ON "default" 或 ON [default] 中所示   。 如果指定了“default”，则当前会话的 QUOTED_IDENTIFIER 选项必须为 ON 。 这是默认设置。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。  
  
 FOREIGN KEY REFERENCES  
 为列中数据提供引用完整性的约束。 FOREIGN KEY 约束要求列中的每个值在引用的表中对应的被引用列中都存在。  
  
 *schema_name*  
 FOREIGN KEY 约束引用的表所属的架构的名称。  
  
 referenced_table_name  
 FOREIGN KEY 约束引用的表。  
  
 ref_column  
 新 FOREIGN KEY 约束引用的带括号的列。  
  
 ON DELETE { NO ACTION \| CASCADE \| SET NULL \| SET DEFAULT }  
 指定如果已更改的表中的行具有引用关系，并且被引用行已从父表中删除，则对这些行所采取的操作。 默认值为 NO ACTION。  
  
 NO ACTION  
 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]将引发错误，并回滚对父表中行的删除操作。  
  
 CASCADE  
 如果从父表中删除一行，则将从引用表中删除相应行。  
  
 SET NULL  
 如果删除父表中与外键相对应的行，组成外键的所有值都将设置为 NULL。 若要执行此约束，外键列必须可为空值。  
  
 SET DEFAULT  
 如果删除父表中与外键相对应的行，组成外键的所有值都将设置为其默认值。 若要执行此约束，所有外键列都必须有默认定义。 如果某个列可为空值，并且未设置显式的默认值，则将使用 NULL 作为该列的隐式默认值。  
  
 如果该表将包含在使用逻辑记录的合并发布中，则不要指定 CASCADE。 有关逻辑记录的详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。  
  
 如果要更改的表已存在 INSTEAD OF 触发器 ON DELETE，则不能定义 ON DELETE CASCADE 操作。  
  
 例如，在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中，ProductVendor 表与 Vendor 表有引用关系 。 ProductVendor.VendorID 外键引用 Vendor.VendorID 主键 。  
  
 如果对 Vendor 表的某行执行 DELETE 语句，并且为 ProductVendor.VendorID 指定 ON DELETE CASCADE 操作，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]将检查 ProductVendor 表中的一个或多个依赖行  。 如果存在依赖行，则除了删除 Vendor 表中被引用的行外，还将删除 ProductVendor 表中的依赖行 。  
  
 相反，如果指定了 NO ACTION，并且 ProductVendor 表中至少有一行引用 Vendor 行，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将引发错误并回滚对 Vendor 行执行的删除操作 。  
  
 ON UPDATE { NO ACTION \| CASCADE \| SET NULL \| SET DEFAULT }  
 指定在发生更改的表中，如果行有引用关系且引用的行在父表中被更新，则对这些行采取什么操作。 默认值为 NO ACTION。  
  
 NO ACTION  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)]将引发错误，并回滚对父表中相应行的更新操作。  
  
 CASCADE  
 如果在父表中更新了一行，则将在引用表中更新相应的行。  
  
 SET NULL  
 如果更新了父表中的相应行，则会将构成外键的所有值设置为 NULL。 若要执行此约束，外键列必须可为空值。  
  
 SET DEFAULT  
 如果更新了父表中的相应行，则会将构成外键的所有值都设置为其默认值。 若要执行此约束，所有外键列都必须有默认定义。 如果某个列可为空值，并且未设置显式的默认值，则将使用 NULL 作为该列的隐式默认值。  
  
 如果该表将包含在使用逻辑记录的合并发布中，则不要指定 CASCADE。 有关逻辑记录的详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。  
  
 如果要更改的表已存在 INSTEAD OF 触发器 ON UPDATE，则不能定义 ON UPDATE CASCADE、SET NULL 或 SET DEFAULT 操作。  
  
 例如，在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中，ProductVendor 表与 Vendor 表有引用关系 。 ProductVendor.VendorID 外键引用 Vendor.VendorID 主键 。  
  
 如果对 Vendor 表中的某行执行了 UPDATE 语句，并且为 ProductVendor.VendorID 指定了 ON UPDATE CASCADE 操作，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将检查 ProductVendor 表中是否有一个或多个依赖行  。 如果存在依赖行，则 ProductVendor 表中的依赖行将随 Vendor 表中的被引用行一同更新 。  
  
 相反，如果指定了 NO ACTION，则当 ProductVendor 表中至少有一行引用了 Vendor 行时，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 将引发错误，并回滚对 Vendor 行的更新操作 。  
  
 NOT FOR REPLICATION  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 可以为 FOREIGN KEY 约束和 CHECK 约束指定该参数。 如果为约束指定了此子句，则当复制代理执行插入、更新或删除操作时，将不会强制执行此约束。  
  
 CHECK  
 一个约束，该约束通过限制可输入一列或多列中的可能值来强制实现域完整性。  
  
 logical_expression  
 用于 CHECK 约束的逻辑表达式，返回 TRUE 或 FALSE。 与 CHECK 约束一起使用的 logical_expression 无法引用其他表，但可以引用同一表中同一行的其他列。 该表达式不能引用别名数据类型。  
  
## <a name="remarks"></a>备注  
 当添加 FOREIGN KEY 或 CHECK 约束时，所有现有数据都要进行约束违反验证，除非指定了 WITH NOCHECK 选项。 如果违反了约束，ALTER TABLE 将失败并返回一个错误。 当在现有列上添加新 PRIMARY KEY 或 UNIQUE 约束时，该列中的数据必须唯一。 如果存在重复值，ALTER TABLE 语句将失败。 当添加 PRIMARY KEY 或 UNIQUE 约束时，WITH NOCHECK 选项不起作用。  
  
 每个 PRIMARY KEY 和 UNIQUE 约束都将生成一个索引。 UNIQUE 和 PRIMARY KEY 约束的数目不能导致表上非聚集索引的数目大于 999，也不能导致聚集索引的数目大于 1。 外键约束不会自动生成索引。 然而，经常在查询的联接条件中使用外键列，方法是将一个表的外键约束中的列与另一个表中的主键列或唯一键列匹配。 针对外键列的索引使[!INCLUDE[ssDE](../../includes/ssde-md.md)]可以在外键表中快速查找相关数据。  
  
## <a name="examples"></a>示例  
 有关示例，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_definition (Transact-SQL)](../../t-sql/statements/alter-table-column-definition-transact-sql.md)  
  
  
