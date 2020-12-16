---
description: ALTER TABLE (Transact-SQL)
title: ALTER TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WAIT_AT_LOW_PRIORITY
- ABORT_AFTER_WAIT
- ABORT_AFTER_WAIT_TSQL
- ALTER_TABLE_TSQL
- ALTER TABLE
- WAIT_AT_LOW_PRIORITY_TSQL
- ALTER_COLUMN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- columns [SQL Server], resizing
- changing column size
- MAXDOP index option, ALTER TABLE statement
- table modifications [SQL Server], ALTER TABLE
- ALTER TABLE statement
- modifying tables
- partitioned tables [SQL Server], lock escalation
- resizing columns
- removing columns
- switching partitions
- reassigning partitions
- removing constraints
- triggers [SQL Server], disabling
- columns [SQL Server], adding
- LOCK_ESCALATION option of ALTER TABLE
- constraints [SQL Server], deleting
- constraints [SQL Server], disabling
- triggers [SQL Server], enabling
- re-enabling constraints
- index modifications [SQL Server]
- disabling constraints
- columns [SQL Server], removing
- max degree of parallelism option
- locking [SQL Server], tables
- ONLINE option
- disabling triggers
- constraints [SQL Server], adding
- deleting constraints
- adding constraints
- adding columns
- SWITCH partitions
- partitioned tables [SQL Server], switching
- lock escalation [SQL Server], option of ALTER TABLE
- constraints [SQL Server], enabling
- dropping constraints
- dropping columns
- data retention policy
- table changes [SQL Server]
ms.assetid: f1745145-182d-4301-a334-18f799d361d1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: aff544048da8bb09a2fd488d26a7bd1f69cf8f56
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489582"
---
# <a name="alter-table-transact-sql"></a>ALTER TABLE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

通过更改、添加或删除列和约束，修改表定义。 ALTER TABLE 还重新分配和重新生成分区，或禁用和启用约束和触发器。

有关语法约定的详细信息，请参阅 [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

> [!IMPORTANT]
> 对于基于磁盘的表和内存优化表，ALTER TABLE 的语法不同。 使用以下链接可以直接跳至适用于表类型的语法块和相应的语法示例：
>
> - 基于磁盘的表：
>
>   - [语法](#syntax-for-disk-based-tables)
>   - [示例](#Example_Top)
> - 内存优化表
>
>   - [语法](#syntax-for-memory-optimized-tables)
>   - [示例](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

## <a name="syntax-for-disk-based-tables"></a>基于磁盘的表的语法

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                 | max
                 | xml_schema_collection
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ] [ SPARSE ]
      | { ADD | DROP }
          { ROWGUIDCOL | PERSISTED | NOT FOR REPLICATION | SPARSE | HIDDEN }
      | { ADD | DROP } MASKED [ WITH ( FUNCTION = ' mask_function ') ]
    }
    [ WITH ( ONLINE = ON | OFF ) ]
    | [ WITH { CHECK | NOCHECK } ]
  
    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <column_set_definition>
    } [ ,...n ]
      | [ system_start_time_column_name datetime2 GENERATED ALWAYS AS ROW START
                [ HIDDEN ] [ NOT NULL ] [ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
                system_end_time_column_name datetime2 GENERATED ALWAYS AS ROW END
                   [ HIDDEN ] [ NOT NULL ][ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
        ]
       PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )
    | DROP
     [ {
         [ CONSTRAINT ][ IF EXISTS ]
         {
              constraint_name
              [ WITH
               ( <drop_clustered_constraint_option> [ ,...n ] )
              ]
          } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }
  
    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | { ENABLE | DISABLE } CHANGE_TRACKING
        [ WITH ( TRACK_COLUMNS_UPDATED = { ON | OFF } ) ]
  
    | SWITCH [ PARTITION source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]

    | SET
        (
            [ FILESTREAM_ON =
                { partition_scheme_name | filegroup | "default" | "NULL" } ]
            | SYSTEM_VERSIONING =
                  {
                      OFF
                  | ON
                      [ ( HISTORY_TABLE = schema_name . history_table_name
                          [, DATA_CONSISTENCY_CHECK = { ON | OFF } ]
                          [, HISTORY_RETENTION_PERIOD =
                          {
                              INFINITE | number {DAY | DAYS | WEEK | WEEKS
                  | MONTH | MONTHS | YEAR | YEARS }
                          }
                          ]
                        )
                      ]
                  }
            | DATA_DELETION =  
                {
                      OFF 
                    | ON  
                        [(  [ FILTER_COLUMN = column_name ]   
                            [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                                    | MONTH | MONTHS | YEAR | YEARS }}]   
                        )]
                   }
    | REBUILD
      [ [PARTITION = ALL]
        [ WITH ( <rebuild_option> [ ,...n ] ) ]
      | [ PARTITION = partition_number
           [ WITH ( <single_partition_rebuild_option> [ ,...n ] ) ]
        ]
      ]
  
    | <table_option>
    | <filetable_option>
    | <stretch_configuration>
}
[ ; ]
  
-- ALTER TABLE options
  
<column_set_definition> ::=
    column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS

<drop_clustered_constraint_option> ::=
    {
        MAXDOP = max_degree_of_parallelism
      | ONLINE = { ON | OFF }
      | MOVE TO
         { partition_scheme_name ( column_name ) | filegroup | "default" }
    }
<table_option> ::=
    {
        SET ( LOCK_ESCALATION = { AUTO | TABLE | DISABLE } )
    }
  
<filetable_option> ::=
    {
       [ { ENABLE | DISABLE } FILETABLE_NAMESPACE ]
       [ SET ( FILETABLE_DIRECTORY = directory_name ) ]
    }
  
<stretch_configuration> ::=
    {
      SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options>)
          | = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED )
          | ( <table_stretch_options> [, ...n] )
        }
            )
    }
  
<table_stretch_options> ::=
    {
     [ FILTER_PREDICATE = { null | table_predicate_function } , ]
       MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
    }
  
<single_partition_rebuild__option> ::=
{
      SORT_IN_TEMPDB = { ON | OFF }
    | MAXDOP = max_degree_of_parallelism
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE} }
    | ONLINE = { ON [( <low_priority_lock_wait> ) ] | OFF }
}
  
<low_priority_lock_wait>::=
{
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ],
        ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )
}
```

> [!NOTE]
> 有关其他信息，请参阅：
>
> - [ALTER TABLE column_constraint](alter-table-column-constraint-transact-sql.md)
> - [ALTER TABLE column_definition](alter-table-column-definition-transact-sql.md)
> - [ALTER TABLE computed_column_definition](alter-table-computed-column-definition-transact-sql.md)
> - [ALTER TABLE index_option](alter-table-index-option-transact-sql.md)
> - [ALTER TABLE table_constraints](alter-table-table-constraint-transact-sql.md)

## <a name="syntax-for-memory-optimized-tables"></a>内存优化表的语法

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ]
    }

    | ALTER INDEX index_name
    {
        [ type_schema_name. ] type_name
        REBUILD
        [ [ NONCLUSTERED ] WITH ( BUCKET_COUNT = bucket_count )
        ]
    }

    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <table_index>
      | <column_index>
    } [ ,...n ]
  
    | DROP
     [ {
         CONSTRAINT [ IF EXISTS ]
         {
              constraint_name
          } [ ,...n ]
        | INDEX [ IF EXISTS ]
      {
         index_name
       } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }

    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | SWITCH [ [ PARTITION ] source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]
    
}
[ ; ]

-- ALTER TABLE options

< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   {PRIMARY KEY | UNIQUE }
     {
       NONCLUSTERED (column [ ASC | DESC ] [ ,... n ])
       | NONCLUSTERED HASH (column [ ,... n ] ) WITH ( BUCKET_COUNT = bucket_count )
                    }
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
    | CHECK ( logical_expression )
}

<column_index> ::=
  INDEX index_name
{ [ NONCLUSTERED ] | [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count)}

<table_index> ::=
  INDEX index_name
{[ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
}

```

## <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Azure Synapse Analytics 和并行数据仓库的语法

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

ALTER TABLE { database_name.schema_name.source_table_name | schema_name.source_table_name | source_table_name }
{
    ALTER COLUMN column_name
        {
            type_name [ ( precision [ , scale ] ) ]
            [ COLLATE Windows_collation_name ]
            [ NULL | NOT NULL ]
        }
    | ADD { <column_definition> | <column_constraint> FOR column_name} [ ,...n ]
    | DROP { COLUMN column_name | [CONSTRAINT] constraint_name } [ ,...n ]
    | REBUILD {
            [ PARTITION = ALL [ WITH ( <rebuild_option> ) ] ]
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_option> ] ]
      }
    | { SPLIT | MERGE } RANGE (boundary_value)
    | SWITCH [ PARTITION source_partition_number
        TO target_table_name [ PARTITION target_partition_number ] [ WITH ( TRUNCATE_TARGET = ON | OFF )
}
[;]

<column_definition>::=
{
    column_name
    type_name [ ( precision [ , scale ] ) ]
    [ <column_constraint> ]
    [ COLLATE Windows_collation_name ]
    [ NULL | NOT NULL ]
}

<column_constraint>::=
    [ CONSTRAINT constraint_name ] 
    {
        DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }
<rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]
}

<single_partition_rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

*database_name*  
要在其中创建表的数据库的名称。

*schema_name*  
表所属的架构的名称。

*table_name*  
要更改的表的名称。 如果表不在当前数据库中，或者不包含在当前用户所拥有的架构中，必须显式指定数据库和架构。

ALTER COLUMN  
指定要更改的命名列。

修改后的列不得为：

- 数据类型为 timestamp 的列。
- 表的 ROWGUIDCOL 列。
- 计算列或用于计算列的列。
- 用于 CREATE STATISTICS 语句生成的统计信息的列。 用户需要运行 DROP STATISTICS 才能删除统计信息，然后 ALTER COLUMN 才能成功。  运行此查询以获取用户创建的所有统计信息和表的统计信息列。

``` sql

SELECT s.name AS statistics_name  
      ,c.name AS column_name  
      ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('<table_name>'); 

```
   > [!NOTE]
   > 由查询优化器自动生成的统计信息将被 ALTER COLUMN 自动删除。

- 用于 PRIMARY KEY 或 [FOREIGN KEY] REFERENCES 约束中的列。
- 用于 CHECK 或 UNIQUE 约束中的列。 不过，可以更改用于 CHECK 或 UNIQUE 约束的长度可变列的长度。
- 与默认定义关联的列。 不过，如果数据类型不改变，可以更改列的长度、精度或确定位数。

只能通过下列方式更改数据类型为 text、ntext 和 image 的列：

- text 更改为 varchar(max)、nvarchar(max) 或 xml
- ntext 更改为 varchar(max)、nvarchar(max) 或 xml
- image 更改为 varbinary(max)

更改某些数据类型可能导致更改相关数据。 例如，将 nchar 或 nvarchar 列更改为 char 或 varchar 可能会导致转换扩展字符。 有关详细信息，请参阅 [CAST 和 CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md)。 减少列的精度或确定位数可能会导致数据截断。

