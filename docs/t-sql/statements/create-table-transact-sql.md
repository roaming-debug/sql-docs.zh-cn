---
description: CREATE TABLE (Transact-SQL)
title: CREATE TABLE (Transact-SQL)
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FILESTREAM_TSQL
- TABLE
- CREATE_TABLE_TSQL
- CREATE TABLE
- FILESTREAM
- TABLE_TSQL
- FILESTREAM_ON
- FILESTREAM_ON_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHECK constraints
- global temporary tables [SQL Server]
- local temporary tables [SQL Server]
- size [SQL Server], tables
- UNIQUE constraints [SQL Server], creating
- constraints [SQL Server], columns
- maximum number of indexes per table
- number of tables per database
- number of indexes per table
- table creation [SQL Server], CREATE TABLE
- temporary tables [SQL Server], creating
- table size [SQL Server]
- nullability [SQL Server]
- partitioned tables [SQL Server], creating
- DEFAULT definition
- null values [SQL Server], columns
- number of rows per table
- maximum number of columns per table
- FOREIGN KEY constraints, creating
- PRIMARY KEY constraints
- number of bytes per row
- CREATE TABLE statement
- number of columns per table
- maximum number of bytes per row
- data retention policy
ms.assetid: 1e068443-b9ea-486a-804f-ce7b6e048e8b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 64992e05c67f9fc3f401b1a8074295dad589a74e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170359"
---
# <a name="create-table-transact-sql"></a>CREATE TABLE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中创建新表。

> [!NOTE]
> 有关 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 语法，请参阅 [CREATE TABLE ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/statements/create-table-azure-sql-data-warehouse.md)。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="simple-syntax"></a>简单语法

```syntaxsql
-- Simple CREATE TABLE Syntax (common if not using options)
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition> } [ ,...n ] )
[ ; ]
```

## <a name="full-syntax"></a>完整语法

```syntaxsql
-- Disk-Based CREATE TABLE Syntax
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ AS FileTable ]
    ( {   <column_definition>
        | <computed_column_definition>
        | <column_set_definition>
        | [ <table_constraint> ] [ ,... n ]
        | [ <table_index> ] }
          [ ,...n ]
          [ PERIOD FOR SYSTEM_TIME ( system_start_time_column_name
             , system_end_time_column_name ) ]
      )
    [ ON { partition_scheme_name ( partition_column_name )
           | filegroup
           | "default" } ]
    [ TEXTIMAGE_ON { filegroup | "default" } ]
    [ FILESTREAM_ON { partition_scheme_name
           | filegroup
           | "default" } ]
    [ WITH ( <table_option> [ ,...n ] ) ]
[ ; ]
  
<column_definition> ::=
column_name <data_type>
    [ FILESTREAM ]
    [ COLLATE collation_name ]
    [ SPARSE ]
    [ MASKED WITH ( FUNCTION = ' mask_function ') ]
    [ [ CONSTRAINT constraint_name ] DEFAULT constant_expression ]
    [ IDENTITY [ ( seed,increment ) ]
    [ NOT FOR REPLICATION ]
    [ GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] ]
    [ NULL | NOT NULL ]
    [ ROWGUIDCOL ]
    [ ENCRYPTED WITH
        ( COLUMN_ENCRYPTION_KEY = key_name ,
          ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,
          ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ) ]
    [ <column_constraint> [, ...n ] ]
    [ <column_index> ]
  
<data_type> ::=
[ type_schema_name . ] type_name
    [ ( precision [ , scale ] | max |
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]
  
<column_constraint> ::=
[ CONSTRAINT constraint_name ]
{     { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        [
            WITH FILLFACTOR = fillfactor
          | WITH ( < index_option > [ , ...n ] )
        ]
        [ ON { partition_scheme_name ( partition_column_name )
            | filegroup | "default" } ]
  
  | [ FOREIGN KEY ]
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
  
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
}
  
<column_index> ::=
 INDEX index_name [ CLUSTERED | NONCLUSTERED ]
    [ WITH ( <index_option> [ ,... n ] ) ]
    [ ON { partition_scheme_name (column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
<computed_column_definition> ::=
column_name AS computed_column_expression
[ PERSISTED [ NOT NULL ] ]
[
    [ CONSTRAINT constraint_name ]
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        [
            WITH FILLFACTOR = fillfactor
          | WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name ( partition_column_name )
        | filegroup | "default" } ]
  
    | [ FOREIGN KEY ]
        REFERENCES referenced_table_name [ ( ref_column ) ]
        [ ON DELETE { NO ACTION | CASCADE } ]
        [ ON UPDATE { NO ACTION } ]
        [ NOT FOR REPLICATION ]
  
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
]
  
<column_set_definition> ::=
column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS
  
< table_constraint > ::=
[ CONSTRAINT constraint_name ]
{
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        (column [ ASC | DESC ] [ ,...n ] )
        [
            WITH FILLFACTOR = fillfactor
           |WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name (partition_column_name)
            | filegroup | "default" } ]
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )

< table_index > ::=
{  
    {  
      INDEX index_name  [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ]
         (column_name [ ASC | DESC ] [ ,... n ] )
    | INDEX index_name CLUSTERED COLUMNSTORE
    | INDEX index_name [ NONCLUSTERED ] COLUMNSTORE (column_name [ ,... n ] )
    }
    [ WITH ( <index_option> [ ,... n ] ) ]
    [ ON { partition_scheme_name (column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
}

<table_option> ::=
{  
    [DATA_COMPRESSION = { NONE | ROW | PAGE }
      [ ON PARTITIONS ( { <partition_number_expression> | <range> }
      [ , ...n ] ) ]]
    [ FILETABLE_DIRECTORY = <directory_name> ]
    [ FILETABLE_COLLATE_FILENAME = { <collation_name> | database_default } ]
    [ FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME = <constraint_name> ]
    [ FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME = <constraint_name> ]
    [ FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME = <constraint_name> ]
    [ SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name . history_table_name
        [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ] ]
    [ REMOTE_DATA_ARCHIVE =
      {
          ON [ ( <table_stretch_options> [,...n] ) ]
        | OFF ( MIGRATION_STATE = PAUSED )
      }
    ]   
    [ DATA_DELETION = ON  
          {( 
             FILTER_COLUMN = column_name,   
             RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                              | MONTH | MONTHS | YEAR | YEARS }
        )}  
     ]
}
  
<table_stretch_options> ::=
{  
    [ FILTER_PREDICATE = { null | table_predicate_function } , ]
      MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
 }   
  
<index_option> ::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | STATISTICS_INCREMENTAL = { ON | OFF }
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS = { ON | OFF }
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
  | COMPRESSION_DELAY= {0 | delay [Minutes]}
  | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }
       [ ON PARTITIONS ( { <partition_number_expression> | <range> }
       [ , ...n ] ) ]
}
<range> ::=
<partition_number_expression> TO <partition_number_expression>
```

