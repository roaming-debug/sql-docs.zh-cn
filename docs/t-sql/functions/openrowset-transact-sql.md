---
description: OPENROWSET (Transact-SQL)
title: OPENROWSET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/30/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENROWSET_TSQL
- OPENROWSET
dev_langs:
- TSQL
helpviewer_keywords:
- data sources [SQL Server]
- OPENROWSET function
- remote data access [SQL Server], OPENROWSET
- ad hoc distributed queries
- OPENROWSET function, Transact-SQL
- OPENROWSET statement
- OLE DB data sources [SQL Server]
- ad hoc connection information
ms.assetid: f47eda43-33aa-454d-840a-bb15a031ca17
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: a2c7af1aaa1840b3c3468a00541f96325d8a5c52
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "100352406"
---
# <a name="openrowset-transact-sql"></a>OPENROWSET (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

包含访问 OLE DB 数据源中的远程数据所需的所有连接信息。 当访问链接服务器中的表时，这种方法是一种替代方法，并且是一种使用 OLE DB 连接并访问远程数据的一次性的临时方法。 对于较频繁引用 OLE DB 数据源的情况，请改为使用链接服务器。 有关详细信息，请参阅 [链接服务器（数据库引擎）](../../relational-databases/linked-servers/linked-servers-database-engine.md)。 `OPENROWSET` 函数可以在查询的 FROM 子句中引用，就好象它是一个表名。 依据 OLE DB 提供程序的功能，还可以将 `OPENROWSET` 函数引用为 `INSERT`、`UPDATE` 或 `DELETE` 语句的目标表。 尽管查询可能返回多个结果集，但 `OPENROWSET` 只返回第一个结果集。

`OPENROWSET` 还通过内置的 BULK 提供程序支持大容量操作，正是有了该提供程序，才能从文件读取数据并将数据作为行集返回。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
OPENROWSET
( { 'provider_name' 
    , { 'datasource' ; 'user_id' ; 'password' | 'provider_string' }
    , {   <table_or_view> | 'query' }
   | BULK 'data_file' ,
       { FORMATFILE = 'format_file_path' [ <bulk_options> ]
       | SINGLE_BLOB | SINGLE_CLOB | SINGLE_NCLOB }
} )

<table_or_view> ::= [ catalog. ] [ schema. ] object