> [!NOTE]
> 无法更改已分区表的列的数据类型。
>
> 除非列的数据类型为 varchar、nvarchar 或 varbinary，且新大小等于或大于旧大小，否则无法更改索引中所含列的数据类型。
>
> 无法将主键约束中的列从非 NULL 更改为 NULL。

使用 Always Encrypted（不含安全 enclave）时，如果要修改的列使用“ENCRYPTED WITH”进行加密，可以将数据类型更改为兼容的数据类型（如从 INT 更改为 BIGINT），但不得更改任何加密设置。

使用 Always Encrypted（含安全 enclave）时，如果用于保护列的列加密密钥（以及新的列加密密钥，若要更改密钥的话）支持 enclave 计算（使用已启用 enclave 的列主密钥进行加密），就可以更改任何加密设置。 有关详细信息，请参阅[具有安全 Enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md)。

column_name  
要更改、添加或删除的列的名称。 column_name 最多为 128 个字符。 对于使用 timestamp 数据类型创建的新列，可以省略 column_name。 如果没有为 timestamp 数据类型的列指定 column_name，便会使用名称 timestamp。

> [!NOTE]
> 在更改表中的所有现有列之后，将添加新列。

[ _type\_schema\_name_ **.** ] _type\_name_  
更改后的列的新数据类型，或添加的列的数据类型。 无法为已分区表的现有列指定 type_name。 type_name 可以是下列任一数据类型：

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型。
- 基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型的别名数据类型。 必须先用 CREATE TYPE 语句创建别名数据类型，然后才能将它们用于表定义。
- [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 用户定义类型及其所属架构。 必须先用 CREATE TYPE 语句创建用户定义类型，然后才能将它们用于表定义。

更改后的列的 type_name 应符合下列条件：

- 以前的数据类型必须可以隐式转换为新数据类型。
- type_name 不得为 timestamp。
- 对于 ALTER COLUMN，ANSI_NULL 默认值始终为 ON；如果没有指定，列可为空。
- 对于 ALTER COLUMN，ANSI_PADDING 填充始终为 ON。
- 如果修改后的列是标识列，则 new_data_type 必须是支持标识属性的数据类型。
- 当前的 SET ARITHABORT 设置将被忽略。 ALTER TABLE 的操作方式与 ARITHABORT 设置为 ON 时相同。

> [!NOTE]
> 如果未指定 COLLATE 子句，那么更改列的数据类型会导致排序规则更改为数据库的默认排序规则。

*精度*  
指定的数据类型的精度。 有关有效精度值的详细信息，请参阅[精度、小数位数和长度](../../t-sql/data-types/precision-scale-and-length-transact-sql.md)。

*scale*  
指定的数据类型的确定位数。 有关有效小数位数值的详细信息，请参阅[精度、小数位数和长度](../../t-sql/data-types/precision-scale-and-length-transact-sql.md)。

**max**  
仅应用于 varchar、nvarchar 和 varbinary 数据类型，以便存储 2^31-1 个字节的字符、二进制数据和 Unicode 数据  。

xml_schema_collection  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

仅应用于 xml 数据类型，用于将 XML 架构与类型相关联。 在架构集合中键入 xml 列前，先使用 [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) 在数据库中创建架构集合。

COLLATE \< *collation_name* >  
指定更改后的列的新排序规则。 如果未指定，则为该列分配数据库的默认排序规则。 排序规则名称既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称。 有关列表和详细信息，请参阅 [Windows 排序规则名称](../../t-sql/statements/windows-collation-name-transact-sql.md)和 [SQL Server 排序规则名称](../../t-sql/statements/sql-server-collation-name-transact-sql.md)。

COLLATE 子句仅更改数据类型为 char、varchar、nchar 和 nvarchar 的列的排序规则。 若要更改用户定义别名数据类型列的排序规则，请单独使用 ALTER TABLE 语句将列更改为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型。 然后，更改它的排序规则，并将列更改回别名数据类型。

如果存在以下一个或多个条件，ALTER COLUMN 无法更改排序规则：

- CHECK 约束、FOREIGN KEY 约束或计算列引用了更改后的列。
- 已为列创建了索引、统计信息或全文检索。 如果更改了列的排序规则，则将删除为更改后的列自动创建的统计信息。
- 架构绑定视图或函数引用了列。

有关详细信息，请参阅 [COLLATE](~/t-sql/statements/collations.md)。

NULL | NOT NULL  
指定列是否可接受空值。 只有在默认定义已指定或表为空时，才能用 ALTER TABLE 语句添加不允许 NULL 值的列。 只有在也指定了 PERSISTED 后，才能为计算列指定 NOT NULL。 如果新列允许 NULL 值，但没有指定默认定义，那么新列对表中的每一行都包含 NULL 值。 如果新列允许 NULL 值，并且指定了新列的默认定义，可使用 WITH VALUES 将默认值存储到表中每个现有行对应的新列中。

如果新列不允许 NULL 值，且表不为空，必须添加新列的 DEFAULT 定义。 另外，每个现有行对应的新列自动加载有默认值。

可以在 ALTER COLUMN 中指定 NULL，强制 NOT NULL 列允许 NULL 值，PRIMARY KEY 约束中的列除外。 只有在列中不包含 NULL 值时，才能在 ALTER COLUMN 中指定 NOT NULL。 必须将 Null 值更新为某个值后，才允许执行 ALTER COLUMN NOT NULL 语句，例如：

```sql
UPDATE MyTable SET NullCol = N'some_value' WHERE NullCol IS NULL;
ALTER TABLE MyTable ALTER COLUMN NullCOl NVARCHAR(20) NOT NULL;
```

如果你用 CREATE TABLE 或 ALTER TABLE 语句创建或更改表，数据库和会话设置会影响（且可能会替代）用于列定义的数据类型的为 Null 性。 请务必始终将非计算列显式定义为 NULL 或 NOT NULL。

如果添加使用用户定义数据类型的列，请务必将列定义为与用户定义数据类型的为 Null 性相同。 此外，指定列的默认值。 有关详细信息，请参阅 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)。

> [!NOTE]
> 如果 ALTER COLUMN 被指定为 NULL 或 NOT NULL，则必须同时指定 new_data_type [(precision [, scale ])]  。 如果未更改数据类型、精度和小数位数，则指定当前的列值。

[ {ADD | DROP} ROWGUIDCOL ]  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定是在指定列中添加还是删除 ROWGUIDCOL 属性。 ROWGUIDCOL 指示列为行 GUID 列。 对于每个表，只能一个 uniqueidentifier 列设置为 ROWGUIDCOL 列。 此外，只能将 ROWGUIDCOL 属性分配给 uniqueidentifier 列。 无法将 ROWGUIDCOL 分配给使用用户定义数据类型的列。

ROWGUIDCOL 不强制要求列中存储的值的唯一性，也不为插入表中的新行自动生成值。 若要为每列生成唯一值，请在 INSERT 语句中使用 NEWID 或 NEWSEQUENTIALID 函数。 或者，将 NEWID 或 NEWSEQUENTIALID 函数指定为列的默认值。

[ {ADD | DROP} PERSISTED ]  
指定在指定列中添加或删除 PERSISTED 属性。 列必须是由确定性表达式定义的计算列。 对于指定为 PERSISTED 的列，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将以物理方式在表中存储计算值；并且，当更新了计算列依赖的任何其他列时，这些值也将被更新。 通过将计算列标记为 PERSISTED，可以对确定（但不精确）的表达式中定义的计算列创建索引。 有关详细信息，请参阅 [计算列上的索引](../../relational-databases/indexes/indexes-on-computed-columns.md)。

任何用作已分区表的分区依据列的计算列都必须显式标记为 PERSISTED。

DROP NOT FOR REPLICATION  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定当复制代理执行插入操作时，标识列中的值递增。 只有当 column_name 是标识列时，才能指定此子句。

SPARSE  
指示列为稀疏列。 稀疏列已针对 NULL 值进行了存储优化。 无法将稀疏列设置为 NOT NULL。 无论是将列从稀疏列转换为非稀疏列，还是从非稀疏列转换为稀疏列，都会导致表在命令执行期间被锁定。 您可能需要使用 REBUILD 子句来回收任何节约的空间。 有关稀疏列的其他限制和详细信息，请参阅[使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)。

ADD MASKED WITH ( FUNCTION = ' mask_function ')  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定动态数据掩码。 mask_function 是具有相应参数的掩码函数的名称。 有三个函数可供选择：

- default()
- email()
- partial()
- random()

要删除掩码，请使用 `DROP MASKED`。 有关函数参数的信息，请参阅[动态数据掩码](../../relational-databases/security/dynamic-data-masking.md)。

WITH ( ONLINE = ON | OFF) \<as applies to altering a column>  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

允许执行多个更改列操作，同时保持表可用。 默认为 OFF。 可以联机对列执行与数据类型、列长度或精度、为 Null 性、稀疏性和排序规则相关的列更改。

通过联机更改列，用户创建的统计信息和自动统计信息可以在 ALTER COLUMN 操作执行期间引用更改后的列，这会让查询照常运行。 在操作结束时，引用列的自动统计信息遭删除，且用户创建的统计信息失效。 完成操作后，用户必须手动更新用户生成的统计信息。 如果列属于任何统计信息或索引的筛选表达式，无法执行更改列操作。

- 当联机更改列操作正在执行时，所有依赖列（索引、视图等）的操作都会遭阻止或失败，并生成相应错误。 此行为可保证联机更改列操作不会由于在执行操作时引入的依赖项而失败。
- 如果更改后的列被非聚集索引引用，不支持将列从 NOT NULL 更改为 NULL 视为联机操作。
- 如果检查约束引用了列，且更改操作在限制列（数值或日期时间）的精度，那么联机更改不受支持。
- `WAIT_AT_LOW_PRIORITY` 选项无法用于联机更改列。
- 联机更改列不支持 `ALTER COLUMN ... ADD/DROP PERSISTED`。
- `ALTER COLUMN ... ADD/DROP ROWGUIDCOL/NOT FOR REPLICATION` 不受联机更改列影响。
- 如果已启用更改跟踪或作为合并复制的发布者，联机更改列不支持更改表。
- 联机更改列不支持更改或更改为 CLR 数据类型。
- 联机更改列不支持更改为，架构集合不同于当前架构集合的 XML 数据类型。
- 联机更改列不放宽对可更改列的时间的限制。 索引/统计信息等的引用可能会导致更改失败。
- 联机更改列不支持同时更改多个列。
- 联机更改列对经系统版本控制的时态表不起任何作用。 无论为 ONLINE 选项指定哪个值，都不会联机运行 ALTER 列。

联机更改列的要求、限制和功能类似于联机索引重新生成，具体包括：

- 如果表包含旧 LOB 或文件流列，或如果表有列存储索引，不支持联机索引重新生成。 相同的限制也适用于联机更改列。
- 要更改的现有列需要两倍的空间分配，用于原始列和新创建的隐藏列。
- 联机更改列操作的锁定策略遵循用于联机索引生成的相同锁定模式。