```
-- Memory optimized CREATE TABLE Syntax
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition>
    | [ <table_constraint> ] [ ,... n ]
    | [ <table_index> ]
      [ ,... n ] }
      [ PERIOD FOR SYSTEM_TIME ( system_start_time_column_name
        , system_end_time_column_name ) ]
)
    [ WITH ( <table_option> [ ,... n ] ) ]
 [ ; ]

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] ]
    [ NULL | NOT NULL ]
[
    [ CONSTRAINT constraint_name ] DEFAULT memory_optimized_constant_expression ]
    | [ IDENTITY [ ( 1, 1 ) ]
]
    [ <column_constraint> ]
    [ <column_index> ]
  
<data_type> ::=
 [type_schema_name . ] type_name [ (precision [ , scale ]) ]

<column_constraint> ::=
 [ CONSTRAINT constraint_name ]
{
  { PRIMARY KEY | UNIQUE }
      {   NONCLUSTERED
        | NONCLUSTERED HASH WITH (BUCKET_COUNT = bucket_count)
      }
  | [ FOREIGN KEY ]
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]
  | CHECK ( logical_expression )
}
  
< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   { PRIMARY KEY | UNIQUE }
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
{   [ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
  
}
  
<table_option> ::=
{  
    MEMORY_OPTIMIZED = ON
  | DURABILITY = {SCHEMA_ONLY | SCHEMA_AND_DATA}
  | SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name . history_table_name
        [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ]
  
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

database_name 要在其中创建表的数据库的名称。 database_name 须指定现有数据库的名称。 如果未指定，则 database_name 默认为当前数据库。 当前连接的登录名必须与 database_name 所指定数据库中的一个现有用户 ID 关联，并且该用户 ID 必须具有 CREATE TABLE 权限。

schema_name 新表所属架构的名称。

table_name 新表的名称。 表名必须遵循有关[标识符](../../relational-databases/databases/database-identifiers.md)的规则。 除了本地临时表名（以单个数字符号 (#) 为前缀的名称）不能超过 116 个字符外，table_name 最多可包含 128 个字符。

**适用于**（AS FileTable）：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）。

将新表创建为 FileTable。 您无需指定列，因为 FileTable 具有固定架构。 有关详细信息，请参阅 [FileTables](../../relational-databases/blob/filetables-sql-server.md)。

column_name
computed_column_expression 定义计算列的值的表达式。 计算列是虚拟列，并非实际存储在表中，除非此列标记为 PERSISTED。 该列由同一表中的其他列通过表达式计算得到。 例如，计算列可以定义为：cost AS price \* qty  。表达式可以是非计算列的名称、常量、函数、变量以及通过一个或多个运算符连接的上述元素的任意组合。 表达式不能是子查询，也不能包含别名数据类型。

计算列可用于选择列表、WHERE 子句、ORDER BY 子句或任何可使用正则表达式的其他位置，但下列情况除外：

- 计算列必须标记为 PERSISTED，才能参与 FOREIGN KEY 或 CHECK 约束。
- 如果计算列的值由具有确定性的表达式定义，并且索引列中可使用计算结果的数据类型，则可将该列用作索引中的键列，或者用作 PRIMARY KEY 或 UNIQUE 约束的一部分。

   例如，如果表中含有整数列 a 和 b，则可以对计算列 a+b 创建索引，但不能对计算列 a+DATEPART(dd, GETDATE()) 创建索引，因为在以后的调用中，其值可能发生改变   。

- 计算列不能作为 INSERT 或 UPDATE 语句的目标。

> [!NOTE]
> 表中计算列所使用的列值因行而异，因此计算列的每一行可能有不同的值。

计算列的为 Null 性是由[!INCLUDE[ssDE](../../includes/ssde-md.md)]根据使用的表达式自动确定的。 即使只有不可为空的列，大多数表达式的结果也认为是可为空的，因为可能的下溢或溢出也将生成 NULL 结果。 使用带 AllowsNull 属性的 `COLUMNPROPERTY` 函数以调查表中任何计算列的为 Null 性。 通过使用 check_expression 常量指定 `ISNULL`（常量是替换所有 NULL 结果的非空值），可以将可为 Null 的表达式转换为不可为 Null 的表达式。 对于基于公共语言运行时 (CLR) 用户定义类型表达式的计算列，需要对此类型有 REFERENCES 权限。

PERSISTED 指定[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]将在表中物理存储计算值，并在计算列依赖的任何其他列发生更新时对这些计算值进行更新。 将计算列标记为 `PERSISTED`可以对具有确定性但不精确的计算列创建索引。 有关详细信息，请参阅 [计算列上的索引](../../relational-databases/indexes/indexes-on-computed-columns.md)。 必须将用作已分区表的分区依据列的任何计算列显式标记为 `PERSISTED`。 指定 `PERSISTED` 时，computed_column_expression 必须具有确定性。

ON { partition_scheme | filegroup | "default" } 指定存储表的分区架构或文件组 。 如果指定了 partition_scheme，则该表将成为已分区表，其分区存储在 partition_scheme 所指定的一个或多个文件组的集合中 。 如果指定了 filegroup，则该表将存储在已命名文件组中。 数据库中必须存在该文件组。 如果指定了 "default"，或者根本未指定 ON，表则存储在默认文件组中。 CREATE TABLE 中指定的表的存储机制以后不能进行更改。

ON {partition_scheme | filegroup | "default"} 也可以在 PRIMARY KEY 约束或 UNIQUE 约束中指定 。 这些约束会创建索引。 如果 filegroup 未指定，则索引会存储在已命名文件组中。 如果指定了 "default"，或者根本未指定 ON，索引则将与表存储在同一文件组中。 如果 `PRIMARY KEY` 或 `UNIQUE` 约束创建聚集索引，则表的数据页将与索引存储在同一文件组中。 如果指定了 `CLUSTERED` 或约束另外创建了聚集索引，并且指定的 partition_scheme 不同于表定义的 partition_scheme 或 filegroup 或相反，则只接受约束定义，而忽略其他定义  。

> [!NOTE]
> 在此上下文中，default 不是关键字。 它是默认文件组的标识符，并且必须进行分隔（类似于 ON "default" 或 ON [default]）  。 如果指定了 "default"，当前会话的 `QUOTED_IDENTIFIER` 选项则必须为 ON。 这是默认设置。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。
>
> 在创建分区表后，请考虑将表的 `LOCK_ESCALATION` 选项设置为 `AUTO`。 这可通过将锁升级到分区 (HoBT) 级而不是表级来改善并发性。 有关详细信息，请参阅 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)。

TEXTIMAGE_ON { filegroup"default" } 指示 text、ntext、image、xml、varchar(max)、nvarchar(max)、varbinary(max) 和 CLR 用户定义类型的列（包括几何图形和地理）存储在指定文件组|        。

如果表中没有较大值列，则不允许使用 `TEXTIMAGE_ON`。 如果指定了 partition_scheme，则不能指定 `TEXTIMAGE_ON`。 如果指定了 "default"，或者根本未指定 `TEXTIMAGE_ON`，较大值列则将存储在默认文件组中。 以后不能对 `CREATE TABLE` 中指定的任何较大值列数据的存储进行更改。

> [!NOTE]
> Varchar(max)、nvarchar(max)、varbinary(max)、xml 和大型 UDT 值直接存储在数据行中（最大限制为 8,000 个字节，只要记录中可以容纳此值）。 如果记录中容纳不下该值，则指针存储在行内，其余内容存储在 LOB 存储空间内的行外。 0 是默认值，表示所有值都直接存储在数据行中。
>
> `TEXTIMAGE_ON` 仅更改“LOB 存储空间”的位置，不影响数据存储在行内的时间。 使用 sp_tableoption 的 large value types out of row 选项将整个 LOB 值存储在行外。
>
> 在此上下文中，default 不是关键字。 它是默认文件组的标识符，并且必须进行分隔（类似于 `TEXTIMAGE_ON "default"` 或 `TEXTIMAGE_ON [default]`）。 如果指定了 "default"，当前会话的 `QUOTED_IDENTIFIER` 选项则必须为 ON。 这是默认设置。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。

FILESTREAM_ON { partition_scheme_name | filegroup | "default" } 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 及更高版本）  。 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 不支持 `FILESTREAM`。

指定 FILESTREAM 数据的文件组。

如果表包含 FILESTREAM 数据并且已分区，则必须包含 FILESTREAM_ON 子句并指定 FILESTREAM 文件组的分区方案。 此分区方案必须使用与表分区方案相同的分区函数和分区列；否则，将引发错误。

如果该表未分区，则无法对 FILESTREAM 列分区。 表的 FILESTREAM 数据必须存储在单个文件组中。 此文件组是在 FILESTREAM_ON 子句中指定的。

如果表未分区并且未指定 `FILESTREAM_ON` 子句，则使用设置了 `DEFAULT` 属性的 FILESTREAM 文件组。 如果没有 FILESTREAM 文件组，将引发错误。

与 ON 和 `TEXTIMAGE_ON` 一样，无法更改通过使用 `CREATE TABLE` 为 `FILESTREAM_ON` 设置的值，但以下情况除外：

- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) 语句将堆转换为聚集索引。 在这种情况下，可以指定不同的 FILESTREAM 文件组、分区方案或 NULL。
- [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md) 语句将聚集索引转换为堆。 在这种情况下，可以指定不同的 FILESTREAM 文件组、分区方案或 "default"。

`FILESTREAM_ON <filegroup>` 子句中的文件组或在分区方案中指定的每个 FILESTREAM 文件组须定义有一个文件。 须使用 [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) 或 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 语句来定义此文件；否则，会引发错误。

有关 FILESTREAM 相关话题，请参阅 [二进制大型对象 - Blob 数据](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)。

[ _type\_schema\_name_ **.** ] type_name 指定列的数据类型以及该列所属的架构。 对于基于磁盘的表，可以为以下数据类型之一：

- 系统数据类型
- 基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型的别名类型。 必须首先用 `CREATE TYPE` 语句创建别名数据类型，然后才能将它们用于表定义中。 在 `CREATE TABLE` 语句中，可以覆盖别名数据类型的 NULL 或 NOT NULL 赋值。 但是，长度规格不能更改；不能在 `CREATE TABLE` 语句中指定别名数据类型的长度。
- CLR 用户定义类型。 必须首先用 `CREATE TYPE` 语句创建 CLR 用户定义类型，然后才能将它们用于表定义中。 若要创建 CLR 用户定义类型的列，则需要对此类型具有 REFERENCES 权限。

如果 type_schema_name 未指定，则 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 按照下列顺序引用 type_name ：

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型。
- 当前数据库中当前用户的默认架构。
- 当前数据库中的 **dbo** 架构。

有关内存优化表的信息，请参阅 [In-Memory OLTP 的受支持数据类型](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md)，获取受支持系统类型。

precision 指定的数据类型的精度。 有关有效精度值的详细信息，请参阅[精度、小数位数和长度](../../t-sql/data-types/precision-scale-and-length-transact-sql.md)。

scale 指定的数据类型的小数位数。 有关有效小数位数值的详细信息，请参阅[精度、小数位数和长度](../../t-sql/data-types/precision-scale-and-length-transact-sql.md)。

max 仅应用于 varchar、nvarchar 和 varbinary 数据类型，存储 2^31 个字节的字符、二进制数据以及 2^30 个字节的 Unicode 数据。

CONTENT 指定 column_name 中 xml 数据类型的每个实例都可包含多个顶级元素。 CONTENT 仅适用于 xml 数据类型，并且只有在同时指定了 xml_schema_collection 时才能指定 CONTENT。 如果未指定，则 CONTENT 为默认行为。

DOCUMENT 指定 column_name 中 xml 数据类型的每个实例仅可包含一个顶级元素。 DOCUMENT 仅适用于 xml 数据类型，并且只有在同时指定了 xml_schema_collection 时才能指定 DOCUMENT。

xml_schema_collection 仅适用于 xml 数据类型，用于将 XML 架构集合与该类型相关联。 在架构中键入 xml 列之前，须先使用 [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) 在数据库中创建该架构。

DEFAULT 如果在插入过程中未显式提供值，则指定为列提供的值。 DEFAULT 定义可适用于除定义为 timestamp 或带 `IDENTITY` 属性的列以外的任何列。 如果为用户定义类型列指定了默认值，则该类型应当支持从 constant_expression 到用户定义类型的隐式转换。 删除表时，将删除 DEFAULT 定义。 只有常量值（例如字符串）、标量函数（系统函数、用户定义函数或 CLR 函数）或 NULL 可用作默认值。 为了与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本兼容，可以为 DEFAULT 分配约束名称。

constant_expression 是用作列的默认值的常量、NULL 或系统函数。

memory_optimized_constant_expression 是一个常量、NULL 或一个支持用作列默认值的系统函数。 本机编译的存储过程中必须支持它。 有关本机编译已存储进程中内置函数的详细信息，请参阅[本机编译 T-SQL 模块的受支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)。

IDENTITY 指示新列是标识列。 在表中添加新行时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将为该列提供一个唯一的增量值。 标识列通常与 PRIMARY KEY 约束一起用作表的唯一行标识符。 可以将 `IDENTITY` 属性分配给 tinyint、smallint、int、bigint、decimal(p,0) 或 numeric(p,0) 列     。 每个表只能创建一个标识列。 不能对标识列使用绑定默认值和 DEFAULT 约束。 必须同时指定种子和增量，或者两者都不指定。 如果二者都未指定，则取默认值 (1,1)。

seed 是加载到表中的第一行所使用的值。

increment 是向加载的前一行的标识值中添加的增量值。

NOT FOR REPLICATION 在 `CREATE TABLE` 语句中，可以为 IDENTITY 属性、FOREIGN KEY 约束和 CHECK 约束指定 `NOT FOR REPLICATION` 子句。 如果为 `IDENTITY` 属性指定了此子句，复制代理执行插入时，标识列中的值将不会增加。 如果为约束指定了此子句，则当复制代理执行插入、更新或删除操作时，将不会强制执行此约束。

GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] [ NOT NULL ] 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定系统将使用指定的 `datetime2` 列来记录记录有效的开始时间或记录有效的结束时间。 此列必须定义为 `NOT NULL`。 如果尝试将此列定义为 `NULL`，系统将引发错误。 如果不为期间列显式指定 NOT NULL，系统则会将此列默认定义为 `NOT NULL`。 将此参数与 `PERIOD FOR SYSTEM_TIME` 和 `WITH SYSTEM_VERSIONING = ON` 参数结合使用来对表启用系统版本控制。 有关详细信息，请参阅 [Temporal Tables](../../relational-databases/tables/temporal-tables.md)。

可将一个或两个时间段列标记为 HIDDEN 标志，隐式隐藏这些列，这样 SELECT \* FROM`<table>` 就不会返回这些列中的值 。 默认情况下，时间段列不会处于隐藏状态。 若要使用隐藏的列，则它必须显式包含在直接引用时态表的所有查询中。 若要更改现有时间段列的 HIDDEN 特性，须先删除 PERIOD，再使用不同的隐藏标志重新创建 。

INDEX index_name [ CLUSTERED | NONCLUSTERED ] (column_name [ ASC | DESC ] [ ,... n ] ) 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  。

指定在表上创建索引。 这可以是聚集索引，也可以是非聚集索引。 该索引包含列出的列，并按照升序或降序对数据进行排序。

INDEX index_name CLUSTERED COLUMNSTORE 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定使用聚集列存储以分列格式存储整个表格。 此操作包含表中的所有列。 数据不按字母或数字顺序排序，因为行是按照可获得列存储压缩好处的原则而组织的。

INDEX index_name [ NONCLUSTERED ] COLUMNSTORE (column_name [ ,... n ] ) 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  。

指定在表中创建非聚集列存储索引。 基础表可以是行存储堆或聚集索引，也可以是聚集列存储索引。 在任何情况下，可在表上创建非聚集列存储索引将这些列的数据的第二个副本存储在索引中。

将非聚集列存储索引作为聚集列存储索引进行存储和管理。 称其为非聚集列存储索引，是因为这些列可能是有限的，且作为表的二级索引存在。

ON partition\_scheme\_name(column\_name) 指定分区方案，该方案定义要将分区索引的分区映射到的文件组。 须通过执行 [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) 或 [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md)，使数据库中存在该分区方案。 column_name 指定对已分区索引进行分区所依据的列。 该列必须与 partition_scheme_name 使用的分区函数参数的数据类型、长度和精度相匹配。 column_name 不限于索引定义中的列。 除了在对 UNIQUE 索引分区时，必须从用作唯一键的列中选择 column_name 外，还可以指定基表中的任何列。 通过此限制，[!INCLUDE[ssDE](../../includes/ssde-md.md)]可验证单个分区中的键值唯一性。

> [!NOTE]
> 在对非唯一的聚集索引进行分区时，如果尚未指定分区依据列，则默认情况下[!INCLUDE[ssDE](../../includes/ssde-md.md)]将在聚集索引键列表中添加分区依据列。 在对非唯一的非聚集索引进行分区时，如果尚未指定分区依据列，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]会添加分区依据列作为索引的非键（包含）列。

如果未指定 partition_scheme_name 或 filegroup 且该表已分区，则索引会与基础表使用相同分区依据列并被放入同一分区方案中 。

> [!NOTE]
> 您不能对 XML 索引指定分区方案。 如果基表已分区，则 XML 索引与该表使用相同的分区方案。

有关分区索引的详细信息，请参阅[已分区表和已分区索引](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。

ON filegroup_name 为指定文件组创建指定索引。 如果未指定位置且表或视图尚未分区，则索引将与基础表或视图使用相同的文件组。 该文件组必须已存在。

ON "default" 为默认文件组创建指定索引。

在此上下文中，“default”一词不是关键字。 它是默认文件组的标识符，并且必须进行分隔（类似于 ON "default" 或 ON [default]） 。 如果指定了 "default"，当前会话的 `QUOTED_IDENTIFIER` 选项则必须为 ON。 这是默认设置。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。

[ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ] 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 及更高版本） 。

在创建聚集索引时，指定表的 FILESTREAM 数据的位置。 FILESTREAM_ON 子句用于将 FILESTREAM 数据移动到不同的 FILESTREAM 文件组或分区方案。

filestream_filegroup_name 是 FILESTREAM 文件组的名称。 该文件组须包含一个使用 [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) 或 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 语句为该文件组定义的文件；否则，会引发错误。

如果表已分区，则必须包含 `FILESTREAM_ON` 子句并且必须指定 FILESTREAM 文件组的分区方案，此分区方案需使用与此表分区方案相同的分区功能和分区列。 否则将引发错误。

如果该表未分区，则无法对 FILESTREAM 列分区。 此表的 FILESTREAM 数据必须存储在一个由 `FILESTREAM_ON` 子句指定的文件组中。

如果正在创建一个聚集索引并且此表不包含 FILESTREAM 列，则可以在 `CREATE INDEX` 语句中指定 `FILESTREAM_ON NULL`。

有关详细信息，请参阅 [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md)。

ROWGUIDCOL 指示新列为行 GUID 列。 对于每个表，只能将其中的一个 uniqueidentifier 列指定为 ROWGUIDCOL 列。 应用 ROWGUIDCOL 属性能够实现通过使用 `$ROWGUID` 引用列。 ROWGUIDCOL 属性只能分配给 uniqueidentifier 列。 用户定义数据类型列不能使用 ROWGUIDCOL 指定。

ROWGUIDCOL 属性并不强制列中所存储值的唯一性。 ROWGUIDCOL 也不会为插入表的新行自动生成值。 若要为每列生成唯一值，请使用 [INSERT](../../t-sql/statements/insert-transact-sql.md) 语句上的 [NEWID](../../t-sql/functions/newid-transact-sql.md) 或 [NEWSEQUENTIALID](../../t-sql/functions/newsequentialid-transact-sql.md) 函数，或将这些函数用作该列的默认值。

ENCRYPTED WITH 使用 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) 功能指定加密列。

COLUMN_ENCRYPTION_KEY = key_name 指定列加密密钥。 有关详细信息，请参阅 [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md)。

ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } 确定性加密对任何给定的纯文本值始终生成相同的加密值。 使用确定性加密可基于加密值进行下列操作：使用相等性比较进行搜索、分组、使用相等性联接联接各表，此外，还允许未经授权的用户通过检查加密列中的模式来猜测加密值的相关信息。 只有使用相同的列加密密钥对两列进行加密，才能在已进行确定性加密的该列上联结两个表。 确定性加密必须使用具有字符列的 binary2 排序顺序的列排序规则。

**随机加密** 使用一种以更不可预测地方式加密数据的方法。 随机加密更为安全，但会阻止对加密列进行任何计算和索引编制，除非你的 SQL Server 实例支持具有安全 enclave 的 Always Encrypted。 请参阅[具有安全 Enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md) 以了解详细信息。

如果使用的是 Always Encrypted（不带安全 enclave），请对要使用参数或分组参数搜索的列使用确定性加密，例如政府 ID 号。 对以下数据使用随机加密，即未与其他记录分组的数据、未用于联结表的数据、因使用其他列（例如事务号）查找包含已加密的兴趣列，而不用于搜索的数据。

如果使用的是具有安全 enclave 的 Always Encrypted，则随机加密是推荐的加密类型。

列必须是符合条件的数据类型。

ALGORITHM 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本）。

须是“AEAD_AES_256_CBC_HMAC_SHA_256”。

有关包括功能约束在内的详细信息，请参阅 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)。

SPARSE 指示列为稀疏列。 稀疏列已针对 NULL 值进行了存储优化。 不能将稀疏列指定为 NOT NULL。 有关稀疏列的其他限制和详细信息，请参阅[使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)。

ADD MASKED WITH ( FUNCTION = ' mask_function ') 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本）。

指定动态数据掩码。 mask_function 是具有相应参数的掩码函数的名称。 有四个函数可供选择：

- default()
- email()
- partial()
- random()

有关函数参数的信息，请参阅[动态数据掩码](../../relational-databases/security/dynamic-data-masking.md)。

FILESTREAM 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 及更高版本）

仅对 varbinary(max) 列有效。 请为 varbinary(max) BLOB 数据指定 FILESTREAM 存储。

表中还必须包含一个具有 ROWGUIDCOL 特性的 uniqueidentifier 数据类型列。 此列不得为空值且必须具有 UNIQUE 或 PRIMARY KEY 单列约束。 该列的 GUID 值可由应用程序在插入数据时提供，也可由使用 NEWID () 函数的 DEFAULT 约束提供。

如果为表定义了 FILESTREAM 列，则不能删除 ROWGUIDCOL 列并且不能更改相关的约束。 仅当删除了最后一个 FILESTREAM 列后，才能删除 ROWGUIDCOL 列。

当为某个列指定了 FILESTREAM 存储属性时，该列的所有值都将存储在文件系统上的 FILESTREAM 数据容器中。

COLLATE collation_name 指定列的排序规则。 排序规则名称既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称。 collation_name 仅适用于 char、varchar、text、nchar、nvarchar 和 ntext 数据类型列     。 如果没有指定该参数，则该列的排序规则是用户定义数据类型的排序规则（如果列为用户定义数据类型）或数据库的默认排序规则。

有关 Windows 和 SQL 排序规则名称的详细信息，请参阅 [Windows 排序规则名称](../../t-sql/statements/windows-collation-name-transact-sql.md)和 [SQL 排序规则名称](../../t-sql/statements/sql-server-collation-name-transact-sql.md)。

有关详细信息，请参阅 [COLLATE](~/t-sql/statements/collations.md)。

CONSTRAINT 可选关键字，表示 PRIMARY KEY、NOT NULL、UNIQUE、FOREIGN KEY 或 CHECK 约束定义的开始。

constraint_name 是约束的名称。 约束名称必须在表所属的架构中唯一。

NULL | NOT NULL 确定列中是否允许使用空值。 严格来讲，NULL 不是约束，但可以像指定 NOT NULL 那样指定它。 只有同时指定了 PERSISTED 时，才能为计算列指定 NOT NULL。

PRIMARY KEY 是通过唯一索引对给定的一列或多列强制实体完整性的约束。 每个表只能创建一个 PRIMARY KEY 约束。

UNIQUE 一个约束，该约束通过唯一索引为一个或多个指定列提供实体完整性。 一个表可以有多个 UNIQUE 约束。

CLUSTERED | NONCLUSTERED 指示为 PRIMARY KEY 或 UNIQUE 约束创建聚集索引还是非聚集索引。 PRIMARY KEY 约束默认为 CLUSTERED，UNIQUE 约束默认为 NONCLUSTERED。

在 `CREATE TABLE` 语句中，可以只为一个约束指定 CLUSTERED。 如果在为 UNIQUE 约束指定 CLUSTERED 的同时又指定了 PRIMARY KEY 约束，则 PRIMARY KEY 将默认为 NONCLUSTERED。

FOREIGN KEY REFERENCES 为列中的数据提供引用完整性的约束。 FOREIGN KEY 约束要求列中的每个值在所引用的表中对应的被引用列中都存在。 FOREIGN KEY 约束只能引用在所引用的表中是 PRIMARY KEY 或 UNIQUE 约束的列，或所引用的表中在 UNIQUE INDEX 内的被引用列。 计算列上的外键也必须标记为 PERSISTED。

[ schema\_name.] referenced_table_name] 是 FOREIGN KEY 约束引用的表的名称，以及该表所属架构的名称。

( ref_column [ ,... n ] ) 是 FOREIGN KEY 约束所引用的表中的一列或多列。

ON DELETE { NO ACTION \| CASCADE \| SET NULL \| SET DEFAULT } 指定如果已创建表中的行具有引用关系，并且被引用行已从父表中删除，则对这些行采取的操作。 默认值为 NO ACTION。

NO ACTION [!INCLUDE[ssDE](../../includes/ssde-md.md)]将引发错误，并回滚对父表中行的删除操作。

CASCADE 如果从父表中删除一行，则将从引用表中删除相应行。

SET NULL 如果父表中对应的行被删除，则组成外键的所有值都将设置为 NULL。 若要执行此约束，外键列必须可为空值。

SET DEFAULT 如果父表中对应的行被删除，则组成外键的所有值都将设置为默认值。 若要执行此约束，所有外键列都必须有默认定义。 如果某个列可为空值，并且未设置显式的默认值，则将使用 NULL 作为该列的隐式默认值。

如果该表将包含在使用逻辑记录的合并发布中，则不要指定 CASCADE。 有关逻辑记录的详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。

如果表中已存在 `INSTEAD OF` 触发器 `ON DELETE`，则无法定义 `ON DELETE CASCADE`。

例如，在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中，ProductVendor 表与 Vendor 表有引用关系 。 ProductVendor.BusinessEntityID 外键引用 Vendor.BusinessEntityID 主键 。

如果对 Vendor 表中的某行执行 `DELETE` 语句，并且为 ProductVendor.BusinessEntityID 指定 `ON DELETE CASCADE` 操作，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将检查 ProductVendor 表中的一个或多个依赖行  。 如果存在依赖行，则 ProductVendor 表中的依赖行将随 Vendor 表中的被引用行一同删除 。

相反，如果指定了 `NO ACTION`，并且 ProductVendor 表中至少有一行引用了 Vendor 行，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 会引发错误并回滚对 Vendor 进行行的删除操作 。

ON UPDATE { NO ACTION \| CASCADE \ SET NULL \| SET DEFAULT } 指定在发生更改的表中，如果行有引用关系且引用的行在父表中被更新，则对这些行采取什么操作。 默认值为 NO ACTION。

NO ACTION [!INCLUDE[ssDE](../../includes/ssde-md.md)]将引发错误，并回滚对父表中相应行的更新操作。

CASCADE 如果在父表中更新了一行，则将在引用表中更新相应的行。

SET NULL 如果更新了父表中的相应行，则会将构成外键的所有值设置为 NULL。 若要执行此约束，外键列必须可为空值。

SET DEFAULT 如果更新了父表中的相应行，则会将构成外键的所有值都设置为其默认值。 若要执行此约束，所有外键列都必须有默认定义。 如果某个列可为空值，并且未设置显式的默认值，则将使用 NULL 作为该列的隐式默认值。

如果此表将包含在使用逻辑记录的合并发布中，则不要指定 `CASCADE`。 有关逻辑记录的详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。

如果被更改的表中已有 `INSTEAD OF` 触发器 `ON UPDATE`，则不能定义 `ON UPDATE CASCADE`、`SET NULL` 或 `SET DEFAULT`。

例如，在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中，ProductVendor 表与 Vendor 表有引用关系 ：ProductVendor.BusinessEntity 外键引用 Vendor.BusinessEntityID 主键 。

如果对 Vendor 表中的行执行 UPDATE 语句，并且为 ProductVendor.BusinessEntityID 指定了 ON UPDATE CASCADE 操作，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将检查 ProductVendor 表中的一个或多个依赖行  。 如果存在依赖行，则 ProductVendor 表中的依赖行将随 Vendor 表中的被引用行一同更新 。

反之，如果指定了 NO ACTION，并且 ProductVendor 表中至少有一行引用 Vendor 行，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将引发错误并回滚对 Vendor 行的更新操作 。

CHECK 一个约束，该约束通过限制可输入一列或多列中的可能值来强制实现域完整性。 计算列上的 CHECK 约束也必须标记为 PERSISTED。

logical_expression 返回 TRUE 或 FALSE 的逻辑表达式。 别名数据类型不能作为表达式的一部分。

column 用括号括起来的一列或多列，在表约束中表示这些列用在约束定义中。

[ ASC | DESC ] 指定加入到表约束中的一列或多列的排序顺序。 默认值为 ASC。

partition_scheme_name 分区架构的名称，该分区架构定义要将已分区表的分区映射到的文件组。 数据库中必须存在该分区架构。

[ _partition\_column\_name_ **.** ] 指定对已分区表进行分区所依据的列。 此列必须与 partition_scheme_name 在数据类型、长度和精度方面使用的分区函数中指定的列相匹配。 必须将参与分区函数的计算列显式标记为 PERSISTED。

> [!IMPORTANT]
> 建议您对分区表的分区列以及作为 ALTER TABLE...SWITCH 操作源或目标的非分区表指定 NOT NULL。 这样做可确保分区列上的所有 CHECK 约束都不必检查 Null 值。

WITH FILLFACTOR =fillfactor 指定[!INCLUDE[ssDE](../../includes/ssde-md.md)]存储索引数据时每个索引页的填充程度。 用户指定的 fillfactor 值的范围可以为 1 到 100。 如果未指定值，则默认值为 0。 填充因子的值 0 和 100 在所有方面都是相同的。

> [!IMPORTANT]
> 将 WITH FILLFACTOR = fillfactor 记录为适用于 PRIMARY KEY 或 UNIQUE 约束的唯一索引选项是为了保持向后兼容，但在未来的版本中将不会以此方式进行记录。

column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS 列集的名称。 列集是一种非类型化的 XML 表示形式，它将表的所有稀疏列合并为一种结构化的输出。 有关列集的详细信息，请参阅 [使用列集](../../relational-databases/tables/use-column-sets.md)。

PERIOD FOR SYSTEM_TIME ( system_start_time_column_name , system_end_time_column_name ) 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。

指定系统用于记录有效记录时间段的列的名称。 将此参数与 GENERATED ALWAYS AS ROW { START | END } 参数和 WITH SYSTEM_VERSIONING = ON 参数结合使用，启用表的系统版本控制。 有关详细信息，请参阅 [Temporal Tables](../../relational-databases/tables/temporal-tables.md)。

COMPRESSION_DELAY 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

为内存优化，延迟指定行须在其能够压缩到列存储索引之前在表中保持不变的最小分钟数。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 根据行的最近更新时间选择要进行压缩的特定行。 例如，如果行在两个小时内频繁更改，则可以设置 `COMPRESSION_DELAY = 120 Minutes` 以确保在 SQL Server 压缩此行之前完成更新。

对于基于磁盘的表，延迟指定增量行组中处于关闭状态的增量行组在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以将它压缩为压缩行组之前必须保持为增量行组的最小分钟数。 由于基于磁盘的表不对单个行跟踪插入和更新时间，因此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将此延迟应用于处于关闭状态的增量行组。

默认为 0 分钟。

有关何时使用 `COMPRESSION_DELAY` 的建议，请参阅[开始使用实时运营分析的列存储索引](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)

\< table_option> ::= 指定一个或多个表选项。

DATA_COMPRESSION 为指定的表、分区号或分区范围指定数据压缩选项。 选项如下：

NONE 不压缩表或指定的分区。

ROW 使用行压缩来压缩表或指定的分区。

PAGE 使用页压缩来压缩表或指定的分区。

COLUMNSTORE

**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

仅适用于列存储索引，包括非聚集列存储索引和聚集列存储索引。 COLUMNSTORE 指定使用性能最高的列存储压缩进行压缩。 这是典型选择。

COLUMNSTORE_ARCHIVE 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

仅适用于列存储索引，包括非聚集列存储索引和聚集列存储索引。 COLUMNSTORE_ARCHIVE 会进一步将表或分区压缩得更小。 这可用于存档，或者用于要求更小存储大小并且可以付出更多时间来进行存储和检索的其他情形。

有关详细信息，请参阅 [Data Compression](../../relational-databases/data-compression/data-compression.md)。

ON PARTITIONS ( { `<partition_number_expression>` | [ ,...n ] ) 指定对其应用 DATA_COMPRESSION 设置的分区。 如果未对表进行分区，则 `ON PARTITIONS` 参数将生成错误。 如果未提供 `ON PARTITIONS` 子句，`DATA_COMPRESSION` 选项则将应用于分区表的所有分区。

可以按以下方式指定 partition_number_expression：

- 提供分区的分区号，例如：`ON PARTITIONS (2)`
- 提供若干单独分区的分区号，并用逗号分隔，例如：`ON PARTITIONS (1, 5)`
- 同时提供范围和单独分区，例如：`ON PARTITIONS (2, 4, 6 TO 8)`

可以将 `<range>` 指定为由单词 TO 隔开的分区号，例如：`ON PARTITIONS (6 TO 8)`。

若要为不同分区设置不同的数据压缩类型，请多次指定 `DATA_COMPRESSION` 选项，例如：

```sql
WITH
(
    DATA_COMPRESSION = NONE ON PARTITIONS (1),
    DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),
    DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)
)
```

\<index_option> ::= 指定一个或多个索引选项。 有关这些操作的完整说明，请参阅 [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)。

PAD_INDEX = { ON | OFF } 如果为 ON，则 FILLFACTOR 指定的可用空间百分比将应用于该索引的中间级别页。 如果未指定 OFF 或 FILLFACTOR 值，则考虑到中间级别页的键集，将中间级别页填充到一个近似容量，以留出足够的空间来容纳至少一个索引的最大行。 默认为 OFF。

FILLFACTOR =fillfactor 指定一个百分比，指示在[!INCLUDE[ssDE](../../includes/ssde-md.md)]创建或修改索引的过程中，应将每个索引页面的叶级填充到什么程度。 fillfactor 必须是 1 到 100 之间的整数。 默认值为 0。 填充因子的值 0 和 100 在所有方面都是相同的。

IGNORE_DUP_KEY = { ON | OFF } 指定在插入操作尝试向唯一索引中插入重复键值时的错误响应。 IGNORE_DUP_KEY 选项仅适用于创建或重新生成索引后发生的插入操作。 当执行 [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)、[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) 或 [UPDATE](../../t-sql/queries/update-transact-sql.md) 时，该选项无效。 默认为 OFF。

ON 向唯一索引插入重复键值时将出现警告消息。 只有违反唯一性约束的行才会失败。

OFF 向唯一索引插入重复键值时将出现错误消息。 整个 INSERT 操作将被回滚。

对于针对视图创建的索引、非唯一索引、XML 索引、空间索引以及筛选的索引，`IGNORE_DUP_KEY` 不能设置为 ON。

若要查看 `IGNORE_DUP_KEY`，请使用 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)。

在后向兼容语法中，`WITH IGNORE_DUP_KEY` 等同于 `WITH IGNORE_DUP_KEY = ON`。

STATISTICS_NORECOMPUTE = { ON | OFF } 如果为 ON，则过期的索引统计信息不会自动重新计算。 如果为 OFF，则启用自动统计信息更新。 默认为 OFF。

ALLOW_ROW_LOCKS = { ON | OFF } 如果为 ON，则访问索引时允许使用行锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用行锁。 如果为 OFF，则不使用行锁。 默认值为 ON。

ALLOW_PAGE_LOCKS = { ON | OFF } 如果为 ON，则访问索引时允许使用页锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用页锁。 如果为 OFF，则不使用页锁。 默认值为 ON。

OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF } **适用于**：[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 及更高版本。 <BR>
指定是否针对最后一页插入争用进行优化。 默认为 OFF。 有关详细信息，请参阅“CREATE INDEX”页的[顺序键](./create-index-transact-sql.md#sequential-keys)部分。

FILETABLE_DIRECTORY = directory_name

**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）。

指定与 windows 兼容的 FileTable 目录名称。 此名称应在数据库的所有 FileTable 目录名称中唯一。 无论排序规则如何设置，唯一性比较都不区分大小写。 如果未指定此值，则使用 filetable 这个名称。

FILETABLE_COLLATE_FILENAME = { collation_name | database_default }

**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）。 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 不支持 `FILETABLE`。

指定要应用于 FileTable 的“名称”  列的排序规则名称。 排序规则必须不区分大小写，以遵守 Windows 操作系统文件命名语义。 如果未指定此值，则使用数据库默认排序规则。 如果数据库默认排序规则区分大小写，将引发错误，CREATE TABLE 操作将失败。

collation_name 不区分大小写的排序规则的名称。

database_default 指定应使用数据库的默认排序规则。 此排序规则必须不区分大小写。

FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME = constraint_name
 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）。

指定要对自动为 FileTable 创建的主键约束使用的名称。 如果未指定此值，则系统将为该约束生成一个名称。

FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME = constraint_name
 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）。

指定要对自动为 FileTable 中的 stream_id 列创建的唯一约束使用的名称。 如果未指定此值，则系统将为该约束生成一个名称。

FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME = constraint_name
 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）。

指定要对自动为 FileTable 中的 parent_path_locator 和 name 列创建的唯一约束使用的名称 。 如果未指定此值，则系统将为该约束生成一个名称。

SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name .history_table_name [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ] 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]）    。

如果数据类型、为 Null 性约束和主键约束需要都满足了，则可启动系统版本控制。 如果未使用 `HISTORY_TABLE` 参数，系统将在与当前表相同的文件组中生成一个符合当前表架构的新历史记录表，从而在两个表之间创建链接，使系统能在历史记录表中记录当前表中每行的历史记录。 此历史记录表的名称为 `MSSQL_TemporalHistoryFor<primary_table_object_id>`。 默认情况下，历史记录表是经过 **PAGE** 压缩的。 如果 `HISTORY_TABLE` 参数用于创建指向现有历史记录表的链接并使用此表，则会在当前表和指定表之间创建链接。 如果当前表已分区，则历史记录表在默认文件组上创建，因为不会自动将分区配置从当前表复制到历史记录表。 如果历史记录表的名称在历史记录表创建期间指定，则必须指定架构和表的名称。 创建现有历史记录表的链接时，可以选择执行数据一致性检查。 数据一致性检查可确保现有记录不重叠。 系统默认执行数据一致性检查。 将此参数与 `PERIOD FOR SYSTEM_TIME` 和 `GENERATED ALWAYS AS ROW { START | END }` 参数结合使用来对表启用系统版本控制。 有关详细信息，请参阅 [Temporal Tables](../../relational-databases/tables/temporal-tables.md)。

REMOTE_DATA_ARCHIVE = { ON [ ( table_stretch_options [,...n] ) ] | OFF ( MIGRATION_STATE = PAUSED ) }   
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本）。

创建已启用或禁用 Stretch Database 的新表。 有关详细信息，请参阅 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)。

为表启用 Stretch Database

指定 `ON` 为表启用 Stretch 时，可选择指定 `MIGRATION_STATE = OUTBOUND` 立即开始迁移数据，也可指定 `MIGRATION_STATE = PAUSED` 推迟迁移数据。 默认值是 `MIGRATION_STATE = OUTBOUND`。 有关为表启用 Stretch 的详细信息，请参阅[为表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)。

**先决条件**。 为表启用 Stretch 之前，必须在服务器和数据库上启用 Stretch。 有关详细信息，请参阅 [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)。

**权限**。 为数据库或表启用 Stretch 需要 db_owner 权限。 为表启用 Stretch 还需具有表的 ALTER 权限。

[ FILTER_PREDICATE = { null | predicate } ] 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本）。

根据需要，指定一个筛选器谓词，从包含历史数据和最新数据的表中选择要迁移的行。 该谓词必须调用确定性的内联表值函数。 有关详细信息，请参阅[为表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)和[使用筛选器函数选择要迁移的行](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md)。

> [!IMPORTANT]
> 如果提供的筛选器谓词性能不佳，则数据迁移性能也不佳。 Stretch Database 通过使用 CROSS APPLY 运算符将筛选器谓词应用到表中。

如果未指定筛选器谓词，则将迁移整个表。

指定筛选器谓词时，还须同时指定 MIGRATION_STATE。

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED } 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

- 指定 `OUTBOUND` 以将数据从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移到 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。
- 指定 `INBOUND` 以将表的远程数据从 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 复制回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，然后为此表禁用 Stretch Database。 有关详细信息，请参阅 [禁用 Stretch Database 并恢复远程数据](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md)。

   此操作会产生数据传输费用，并且不可取消。

- 指定 `PAUSED` 可暂停或推迟数据迁移。 有关详细信息，请参阅[暂停和恢复数据迁移 - Stretch Database](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)。

[DATA_DELETION = ON  
    {( FILTER_COLUMN = column_name,   
           RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS } )}] 适用范围 **：** 仅限 Azure SQL Edge

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

MEMORY_OPTIMIZED 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]）。 Azure SQL 托管实例不支持内存优化表。

ON 值指示表是否为内存优化表。 内存优化表是内存中 OLTP 功能的一部分，用于优化事务处理的性能。 要开始使用内存中 OLTP，请参阅[快速入门 1：可提高 Transact SQL 性能的内存中 OLTP 技术](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)。 有关内存优化表的详细信息，请参阅[内存优化表](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md)。

默认值 OFF 指示表是基于磁盘的表。

DURABILITY 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

`SCHEMA_AND_DATA` 的值指示表具有持久性，这意味着更改会持久保留在磁盘上，重新启动或故障转移后仍然存在。 SCHEMA_AND_DATA 是默认值。

`SCHEMA_ONLY` 的值指示表是非持久表。 表架构具有持久化性，但是，数据库重新启动或故障转移之后，任何数据更改都不会保留。 仅允许将 `DURABILITY = SCHEMA_ONLY` 用于 `MEMORY_OPTIMIZED = ON`。

> [!WARNING]
> 当使用 DURABILITY = SCHEMA_ONLY 创建表，随后使用 ALTER DATABASE 更改 READ_COMMITTED_SNAPSHOT 时，表中的数据将丢失  。

BUCKET_COUNT 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指示应在哈希索引中创建的存储桶数。 哈希索引中 BUCKET_COUNT 的最大值为 1,073,741,824。 有关桶计数的详细信息，请参阅[内存优化表索引](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)。

Bucket_count 是必需的参数。

INDEX **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 和更高版本及 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]）。

可将列索引和表索引指定为 CREATE TABLE 语句的一部分。 有关在内存优化表中添加和删除索引的详细信息，请参阅：[更改内存优化表](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

HASH 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指示创建哈希索引。

只有内存优化表支持哈希索引。

## <a name="remarks"></a>备注

有关获批准的表、列、约束和索引数目的信息，请参阅 [SQL Server 的最大容量规范](../../sql-server/maximum-capacity-specifications-for-sql-server.md)。

通常情况下，为表和索引分配空间时，每次以一个区为增量单位。 将 `ALTER DATABASE` 的 `SET MIXED_PAGE_ALLOCATION` 选项设置为 TRUE 或设置为始终先于 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 时，在创建表或索引后，将从混合盘区为此表或索引分配页面，直到其拥有足够的页面以形成统一盘区为止。 当足够的页填满统一区后，每当当前分配的区填满时，将再为其分配另一个区。 若要获得关于由表分配和使用的空间量的报表，请执行 `sp_spaceused`。

[!INCLUDE[ssDE](../../includes/ssde-md.md)]在列定义中并不强制以特定的顺序指定 DEFAULT、IDENTITY、ROWGUIDCOL 或列约束。

创建表后，即使 QUOTED IDENTIFIER 选项在创建表时设置为 OFF，该选项在表的元数据中仍存储为 ON。

## <a name="temporary-tables"></a>临时表

可以创建本地临时表和全局临时表。 本地临时表仅在当前会话中可见，而全局临时表在所有会话中都可见。 临时表不能分区。

本地临时表的名称前缀有一个数字符号 (#table_name)，而全局临时表的名称前缀有两个数字符号 (##table_name) 。

[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句通过使用 `CREATE TABLE` 语句中为 table_name 指定的值引用临时表，例如：

```sql
CREATE TABLE #MyTempTable (
    col1 INT PRIMARY KEY
);