<bulk_options> ::=

   [ , DATASOURCE = 'data_source_name' ]

   [ , ERRORFILE = 'file_name' ]
   [ , ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ , MAXERRORS = maximum_errors ]

   [ , FIRSTROW = first_row ]
   [ , LASTROW = last_row ]
   [ , ROWS_PER_BATCH = rows_per_batch ]
   [ , ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) [ UNIQUE ] ]
  
   -- bulk_options related to input file format
   [ , CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ , FORMAT = 'CSV' ]
   [ , FIELDQUOTE = 'quote_characters']
   [ , FORMATFILE = 'format_file_path' ]
   [ , FORMATFILE_DATA_SOURCE = 'data_source_name' ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

### <a name="provider_name"></a>'*provider_name*'
字符串，表示在注册表中指定的 OLE DB 访问接口的友好名称（或 PROGID）。 provider_name 没有默认值  。 提供程序名称示例是 `Microsoft.Jet.OLEDB.4.0`、`SQLNCLI` 或 `MSDASQL`。

### <a name="datasource"></a>'*datasource*'
对应于特定 OLE DB 数据源的字符串常量。 datasource 是要传递给提供程序的 IDBProperties 接口的 DBPROP_INIT_DATASOURCE 属性，该属性用于初始化提供程序  。 通常，此字符串包含数据库文件的名称、数据库服务器的名称，或者访问接口能理解的用于定位数据库的名称。
数据源可以是 `Microsoft.Jet.OLEDB.4.0` 提供程序的文件路径 `C:\SAMPLES\Northwind.mdb'`，也可以是 `SQLNCLI` 提供程序的连接字符串 `Server=Seattle1;Trusted_Connection=yes;`。

### <a name="user_id"></a>'*user_id*'
字符串常量，它是传递给指定 OLE DB 访问接口的用户名。 user_id 为连接指定安全上下文，并作为 DBPROP_AUTH_USERID 属性传入以初始化提供程序  。 user_id 不能是 Microsoft Windows 登录名  。

### <a name="password"></a>'*password*'
字符串常量，它是传递给 OLE DB 访问接口的用户密码。 在初始化提供程序时，password 作为 DBPROP_AUTH_PASSWORD 属性传入  。 password 不能是 Microsoft Windows 密码  。

```sql
SELECT a.*
   FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
                   'C:\SAMPLES\Northwind.mdb';
                   'admin';
                   'password',
                   Customers) AS a;
```

### <a name="provider_string"></a>'*provider_string*'
访问接口特定的连接字符串，作为 DBPROP_INIT_PROVIDERSTRING 属性传入以初始化 OLE DB 访问接口。 provider_string 通常封装初始化提供程序所需的所有连接信息  。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序识别的关键字列表，请参阅[初始化和授权属性](../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md)。

```sql
SELECT d.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
                            Department) AS d;
```

### <a name="table_or_view"></a><table_or_view>
包含 `OPENROWSET` 应读取的数据的远程表或视图。 它可以是包含以下组件的三部分名称对象：
- catalog（可选）- 指定对象所在的目录或数据库的名称  。
- schema（可选）- 架构名称或指定对象的对象所有者名称  。
- object - 对象名称，唯一地标识出将要操作的对象  。

```sql
SELECT d.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
                 AdventureWorks2012.HumanResources.Department) AS d;
```

### <a name="query"></a>'*query*'
字符串常量，发送到访问接口并由访问接口执行。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地实例不处理该查询，但处理由访问接口返回的查询结果（传递查询）。 有些访问接口并不通过表名而是通过命令语言提供其表格格式数据，将传递查询用于这些访问接口是非常有用的。 只要查询提供程序支持 OLE DB Command 对象及其强制接口，那么在远程服务器上就支持传递查询。 有关详细信息，请参阅 [SQL Server Native Client (OLE DB) 参考](../../relational-databases/native-client-ole-db-interfaces/sql-server-native-client-ole-db-interfaces.md)。

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
     'SELECT TOP 10 GroupName, Name
     FROM AdventureWorks2012.HumanResources.Department') AS a;
```

### <a name="bulk"></a>BULK
使用 OPENROWSET 的 BULK 行集访问接口读取文件中的数据。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，OPENROWSET 无需将数据文件中的数据加载到目标表，便可读取这些数据。 这样便可在单个 SELECT 语句中使用 OPENROWSET。

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

BULK 选项的参数可对何时开始和结束数据读取、如何处理错误以及如何解释数据提供有效控制。 例如，可以指定以类型为 varbinary、varchar 或 nvarchar 的单行单列行集的形式读取数据文件    。 默认行为详见随后的参数说明。

 有关如何使用 BULK 选项的信息，请参阅本主题后面部分的“备注”。 有关 BULK 选项所需权限的信息，请参阅本主题后面的“权限”。

> [!NOTE]
> 当用于以完整恢复模式导入数据时，OPENROWSET (BULK ...) 不优化日志记录。

有关准备数据以进行批量导入的信息，请参阅 [准备用于批量导出或导入的数据 (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)。

#### <a name="bulk-data_file"></a>BULK 'data_file' 
数据文件的完整路径，该文件的数据将被复制到目标表中。

```sql
SELECT * FROM OPENROWSET(
   BULK 'C:\DATA\inv-2017-01-19.csv',
   SINGLE_CLOB) AS DATA;
```

**适用于：** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]CTP 1.1。
自 [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 起，data_file 可位于 Azure Blob 存储中。 例如，请参阅[批量访问 Azure Blob 存储中数据的示例](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md)。

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

#### <a name="bulk-error-handling-options"></a>BULK 错误处理选项

##### <a name="errorfile"></a>ERRORFILE
`ERRORFILE` ='file_name' 指定用于收集格式有误且不能转换为 OLE DB 行集的行的文件  。 这些行将按原样从数据文件复制到此错误文件中。

错误文件在开始执行命令时创建。 如果该文件已存在，将引发一个错误。 此外，还创建了一个扩展名为 .ERROR.txt 的控制文件。 此文件引用错误文件中的每一行并提供错误诊断。 纠正错误后即可加载数据。
**适用于：** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]CTP 1.1。
从 [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 开始，`error_file_path` 可位于 Azure Blob 存储中。

##### <a name="errorfile_data_source_name"></a>ERRORFILE_DATA_SOURCE_NAME
**适用于：** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]CTP 1.1。
是命名的外部数据源，指向错误文件的 Azure Blob 存储位置，该错误文件包含导入过程中发现的错误。 外部数据源必须使用 [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 中添加的 `TYPE = BLOB_STORAGE` 选项创建。 有关详细信息，请参阅 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)。

##### <a name="maxerrors"></a>MAXERRORS
`MAXERRORS` =maximum_errors 指定在 OPENROWSET 引发异常之前允许发生的最多语法错误数或格式有误的行数（如格式化文件中所述）  。 在达到 MAXERRORS 之前，OPENROWSET 会忽略每个错误行，不加载它，并将其计为一个错误。

默认 maximum_errors 为 10  。

> [!NOTE]
> `MAX_ERRORS` 不适用于 CHECK 约束，也不适用于“money”和“bigint”数据类型的转换   。

#### <a name="bulk-data-processing-options"></a>BULK 数据处理选项

##### <a name="firstrow"></a>FIRSTROW
`FIRSTROW` = first_row 指定要加载的第一行的行号  。 默认值为 1。 这表示指定数据文件中的第一行。 通过对行终止符进行计数来确定行号。 FIRSTROW 从 1 开始。

##### <a name="lastrow"></a>LASTROW
`LASTROW` = last_row 指定要加载的最后一行的行号  。 默认值为 0。 这表示指定数据文件中的最后一行。

##### <a name="rows_per_batch"></a>ROWS_PER_BATCH
`ROWS_PER_BATCH` =rows_per_batch 指定数据文件中近似的数据行数量  。 该值应与实际行数相同。

`OPENROWSET` 始终以单批形式导入数据文件。 但是，如果指定 rows_per_batch 值 > 0，则查询处理器将使用 rows_per_batch 的值作为在查询计划中分配资源的提示   。

默认情况下，ROWS_PER_BATCH 是未知的。 指定 ROWS_PER_BATCH = 0 相当于忽略 ROWS_PER_BATCH。

##### <a name="order"></a>ORDER
`ORDER` ( { column [ ASC | DESC ] } [ ,... n ] [ UNIQUE ] ) 可选提示，用于指定数据文件中数据的排序方式   。 默认情况下，大容量操作假定数据文件未排序。 如果查询优化器能够利用指定顺序来生成更有效的查询计划，则性能可能会得到改善。 指定一个排序可以取得益处的示例包括：

- 将行插入到具有聚集索引的表，其中行集数据按聚集索引键进行排序。
- 将行集与另一个表联接，其中排序列和联接列匹配。
- 通过排序列聚合行集数据。
- 将行集用作查询的 FROM 子句中的源表，其中排序列和联接列匹配。

##### <a name="unique"></a>UNIQUE
`UNIQUE` 指定数据文件不能有重复条目。

如果数据文件中的实际行没有根据指定的顺序进行排序，或者如果指定了 UNIQUE 提示并且存在重复键，则返回错误。

使用 ORDER 时列别名是必需的。 列别名列表必须引用由 BULK 子句正在访问的派生表。 在 ORDER 子句中指定的列名将引用此列别名列表。 不能指定大值类型（varchar(max)、nvarchar(max)、varbinary(max) 和 xml）以及大型对象 (LOB) 类型（text、ntext 和 image）列        。

##### <a name="single_blob"></a>SINGLE_BLOB
将 data_file 内容返回为类型 varbinary(max) 单行、单列的行集   。

> [!IMPORTANT]
> 我们建议您仅使用 SINGLE_BLOB 选项（而不是 SINGLE_CLOB 和 SINGLE_NCLOB）导入 XML 数据，因为只有 SINGLE_BLOB 支持所有的 Windows 编码转换。

##### <a name="single_clob"></a>SINGLE_CLOB
通过以 ASCII 格式读取 data_file，使用当前数据库的排序规则将内容作为类型为 varchar(max) 的单行单列行集返回   。

##### <a name="single_nclob"></a>SINGLE_NCLOB
通过以 UNICODE 格式读取 data_file，使用当前数据库的排序规则将内容作为类型为 nvarchar(max) 的单行单列行集返回   。

```sql
SELECT *
   FROM OPENROWSET(BULK N'C:\Text1.txt', SINGLE_NCLOB) AS Document;
```

#### <a name="bulk-input-file-format-options"></a>BULK 输入文件格式选项

##### <a name="codepage"></a>CODEPAGE
`CODEPAGE` = { 'ACP' \| 'OEM' \| 'RAW' \| 'code_page' } 指定数据文件中数据的代码页。 仅当数据含有字符值大于 127 或小于 32 的 char、varchar 或 text 列时，CODEPAGE 才适用    。

> [!IMPORTANT]
> `CODEPAGE` 不是 Linux 上的支持项。

> [!NOTE]
> 我们建议为格式文件中的每个列指定一个排序规则名称，除非你希望 65001 选项优先于排序规则/代码页规范。

|CODEPAGE 值|说明|
|--------------------|-----------------|
|ACP|将数据类型为 char、varchar 或 text 的列由 ANSI/[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 代码页 (ISO 1252) 转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代码页    。|
|OEM（默认值）|将数据类型为 char、varchar 或 text 的列由系统 OEM 代码页 (ISO 1252) 转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代码页    。|
|RAW|不执行从一个代码页到另一个代码页的转换。 这是执行最快的选项。|
|*code_page*|指示数据文件中字符数据已编码的源代码页，例如 850。<br /><br /> **重要提示**：低于 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 的版本不支持代码页 65001（UTF-8 编码）。|

##### <a name="format"></a>FORMAT
`FORMAT` = 'CSV' 适用范围：   [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]CTP 1.1。
指定符合 [RFC 4180](https://tools.ietf.org/html/rfc4180) 标准的逗号分隔值文件。

```sql
SELECT *
FROM OPENROWSET(BULK N'D:\XChange\test-csv.csv',
    FORMATFILE = N'D:\XChange\test-csv.fmt',
    FIRSTROW=2,
    FORMAT='CSV') AS cars;
```

##### <a name="formatfile"></a>FORMATFILE
`FORMATFILE` ='format_file_path' 指定格式化文件的完整路径  。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持两种格式化文件类型：XML 和非 XML。

格式化文件对定义结果集中的列类型是必需的。 唯一的例外情况是指定 SINGLE_CLOB、SINGLE_BLOB 或 SINGLE_NCLOB 时；在这种情况下，不需要格式化文件。

有关格式化文件的信息，请参阅[使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)。

**适用于：** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]CTP 1.1。
从 [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 开始，format_file_path 可位于 Azure Blob 存储中。 例如，请参阅[批量访问 Azure Blob 存储中数据的示例](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md)。

##### <a name="fieldquote"></a>FIELDQUOTE
`FIELDQUOTE`= 'field_quote' 适用范围   ：[!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]CTP 1.1。
指定将用作 CSV 文件引号字符的字符。 如果未指定，根据 [RFC 4180](https://tools.ietf.org/html/rfc4180) 标准中的定义，引号字符 (") 将用作引号字符。

## <a name="remarks"></a>备注

仅当 DisallowAdhocAccess 注册表选项针对指定的提供程序显式设置为 0，并且启用 Ad Hoc Distributed Queries 高级配置选项时，`OPENROWSET` 才可用于访问 OLE DB 数据源中的远程数据  。 如果未设置这些选项，则默认行为不允许即席访问。

访问远程 OLE DB 数据源时，服务器不会自动委托可信连接的登录标识，客户端通过此登录标识才能连接到正在查询的服务器。 必须配置身份验证委托。

如果 OLE DB 访问接口在指定的数据源中支持多个目录和架构，那么就需要目录及架构名称。 如果 OLE DB 提供程序并不支持多个目录和架构，则可以忽略 catalog 和 schema 的值   。 如果提供程序只支持架构名称，那么必须指定一个格式为 schema.object 的两部分名称    。 如果提供程序只支持目录名称，那么必须指定一个格式为 catalog.schema.object 的三部分名称      。 必须为使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序的传递查询指定由三部分组成的名称。 有关详细信息，请参阅 [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

`OPENROWSET` 不接受其参数的变量。

`FROM` 子句中对 `OPENDATASOURCE`、`OPENQUERY` 或 `OPENROWSET` 的任何调用与对用作更新目标的这些函数的任何调用都是分开独立计算的，即使为两个调用提供的参数相同也是如此。 具体而言，应用到上述任一调用的结果的筛选器或联接条件不会影响其他调用的结果。

## <a name="using-openrowset-with-the-bulk-option"></a>使用带有 BULK 选项的 OPENROWSET

以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 增强功能支持 OPENROWSET(BULK...) 函数：

- 与 `SELECT` 一起使用的 FROM 子句可以调用有完整 `SELECT` 功能的 `OPENROWSET(BULK...)`，而不是表名。

   带有 `BULK` 选项的 `OPENROWSET` 在 `FROM` 子句中需要有一个相关名称，也称为范围变量或别名。 可以指定列别名。 如果未指定列别名列表，则格式化文件必须具有列名。 指定列别名会覆盖格式化文件中的列名，例如：

  - `FROM OPENROWSET(BULK...) AS table_alias`
  - `FROM OPENROWSET(BULK...) AS table_alias(column_alias,...n)`

  > [!IMPORTANT]
  > 如果添加 `AS <table_alias>` 失败，将导致错误：消息 491，级别 16，状态 1，第 20 行 必须在 FROM 子句中为大容量行集指定相关名称。

- `SELECT...FROM OPENROWSET(BULK...)` 语句将直接查询文件中的数据，无需将数据导入表中。 `SELECT...FROM OPENROWSET(BULK...)` 语句还可以通过使用格式化文件指定列名和数据类型，从而列出大容量列别名。
- 将 `OPENROWSET(BULK...)` 用作 `INSERT` 或 `MERGE` 语句中的源表，将数据文件中的数据大容量导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。 有关详细信息，请参阅[使用 BULK INSERT 或 OPENROWSET&#40;BULK...&#41; 导入批量数据 &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)。
- `OPENROWSET BULK` 选项与 `INSERT` 语句一起使用时，BULK 子句支持表提示。 除了常规表提示（例如 `TABLOCK`），`BULK` 子句还可以接受以下专用表提示：`IGNORE_CONSTRAINTS`（仅忽略 `CHECK` 和 `FOREIGN KEY` 约束）、`IGNORE_TRIGGERS`、`KEEPDEFAULTS` 和 `KEEPIDENTITY`。 有关详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。

  有关如何使用 `INSERT...SELECT * FROM OPENROWSET(BULK...)` 语句的信息，请参阅[批量导入和导出数据 (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)。 有关何时在事务日志中记录由批量导入执行的行插入操作的信息，请参阅 [《Prerequisites for Minimal Logging in Bulk Import》](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)（批量导入的最小日志记录的先决条件）。

> [!NOTE]
> 使用 `OPENROWSET` 时，请务必了解 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是如何处理模拟的。 有关安全注意事项的信息，请参阅[使用 BULK INSERT 或 OPENROWSET (BULK...) 批量导入数据 (SQL Server)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)。

### <a name="bulk-importing-sqlchar-sqlnchar-or-sqlbinary-data"></a>大容量导入 SQLCHAR、SQLNCHAR 或 SQLBINARY 数据

OPENROWSET(BULK...) 假定（如果未指定）SQLCHAR、SQLNCHAR 或 SQLBINARY 数据的最大长度不会超过 8000 个字节。 如果要导入的数据所在的 LOB 数据字段包含超过 8000 字节的任意 varchar(max)、nvarchar(max) 或 varbinary(max) 对象，则必须使用为该数据字段定义最大长度的 XML 格式文件    。 若要指定最大长度，请编辑格式文件并声明 MAX_LENGTH 属性。

> [!NOTE]
> 自动生成的格式文件不会为 LOB 字段指定长度或最大长度。 不过，您可以手动编辑格式文件并指定长度或最大长度。

### <a name="bulk-exporting-or-importing-sqlxml-documents"></a>批量导出或导入 SQLXML 文档

若要批量导出或导入 SQLXML 数据，请在格式化文件中使用下列数据类型之一。

|数据类型|效果|
|---------------|------------|
|SQLCHAR 或 SQLVARYCHAR|在客户端代码页或排序规则隐含的代码页中发送数据。|
|SQLNCHAR 或 SQLNVARCHAR|以 Unicode 格式发送数据。|
|SQLBINARY 或 SQLVARYBIN|不经任何转换即发送数据。|

## <a name="permissions"></a>权限

`OPENROWSET` 权限由传递给 OLE DB 访问接口的用户名的权限确定。 使用 `BULK` 选项需要 `ADMINISTER BULK OPERATIONS` 或 `ADMINISTER DATABASE BULK OPERATIONS` 权限。

## <a name="examples"></a>示例

### <a name="a-using-openrowset-with-select-and-the-sql-server-native-client-ole-db-provider"></a>A. 将 OPENROWSET 与 SELECT 和 SQL Server Native Client OLE DB 访问接口一起使用

以下示例使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序访问 `HumanResources.Department` 表，该表位于远程服务器 `Seattle1` 上的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中。 （使用 SQLNCLI 并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将重定向到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口的最新版本。）使用 `SELECT` 语句定义返回的行集。 访问接口字符串包含 `Server` 和 `Trusted_Connection` 关键字。 这些关键字由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序识别。

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
     'SELECT GroupName, Name, DepartmentID
      FROM AdventureWorks2012.HumanResources.Department
      ORDER BY GroupName, Name') AS a;
```

### <a name="b-using-the-microsoft-ole-db-provider-for-jet"></a>B. 使用 Microsoft OLE DB Provider for Jet

以下示例通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for Jet 访问 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access `Customers` 数据库中的 `Northwind` 表。

> [!NOTE]
> 此示例假定已经安装了 Access。 若要运行该示例，则必须安装 Northwind 数据库。

```sql
SELECT CustomerID, CompanyName
   FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
      'C:\Program Files\Microsoft Office\OFFICE11\SAMPLES\Northwind.mdb';
      'admin';'',Customers);
```

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

### <a name="c-using-openrowset-and-another-table-in-an-inner-join"></a>C. 使用 OPENROWSET 和 INNER JOIN 中的另一个表

以下示例从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `Northwind` 数据库本地实例中的 `Customers` 表以及存储在同一计算机上的 Access `Northwind` 数据库中的 `Orders` 表中选择所有数据。

> [!NOTE]
> 此示例假定已经安装了 Access。 若要运行该示例，则必须安装 Northwind 数据库。

```sql
USE Northwind;
GO
SELECT c.*, o.*
FROM Northwind.dbo.Customers AS c
   INNER JOIN OPENROWSET('Microsoft.Jet.OLEDB.4.0',
   'C:\Program Files\Microsoft Office\OFFICE11\SAMPLES\Northwind.mdb';'admin';'', Orders)
   AS o
   ON c.CustomerID = o.CustomerID ;
```

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 仅支持从 Azure blob 存储读取内容。

### <a name="d-using-openrowset-to-bulk-insert-file-data-into-a-varbinarymax-column"></a>D. 使用 OPENROWSET 将文件数据大容量插入 varbinary(max) 列中

 以下示例创建一个用于演示的小型表，并将名为 `Text1.txt` 的文件（位于 `C:` 根目录）中的文件数据插入 `varbinary(max)` 列中。

```sql
CREATE TABLE myTable(FileName NVARCHAR(60),
  FileType NVARCHAR(60), Document VARBINARY(max));
GO

INSERT INTO myTable(FileName, FileType, Document)
   SELECT
      'Text1.txt' AS FileName
      , '.txt' AS FileType
      , *
   FROM OPENROWSET(BULK N'C:\Text1.txt', SINGLE_BLOB) AS Document;
GO
```

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

### <a name="e-using-the-openrowset-bulk-provider-with-a-format-file-to-retrieve-rows-from-a-text-file"></a>E. 将 OPENROWSET BULK 访问接口用于格式化文件以检索文本文件中的行

以下示例使用格式化文件检索用制表符分隔的文本文件 `values.txt` 中的行，该文件包含下列数据：

```
1     Data Item 1
2     Data Item 2
3     Data Item 3
```

格式化文件 `values.fmt` 说明 `values.txt` 中的列：

```
9.0
2  
1  SQLCHAR  0  10 "\t"        1  ID                      SQL_Latin1_General_Cp437_BIN
2  SQLCHAR  0  40 "\r\n"      2  Description        SQL_Latin1_General_Cp437_BIN
```

下面的语句是检索此数据的查询：

```sql
SELECT a.* FROM OPENROWSET( BULK 'c:\test\values.txt',
   FORMATFILE = 'c:\test\values.fmt') AS a;
```

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

### <a name="f-specifying-a-format-file-and-code-page"></a>F. 指定格式文件和代码页

下例演示如何同时使用格式化文件和代码页选项。

```sql
INSERT INTO MyTable SELECT a.* FROM
OPENROWSET (BULK N'D:\data.csv', FORMATFILE =
    'D:\format_no_collation.txt', CODEPAGE = '65001') AS a;
```

### <a name="g-accessing-data-from-a-csv-file-with-a-format-file"></a>G. 访问有格式化文件的 CSV 文件的数据

**适用于：** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]CTP 1.1。