WITH CHECK | WITH NOCHECK  
指定是否根据新添加或重新启用的 FOREIGN KEY 或 CHECK 约束验证表中的数据。 如果未指定，假定对新约束使用 WITH CHECK，对重新启用的约束使用 WITH NOCHECK。

如果不想对现有数据验证新 CHECK 或 FOREIGN KEY 约束，请使用 WITH NOCHECK。 不建议这样做，极少数情况除外。 新约束在以后的所有数据更新中都会进行评估。 如果在添加约束时任何约束冲突被 WITH NOCHECK 封锁，且行更新数据未遵循约束，约束冲突可能会导致未来更新失败。

> [!NOTE]
> 查询优化器不考虑使用 WITH NOCHECK 定义的约束。 在使用 `ALTER TABLE table WITH CHECK CHECK CONSTRAINT ALL` 重新启用这些约束之前，将忽略这些约束。

ALTER INDEX index_name  
指定要变更或更改 index_name 的桶计数。

语法 ALTER TABLE...内存优化表仅支持 ADD/DROP/ALTER INDEX。

> [!IMPORTANT]
> 如果不使用 ALTER TABLE 语句，内存优化表的索引不支持 [CREATE INDEX](create-index-transact-sql.md)、[DROP INDEX](drop-index-transact-sql.md)[ALTER INDEX](alter-index-transact-sql.md) 和 [PAD_INDEX](alter-table-index-option-transact-sql.md) 语句。

ADD  
指定添加一个或多个列定义、计算列定义或者表约束。 或者，添加系统用于系统版本控制的列。 对于内存优化表，可以添加索引。

> [!NOTE]
> 在更改表中的所有现有列之后，将添加新列。

> [!IMPORTANT]
> 如果不使用 ALTER TABLE 语句，内存优化表的索引不支持 [CREATE INDEX](create-index-transact-sql.md)、[DROP INDEX](drop-index-transact-sql.md)、[ALTER INDEX](alter-index-transact-sql.md) 和 [PAD_INDEX](alter-table-index-option-transact-sql.md) 语句。

PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定系统用于记录有效记录时间段的列的名称。 可以指定现有列或创建新列作为 ADD PERIOD FOR SYSTEM_TIME 参数的一部分。 创建数据类型为 datetime2 的列，并将它们定义为 NOT NULL。 如果将时间段列定义为 NULL，便会导致错误生成。 可以为 system_start_time 和 system_end_time 列定义 [column_constraint](../../t-sql/statements/alter-table-column-constraint-transact-sql.md) 和/或[指定列的默认值](../../relational-databases/tables/specify-default-values-for-columns.md)。 请参阅下面的[系统版本控制](#system_versioning)示例中的示例 A，它展示了如何对 system_end_time 列使用默认值。

将此参数与 SET SYSTEM_VERSIONING 参数结合使用，可对现有表启用系统版本控制。 有关详细信息，请参阅[时态表](../../relational-databases/tables/temporal-tables.md)和 [Azure SQL 数据库中的时态表入门](/azure/azure-sql/temporal-tables)。

自 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 起，用户可以使用 HIDDEN 标志标记一个或两个时间段列，以隐式隐藏这些列，这样 SELECT \* FROM \<table_name> 就不会返回这些列的值。 默认情况下，时间段列不处于隐藏状态。 若要使用隐藏的列，则它必须显式包含在直接引用时态表的所有查询中。

DROP  
指定删除一个或多个列定义、计算列定义或表约束，或要删除系统用于系统版本控制的列的规范。

CONSTRAINT constraint_name  
指定从表中删除 constraint_name。 可以列出多个约束。

通过查询 sys.check_constraint、sys.default_constraints、sys.key_constraints 和 sys.foreign_keys 目录视图，可确定用户定义或系统提供的约束名称。

如果表有 XML 索引，无法删除 PRIMARY KEY 约束。

INDEX index_name  
指定要从表中删除的 index_name。

语法 ALTER TABLE...内存优化表仅支持 ADD/DROP/ALTER INDEX。

> [!IMPORTANT]
> 如果不使用 ALTER TABLE 语句，内存优化表的索引不支持 [CREATE INDEX](create-index-transact-sql.md)、[DROP INDEX](drop-index-transact-sql.md)[ALTER INDEX](alter-index-transact-sql.md) 和 [PAD_INDEX](alter-table-index-option-transact-sql.md) 语句。

COLUMN column_name  
指定从表中删除 constraint_name 或 column_name 。 可以列出多个列。

 无法删除以下列：

- 在索引中作为键列或 INCLUDE 使用
- 用于 CHECK、FOREIGN KEY、UNIQUE 或 PRIMARY KEY 约束的列。
- 与使用 DEFAULT 关键字定义的默认值相关联的列，或绑定到默认对象的列。
- 绑定到规则的列。

> [!NOTE]
> 删除列并不回收列所占的磁盘空间。 当表的行大小接近或超过其限额时，必须回收已删除的列占用的磁盘空间。 通过创建表的聚集索引或使用 [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) 重新生成现有的聚集索引，可以回收空间。 有关删除 LOB 数据类型的影响的信息，请参阅此 [CSS 博客文章](/archive/blogs/psssql/how-it-works-gotcha-varcharmax-caused-my-queries-to-be-slower)。

PERIOD FOR SYSTEM_TIME  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

删除系统将用于系统版本控制的列的规范。

WITH \<drop_clustered_constraint_option>  
指定设置一个或多个删除聚集约束选项。

MAXDOP = max_degree_of_parallelism  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

只在操作期间覆盖 max degree of parallelism 配置选项。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。

使用 MAXDOP 选项来限制执行并行计划时所用的处理器数量。 最大数量为 64 个处理器。

max_degree_of_parallelism 可以是下列值之一：

1  
取消生成并行计划。

\>1  
将并行索引操作中使用的最大处理器数量限制为指定数量。

0（默认值）  
根据当前系统工作负荷使用实际的处理器数量或更少数量的处理器。

有关详细信息，请参阅 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。

> [!NOTE]
> 并行索引操作并不适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的所有版本。 有关详细信息，请参阅 [SQL Server 2016 的版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)和 [SQL Server 2017 的各个版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)。

ONLINE = { ON | OFF } \<as applies to drop_clustered_constraint_option>  
指定在索引操作期间基础表和关联的索引是否可用于查询和数据修改操作。 默认为 OFF。 可以将 REBUILD 运行为 ONLINE 操作。

ON  
长期表锁在索引操作执行期间不保留。 在索引操作的主要阶段，源表上只使用意向共享 (IS) 锁。 通过此行为，可以继续查询或更新基础表和索引。 在操作开始时，共享 (S) 锁对源对象短时间保留。 在操作结束时，若要创建非聚集索引，便会对源短时间获取 S（共享）锁。 或者，如果联机创建或删除聚集索引，且重新生成聚集索引或非聚集索引，便会获取 SCH-M （架构修改）锁。 对本地临时表创建索引时，无法将 ONLINE 设置为 ON。 仅允许单线程堆重新生成操作。

必须完成对特定表运行的所有正在阻塞的事务，才能为 SWITCH 或联机索引重新生成操作运行 DDL。 执行时，SWITCH 或重新生成操作会阻止新事务启动，并可能会严重影响工作负载吞吐量，以及暂时延迟对基础表的访问。

OFF  
表锁在索引操作执行期间应用。 创建、重新生成或删除聚集索引或者重新生成或删除非聚集索引的脱机索引操作将对表获取架构修改 (Sch-M) 锁。 此锁可阻止所有用户在操作执行期间访问基础表。 创建非聚集索引的脱机索引操作将对表获取共享 (S) 锁。 此锁可阻止更新基础表，但允许执行读取操作（如 SELECT 语句）。 允许多线程堆重新生成操作。

有关详细信息，请参阅[联机索引操作的工作方式](../../relational-databases/indexes/how-online-index-operations-work.md)。

> [!NOTE]
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的各版本中均不提供联机索引操作。 有关详细信息，请参阅 [SQL Server 2016 的版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)和 [SQL Server 2017 的各个版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)。

MOVE TO { _partition\_scheme\_name_ **(** _column\_name_ [ 1 **,** ... *n*] **)**  | *filegroup* |  **"** default **"** }  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定一个位置以移动聚集索引的叶级别中的当前数据行。 表被移至新位置。 此选项仅适用于创建聚集索引的约束。

> [!NOTE]
> 在此上下文中，default 不是关键字。 它是默认文件组的标识符，必须对其进行分隔，就像在 MOVE TO "default" 或 MOVE TO [default] 中一样   。 如果指定了“default”，则当前会话的 QUOTED_IDENTIFIER 选项必须为 ON 。 这是默认设置。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。

{ CHECK | NOCHECK } CONSTRAINT  
指定启用或禁用 constraint_name。 此选项只能与 FOREIGN KEY 和 CHECK 约束一起使用。 如果指定了 NOCHECK，则将禁用约束，从而在将来插入或更新列时，不根据约束条件进行验证。 无法禁用 DEFAULT、PRIMARY KEY 和 UNIQUE 约束。

ALL  
指定使用 NOCHECK 选项禁用所有约束，或者使用 CHECK 选项启用所有约束。

{ ENABLE | DISABLE } TRIGGER  
指定启用或禁用 trigger_name。 仍可为表定义禁用的触发器。 不过，如果对表运行 INSERT、UPDATE 或 DELETE 语句，那么在重新启用触发器之前，触发器中的操作不会执行。

ALL  
指定启用或禁用表中的所有触发器。

trigger_name  
指定要启用或禁用的触发器的名称。

{ ENABLE | DISABLE } CHANGE_TRACKING  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定是启用还是禁用表的更改跟踪。 默认情况下会禁用更改跟踪。

只有对数据库启用了更改跟踪，此选项才可用。 有关详细信息，请参阅 [ALTER DATABASE SET 选项](../../t-sql/statements/alter-database-transact-sql-set-options.md)。

若要启用更改跟踪，表必须具有一个主键。

WITH ( TRACK_COLUMNS_UPDATED = { ON | OFF } )  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 是否跟踪哪些更改跟踪列已更新。 默认值为 OFF。

SWITCH [ PARTITION *source_partition_number_expression* ] TO [ _schema\_name_ **.** ] *target_table* [ PARTITION *target_partition_number_expression* ]  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

用下列方式之一切换数据块：

- 将表的所有数据作为分区重新分配给现有的已分区表。
- 将分区从一个已分区表切换到另一个已分区表。
- 将已分区表的一个分区中的所有数据重新分配给现有的未分区的表。

如果 table 是已分区表，必须指定 source_partition_number_expression。 如果 target_table 已分区，必须指定 target_partition_number_expression。 若要将表的数据作为分区重新分配给现有的已分区表，或要将分区由一个已分区表切换到另一个已分区表，那么目标分区必须存在且为空。

若要重新分配一个分区的数据来形成单个表，那么目标表必须已存在且为空。 源表或分区以及目标表或分区必须位于同一个文件组中。 相应的索引或索引分区也必须位于同一个文件组中。 切换分区还有许多其他限制。 table 和 target_table 不得相同。 target_table 可以是由多个部分组成的标识符。

source_partition_number_expression 和 target_partition_number_expression 是可以引用变量和函数的常量表达式 。 其中包括用户定义类型变量和用户定义函数。 它们无法引用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式。

含有聚集列存储索引的已分区表的行为与已区分堆类似：

- 主键必须包含分区键。
- 唯一索引必须包含分区键。 不过，在现有唯一索引中添加分区键可能会改变唯一性。
- 所有非聚集索引都必须包含分区键才能切换分区。

有关使用复制时的 SWITCH 限制的信息，请参阅[复制已分区表和索引](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md)。

在版本 V12 成为只读格式之前，为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016 CTP1 和 SQL 数据库生成的非聚集列存储索引。 必须将非聚集列存储索引重新生成为当前格式（可更新），才能执行任何 PARTITION 操作。

SET ( FILESTREAM_ON = { partition_scheme_name \| filestream_filegroup_name \| "default" \| "NULL" })  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）。 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 不支持 `FILESTREAM`。

指定 FILESTREAM 数据的存储位置。

只有在表不包含任何 FILESTREAM 列时，含 SET FILESTREAM_ON 子句的 ALTER TABLE 才能成功运行。 可使用第二个 ALTER TABLE 语句来添加 FILESTREAM 列。

如果指定 partition_scheme_name，则会应用 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 规则。 请确保表已针对行数据进行了分区，且它的分区方案使用与 FILESTREAM 分区方案相同的分区功能和分区列。

filestream_filegroup_name 指定 FILESTREAM 文件组的名称。 文件组必须包含一个使用 [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) 或 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 语句为文件组定义的文件；否则，便会导致错误生成。

"default" 指定具有 DEFAULT 属性集的 FILESTREAM 文件组 。 如果没有 FILESTREAM 文件组，便会导致错误生成。

"NULL"  指定删除对表的 FILESTREAM 文件组的所有引用。 首先必须删除所有 FILESTREAM 列。 使用 SET FILESTREAM_ON="NULL" 删除与表关联的所有 FILESTREAM 数据。

SET ( SYSTEM_VERSIONING = { OFF | ON [ ( HISTORY_TABLE = schema_name . history_table_name [ , DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ] } )  
 **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

禁用或启用表的系统版本控制。 若要启用表的系统版本控制，系统将验证是否满足系统版本控制的数据类型、为 Null 性约束和主键约束要求。 如果你未使用 HISTORY_TABLE 参数，系统生成符合现有表的架构的新历史记录表，在两个表之间建立关联，让系统能够在历史记录表中记录当前表中每个记录的历史记录。 此历史记录表的名称为 `MSSQL_TemporalHistoryFor<primary_table_object_id>`。 如果你使用 HISTORY_TABLE 参数关联到现有历史记录表并使用此表，系统关联当前表和指定表。 关联到现有历史记录表时，可以选择执行数据一致性检查。 数据一致性检查可确保现有记录不重叠。 系统默认运行数据一致性检查。 有关详细信息，请参阅 [Temporal Tables](../../relational-databases/tables/temporal-tables.md)。

HISTORY_RETENTION_PERIOD = { INFINITE \| number {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS} }  
适用范围：[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定时态表中历史数据的有限保留期或无限保留期。 如果省略，则假定为无限期保留。

SET (DATA_DELETION =  
      { OFF | ON  
                [(  [ FILTER_COLUMN = column_name ]   
                    [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS }}]   
                )] }   
**适用于：** 仅限 Azure SQL Edge

允许基于保留策略清除数据库内各表中的旧数据或过期数据。 有关详细信息，请参阅[启用和禁用数据保留](/azure/azure-sql-edge/data-retention-enable-disable)。 若要启用数据保留，必须指定以下参数。 

- FILTER_COLUMN = { column_name }  
指定应用于确定表中的行是否过时的列。 筛选器列支持以下数据类型。
  - Date
  - DateTime
  - DateTime2
  - SmallDateTime
  - DateTimeOffset

- RETENTION_PERIOD = { INFINITE \| number {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS }}       
指定表的保留期策略。 保留期指定为正整数值和日期部分单位的组合。 

SET ( LOCK_ESCALATION = { AUTO \| TABLE \| DISABLE } )  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定允许的对表的锁进行升级的方法。

AUTO  
借助此选项，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 可选择适合于表架构的锁升级粒度。

- 如果该表已分区，则允许锁升级到堆或 B 树 (HoBT) 粒度。 换句话说，将允许升级到分区级别。 锁升级到 HoBT 级别之后，该锁以后将不会升级到 TABLE 粒度。
- 如果表未分区，锁升级到 TABLE 粒度。

TABLE  
无论表是否已分区，锁都升级到表级粒度。 默认值为 TABLE。

DISABLE  
在大多数情况下禁止锁升级。 表级锁并未完全被禁止。 例如，如果扫描的表在可序列化隔离级别下没有聚集索引，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 必须使用表锁来保证数据完整性。

REBUILD  
使用 REBUILD WITH 语法可重新生成包含分区表中的所有分区的整个表。 如果表具有聚集索引，则 REBUILD 选项将重新生成该聚集索引。 REBUILD 可以运行为 ONLINE 操作。

使用 REBUILD PARTITION 语法可重新生成分区表中的单个分区。

PARTITION = ALL  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

更改分区压缩设置时重新生成所有分区。

REBUILD WITH ( \<rebuild_option> )  
为具有聚集索引的表应用所有选项。 如果表没有聚集索引，堆结构只受部分选项影响。

如果未使用 REBUILD 操作指定具体压缩设置，使用的是分区的当前压缩设置。 若要返回当前设置，请在 sys.partitions 目录视图中查询 data_compression 列 。

有关重新生成选项的完整说明，请参阅 [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md)。

DATA_COMPRESSION  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

为指定的表、分区号或分区范围指定数据压缩选项。 选项如下：

NONE 不压缩表或指定的分区。 此选项不适用于列存储表。

ROW 使用行压缩来压缩表或指定的分区。 此选项不适用于列存储表。

PAGE 使用页压缩来压缩表或指定的分区。 此选项不适用于列存储表。

COLUMNSTORE  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

仅适用于列存储表。 COLUMNSTORE 指定对使用 COLUMNSTORE_ARCHIVE 选项压缩的分区进行解压缩。 还原数据时，继续通过用于所有列存储表的列存储压缩来压缩数据。

COLUMNSTORE_ARCHIVE  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

仅适用于列存储表，这是使用聚集列存储索引存储的表。 COLUMNSTORE_ARCHIVE 会进一步将指定分区压缩为更小的大小。 此选项可用于存档，或其他要求更少存储但能花更多时间用于存储和检索的情形。

若要同时重新生成多个分区，请参阅 [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md)。 如果表没有聚集索引，更改数据压缩会重新生成堆和非聚集索引。 有关压缩的详细信息，请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)。

ONLINE = { ON | OFF } \<as applies to single_partition_rebuild_option>  
指定基础表和相关索引的单个分区能否在索引操作执行期间用于查询和数据修改。 默认为 OFF。 可以将 REBUILD 运行为 ONLINE 操作。

ON  
长期表锁在索引操作执行期间不保留。 在索引重新生成开始时，表需要 S 锁；在联机索引重新生成结束时，表需要 Sch-M 锁。 尽管两个锁都是短元数据锁，但 Sch-M 锁必须等待所有正在阻塞的事务都完成。 在等待期间，Sch-M 锁阻止在访问同一表时在此锁后等待的其他所有事务。

> [!NOTE]
> 联机索引重新生成可以设置本节稍后介绍的 low_priority_lock_wait 选项。

OFF  
在索引操作期间应用表锁。 这样可以防止所有用户在操作期间访问基础表。

column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

列集的名称。 列集是一种非类型化的 XML 表示形式，它将表的所有稀疏列合并为一种结构化的输出。 无法将列集添加到包含稀疏列的表。 有关列集的详细信息，请参阅 [使用列集](../../relational-databases/tables/use-column-sets.md)。

{ ENABLE | DISABLE } FILETABLE_NAMESPACE  
 **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）。

启用或禁用针对 FileTable 的系统定义约束。 仅可与 FileTable 一起使用。

SET ( FILETABLE_DIRECTORY = directory_name )  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）。 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 不支持 `FILETABLE`。

指定与 Windows 兼容的 FileTable 目录名称。 此名称应在数据库的所有 FileTable 目录名称中唯一。 无论 SQL 排序规则设置如何，唯一性比较都不区分大小写。 仅可与 FileTable 一起使用。

```syntaxsql
 SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options> )
          | = OFF_WITHOUT_DATA_RECOVERY
          ( MIGRATION_STATE = PAUSED ) | ( <table_stretch_options> [, ...n] )
        } )
```

**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 及更高版本）。

为表启用或禁用 Stretch Database。 有关详细信息，请参阅 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)。

为表启用 Stretch Database

通过指定 `ON` 为表启用 Stretch 时，必须同时指定 `MIGRATION_STATE = OUTBOUND` 立即开始迁移数据或指定 `MIGRATION_STATE = PAUSED` 推迟迁移数据。 默认值是 `MIGRATION_STATE = OUTBOUND`。 若要详细了解如何为表启用 Stretch，请参阅[为表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)。

**先决条件**。 为表启用 Stretch 之前，必须在服务器和数据库上启用 Stretch。 有关详细信息，请参阅[为数据库启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)。

**权限**。 为数据库或表启用 Stretch 需要 db_owner 权限。 为表启用 Stretch 还需具有表的 ALTER 权限。

为表禁用 Stretch Database

为表禁用 Stretch 后，已迁移到 Azure 的远程数据的处理方式有两种。 有关详细信息，请参阅[禁用 Stretch Database 并恢复远程数据](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md)。

- 要针对某个表禁用 Stretch 并将该表的远程数据从 Azure 复制回 SQL Server，请运行以下命令。 此命令不能取消。

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    ```

此操作会产生数据传输费用，并且不可取消。 有关详细信息，请参阅[数据传输定价详细信息](https://azure.microsoft.com/pricing/details/data-transfers/)。

将所有远程数据从 Azure 复制回到 SQL Server 之后，将为表禁用延伸。

- 若要为某个表禁用延伸并放弃远程数据，请运行以下命令。

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

为表禁用 Stretch Database 之后，数据迁移会停止，查询结果不再包括来自远程表的结果。

禁用 Stretch 不会删除远程表。 若要删除远程表，请使用 Azure 门户删除它。

[ FILTER_PREDICATE = { null | predicate } ]  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 及更高版本）。

根据需要，指定一个筛选器谓词，从包含历史数据和最新数据的表中选择要迁移的行。 该谓词必须调用确定性的内联表值函数。 有关详细信息，请参阅[为表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) 和[使用筛选器函数选择要迁移的行](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md)。

> [!IMPORTANT]
> 如果提供的筛选器谓词性能不佳，则数据迁移性能也不佳。 Stretch Database 通过使用 CROSS APPLY 运算符将筛选器谓词应用到表中。

如果未指定筛选器谓词，则将迁移整个表。

指定筛选器谓词时，还须同时指定 MIGRATION_STATE。

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 及更高版本）。

- 指定 `OUTBOUND` 可将数据从 SQL Server 迁移到 Azure。
- 指定 `INBOUND` 可将表中的远程数据从 Azure 复制回 SQL Server，然后对该表禁用 Stretch Database。 有关详细信息，请参阅[禁用 Stretch Database 并恢复远程数据](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md)。

    此操作会产生数据传输费用，并且不可取消。

- 指定 `PAUSED` 可暂停或推迟数据迁移。 有关详细信息，请参阅[暂停和恢复数据迁移 - Stretch Database](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)。

WAIT_AT_LOW_PRIORITY  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

联机索引重新生成必须等待对此表执行的阻塞操作。 WAIT_AT_LOW_PRIORITY 表示，联机索引重新生成操作等待低优先级锁，从而允许其他操作在联机索引生成操作正在等待时执行。 省略 WAIT AT LOW PRIORITY 选项与 `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)` 等效。

MAX_DURATION = time [MINUTES ]  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

SWITCH 或联机索引重新生成锁在运行 DDL 命令时以低优先级等待的等待时间（以分钟为单位指定的整数值）。 如果操作在 MAX_DURATION 时间内遭阻止，ABORT_AFTER_WAIT 操作之一便会运行。 MAX_DURATION 时间始终以分钟为单位，可以省略 MINUTES 一词。

ABORT_AFTER_WAIT = [NONE | SELF | BLOCKERS } ]  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

无  
继续以普通（常规）优先级等待锁。

SELF  
不采取任何操作，直接退出当前运行的 SWITCH 或联机索引重新生成 DDL 操作。

BLOCKERS  
终止所有当前阻止 SWITCH 或联机索引重新生成 DDL 操作的用户事务，以便操作能够继续执行。

要求具有 ALTER ANY CONNECTION 权限。

IF EXISTS  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

有条件地删除列或约束（仅当存在时）。

## <a name="remarks"></a>备注

要添加新数据行，请使用 [INSERT](../../t-sql/statements/insert-transact-sql.md)。 要删除数据行，请使用 [DELETE](../../t-sql/statements/delete-transact-sql.md) 或 [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md)。 要更改现有行中的值，请使用 [UPDATE](../../t-sql/queries/update-transact-sql.md)。

如果过程高速缓存中存在引用表的执行计划，ALTER TABLE 会将这些执行计划标记为下次执行时重新编译。

## <a name="changing-the-size-of-a-column"></a>更改列的大小

可以指定列数据类型的新大小，从而更改列的长度、精度或确定位数。 使用 ALTER COLUMN 子句。 如果列中有数据，新大小不得小于数据的大小上限。 此外，除非列的数据类型为 varchar、nvarchar 或 varbinary，且索引不是 PRIMARY KEY 约束的结果，否则无法在索引中定义列。 请参阅标题为[更改列定义](#alter_column)的小节中的示例。

## <a name="locks-and-alter-table"></a>锁和 ALTER TABLE

在 ALTER TABLE 中指定的更改会立即实现。 如果这些更改需要修改表中的行，ALTER TABLE 将更新这些行。 ALTER TABLE 对表获取架构修改 (SCH-M) 锁，以确保在更改期间没有其他连接引用表的元数据，在结束时需要短 SCH-M 锁的联机索引操作除外。 在 `ALTER TABLE...SWITCH` 操作中，源表和目标表都需要锁。 对表进行的更改将记录于日志中，并且可以完整恢复。 影响大型表中所有行的更改（如删除列，或在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的一些版本中添加有默认值的 NOT NULL 列）可能需要较长时间才能完成，并生成大量日志记录。 慎重运行这些 ALTER TABLE 语句，就像运行影响许多行的任何 INSERT、UPDATE 或 DELETE 语句一样。

### <a name="adding-not-null-columns-as-an-online-operation"></a>以联机操作的形式添加 NOT NULL 列

从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise Edition 开始，当默认值为“运行时常量”时，添加具有该默认值的 NOT NULL 列是一个联机操作。 也就是说，无论表中有多少行，此操作都几乎可以瞬间完成。 这是因为，表中的现有行在操作执行期间不更新。 相反，默认值仅存储在表的元数据中，且能根据需要通过访问这些行的查询来查找默认值。 这种行为是自动的。 除了 ADD COLUMN 语法之外，无需其他任何语法，即可实现联机操作。 运行时常量是表达式，它可以在运行时为表中的每一行都生成相同的值，无论确定性如何。 例如，常量表达式“My temporary data”或系统函数 GETUTCDATETIME() 均为运行时常量。 相比之下，函数 `NEWID()` 或 `NEWSEQUENTIALID()` 就不是运行时常量，因为这些函数为表中的每一行都生成唯一值。 添加有默认值（不是运行时常量）的 NOT NULL 列始终是脱机运行，并且在操作执行期间需要排他 (SCH-M) 锁。

尽管现有行引用元数据中存储的值，但对于已插入但不为列指定其他值的任何新行，默认值存储在行中。 在行更新（即使未在 UPDATE 语句中指定实际列）或重新生成表或聚集索引时，存储在元数据中的默认值移到现有行中。

无法在联机操作中添加 varchar(max)、nvarchar(max)、varbinary(max)、xml、text、ntext、image、hierarchyid、geometry、geography 或 CLR UDTS 类型的列。 如果列会导致最大可能行大小超过 8,060 字节限制，无法联机添加列。 在这种情况下将在脱机操作中添加列。

## <a name="parallel-plan-execution"></a>并行计划执行

在 [!INCLUDE[ssEnterpriseEd11](../../includes/ssenterpriseed11-md.md)] 及更高版本中，用来运行单个 ALTER TABLE ADD（基于索引）CONSTRAINT 或 DROP（聚集索引）CONSTRAINT 语句的处理器数的确定依据为，“最大并行度”配置选项和当前工作负载。 如果[!INCLUDE[ssDE](../../includes/ssde-md.md)]检测到系统正忙，则在语句执行开始之前将自动降低操作并行度。 可以通过指定 MAXDOP 选项，手动配置用于运行此语句的处理器数。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。

## <a name="partitioned-tables"></a>已分区表

除了执行涉及已分区表的 SWITCH 操作外，还可以使用 ALTER TABLE 更改已分区表的列、约束和触发器状态，就像用于非分区表一样。 不过，无法使用此语句来更改表本身的分区方式。 若要对已分区表进行重新分区，请使用 [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md) 和 [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md)。 此外，无法更改已分区表中列的数据类型。

## <a name="restrictions-on-tables-with-schema-bound-views"></a>对包含绑定到架构视图的表的限制

应用于包含架构绑定视图的表的 ALTER TABLE 语句的限制，与当前修改包含简单索引的表时应用的限制相同。 允许添加列。 不过，禁止删除或更改参与任何架构绑定视图的列。 如果 ALTER TABLE 语句要求更改用于架构绑定视图中的列，ALTER TABLE 将失败，并且[!INCLUDE[ssDE](../../includes/ssde-md.md)]将引发错误消息。 有关架构绑定和索引视图的详细信息，请参阅 [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)。

创建引用表的架构绑定视图不会影响在基表上添加或删除触发器。

## <a name="indexes-and-alter-table"></a>索引和 ALTER TABLE

删除约束时，作为约束的一部分而创建的索引也将被删除。 必须使用 DROP INDEX 删除由 CREATE INDEX 创建的索引。 使用 ALTER INDEX 语句重新生成约束定义的索引部分；而不必再使用 ALTER TABLE 来删除和添加约束。

必须在删除所有基于列的索引和约束之后，才能删除列。

如果删除了创建聚集索引的约束，存储在聚集索引叶级别的数据行存储在非聚集表中。 通过指定 MOVE TO 选项，可以在单个事务中删除聚集索引并将生成的表移动到另一个文件组或分区方案。 MOVE TO 选项有以下限制：

- MOVE TO 对索引视图或非聚集索引无效。
- 分区方案或文件组必须已经存在。
- 如果没有指定 MOVE TO，表位于为聚集索引定义的同一分区方案或文件组中。

删除聚集索引时，指定 ONLINE = ON 选项，这样 DROP INDEX 事务就不会阻止对基础数据和相关非聚集索引进行查询和修改。

ONLINE = ON 具有下列限制：

- ONLINE = ON 对也遭禁用的聚集索引无效。 必须使用 ONLINE = OFF 删除禁用的索引。
- 一次只能删除一个索引。
- ONLINE = ON 对索引视图、非聚集索引或本地时态表上的索引无效。
- ONLINE = ON 对列存储索引无效。

删除聚集索引时，需要大小等于现有聚集索引的大小的临时磁盘空间。 操作完成后，即可释放此额外空间。

> [!NOTE]
> \<drop_clustered_constraint_option> 下列出的选项适用于表上的聚集索引，但不适用于视图上的聚集索引或非聚集索引。

## <a name="replicating-schema-changes"></a>复制架构更改

默认情况下，当在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器中对发布的表运行 ALTER TABLE 时，此更改传播到所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。 此功能存在一些限制。 可以禁用它。 有关详细信息，请参阅[对发布数据库进行架构更改](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。

## <a name="data-compression"></a>数据压缩

无法为系统表启用压缩。 如果表是堆，ONLINE 模式的重新生成操作将在单个线程内完成。 请为多线程堆重新生成操作使用 OFFLINE 模式。 有关数据压缩的详细信息，请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)。

若要评估更改压缩状态将对表、索引或分区有何影响，请使用 [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) 存储过程。

以下限制适用于已分区表：

- 如果表有非对齐索引，无法更改单个分区的压缩设置。
- ALTER TABLE \<table> REBUILD PARTITION ... 语法会重新生成指定分区。
- ALTER TABLE \<table> REBUILD WITH ... 语法会重新生成所有分区。

## <a name="dropping-ntext-columns"></a>删除 NTEXT 列

删除 NTEXT 列时，将以序列化操作的形式在所有行中清除所删除的数据。 清理可能需要大量时间才能完成。 在有大量行的表中删除 NTEXT 列时，首先将 NTEXT 列更新为 NULL 值，再删除此列。 可以使用并行操作运行此选项，并加快速度。

## <a name="online-index-rebuild"></a>联机索引重新生成

若要为联机索引重新生成操作运行 DDL 语句，必须完成对特定表运行的所有正在阻塞的事务。 在联机索引重新生成启动时，它会阻止所有准备开始对此表运行的新事务。 尽管联机索引重新生成锁的持续时间较短，但等待给定表的所有待处理事务完成，以及阻止新事务启动可能会对吞吐量产生很大影响。 这可能会导致工作负载变慢或超时，并严重限制对基础表的访问。 使用 WAIT_AT_LOW_PRIORITY 选项，DBA 可以管理联机索引重新生成需要的 S 锁和 Sch-M 锁，并能选择三个选项之一。 在所有三种情况下，如果等待期间 (`(MAX_DURATION =n [minutes])`) 没有阻塞活动，联机索引重新生成就会立即运行而不等待，且 DDL 语句会完成。

## <a name="compatibility-support"></a>兼容性支持

ALTER TABLE 语句只支持包含两部分 (schema.object) 的表名称。 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中，使用以下格式指定表名称时，在编译时会出现错误 117。

- server.database.schema.table
- .database.schema.table
- ..schema.table

在旧版中，指定格式 server.database.schema.table 返回了错误 4902。 指定格式 .database.schema.table 或 ..schema.table 则会成功。

若要解决此问题，请不要使用包含四部分的前缀。

## <a name="permissions"></a>权限

需要对表的 ALTER 权限。

ALTER TABLE 权限适用于 ALTER TABLE SWITCH 语句涉及的两个表。 已切换的所有数据都继承目标表的安全性。

如果已将 ALTER TABLE 语句中的任何列定义为使用公共语言运行时 (CLR) 用户定义类型或别名数据类型，必须对类型拥有 REFERENCES 权限。

添加或更改可更新表中的行的列要求对该表具有 UPDATE 权限。 例如，在表不为空时，添加有默认值的 NOT NULL 列，或添加标识列。

## <a name="examples"></a><a name="Example_Top"></a> 示例

|类别|作为特征的语法元素|
|--------------|------------------------------|
|[添加列和约束](#add)|ADD * PRIMARY KEY with index options * sparse columns and column sets *|
|[删除列和约束](#Drop)|DROP|
|[更改列定义](#alter_column)|change data type * change column size * collation|
|[更改表定义](#alter_table)|DATA_COMPRESSION * SWITCH PARTITION * LOCK ESCALATION * change tracking|
|[禁用和启用约束和触发器](#disable_enable)|CHECK * NO CHECK * ENABLE TRIGGER * DISABLE TRIGGER|
| &nbsp; | &nbsp; |

### <a name="adding-columns-and-constraints"></a><a name="add"></a>添加列和约束

本节中的示例说明将列和约束添加到表中。

#### <a name="a-adding-a-new-column"></a>A. 添加新列

以下示例将添加一个允许 Null 值的列，而且没有通过 DEFAULT 定义提供的值。 在该新列中，每一行都将有 `NULL` 值。

```sql
CREATE TABLE dbo.doc_exa (column_a INT) ;
GO
ALTER TABLE dbo.doc_exa ADD column_b VARCHAR(20) NULL ;
GO
```

#### <a name="b-adding-a-column-with-a-constraint"></a>B. 添加包含约束的列

以下示例将添加一个包含 `UNIQUE` 约束的新列。

```sql
CREATE TABLE dbo.doc_exc (column_a INT) ;
GO
ALTER TABLE dbo.doc_exc ADD column_b VARCHAR(20) NULL
    CONSTRAINT exb_unique UNIQUE ;