INSERT INTO #MyTempTable
VALUES (1);
```

如果在单个存储过程或批处理中创建了多个临时表，则它们必须有不同的名称。

创建或访问临时表时，如果包括 schema_name，它将被忽略。 所有临时表都在 dbo 架构中进行创建。

如果本地临时表由存储过程创建或由多个用户同时执行的应用程序创建，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]必须能够区分由不同用户创建的表。 为此，[!INCLUDE[ssDE](../../includes/ssde-md.md)]在内部为每个本地临时表的表名追加一个数字后缀。 存储在 tempdb 的 sysobjects 表中的临时表，其全名由 CREATE TABLE 语句中指定的表名和系统生成的数字后缀组成 。 为了可追加后缀，为本地临时表指定的 table_name 不能超过 116 个字符。

除非使用 DROP TABLE 显式删除临时表，否则临时表将在退出其作用域时由系统自动删除：

- 当存储过程完成时，将自动删除在存储过程中创建的本地临时表。 由创建表的存储过程执行的所有嵌套存储过程都可以引用此表。 但调用创建此表的存储过程的进程无法引用此表。
- 所有其他本地临时表在当前会话结束时都将被自动删除。
- 全局临时表在创建此表的会话结束且其他所有任务停止对其引用时将被自动删除。 任务与表之间的关联只在单个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的生存周期内保持。 换言之，当创建全局临时表的会话结束时，最后一条引用此表的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句完成后，将自动删除此表。

在存储过程或触发器中创建的本地临时表的名称可以与在调用存储过程或触发器之前创建的临时表名称相同。 但是，如果查询引用临时表，而同时有两个同名的临时表，则不定义针对哪个表解析该查询。 嵌套存储过程同样可以创建与调用它的存储过程所创建的临时表同名的临时表。 但是，为了对其进行修改以解析为在嵌套过程中创建的表，此表必须与调用过程创建的表具有相同的结构和列名。 下面的示例说明了这一点。

```sql
CREATE PROCEDURE dbo.Test2
AS
    CREATE TABLE #t (x INT PRIMARY KEY);
    INSERT INTO #t VALUES (2);
    SELECT Test2Col = x FROM #t;