```sql
SELECT *
FROM OPENROWSET(BULK N'D:\XChange\test-csv.csv',
    FORMATFILE = N'D:\XChange\test-csv.fmt',
    FIRSTROW=2,
    FORMAT='CSV') AS cars;
```

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 仅支持从 Azure blob 存储读取内容。

### <a name="h-accessing-data-from-a-csv-file-without-a-format-file"></a>H. 访问没有格式文件的 CSV 文件的数据

```sql
SELECT * FROM OPENROWSET(
   BULK 'C:\Program Files\Microsoft SQL Server\MSSQL14.CTP1_1\MSSQL\DATA\inv-2017-01-19.csv',
   SINGLE_CLOB) AS DATA;
```

```sql
SELECT *
FROM OPENROWSET
   (  'MSDASQL'
     ,'Driver={Microsoft Access Text Driver (*.txt, *.csv)}'
     ,'select * from E:\Tlog\TerritoryData.csv')
;
```

> [!IMPORTANT]
>
> - ODBC 驱动程序应为 64 位。 在 Windows 中打开 [OBDC 数据源](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md)应用程序的“驱动程序”选项卡来验证这一点  。 32 位的 `Microsoft Text Driver (*.txt, *.csv)` 无法用于 64 位版本的 sqlservr.exe。
> - Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