GO
EXEC sp_help doc_exc ;
GO
DROP TABLE dbo.doc_exc ;
GO
```

#### <a name="c-adding-an-unverified-check-constraint-to-an-existing-column"></a>C. 在现有列中添加一个未经验证的 CHECK 约束

下面的示例将在表中的现有列中添加一个约束。 该列包含一个违反约束的值。 因此，将使用 `WITH NOCHECK` 以避免根据现有行验证该约束，从而允许添加该约束。

```sql
CREATE TABLE dbo.doc_exd (column_a INT) ;
GO
INSERT INTO dbo.doc_exd VALUES (-1) ;
GO
ALTER TABLE dbo.doc_exd WITH NOCHECK
ADD CONSTRAINT exd_check CHECK (column_a > 1) ;
GO
EXEC sp_help doc_exd ;
GO
DROP TABLE dbo.doc_exd ;
GO
```

#### <a name="d-adding-a-default-constraint-to-an-existing-column"></a>D. 在现有列中添加一个 DEFAULT 约束

下面的示例将创建一个包含两列的表，在第一列插入一个值，另一列保持为 NULL。 然后在第二列中添加一个 `DEFAULT` 约束。 验证是否已应用了默认值，另一个值是否已插入第一列以及是否已查询表。

```sql
CREATE TABLE dbo.doc_exz (column_a INT, column_b INT) ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (7) ;
GO
ALTER TABLE dbo.doc_exz
  ADD CONSTRAINT col_b_def
  DEFAULT 50 FOR column_b ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (10) ;