GO

CREATE PROCEDURE dbo.Test1
AS
    CREATE TABLE #t (x INT PRIMARY KEY);
    INSERT INTO #t VALUES (1);
    SELECT Test1Col = x FROM #t;
    EXEC Test2;
GO

CREATE TABLE #t(x INT PRIMARY KEY);
INSERT INTO #t VALUES (99);
GO

EXEC Test1;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
(1 row(s) affected)
Test1Col
-----------
1

(1 row(s) affected)
 Test2Col
 -----------
 2
 ```

当创建了本地或全局临时表后，`CREATE TABLE` 语法将支持除 FOREIGN KEY 约束以外的其他所有约束定义。 如果临时表中指定了 FOREIGN KEY 约束，则该语句将返回一条表明已跳过此约束的警告消息。 此表仍将创建，但不使用 FOREIGN KEY 约束。 在 FOREIGN KEY 约束中不能引用临时表。

如果某个临时表是使用命名约束创建的，并且该临时表是在用户定义的事务的作用域内创建的，则一次只能有一个用户执行创建该临时表的语句。 例如，如果某一存储过程使用命名主键约束创建一个临时表，则多个用户无法同时执行该存储过程。

## <a name="database-scoped-global-temporary-tables-azure-sql-database"></a>数据库作用域内全局临时表（Azure SQL 数据库）

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的全局临时表（表名以 ## 开头）存储在 tempdb 中，并跨整个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例在所有用户会话之间共享。 有关 SQL 表类型的信息，请参阅上述“创建表”章节。

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]支持存储在 tempdb 中，且范围限定为数据库级别的全局临时表。 也就是说，全局临时表对同一 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中的所有用户会话进行共享。 其他数据库中的用户会话无法访问全局临时表。

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]的全局临时表遵循 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对临时表使用的相同语法和语义。 同样，全局临时存储过程也在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]中将范围限定为数据库级别。 局部临时表（表名以 # 开头）也受 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]支持，并遵循 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用的相同语法和语义。 请参阅上述[临时表](#temporary-tables)章节。  

> [!IMPORTANT]
> 此功能适用于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

### <a name="troubleshooting-global-temporary-tables-for-azure-sql-database"></a>排查 Azure SQL 数据库的全局临时表存在的问题

有关 tempdb 疑难解答，请参阅[如何监视 tempdb 使用情况](../../relational-databases/databases/tempdb-database.md#monitoring-tempdb-use)。

> [!NOTE]
> 只有服务器管理员才能访问 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]中的疑难解答 DMV。

### <a name="permissions"></a>权限

任何用户都可以创建全局临时对象。 用户只能访问自己的对象，除非他们获得更多的权限。

## <a name="partitioned-tables"></a>分区表

使用 CREATE TABLE 创建已分区表前，必须首先创建分区函数以指定表分区的方式。 分区函数是使用 [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md) 创建的。 其次，必须创建分区架构，以指定将保存由分区函数指示的分区的文件组。 分区方案是使用 [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) 创建的。 对于已分区表，不能指定用于分隔文件组的 PRIMARY KEY 或 UNIQUE 约束的位置。 有关详细信息，请参阅 [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。

## <a name="primary-key-constraints"></a>PRIMARY KEY 约束

- 一个表只能包含一个 PRIMARY KEY 约束。
- 由 PRIMARY KEY 约束生成的索引不会使表中的非聚集索引超过 999 个，聚集索引超过 1 个。
- 如果没有为 PRIMARY KEY 约束指定 CLUSTERED 或 NONCLUSTERED，并且没有为 UNIQUE 约束指定聚集索引，则将对该 PRIMARY KEY 约束使用 CLUSTERED。
- 在 PRIMARY KEY 约束中定义的所有列都必须定义为 NOT NULL。 如果没有指定为 Null 性，则加入 PRIMARY KEY 约束的所有列的为 Null 性都将设置为 NOT NULL。

    > [!NOTE]
    > 内存优化表可具有可为空的键列。

- 如果在 CLR 用户定义类型的列中定义主键，则该类型的实现必须支持二进制排序。 有关详细信息，请参阅 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)。

## <a name="unique-constraints"></a>UNIQUE 约束

- 如果没有为 UNIQUE 约束指定 CLUSTERED 或 NONCLUSTERED，则默认使用 NONCLUSTERED。
- 每个 UNIQUE 约束都生成一个索引。 UNIQUE 约束的数目不会使表中的非聚集索引超过 999 个，聚集索引超过 1 个。
- 如果在 CLR 用户定义类型的列中定义唯一约束，则该类型的实现必须支持二进制或基于运算符的排序。 有关详细信息，请参阅 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)。

## <a name="foreign-key-constraints"></a>Foreign Key 约束

- 如果在 FOREIGN KEY 约束的列中输入非 NULL 值，则此值必须在被引用列中存在；否则，将返回违反外键约束的错误信息。
- 如果未指定源列，则 FOREIGN KEY 约束适用于前面所讲的列。
- FOREIGN KEY 约束仅能引用位于同一服务器上的同一数据库中的表。 跨数据库的引用完整性必须通过触发器实现。 有关详细信息，请参阅 [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md)。
- FOREIGN KEY 约束可引用同一表中的其他列。 此行为称为自引用。
- 列级 FOREIGN KEY 约束的 REFERENCES 子句只能列出一个引用列。 此列的数据类型必须与定义约束的列的数据类型相同。
- 表级 FOREIGN KEY 约束的 REFERENCES 子句中引用列的数目必须与约束列列表中的列数相同。 每个引用列的数据类型也必须与列表中相应列的数据类型相同。 引用列的指定顺序必须与指定主键列或引用表上的唯一约束的列时所用的顺序相同。
- 如果类型为 timestamp 的列是外键或被引用键的一部分，则不能指定 CASCADE、SET NULL 或 SET DEFAULT。
- 可将 CASCADE、SET NULL、SET DEFAULT 和 NO ACTION 在相互存在引用关系的表上进行组合。 如果 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 遇到 NO ACTION，它将停止并回滚相关的 CASCADE、SET NULL 和 SET DEFAULT 操作。 如果 DELETE 语句导致 CASCADE、SET NULL、SET DEFAULT 和 NO ACTION 操作的组合，则在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 检查所有 NO ACTION 前，将应用所有 CASCADE、SET NULL 和 SET DEFAULT 操作。
- 对于表可包含的引用其他表的 FOREIGN KEY 约束的数目或其他表所拥有的引用特定表的 FOREIGN KEY 约束的数目， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 都没有预定义的限制。

  尽管如此，可使用的 FOREIGN KEY 约束的实际数目还是受硬件配置以及数据库和应用程序设计的限制。 建议表中包含的 FOREIGN KEY 约束不要超过 253 个，并且引用该表的 FOREIGN KEY 约束也不要超过 253 个。 有效的限制还是或多或少取决于应用程序和硬件。 在设计数据库和应用程序时应考虑强制 FOREIGN KEY 约束的开销。

- 对于临时表不强制 FOREIGN KEY 约束。
- FOREIGN KEY 约束只能引用所引用的表的 PRIMARY KEY 或 UNIQUE 约束中的列或所引用的表上 UNIQUE INDEX 中的列。
- 如果在 CLR 用户定义类型的列上定义外键，则该类型的实现必须支持二进制排序。 有关详细信息，请参阅 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)。
- 参与构造外键关系的列必须定义为具有同一长度和小数位数。

## <a name="default-definitions"></a>DEFAULT 定义

- 每列只能有一个 DEFAULT 定义。
- DEFAULT 定义可以包含常量值、函数、SQL 标准 niladic 函数或 NULL。 下表显示 niladic 函数及其在执行 INSERT 语句时返回的默认值。

    |SQL-92 niladic 函数|返回的值|
    |------------------------------|--------------------|
    |CURRENT_TIMESTAMP|当前日期和时间。|
    |CURRENT_USER|执行插入的用户的名称。|
    |SESSION_USER|执行插入的用户的名称。|
    |SYSTEM_USER|执行插入的用户的名称。|
    |USER|执行插入的用户的名称。|
- DEFAULT 定义中的 constant_expression 不能引用表中的其他列，也不能引用其他表、视图或存储过程。
- 不能对数据类型为 timestamp 的列或具有 IDENTITY 属性的列创建 DEFAULT 定义。
- 如果别名数据类型绑定到默认对象，则不能对该别名数据类型的列创建 DEFAULT 定义。

## <a name="check-constraints"></a>CHECK 约束

- 列可以有任意多个 CHECK 约束，并且约束条件中可以包含用 AND 和 OR 组合起来的多个逻辑表达式。 列上的多个 CHECK 约束按创建顺序进行验证。
- 搜索条件必须取值为布尔表达式，并且不能引用其他表。
- 列级 CHECK 约束只能引用被约束的列，表级 CHECK 约束只能引用同一表中的列。

  当执行 INSERT 和 UPDATE 语句时，CHECK CONSTRAINTS 和规则具有相同的数据验证功能。

- 当列上存在规则和一个或多个 CHECK 约束时，将验证所有限制。
- 不能在 text、ntext 或 image 列上定义 CHECK 约束  。

## <a name="additional-constraint-information"></a>其他约束信息

- 无法使用 `DROP INDEX` 删除为约束创建的索引；必须使用 `ALTER TABLE` 来删除约束。 可以使用 `ALTER INDEX ... REBUILD` 重新生成已创建的约束用索引。 有关详细信息，请参阅 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)。
- 除了不能以数字符号 (#) 开头以外，约束名称还必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 如果未提供 constraint_name，则会将系统生成的名称分配给约束。 约束名将出现在所有与违反约束有关的错误信息中。
- 当 `INSERT`、`UPDATE` 或 `DELETE` 语句中违反了约束时，将终止执行该语句。 但是，当 `SET XACT_ABORT` 设置为 OFF 时，如果该语句是显式事务的一部分，则继续处理此语句。 当 `SET XACT_ABORT` 设置为 ON 时，将回滚整个事务。 还可以通过检查 `@@ERROR` 系统函数将 `ROLLBACK TRANSACTION` 语句与事务定义一起使用。
- 如果 `ALLOW_ROW_LOCKS = ON` 且 `ALLOW_PAGE_LOCK = ON`，可以在访问索引时使用行级别、页级别和表级别锁定。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]将选择相应的锁，并且可以将锁从行锁或页锁升级到表锁。 如果 `ALLOW_ROW_LOCKS = OFF` 并且 `ALLOW_PAGE_LOCK = OFF`，则当访问索引时将仅允许表级别的锁。
- 如果某个表具有 FOREIGN KEY 或 CHECK CONSTRAINTS 及触发器，则将在触发器执行前先检查约束条件。

若要获得关于表以及其列的报告，请使用 `sp_help` 或 `sp_helpconstraint`。 若要重命名表，请使用 `sp_rename`。 若要获得依赖于表的视图和存储过程的报表，请使用 [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) 和 [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)。

## <a name="nullability-rules-within-a-table-definition"></a>表定义中的为 Null 性规则

列的为 Null 性决定该列中是否允许以空值 (NULL) 作为其数据。 NULL 不为零或空白：NULL 表示没有生成任何项或没有提供显式 NULL，它通常暗指该值未知或不可用。

使用 `CREATE TABLE` 或 `ALTER TABLE` 来创建或更改表时，数据库和会话设置会影响并且可能会替代列定义中所用的数据类型的为 Null 性。 建议您始终将列显式定义为非计算列的 NULL 或 NOT NULL，或者，如果使用用户定义的数据类型，则建议您允许该列使用此数据类型的默认为空性。 稀疏列必须始终允许 NULL。

如果未显式指定列的为 Null 性，则遵循下表显示的规则。

|列数据类型|规则|
|----------------------|----------|
|别名数据类型|[!INCLUDE[ssDE](../../includes/ssde-md.md)]使用创建数据类型时指定的为 Null 性。 若要确定数据类型的默认为 Null 性，请使用 sp_help。|
|CLR 用户定义类型 (CLR user-defined type)|根据列定义确定为 Null 性。|
|系统提供的数据类型|如果系统提供的数据类型只有一个选项，则优先使用该选项。 timestamp 数据类型必须为 NOT NULL。 当任何会话设置通过 SET 设置为 ON 时：<br />如果 ANSI_NULL_DFLT_ON = ON，则分配 NULL。 <br />如果 ANSI_NULL_DFLT_OFF = ON，则分配 NOT NULL。<br /><br /> 当任何数据库设置通过 ALTER DATABASE 进行配置时：<br />如果 ANSI_NULL_DEFAULT_ON = ON，则分配 NULL。 <br />如果 ANSI_NULL_DEFAULT_OFF = ON，则分配 NOT NULL。<br /><br /> 若要查看 ANSI_NULL_DEFAULT 的数据库设置，请使用 sys.databases 目录视图|

如果没有为会话设置任何 ANSI_NULL_DFLT 选项，并且将数据库设置为默认值（ANSI_NULL_DEFAULT 为 OFF），则会分配默认值 NOT NULL。

如果该列是计算列，则其为 Null 性总是由[!INCLUDE[ssDE](../../includes/ssde-md.md)]自动确定。 若要查找此类型列的为 Null 性，请使用带 AllowsNull 属性的 `COLUMNPROPERTY` 函数。

> [!NOTE]
> SQL Server ODBC 驱动程序和 SQL Server OLE DB 驱动程序都默认将 ANSI_NULL_DFLT_ON 设置为 ON。 ODBC 和 OLE DB 用户可以在 ODBC 数据源中配置该设置，或通过应用程序设置的连接特性或属性配置该设置。

## <a name="data-compression"></a>数据压缩

不能为系统表启用压缩功能。 在创建表时，除非另外指定，否则，将数据压缩设置为 NONE。 如果指定的分区列表或分区超出范围，将生成错误。 有关数据压缩的详细信息，请参阅 [数据压缩](../../relational-databases/data-compression/data-compression.md)。

若要评估更改压缩状态将对表、索引或分区有何影响，请使用 [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) 存储过程。

## <a name="permissions"></a>权限

需要在数据库中具有 `CREATE TABLE` 权限，以及对创建表所在的架构具有 `ALTER` 权限。

如果 `CREATE TABLE` 语句中的任何列被定义为用户定义类型，则需要对用户定义类型具有 `REFERENCES` 权限。

如果 `CREATE TABLE` 语句中的任何列被定义为 CLR 用户定义类型，则需要具有对此类型的所有权或 `REFERENCES` 权限。

如果 `CREATE TABLE` 语句中的任何列具有与其关联的 XML 架构集合，则需要具有对 XML 架构集合的所有权或 `REFERENCES` 权限。

任何用户都可以在 tempdb 中创建临时表。

## <a name="examples"></a>示例

### <a name="a-create-a-primary-key-constraint-on-a-column"></a>A. 对列创建 PRIMARY KEY 约束

以下示例显示对 `EmployeeID` 表的 `Employee` 列具有聚集索引的 PRIMARY KEY 约束的列定义。 因为未指定约束名称，所以系统提供了约束名称。

```sql
CREATE TABLE dbo.Employee (
    EmployeeID INT PRIMARY KEY CLUSTERED
);
```

### <a name="b-using-foreign-key-constraints"></a>B. 使用 FOREIGN KEY 约束

FOREIGN KEY 约束用于引用其他表。 FOREIGN KEY 可以是单列键或多列键。 以下示例显示 `SalesOrderHeader` 表上引用 `SalesPerson` 表的单列 FOREIGN KEY 约束。 对于单列 FOREIGN KEY 约束，只需要 REFERENCES 子句。

```sql
SalesPersonID INT NULL REFERENCES SalesPerson(SalesPersonID)
```

也可以显式使用 FOREIGN KEY 子句并复述列特性。 请注意，在这两个表中列名不必相同。

```sql
FOREIGN KEY (SalesPersonID) REFERENCES SalesPerson(SalesPersonID)
```

多列键约束作为表约束创建。 在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中，`SpecialOfferProduct` 表包含多列 PRIMARY KEY。 以下示例显示如何从其他表中引用此键（可选择显式约束名）。

```sql
CONSTRAINT FK_SpecialOfferProduct_SalesOrderDetail
    FOREIGN KEY (ProductID, SpecialOfferID)
    REFERENCES SpecialOfferProduct (ProductID, SpecialOfferID)