### <a name="i-accessing-data-from-a-file-stored-on-azure-blob-storage"></a>I. 访问 Azure Blob 存储上存储的文件的数据

**适用于：** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]CTP 1.1。
下例使用外部数据源，该外部数据源指向 Azure 存储帐户中的容器和为共享访问签名创建的数据库范围的凭据。

```sql
SELECT * FROM OPENROWSET(
   BULK 'inv-2017-01-19.csv',
   DATA_SOURCE = 'MyAzureInvoices',
   SINGLE_CLOB) AS DataFile;
```

有关完整的 `OPENROWSET` 示例（包括配置凭据和外部数据源），请参阅[有关批量访问 Azure Blob 存储中数据的示例](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md)。

### <a name="j-importing-into-a-table-from-a-file-stored-on-azure-blob-storage"></a>J. 从 Azure Blob 存储上存储的文件导入表

下面的示例演示如何使用 OPENROWSET 命令从已创建 SAS 密钥的 Azure Blob 存储位置中的 csv 文件加载数据。 Azure Blob 存储位置配置为外部数据源。 这需要使用共享访问签名的数据库范围的凭据，该签名通过用户数据库中的主密钥进行加密。

```sql
--> Optional - a MASTER KEY is not required if a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'YourStrongPassword1';
GO
--> Optional - a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE DATABASE SCOPED CREDENTIAL MyAzureBlobStorageCredential
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************';

 -- NOTE: Make sure that you don't have a leading ? in SAS token, and
 -- that you have at least read permission on the object that should be loaded srt=o&sp=r, and
 -- that expiration period is valid (all dates are in UTC time)

CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
WITH ( TYPE = BLOB_STORAGE,
          LOCATION = 'https://****************.blob.core.windows.net/curriculum'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

INSERT INTO achievements with (TABLOCK) (id, description)
SELECT * FROM OPENROWSET(
   BULK  'csv/achievements.csv',
   DATA_SOURCE = 'MyAzureBlobStorage',
   FORMAT ='CSV',
   FORMATFILE='csv/achievements-c.xml',
   FORMATFILE_DATA_SOURCE = 'MyAzureBlobStorage'
    ) AS DataFile;
```

> [!IMPORTANT]
> Azure SQL 数据库仅支持使用 SAS 令牌从 Azure Blob 存储读取内容。

### <a name="additional-examples"></a>其他示例

有关使用 `INSERT...SELECT * FROM OPENROWSET(BULK...)` 的其他示例，请参阅以下主题：

- [批量导入和导出 XML 文档的示例 (SQL Server)](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [批量导入数据时保留标识值 (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [在批量导入期间保留 Null 或使用默认值 (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)
- [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>另请参阅

- [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)
- [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)
- [批量导入和导出数据 (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)
- [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)
- [OPENQUERY &#40;Transact-SQL&#41;](../../t-sql/functions/openquery-transact-sql.md)
- [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)
- [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)
- [sp_serveroption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)
- [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)
- [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)