GO
SELECT * FROM dbo.doc_exz ;
GO
DROP TABLE dbo.doc_exz ;
GO
```

#### <a name="e-adding-several-columns-with-constraints"></a>E. 添加多个包含约束的列

以下示例将添加多个包含随新列定义的约束的列。 第一个新列具有 `IDENTITY` 属性。 表中的每一行在标识列中都有新的增量值。

```sql
CREATE TABLE dbo.doc_exe (column_a INT CONSTRAINT column_a_un UNIQUE) ;
GO
ALTER TABLE dbo.doc_exe ADD

-- Add a PRIMARY KEY identity column.
column_b INT IDENTITY
CONSTRAINT column_b_pk PRIMARY KEY,

-- Add a column that references another column in the same table.
column_c INT NULL
CONSTRAINT column_c_fk
REFERENCES doc_exe(column_a),

-- Add a column with a constraint to enforce that
-- nonnull data is in a valid telephone number format.
column_d VARCHAR(16) NULL
CONSTRAINT column_d_chk
CHECK
(column_d LIKE '[0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]' OR
column_d LIKE
'([0-9][0-9][0-9]) [0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]'),

-- Add a nonnull column with a default.
column_e DECIMAL(3,3)
CONSTRAINT column_e_default
DEFAULT .081 ;
GO
EXEC sp_help doc_exe ;
GO
DROP TABLE dbo.doc_exe ;
GO
```

#### <a name="f-adding-a-nullable-column-with-default-values"></a>F. 添加包含默认值的可为空的列

下面的示例将添加一个包含 `DEFAULT` 定义的可为 Null 的列，并使用 `WITH VALUES` 为表中的各个现有行提供值。 如果没有使用 WITH VALUES，那么每一行的新列中都有 NULL 值。

```sql
CREATE TABLE dbo.doc_exf (column_a INT) ;
GO
INSERT INTO dbo.doc_exf VALUES (1) ;
GO
ALTER TABLE dbo.doc_exf
ADD AddDate smalldatetime NULL
CONSTRAINT AddDateDflt
DEFAULT GETDATE() WITH VALUES ;
GO
DROP TABLE dbo.doc_exf ;
GO
```

#### <a name="g-creating-a-primary-key-constraint-with-index-or-data-compression-options"></a>G. 使用索引或数据压缩选项创建 PRIMARY KEY 约束

下面的示例将创建 PRIMARY KEY 约束 `PK_TransactionHistoryArchive_TransactionID`，并设置 `FILLFACTOR`、`ONLINE` 和 `PAD_INDEX` 选项。 生成的聚集索引将与约束同名。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (FILLFACTOR = 75, ONLINE = ON, PAD_INDEX = ON);
GO
```

此类似示例在应用群集主键时应用页面压缩。

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (DATA_COMPRESSION = PAGE);
GO
```

#### <a name="h-adding-a-sparse-column"></a>H. 添加稀疏列

下面的示例演示如何在表 T1 中添加和修改稀疏列。 创建表 `T1` 的代码如下所示。

```sql
CREATE TABLE T1 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) SPARSE NULL,
  C3 INT SPARSE NULL,
  C4 INT) ;
GO
```

若要添加另一个稀疏列 `C5`，请执行以下语句。

```sql
ALTER TABLE T1
ADD C5 CHAR(100) SPARSE NULL ;
GO
```

若要将 `C4` 非稀疏列转换为稀疏列，请执行以下语句。

```sql
ALTER TABLE T1
ALTER COLUMN C4 ADD SPARSE ;
GO
```

若要将 `C4` 稀疏列转换为非稀疏列，请执行以下语句。

```sql
ALTER TABLE T1
ALTER COLUMN C4 DROP SPARSE ;
GO
```

#### <a name="i-adding-a-column-set"></a>I. 添加列集

下面的示例演示如何向表 `T2` 中添加一列。 无法将列集添加到已包含稀疏列的表。 创建表 `T2` 的代码如下所示。

```sql
CREATE TABLE T2 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

下面的三个语句添加名为 `CS` 的列集，然后将列 `C2` 和 `C3` 修改为 `SPARSE`。

```sql
ALTER TABLE T2
ADD CS XML COLUMN_SET FOR ALL_SPARSE_COLUMNS ;
GO

ALTER TABLE T2
ALTER COLUMN C2 ADD SPARSE ;
GO

ALTER TABLE T2
ALTER COLUMN C3 ADD SPARSE ;
GO
```

#### <a name="j-adding-an-encrypted-column"></a>J. 添加加密列

以下语句将添加名为 `PromotionCode` 的加密列。

```sql
ALTER TABLE Customers ADD
    PromotionCode nvarchar(100)
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,
    ENCRYPTION_TYPE = RANDOMIZED,
    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') ;
```

### <a name="dropping-columns-and-constraints"></a><a name="Drop"></a>删除列和约束

本节中的示例说明如何删除列和约束。

#### <a name="a-dropping-a-column-or-columns"></a>A. 删除一列或多列

第一个示例将修改一个表以删除列。 第二个示例删除多列。

```sql
CREATE TABLE dbo.doc_exb (
     column_a INT,
     column_b VARCHAR(20) NULL,
     column_c DATETIME,
     column_d INT) ;
GO  
-- Remove a single column.
ALTER TABLE dbo.doc_exb DROP COLUMN column_b ;
GO
-- Remove multiple columns.
ALTER TABLE dbo.doc_exb DROP COLUMN column_c, column_d;
```

#### <a name="b-dropping-constraints-and-columns"></a>B. 删除约束和列

第一个示例将从表中删除 `UNIQUE` 约束。 第二个示例将删除两个约束和一列。

```sql
CREATE TABLE dbo.doc_exc (column_a INT NOT NULL CONSTRAINT my_constraint UNIQUE) ;
GO

-- Example 1. Remove a single constraint.
ALTER TABLE dbo.doc_exc DROP my_constraint ;
GO

DROP TABLE dbo.doc_exc;
GO

CREATE TABLE dbo.doc_exc ( column_a INT
                          NOT NULL CONSTRAINT my_constraint UNIQUE
                          ,column_b INT
                          NOT NULL CONSTRAINT my_pk_constraint PRIMARY KEY) ;
GO

-- Example 2. Remove two constraints and one column
-- The keyword CONSTRAINT is optional. The keyword COLUMN is required.
ALTER TABLE dbo.doc_exc
DROP CONSTRAINT my_constraint, my_pk_constraint, COLUMN column_b ;
GO
```