```

### <a name="c-using-unique-constraints"></a>C. 使用 UNIQUE 约束

UNIQUE 约束用于强制非主键列的唯一性。 以下示例强制的限制是，`Name` 表的 `Product` 列必须唯一。

```sql
Name NVARCHAR(100) NOT NULL
UNIQUE NONCLUSTERED
```

### <a name="d-using-default-definitions"></a>D. 使用 DEFAULT 定义

使用 INSERT 和 UPDATE 语句时，如果没有提供值，则默认值会提供值。 例如，[!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库可包括一个查找表，此表列出该公司的员工可以填充的不同工作。 在描述每个工作的列的下面，如果没有显式输入实际的描述，则字符串默认值可提供一个描述。

```sql
DEFAULT 'New Position - title not formalized yet'
```

除了常量以外，DEFAULT 定义还可以包含函数。 使用以下示例获取输入项的当前日期。

```sql
DEFAULT (GETDATE())
```

niladic 函数扫描也可改善数据完整性。 若要跟踪插入行的用户，请使用 USER 的 niladic 函数。 不要用括号将 niladic 函数括起来。

```sql
DEFAULT USER
```

### <a name="e-using-check-constraints"></a>E. 使用 CHECK 约束

以下示例显示对于在 `CreditRating` 表的 `Vendor` 列中输入的值所做的限制。 此约束未命名。

```sql
CHECK (CreditRating >= 1 and CreditRating <= 5)
```

此示例显示一个命名约束，它对于在表的列中输入的字符数据有模式限制。

```sql
CONSTRAINT CK_emp_id CHECK (
    emp_id LIKE '[A-Z][A-Z][A-Z][1-9][0-9][0-9][0-9][0-9][FM]'
    OR emp_id LIKE '[A-Z]-[A-Z][1-9][0-9][0-9][0-9][0-9][FM]'
)
```

此示例指定这些值必须在特定的列表中或遵循指定的模式。

```sql
CHECK (
    emp_id IN ('1389', '0736', '0877', '1622', '1756')
    OR emp_id LIKE '99[0-9][0-9]'
)
```

### <a name="f-showing-the-complete-table-definition"></a>F. 显示完整的表定义

以下示例显示在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中创建的 `PurchaseOrderDetail` 表的完整表定义，其中包含所有约束定义。 请注意，若要运行此示例，表架构应改为 `dbo`。

```sql
CREATE TABLE dbo.PurchaseOrderDetail
(
    PurchaseOrderID int NOT NULL
        REFERENCES Purchasing.PurchaseOrderHeader(PurchaseOrderID),
    LineNumber smallint NOT NULL,
    ProductID int NULL
        REFERENCES Production.Product(ProductID),
    UnitPrice money NULL,
    OrderQty smallint NULL,
    ReceivedQty float NULL,
    RejectedQty float NULL,
    DueDate datetime NULL,
    rowguid uniqueidentifier ROWGUIDCOL NOT NULL
        CONSTRAINT DF_PurchaseOrderDetail_rowguid DEFAULT (NEWID()),
    ModifiedDate datetime NOT NULL
        CONSTRAINT DF_PurchaseOrderDetail_ModifiedDate DEFAULT (GETDATE()),
    LineTotal AS ((UnitPrice*OrderQty)),
    StockedQty AS ((ReceivedQty-RejectedQty)),
    CONSTRAINT PK_PurchaseOrderDetail_PurchaseOrderID_LineNumber
               PRIMARY KEY CLUSTERED (PurchaseOrderID, LineNumber)
               WITH (IGNORE_DUP_KEY = OFF)
)
ON PRIMARY;
```

### <a name="g-creating-a-table-with-an-xml-column-typed-to-an-xml-schema-collection"></a>G. 创建其 xml 列键入 XML 架构集合的表

以下示例创建一个表，其 `xml` 列将键入 XML 架构集合 `HRResumeSchemaCollection`。 `DOCUMENT` 关键字指定 column_name 中 `xml` 数据类型的每个实例只能包含一个顶级元素。

```sql
CREATE TABLE HumanResources.EmployeeResumes
(
    LName nvarchar(25),
    FName nvarchar(25),
    Resume xml(DOCUMENT HumanResources.HRResumeSchemaCollection)
);
```

### <a name="h-creating-a-partitioned-table"></a>H. 创建已分区表

以下示例创建一个分区函数，将表或索引分为四个分区。 然后，此示例创建用于指定保存四个分区的文件组的分区架构。 最后，此示例创建使用此分区架构的表。 此示例假定数据库中已经存在文件组。

```sql
CREATE PARTITION FUNCTION myRangePF1 (int)
    AS RANGE LEFT FOR VALUES (1, 100, 1000);