#### <a name="c-dropping-a-primary-key-constraint-in-the-online-mode"></a>C. 在 ONLINE 模式下删除 PRIMARY KEY 约束

下面的示例在 `ONLINE` 选项设置为 `ON` 的情况下删除 PRIMARY KEY 约束。

```sql
ALTER TABLE Production.TransactionHistoryArchive
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID
WITH (ONLINE = ON) ;
GO
```

#### <a name="d-adding-and-dropping-a-foreign-key-constraint"></a>D. 添加和删除 FOREIGN KEY 约束

下面的示例将创建 `ContactBackup` 表，然后更改此表。首先添加引用 `FOREIGN KEY` 表的 `Person.Person` 约束，然后再删除 `FOREIGN KEY` 约束。

```sql
CREATE TABLE Person.ContactBackup
    (ContactID INT) ;
GO

ALTER TABLE Person.ContactBackup
ADD CONSTRAINT FK_ContactBackup_Contact FOREIGN KEY (ContactID)
    REFERENCES Person.Person (BusinessEntityID) ;
GO

ALTER TABLE Person.ContactBackup
DROP CONSTRAINT FK_ContactBackup_Contact ;
GO

DROP TABLE Person.ContactBackup ;
```

![用于“返回页首”链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [示例](#Example_Top)

### <a name="altering-a-column-definition"></a><a name="alter_column"></a>更改列定义

#### <a name="a-changing-the-data-type-of-a-column"></a>A. 更改列的数据类型

下面的示例将表中列的数据类型由 `INT` 改为 `DECIMAL`。

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy ALTER COLUMN column_a DECIMAL (5, 2) ;
GO
DROP TABLE dbo.doc_exy ;
GO
```

#### <a name="b-changing-the-size-of-a-column"></a>B. 更改列的大小

以下示例增加 varchar 列的大小和 decimal 列的精度和小数位数 。 因为列包含数据，所以只能增加列的大小。 此外，请注意：`col_a` 是在一个唯一索引中定义的。 `col_a` 的大小仍可以增加，因为数据类型为 varchar，且索引不是 PRIMARY KEY 约束的结果。

```sql
-- Create a two-column table with a unique index on the varchar column.
CREATE TABLE dbo.doc_exy (col_a varchar(5) UNIQUE NOT NULL, col_b decimal (4,2)) ;
GO
INSERT INTO dbo.doc_exy VALUES ('Test', 99.99) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
GO
-- Increase the size of the varchar column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_a varchar(25) ;
GO
-- Increase the scale and precision of the decimal column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_b decimal (10,4) ;
GO
-- Insert a new row.
INSERT INTO dbo.doc_exy VALUES ('MyNewColumnSize', 99999.9999) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
```

#### <a name="c-changing-column-collation"></a>C. 更改列排序规则

下面的示例说明了如何更改列的排序规则。 首先，创建一个表以及默认的用户排序规则。

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

接着，将列 `C2` 排序规则更改为 Latin1_General_BIN。 必须有数据类型，即使它未改变。

```sql
ALTER TABLE T3
ALTER COLUMN C2 varchar(50) COLLATE Latin1_General_BIN ;
GO
```

#### <a name="d-encrypting-a-column"></a>D. 加密列

以下示例演示如何使用[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md) 加密列。

首先，将创建没有任何加密列的表。

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

接着，使用列加密密钥（名为 CEK1）和随机加密来加密列“C2”。 若要成功执行下面的语句，必须满足以下条件：

- 列加密密钥必须已启用 enclave。 也就是说，必须使用允许 enclave 计算的列主密钥进行加密。
- 目标 SQL Server 实例必须支持具有安全 enclave 的 Always Encrypted。
- 必须通过为具有安全 enclave 的 Always Encrypted 设置的连接且使用受支持的客户端驱动程序发出该语句。
- 调用应用程序必须有权访问列主密钥，用来保护 CEK1。

```sql
ALTER TABLE T3
ALTER COLUMN C2 VARCHAR(50) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL;
GO
```

### <a name="altering-a-table-definition"></a><a name="alter_table"></a>更改表定义

本节中的示例说明如何更改表定义。

#### <a name="a-modifying-a-table-to-change-the-compression"></a>A. 修改表以更改压缩

下面的示例更改未分区表的压缩。 将会重新生成堆或聚集索引。 如果表是一个堆，将重新生成所有非聚集索引。

```sql
ALTER TABLE T1
REBUILD WITH (DATA_COMPRESSION = PAGE) ;
```

下面的示例更改已分区表的压缩。 `REBUILD PARTITION = 1` 语法仅仅导致重新生成编号为 `1` 的分区。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =NONE) ;
GO
```

使用以下替代语法的相同操作则会导致重新生成表中的所有分区。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = ALL
WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(1)) ;
```

有关其他数据压缩示例，请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)。

#### <a name="b-modifying-a-columnstore-table-to-change-archival-compression"></a>B. 修改列存储表以便更改存档压缩

下面的示例通过应用附加的压缩算法，进一步压缩列存储表分区。 此压缩会缩小表大小，但也延长了存储和检索所需的时间。 这可用于存档，或者用于要求更少空间并且可以付出更多时间来进行存储和检索的其他情形。

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE) ;
GO
```

下面的示例解压缩已使用 COLUMNSTORE_ARCHIVE 选项进行了压缩的列存储表分区。 还原后的数据继续通过用于所有列存储表的列存储压缩进行压缩。

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE) ;
GO
```

#### <a name="c-switching-partitions-between-tables"></a>C. 在表之间切换分区

以下示例创建一个已分区表，并假定在数据库中已经创建了分区方案 `myRangePS1`。 然后，在 `PARTITION 2` 表的 `PartitionTable` 所在的同一文件组中，创建与已分区表结构相同的未分区的表。 最后，将 `PARTITION 2` 表的 `PartitionTable` 中的数据切换到 `NonPartitionTable` 表中。

```sql
CREATE TABLE PartitionTable (col1 INT, col2 CHAR(10))
ON myRangePS1 (col1) ;
GO
CREATE TABLE NonPartitionTable (col1 INT, col2 CHAR(10))
ON test2fg ;
GO
ALTER TABLE PartitionTable SWITCH PARTITION 2 TO NonPartitionTable ;
GO
```

#### <a name="d-allowing-lock-escalation-on-partitioned-tables"></a>D. 允许已分区表中的锁升级

以下示例在已分区表的分区级别启用锁升级。 如果表未分区，锁升级在 TABLE 一级进行设置。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

```sql
ALTER TABLE dbo.T1 SET (LOCK_ESCALATION = AUTO) ;
GO
```

#### <a name="e-configuring-change-tracking-on-a-table"></a>E. 配置表的更改跟踪

下面的示例对 `Person.Person` 表启用了更改跟踪。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

```sql
USE AdventureWorks;
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING ;
```

下面的示例启用更改跟踪，并启用在进行某项更改期间会进行更新的列的跟踪。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING
WITH (TRACK_COLUMNS_UPDATED = ON)
```

下面的示例对 `Person.Person` 表禁用更改跟踪。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
DISABLE CHANGE_TRACKING ;
```

### <a name="disabling-and-enabling-constraints-and-triggers"></a><a name="disable_enable"></a>禁用和启用约束和触发器

#### <a name="a-disabling-and-re-enabling-a-constraint"></a>A. 禁用和重新启用约束

以下示例中禁用了用于限制数据中所接受薪水的约束。 `NOCHECK CONSTRAINT` 与 `ALTER TABLE` 一起使用以禁用该约束，从而允许执行通常会违反约束的插入操作。 `CHECK CONSTRAINT` 重新启用约束。

```sql
CREATE TABLE dbo.cnst_example (
  id INT NOT NULL,
  name VARCHAR(10) NOT NULL,
  salary MONEY NOT NULL
  CONSTRAINT salary_cap CHECK (salary < 100000)) ;

-- Valid inserts
INSERT INTO dbo.cnst_example VALUES (1,'Joe Brown',65000) ;
INSERT INTO dbo.cnst_example VALUES (2,'Mary Smith',75000) ;

-- This insert violates the constraint.
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Disable the constraint and try again.
ALTER TABLE dbo.cnst_example NOCHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Re-enable the constraint and try another insert; this will fail.
ALTER TABLE dbo.cnst_example CHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (4,'Eric James',110000) ;
```

#### <a name="b-disabling-and-re-enabling-a-trigger"></a>B. 禁用和重新启用触发器

下面的示例将使用 `DISABLE TRIGGER` 的 `ALTER TABLE` 选项来禁用触发器，以允许执行通常会违反此触发器的插入操作。 然后，使用 `ENABLE TRIGGER` 重新启用触发器。

```sql
CREATE TABLE dbo.trig_example (
  id INT,
  name VARCHAR(12),
  salary MONEY) ;
GO
-- Create the trigger.
CREATE TRIGGER dbo.trig1 ON dbo.trig_example FOR INSERT
AS
IF (SELECT COUNT(*) FROM INSERTED
WHERE salary > 100000) > 0
BEGIN
    print 'TRIG1 Error: you attempted to insert a salary > $100,000'
    ROLLBACK TRANSACTION
END ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (1,'Pat Smith',100001) ;
GO
-- Disable the trigger.
ALTER TABLE dbo.trig_example DISABLE TRIGGER trig1 ;
GO
-- Try an insert that would typically violate the trigger.
INSERT INTO dbo.trig_example VALUES (2,'Chuck Jones',100001) ;
GO
-- Re-enable the trigger.
ALTER TABLE dbo.trig_example ENABLE TRIGGER trig1 ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (3,'Mary Booth',100001) ;
GO
```

### <a name="online-operations"></a><a name="online"></a>联机操作

#### <a name="a-online-index-rebuild-using-low-priority-wait-options"></a>A. 使用低优先级等待选项的联机索引重新生成

下面的示例展示了如何执行指定低优先级等待选项的联机索引重新生成。

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

```sql
ALTER TABLE T1
REBUILD WITH
(
    PAD_INDEX = ON,
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES,
                                         ABORT_AFTER_WAIT = BLOCKERS ) )
) ;
```

#### <a name="b-online-alter-column"></a>B. 联机更改列

下面的示例展示了如何使用 ONLINE 选项运行更改列操作。

**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy
    ALTER COLUMN column_a DECIMAL (5, 2) WITH (ONLINE = ON) ;
GO
sp_help doc_exy;
DROP TABLE dbo.doc_exy ;
GO
```

### <a name="system-versioning"></a><a name="system_versioning"></a>系统版本控制

以下四个示例将帮助你熟悉使用系统版本控制的语法。 如需其他帮助，请参阅[系统版本控制时态表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)。

**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

#### <a name="a-add-system-versioning-to-existing-tables"></a>A. 向现有表添加系统版本控制

以下示例演示如何向现有表添加系统版本控制和创建后续的历史记录表。 此示例假定存在已定义主键的现有表 `InsurancePolicy`。 此示例使用开始时间和结束时间的默认值填充新创建的时间段列，以实现系统版本控制，因为这些值不得为 NULL。 此示例使用 HIDDEN 子句来确保不会影响现有应用程序与当前表之间的交互。 它还使用仅适用于 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 的 HISTORY_RETENTION_PERIOD。

```sql
--Alter non-temporal table to define periods for system versioning
ALTER TABLE InsurancePolicy
ADD PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime),
SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL
    DEFAULT SYSUTCDATETIME(),
SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL
    DEFAULT CONVERT(DATETIME2, '9999-12-31 23:59:59.99999999') ;

--Enable system versioning with 1 year retention for historical data
ALTER TABLE InsurancePolicy
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 1 YEAR)) ;
```

#### <a name="b-migrate-an-existing-solution-to-use-system-versioning"></a>B. 迁移现有解决方案以使用系统版本控制

以下示例演示如何从使用触发器模拟临时支持的解决方案迁移到系统版本控制。 此示例假定现有解决方案使用 `ProjectTask` 表和 `ProjectTaskHistory` 表作为现有解决方案，并使用 `Changed Date` 和 `Revised Date` 列作为时间段，这些时间段列不使用 `datetime2` 数据类型，且 `ProjectTask` 表已定义主键。

```sql
-- Drop existing trigger
DROP TRIGGER ProjectTask_HistoryTrigger;

-- Adjust the schema for current and history table
-- Change data types for existing period columns
ALTER TABLE ProjectTask ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTask ALTER COLUMN [Revised Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Revised Date] datetime2 NOT NULL ;

-- Add SYSTEM_TIME period and set system versioning with linking two existing tables
-- (a certain set of data checks happen in the background)
ALTER TABLE ProjectTask
ADD PERIOD FOR SYSTEM_TIME ([Changed Date], [Revised Date])

ALTER TABLE ProjectTask
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProjectTaskHistory, DATA_CONSISTENCY_CHECK = ON))
```

#### <a name="c-disabling-and-re-enabling-system-versioning-to-change-table-schema"></a>C. 禁用并重新启用系统版本控制以更改表架构

此示例演示如何对 `Department` 表禁用系统版本控制、添加列以及重新启用系统版本控制。 必须先禁用系统版本控制，然后才能修改表架构。 在事务中执行这些步骤可阻止在更新表架构时更新两个表，这样可让 DBA 在重新启用系统版本控制时跳过数据一致性检查，并获得性能优势。 创建统计信息、切换分区或压缩一个或两个表等任务不需要禁用系统版本控制。

```sql
BEGIN TRAN
/* Takes schema lock on both tables */
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
/* expand table schema for temporal table */
ALTER TABLE Department  
     ADD Col5 int NOT NULL DEFAULT 0 ;
/* Expand table schema for history table */
ALTER TABLE DepartmentHistory
    ADD Col5 int NOT NULL DEFAULT 0 ;
/* Re-establish versioning again*/
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE=dbo.DepartmentHistory,
                                 DATA_CONSISTENCY_CHECK = OFF)) ;
COMMIT
```

#### <a name="d-removing-system-versioning"></a>D. 删除系统版本控制

此示例演示如何完全删除 Department 表中的系统版本控制并删除 `DepartmentHistory` 表。 也可以根据需要删除系统用于记录系统版本控制信息的时间段列。 当系统版本控制处于启用状态时，无法删除 `Department` 或 `DepartmentHistory` 表。

```sql
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
ALTER TABLE Department
DROP PERIOD FOR SYSTEM_TIME ;
DROP TABLE DepartmentHistory ;
```

## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

以下 A 到 C 示例均使用 [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)] 数据库中的 `FactResellerSales` 表。

### <a name="a-determining-if-a-table-is-partitioned"></a>A. 确定表是否分区

如果表 `FactResellerSales` 已分区，以下查询将返回一个或多个行。 如果表未分区，则不返回任何行。

```sql
SELECT * FROM sys.partitions AS p
JOIN sys.tables AS t
    ON p.object_id = t.object_id
WHERE p.partition_id IS NOT NULL
    AND t.name = 'FactResellerSales' ;
```

### <a name="b-determining-boundary-values-for-a-partitioned-table"></a>B. 确定已分区表的边界值

以下查询对于 `FactResellerSales` 表中的每个分区返回边界值。

```sql
SELECT t.name AS TableName, i.name AS IndexName, p.partition_number,
    p.partition_id, i.data_space_id, f.function_id, f.type_desc,
    r.boundary_id, r.value AS BoundaryValue
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.partitions AS p
    ON i.object_id = p.object_id AND i.index_id = p.index_id
JOIN  sys.partition_schemes AS s
    ON i.data_space_id = s.data_space_id
JOIN sys.partition_functions AS f
    ON s.function_id = f.function_id
LEFT JOIN sys.partition_range_values AS r
    ON f.function_id = r.function_id and r.boundary_id = p.partition_number
WHERE t.name = 'FactResellerSales' AND i.type <= 1
ORDER BY p.partition_number ;
```

### <a name="c-determining-the-partition-column-for-a-partitioned-table"></a>C. 确定已分区表的分区列

以下查询返回表的分区列的名称。 `FactResellerSales`.

```sql
SELECT t.object_id AS Object_ID, t.name AS TableName,
    ic.column_id as PartitioningColumnID, c.name AS PartitioningColumnName
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.columns AS c
    ON t.object_id = c.object_id
JOIN sys.partition_schemes AS ps
    ON ps.data_space_id = i.data_space_id
JOIN sys.index_columns AS ic
    ON ic.object_id = i.object_id
    AND ic.index_id = i.index_id AND ic.partition_ordinal > 0
WHERE t.name = 'FactResellerSales'
AND i.type <= 1
AND c.column_id = ic.column_id ;
```

### <a name="d-merging-two-partitions"></a>D. 合并两个分区

以下示例合并表上的两个分区。

`Customer` 表具有以下定义：

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100))) ;
```

以下命令合并 10 和 25 分区边界。

```sql
ALTER TABLE Customer MERGE RANGE (10);
```

表的新 DDL 是：

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 25, 50, 100))) ;
```

### <a name="e-splitting-a-partition"></a>E. 拆分分区

以下示例拆分表上的分区。

`Customer` 表具有以下 DDL：

```sql
DROP TABLE Customer;

CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100 ))) ;
```

以下命令创建值为 75（50 到 100 之间）的新分区边界。

```sql
ALTER TABLE Customer SPLIT RANGE (75);
```

表的新 DDL 是：

```sql
CREATE TABLE Customer (
   id INT NOT NULL,
   lastName VARCHAR(20),
   orderCount INT,
   orderDate DATE)
   WITH DISTRIBUTION = HASH(id),
   PARTITION ( orderCount (RANGE LEFT
      FOR VALUES (1, 5, 10, 25, 50, 75, 100))) ;
```

### <a name="f-using-switch-to-move-a-partition-to-a-history-table"></a>F. 使用 SWITCH 将分区移至历史记录表

以下示例将 `Orders` 表的分区中的数据移动到 `OrdersHistory` 表的分区中。

`Orders` 表具有以下 DDL：

```sql
CREATE TABLE Orders (
    id INT,
    city VARCHAR (25),
    lastUpdateDate DATE,
    orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01', '2005-01-01', '2006-01-01', '2007-01-01'))) ;
```

在此示例中，`Orders` 表具有以下分区。 每个分区中均包含数据。

|分区|是否有数据？|边界范围|
|---------------|---------------|--------------------|
|1|是|OrderDate < '2004-01-01'|
|2|是|'2004-01-01' <= OrderDate < '2005-01-01'|
|3|是|'2005-01-01' <= OrderDate< '2006-01-01'|
|4|是|'2006-01-01'<= OrderDate < '2007-01-01'|
|5|是|'2007-01-01' <= OrderDate|
| &nbsp; | &nbsp; | &nbsp; |

- 分区 1（有数据）：OrderDate < '2004-01-01'
- 分区 2（有数据）：'2004-01-01' <= OrderDate < '2005-01-01'
- 分区 3（有数据）：'2005-01-01' <= OrderDate< '2006-01-01'
- 分区 4（有数据）：'2006-01-01'<= OrderDate < '2007-01-01'
- 分区 5（有数据）：'2007-01-01' <= OrderDate

`OrdersHistory` 表具有以下 DDL，其列和列名称与 `Orders` 表一致。 两者在 `id` 列上都是哈希分布式。

```sql
CREATE TABLE OrdersHistory (
   id INT,
   city VARCHAR (25),
   lastUpdateDate DATE,
   orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01'))) ;
```

虽然列和列名必须相同，但分区边界不必相同。 在此示例中，`OrdersHistory` 表具有以下两个分区，并且这两个分区都为空：

- 分区 1（无数据）：OrderDate < '2004-01-01'
- 分区 2（空）：'2004-01-01' <= OrderDate

对于上述两个表，以下命令将具有 `OrderDate < '2004-01-01'` 的所有行从 `Orders` 表移到 `OrdersHistory` 表。

```sql
ALTER TABLE Orders SWITCH PARTITION 1 TO OrdersHistory PARTITION 1;
```

因此，`Orders` 中的第一个分区为空，`OrdersHistory` 中的第一个分区中包含数据。 现在的表如下所示：

 `Orders` 表

- 分区 1（空）：OrderDate < '2004-01-01'
- 分区 2（有数据）：'2004-01-01' <= OrderDate < '2005-01-01'
- 分区 3（有数据）：'2005-01-01' <= OrderDate< '2006-01-01'
- 分区 4（有数据）：'2006-01-01'<= OrderDate < '2007-01-01'
- 分区 5（有数据）：'2007-01-01' <= OrderDate

`OrdersHistory` 表

- 分区 1（有数据）：OrderDate < '2004-01-01'
- 分区 2（空）：'2004-01-01' <= OrderDate

若要清除 `Orders` 表，可通过合并分区 1 和 2 来删除空的分区，如下所示：

```sql
ALTER TABLE Orders MERGE RANGE ('2004-01-01');
```

合并后，`Orders` 表具有以下分区：

`Orders` 表

- 分区 1（有数据）：OrderDate < '2005-01-01'
- 分区 2（有数据）：'2005-01-01' <= OrderDate< '2006-01-01'
- 分区 3（有数据）：'2006-01-01'<= OrderDate < '2007-01-01'
- 分区 4（有数据）：'2007-01-01' <= OrderDate

假设又过去了一年，你准备存档 2005 年的数据。 可以通过拆分空分区为 `OrdersHistory` 表中的 2005 年分配空的分区，如下所示：

```sql
ALTER TABLE OrdersHistory SPLIT RANGE ('2005-01-01');
```

拆分后，`OrdersHistory` 表具有以下分区：

 `OrdersHistory` 表

- 分区 1（有数据）：OrderDate < '2004-01-01'
- 分区 2（空）：'2004-01-01' < '2005-01-01'
- 分区 3（空）：'2005-01-01' <= OrderDate

## <a name="see-also"></a>另请参阅

- [sys.tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md)
- [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