GO

CREATE PARTITION SCHEME myRangePS1
    AS PARTITION myRangePF1
    TO (test1fg, test2fg, test3fg, test4fg);
GO  
  
CREATE TABLE PartitionTable (col1 int, col2 char(10))
    ON myRangePS1 (col1);
GO
```

根据 `col1` 的 `PartitionTable` 列的值，将分区按照下列方式分配。

|文件组|test1fg|test2fg|test3fg|test4fg|
|---------------|-------------|-------------|-------------|-------------|
|分区|1|2|3|4|
|**值**|col 1 \<= 1|col1 > 1 AND col1 \<= 100|col1 > 100 AND col1 \<= 1,000|col1 > 1000|

### <a name="i-using-the-uniqueidentifier-data-type-in-a-column"></a>I. 在列中使用 uniqueidentifier 数据类型

下面的示例创建一个包含 `uniqueidentifier` 列的表。 该示例使用 PRIMARY KEY 约束以确保用户不会在表中插入重复的值，并在 `NEWSEQUENTIALID()` 约束中使用 `DEFAULT` 函数为新行提供值。 将 ROWGUIDCOL 属性应用到 `uniqueidentifier` 列，以便可以使用 $ROWGUID 关键字对其进行引用。

```sql
CREATE TABLE dbo.Globally_Unique_Data
(
    GUID UNIQUEIDENTIFIER
        CONSTRAINT Guid_Default DEFAULT
        NEWSEQUENTIALID() ROWGUIDCOL,
    Employee_Name VARCHAR(60)
    CONSTRAINT Guid_PK PRIMARY KEY (GUID)
);
```

### <a name="j-using-an-expression-for-a-computed-column"></a>J. 对计算列使用表达式

以下示例显示如何使用用于计算 `(low + high)/2` 计算列的表达式 (`myavg`)。

```sql
CREATE TABLE dbo.mytable
(
    low INT,
    high INT,
    myavg AS (low + high)/2
);
```

### <a name="k-creating-a-computed-column-based-on-a-user-defined-type-column"></a>K. 基于用户定义类型列创建计算列

以下示例将创建一个表，其中一列定义为用户定义类型 `utf8string`，并假设此类型的程序集和类型本身已在当前数据库中创建。 另一列是基于 `utf8string` 定义的，使用 type(class)`utf8string` 的 `ToString()` 方法计算列值。

```sql
CREATE TABLE UDTypeTable
(
    u UTF8STRING,
    ustr AS u.ToString() PERSISTED
);
```

### <a name="l-using-the-user_name-function-for-a-computed-column"></a>L. 对计算列使用 USER_NAME 函数

以下示例在 `USER_NAME()` 列中使用 `myuser_name` 函数。

```sql
CREATE TABLE dbo.mylogintable
(
    date_in DATETIME,
    user_id INT,
    myuser_name AS USER_NAME()
);
```

### <a name="m-creating-a-table-that-has-a-filestream-column"></a>M. 创建具有 FILESTREAM 列的表

下面的示例创建一个包含 `FILESTREAM` 列 `Photo` 的表。 如果某个表包含一个或多个 `FILESTREAM` 列，该表必须包含一个 `ROWGUIDCOL` 列。

```sql
CREATE TABLE dbo.EmployeePhoto
(
    EmployeeId INT NOT NULL PRIMARY KEY,
    Photo VARBINARY(MAX) FILESTREAM NULL,
    MyRowGuidColumn UNIQUEIDENTIFIER NOT NULL ROWGUIDCOL UNIQUE DEFAULT NEWID()
);
```

### <a name="n-creating-a-table-that-uses-row-compression"></a>N. 创建使用行压缩的表

下面的示例创建一个使用行压缩的表。

```sql
CREATE TABLE dbo.T1
(
    c1 INT,
    c2 NVARCHAR(200)
)
WITH (DATA_COMPRESSION = ROW);
```

有关其他数据压缩示例，请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)。

### <a name="o-creating-a-table-that-has-sparse-columns-and-a-column-set"></a>O. 创建具有稀疏列和列集的表

下面的示例说明了如何创建具有稀疏列的表，以及具有两个稀疏列和一个列集的表。 这些示例使用基本语法。 有关更复杂的示例，请参阅[使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)和[使用列集](../../relational-databases/tables/use-column-sets.md)。

此示例创建一个包含稀疏列的表。

```sql
CREATE TABLE dbo.T1
(
    c1 INT PRIMARY KEY,
    c2 VARCHAR(50) SPARSE NULL
);
```

此示例创建一个表，该表具有两个稀疏列和一个名 `CSet` 的列集。

```sql
CREATE TABLE T1
(
    c1 INT PRIMARY KEY,
    c2 VARCHAR(50) SPARSE NULL,
    c3 INT SPARSE NULL,
    CSet XML COLUMN_SET FOR ALL_SPARSE_COLUMNS
);
```

### <a name="p-creating-a-system-versioned-disk-based-temporal-table"></a>P. 创建由系统版本控制的、基于磁盘的临时表

**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

下列示例显示如何创建链接到新历史记录表的临时表，以及如何创建链接到现有历史记录表的临时表。 请注意，临时表须有主键，定义为为表启用和为系统版本控制启用。 有关显示如何在现有表中添加或删除系统版本控制的示例，请参阅[示例](../../t-sql/statements/alter-table-transact-sql.md#Example_Top)中的系统版本控制。 有关使用情况，请参阅[临时表](../../relational-databases/tables/temporal-tables.md)。

此例表示创建链接到新历史记录表的新临时表。

```sql
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON);
```

此例表示创建链接到现有历史记录表的新临时表。

```sql
-- Existing table
CREATE TABLE Department_History
(
    DepartmentNumber CHAR(10) NOT NULL,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 NOT NULL,
    SysEndTime DATETIME2 NOT NULL
);

-- Temporal table
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Department_History, DATA_CONSISTENCY_CHECK = ON));
```

### <a name="q-creating-a-system-versioned-memory-optimized-temporal-table"></a>Q. 创建系统版本控制的内存优化临时表

**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

下列示例显示如何创建链接到基于磁盘的新历史记录表的新系统版本控制的内存优化临时表。

此例表示创建链接到新历史记录表的新临时表。

```sql
CREATE SCHEMA History;
GO

CREATE TABLE dbo.Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY NONCLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH
(
    MEMORY_OPTIMIZED = ON,
    DURABILITY = SCHEMA_AND_DATA,
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.DepartmentHistory)
);
```

此例表示创建链接到现有历史记录表的新临时表。

```sql
-- Existing table
CREATE TABLE Department_History
(
    DepartmentNumber CHAR(10) NOT NULL,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 NOT NULL,
    SysEndTime DATETIME2 NOT NULL
);

-- Temporal table
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH
(
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Department_History, DATA_CONSISTENCY_CHECK = ON)
);
```

### <a name="r-creating-a-table-with-encrypted-columns"></a>R. 创建具有加密列的表

下列示例表示创建包含两个加密列的表。 有关详细信息，请参阅 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)。

```sql
CREATE TABLE Customers (
    CustName NVARCHAR(60)
        ENCRYPTED WITH (
            COLUMN_ENCRYPTION_KEY = MyCEK,
            ENCRYPTION_TYPE = RANDOMIZED,
            ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ),
    SSN VARCHAR(11) COLLATE Latin1_General_BIN2
        ENCRYPTED WITH (
            COLUMN_ENCRYPTION_KEY = MyCEK,
            ENCRYPTION_TYPE = DETERMINISTIC ,
            ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ),
    Age INT NULL
);
```

### <a name="s-create-an-inline-filtered-index"></a>S. 创建内联筛选索引

创建内联已筛选索引的表。

```sql
CREATE TABLE t1
(
    c1 INT,
    index IX1 (c1) WHERE c1 > 0
);
```

### <a name="t-create-an-inline-index"></a>T. 创建内联索引

下面演示如何为基于磁盘的表使用 NONCLUSTERED 内联：

```sql
CREATE TABLE t1
(
    c1 INT,
    INDEX ix_1 NONCLUSTERED (c1)
);

CREATE TABLE t2
(
    c1 INT,
    c2 INT INDEX ix_1 NONCLUSTERED
);

CREATE TABLE t3
(
    c1 INT,
    c2 INT,
    INDEX ix_1 NONCLUSTERED (c1,c2)
);
```

### <a name="u-create-a-temporary-table-with-an-anonymously-named-compound-primary-key"></a>U. 创建包含匿名复合主键的临时表

创建包含匿名复合主键的临时表。 这有助于避免发生以下运行时冲突：两个会话范围内临时表（分别位于单独的会话中）对约束的命名相同。

```sql
CREATE TABLE #tmp
(
    c1 INT,
    c2 INT,
    PRIMARY KEY CLUSTERED ([c1], [c2])
);
GO
```

如果显式命名约束，另一个会话就会生成如下错误：

```
Msg 2714, Level 16, State 5, Line 1
There is already an object named 'PK_#tmp' in the database.
Msg 1750, Level 16, State 1, Line 1
Could not create constraint or index. See previous errors.
```

之所以会出现问题是因为，虽然临时表名称是唯一的，但约束名称并不是唯一的。

### <a name="v-using-global-temporary-tables-in-azure-sql-database"></a>V. 使用 Azure SQL 数据库中的全局临时表

会话 A 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb1 中创建全局临时表 ##test，并添加 1 行

```sql
CREATE TABLE ##test (
    a INT,
    b INT
);

INSERT INTO ##test
VALUES (1, 1);

-- Obtain object ID for temp table ##test
SELECT OBJECT_ID('tempdb.dbo.##test') AS 'Object ID';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
1253579504
```

获取 tempdb (2) 中给定对象 ID 1253579504 的全局临时表名称

```sql
SELECT name FROM tempdb.sys.objects WHERE object_id = 1253579504;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
##test
```

会话 B 连接到 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb1，并能访问会话 A 创建的 ##test 表

```sql
SELECT * FROM ##test;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
1, 1
```

会话 C 连接到 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb2 中的另一个数据库，并希望访问在 testdb1 中创建的 ##test 表。 此选择因全局临时表的数据库作用域而失败

```sql
SELECT * FROM ##test
```

这将生成以下错误：

```
Msg 208, Level 16, State 0, Line 1
Invalid object name '##test'
```

从当前用户数据库 testdb1 寻址 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] tempdb 中的系统对象

```sql
SELECT * FROM tempdb.sys.objects;
SELECT * FROM tempdb.sys.columns;
SELECT * FROM tempdb.sys.database_files;
```   

### <a name="w-enable-data-retention-policy-on-a-table"></a>W. 对表启用数据保留策略

以下示例创建了一个表，该表启用了数据保留，并且保留期为 1 周。 此示例仅适用于 Azure SQL Edge。

```sql
CREATE TABLE [dbo].[data_retention_table] 
(
  [dbdatetime2] datetime2(7), 
  [product_code] int, 
  [value] char(10)
) 
WITH (DATA_DELETION = ON ( FILTER_COLUMN = [dbdatetime2], RETENTION_PERIOD = 1 WEEKS ))
```

## <a name="next-steps"></a>后续步骤

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md)
- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)
- [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)
- [数据类型](../../t-sql/data-types/data-types-transact-sql.md)
- [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)
- [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)
- [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)
- [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)
- [CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [sp_helpconstraint](../../relational-databases/system-stored-procedures/sp-helpconstraint-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
