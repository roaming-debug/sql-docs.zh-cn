---
title: BULK INSERT (Transact-SQL) | Microsoft Docs
description: BULK INSERT 语句的 Transact-SQL 参考。
ms.date: 02/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BULK_TSQL
- BULK_INSERT
- BULK_INSERT_TSQL
- BULK INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- tables [SQL Server], importing data into
- inserting files
- views [SQL Server], importing data into
- BULK INSERT statement
- views [SQL Server], exporting data from
- importing data, bulk import
- bulk importing [SQL Server], BULK INSERT statement
- file importing [SQL Server]
ms.assetid: be3984e1-5ab3-4226-a539-a9f58e1e01e2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 12c1273473fd91c0ea5222add0288fac1b06f37b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170589"
---
# <a name="bulk-insert-transact-sql"></a>BULK INSERT (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中以用户指定的格式将数据文件导入到数据库表或视图中

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
BULK INSERT
   { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }
      FROM 'data_file'
     [ WITH
    (
   [ [ , ] BATCHSIZE = batch_size ]
   [ [ , ] CHECK_CONSTRAINTS ]
   [ [ , ] CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ [ , ] DATAFILETYPE =
      { 'char' | 'native'| 'widechar' | 'widenative' } ]
   [ [ , ] DATA_SOURCE = 'data_source_name' ]
   [ [ , ] ERRORFILE = 'file_name' ]
   [ [ , ] ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] FIRSTROW = first_row ]
   [ [ , ] FIRE_TRIGGERS ]
   [ [ , ] FORMATFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] KEEPIDENTITY ]
   [ [ , ] KEEPNULLS ]
   [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]
   [ [ , ] LASTROW = last_row ]
   [ [ , ] MAXERRORS = max_errors ]
   [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]
   [ [ , ] ROWS_PER_BATCH = rows_per_batch ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
   [ [ , ] TABLOCK ]

   -- input file format options
   [ [ , ] FORMAT = 'CSV' ]
   [ [ , ] FIELDQUOTE = 'quote_characters']
   [ [ , ] FORMATFILE = 'format_file_path' ]
   [ [ , ] FIELDTERMINATOR = 'field_terminator' ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
    )]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

database_name 是指定的表或视图所在的数据库的名称。 如果未指定，则默认为当前数据库。

schema_name 是表或视图架构的名称。 如果用户执行批量导入操作的默认架构为指定表或视图的架构，则 *schema_name* 是可选的。 如果未指定 *schema* 且用户执行批量导入操作的默认架构不是指定表或视图的架构，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会返回一条错误消息，同时取消批量导入操作。

table_name 是要将数据批量导入其中的表或视图的名称。 只能使用所有列均引用相同基表的视图。 有关将数据加载到视图中的限制的详细信息，请参阅 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)。

' data_file ' 是数据文件的完整路径，该数据文件包含要导入到指定表或视图的数据。 使用 BULK INSERT 可以从磁盘或 Azure Blob 存储（包括网络、软盘、硬盘等）导入数据。

*data_file* 必须基于运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的服务器指定一个有效路径。 如果 *data_file* 为远程文件，则指定通用命名约定 (UNC) 名称。 UNC 名称采用以下格式：\\\\*系统名称*\\*共享名称*\\*路径*\\*文件名*。 例如：

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.dat';
```

**适用于：** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 和 Azure SQL 数据库。
从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 开始，data_file 可位于 Azure Blob 存储中。 在这种情况下，需要指定 data_source_name 选项。 有关示例，请参阅[从 Azure Blob 存储中的文件导入数据](#f-importing-data-from-a-file-in-azure-blob-storage)。

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

' data_source_name '
 适用对象： ：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 和 Azure SQL 数据库。
命名的外部数据源，指向将导入文件的 Azure Blob 存储位置。 外部数据源必须使用 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 中添加的 `TYPE = BLOB_STORAGE` 选项创建。 有关详细信息，请参阅 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)。 有关示例，请参阅[从 Azure Blob 存储中的文件导入数据](#f-importing-data-from-a-file-in-azure-blob-storage)。

BATCHSIZE =batch_size 指定批中的行数。 每个批处理作为一个事务复制到服务器。 如果复制操作失败，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将提交或回滚每个批处理的事务。 默认情况下，指定数据文件中的所有数据为一个批处理。 有关性能注意事项的信息，请参阅本主题后面的“备注”。

CHECK_CONSTRAINTS 指定在批量导入操作期间，必须检查所有对目标表或视图的约束。 若没有 CHECK_CONSTRAINTS 选项，则忽略所有 CHECK 和 FOREIGN KEY 约束，并在该操作后将表的约束标记为不可信。

> [!NOTE]
> 始终强制使用 UNIQUE 和 PRIMARY KEY 约束。 导入使用 NOT NULL 约束定义的字符列时，当文本文件中没有值时，BULK INSERT 插入一个空白字符串。

有时必须检查针对整个表的约束。 执行大容量导入操作之前，如果表不为空，则重新验证约束的代价可能会超出对增量数据应用 CHECK 约束的代价。

当输入数据包含违反约束的行时，您可能希望禁用约束（默认行为）。 禁用 CHECK 约束后，您可以导入数据并使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句删除无效数据。

> [!NOTE]
> MAXERRORS 选项不适用于约束检查。

CODEPAGE = { 'ACP' \| 'OEM' \| 'RAW' \| 'code_page' }：指定数据文件中数据的代码页        。 仅当数据含有字符值大于 **127** 或小于 **32** 的 **char**、**varchar** 或 **text** 列时，CODEPAGE 才适用。 有关示例，请参阅[指定代码页](#d-specifying-a-code-page)。

> [!IMPORTANT]
> Linux for [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 不支持 CODEPAGE 选项。 对于 [!INCLUDE[ssSQLv15_md](../../includes/sssqlv15-md.md)]，只能对 CODEPAGE 使用“RAW”选项。

> [!NOTE]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议在[格式化文件](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)中为每个列指定一个排序规则名称。

|CODEPAGE 值|说明|
|--------------------|-----------------|
|ACP|数据类型为 **char**、**varchar** 或 **text** 的列从 [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]/[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 代码页 (ISO 1252) 转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代码页。|
|OEM（默认值）|数据类型为 **char**、**varchar** 或 **text** 的列从系统 OEM 代码页转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代码页。|
|RAW|不进行从一个代码页到另一个代码页的转换；这是最快的选项。|
|*code_page*|特定的代码页码，例如 850。<br /><br /> &#42;&#42; 重要提示 &#42;&#42; 低于 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 的版本不支持代码页 65001（UTF-8 编码）。|
| &nbsp; | &nbsp; |

DATAFILETYPE = { 'char' \| 'native' \| 'widechar' \| 'widenative' }：指定 BULK INSERT 使用指定的数据文件类型值执行导入操作    。

&nbsp;

|DATAFILETYPE 值|所有数据都表示为：|
|------------------------|------------------------------|
|**char**（默认值）|字符格式。<br /><br /> 有关详细信息，请参阅[使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)。|
|**native**|本机（数据库）数据类型。 通过使用 **bcp** 实用工具从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 批量导入数据来创建本机数据文件。<br /><br /> 与 char 值相比，本机值提供更高的性能。 使用不包含任何扩展/双字节字符集 (DBCS) 字符的数据文件在多个 SQL Server 实例之间批量传输数据时，建议使用本机格式。<br /><br /> 有关详细信息，请参阅[使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)。|
|**widechar**|Unicode 字符。<br /><br /> 有关详细信息，请参阅 [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)。|
|**widenative**|本机（数据库）数据类型，**char**、**varchar** 和 **text** 列除外，这些列中的数据均以 Unicode 格式存储。 通过使用 **bcp** 实用工具从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 批量导入数据来创建 **widenative** 数据文件。<br /><br /> 与 **widechar** 相比，**widenative** 值可以提供更高的性能。 如果数据文件包含 [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)] 扩展字符，则指定 **widenative**。<br /><br /> 有关详细信息信息，请参阅 [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)。|
| &nbsp; | &nbsp; |

ERRORFILE ='file_name' 指定用于收集格式有误且不能转换为 OLE DB 行集的行的文件。 这些行将按原样从数据文件复制到此错误文件中。

错误文件是执行命令时创建的。 如果该文件已经存在，则会发生错误。 此外，还创建了一个扩展名为 .ERROR.txt 的控制文件。 此文件引用错误文件中的每一行并提供错误诊断。 纠正错误后即可加载数据。
**适用于：** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]CTP 1.1。
从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 开始，`error_file_path` 可位于 Azure Blob 存储中。

'errorfile_data_source_name' 适用范围：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]CTP 1.1。
是命名的外部数据源，指向错误文件的 Azure Blob 存储位置，该错误文件包含导入过程中发现的错误。 外部数据源必须使用 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 中添加的 `TYPE = BLOB_STORAGE` 选项创建。 有关详细信息，请参阅 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)。

FIRSTROW = first_row 指定要加载的第一行的行号。 默认值是指定数据文件中的第一行。 FIRSTROW 从 1 开始。

> [!NOTE]
> FIRSTROW 属性不可用于跳过列标题。 BULK INSERT 语句不支持跳过标题。 跳过行时，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]只考虑字段终止符，而不会对所跳过行的字段中的数据进行验证。

FIRE_TRIGGERS 指定将在批量导入操作期间执行目标表中定义的所有插入触发器。 如果为针对目标表的 INSERT 操作定义了触发器，则每次完成批处理操作时均激发触发器。

如果没有指定 FIRE_TRIGGERS，将不执行任何插入触发器。

FORMATFILE_DATA_SOURCE **=** 'data_source_name' **适用范围：** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 1.1。
命名的外部数据源，指向格式化文件（定义导入数据的架构）的 Azure Blob 存储位置。 外部数据源必须使用 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 中添加的 `TYPE = BLOB_STORAGE` 选项创建。 有关详细信息，请参阅 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)。

KEEPIDENTITY 指定导入数据文件中的标识值用于标识列。 如果没有指定 KEEPIDENTITY，则此列的标识值可被验证但不能导入，并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将根据创建表的过程中指定的种子值和增量值自动分配唯一值。 如果数据文件不包含该表或视图中标识列的值，请使用格式化文件指定在导入数据时跳过表或视图中的标识列；[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会自动为该列分配唯一的值。 有关详细信息，请参阅 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)。

有关如何保留标识值的详细信息，请参阅[批量导入数据时保留标识值 (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)。

KEEPNULLS 指定空列在批量导入操作期间应保留 Null 值，而不插入列的任何默认值。 有关详细信息，请参阅[在批量导入期间保留 Null 或使用默认值 (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)。

KILOBYTES_PER_BATCH = kilobytes_per_batch 将每个批处理中数据的近似千字节数 (KB) 指定为 kilobytes_per_batch。 默认情况下，KILOBYTES_PER_BATCH 是未知的。 有关性能注意事项的信息，请参阅本主题后面的“备注”。

LASTROW = last_row 指定要加载的最后一行的行号。 默认值为 0，表示指定数据文件中的最后一行。

MAXERRORS = max_errors 指定允许在数据中出现的最大语法错误数，超过该数量后将取消大容量导入操作。 大容量导入操作无法导入的每一行都将被忽略并且计为一个错误。 如果未指定 *max_errors*，则默认值为 10。

> [!NOTE]
> MAX_ERRORS 选项不适用于约束检查，也不适用于转换 **money** 和 **bigint** 数据类型。

ORDER ( { column [ ASC | DESC ] } [ ,... n ] ) 指定数据文件中数据的排序方式。 如果根据表中的聚集索引（如果有）对要导入的数据排序，则可提高批量导入的性能。 如果数据文件按不同于聚集索引键的顺序排序，或者该表没有聚集索引，则忽略 ORDER 子句。 提供的列名必须是目标表中有效的列名。 默认情况下，大容量插入操作假设数据文件未排序。 对于经过优化的批量导入， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还将验证导入的数据是否已排序。

n 是指示可以指定多个列的占位符。

ROWS_PER_BATCH = rows_per_batch 指示数据文件中近似的数据行数量。

默认情况下，数据文件中所有的数据都作为单一事务发送到服务器，批处理中的行数对于查询优化器是未知的。 如果指定了 ROWS_PER_BATCH（值 > 0），则服务器将使用此值优化批量导入操作。 为 ROWS_PER_BATCH 指定的值应当与实际行数大致相同。 有关性能注意事项的信息，请参阅本主题后面的“备注”。

TABLOCK 指定在批量导入操作持续时间内获取一个表级锁。 如果表没有索引并且指定了 TABLOCK，则该表可以同时由多个客户端加载。 默认情况下，锁定行为由表选项 **table lock on bulk load** 决定。 通过在大容量导入操作期间保留锁，可减少对表争用锁的情况，有时可显著提高性能。 有关性能注意事项的信息，请参阅本主题后面的“备注”。

对于列存储索引， 锁定行为有所不同，因为它在内部划分为多个行集。每个线程通过在行集上使用 X 锁，将数据专门加载到每个行集中，从而允许使用并发数据加载会话并行加载数据。 使用 TABLOCK 选项会导致线程在表上使用 X 锁（与传统行集的 BU 锁不同），这会阻止其他并发线程并发加载数据。

### <a name="input-file-format-options"></a>输入文件格式选项

FORMAT = 'CSV' 适用范围 ：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]CTP 1.1。
指定符合 [RFC 4180](https://tools.ietf.org/html/rfc4180) 标准的逗号分隔值文件。

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.csv'
WITH ( FORMAT='CSV');
```

FIELDQUOTE = 'field_quote' 适用范围 ：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]CTP 1.1。
指定将用作 CSV 文件引号字符的字符。 如果未指定，根据 [RFC 4180](https://tools.ietf.org/html/rfc4180) 标准中的定义，引号字符 (") 将用作引号字符。

FORMATFILE = 'format_file_path' 指定格式化文件的完整路径。 描述数据文件的格式化文件，数据文件中包含存储的响应，而存储的响应则是使用 **bcp** 实用工具在同一表或视图中创建的。 在下列情况下应使用格式化文件：

- 数据文件包含的列多于或少于表或视图包含的列。
- 列的顺序不同。
- 列分隔符不同。
- 数据格式有其他更改。 格式化文件通常使用 **bcp** 实用工具创建，并可根据需要使用文本编辑器修改。 有关详细信息，请参阅 [bcp 实用工具](../../tools/bcp-utility.md)和[创建格式化文件](../../relational-databases/import-export/create-a-format-file-sql-server.md)。

**适用于：** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 和 Azure SQL 数据库。
从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 开始，format_file_path 可位于 Azure Blob 存储中。

FIELDTERMINATOR ='field_terminator' 指定要用于 char 和 widechar 数据文件的字段终止符  。 默认字段终止符为 \t（制表符）。 有关详细信息，请参阅 [指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)。

ROWTERMINATOR ='row_terminator' 指定要用于 char 和 widechar 数据文件的行终止符  。 默认行终止符为 **\r\n**（换行符）。 有关详细信息，请参阅 [指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)。

## <a name="compatibility"></a>兼容性

BULK INSERT 将对从文件中读取的数据执行严格的数据验证和数据检查，在对无效数据执行这样的验证和检查时，可能导致现有脚本失败。 例如，BULK INSERT 验证：

- **float** 或 **real** 数据类型的本机表示形式是否有效。
- Unicode 数据的字节数是否为偶数。

## <a name="data-types"></a>数据类型

### <a name="string-to-decimal-data-type-conversions"></a>字符串到小数的数据类型转换

BULK INSERT 中使用的字符串到小数的数据类型转换与 [!INCLUDE[tsql](../../includes/tsql-md.md)] [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) 函数遵循相同的规则，该函数拒绝使用科学记数法表示数值的字符串。 因此，BULK INSERT 将此类字符串视为无效值并报告转换错误。

若要绕过此行为，可使用格式化文件将以科学记数法表示的 **float** 数据批量导入小数列中。 在格式化文件中，请将列显式描述为 **real** 或 **float** 数据。 有关这些数据类型的详细信息，请参阅 [float 和 real (Transact-SQL)](../../t-sql/data-types/float-and-real-transact-sql.md)。

> [!NOTE]
> 格式化文件将 **real** 数据表示为 **SQLFLT4** 数据类型，将 **float** 数据表示为 **SQLFLT8** 数据类型。 有关非 XML 格式化文件的详细信息，请参阅[使用 bcp 指定文件存储类型 (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)。

#### <a name="example-of-importing-a-numeric-value-that-uses-scientific-notation"></a>导入使用科学记数法的数值的示例

此示例使用下表：

```sql
CREATE TABLE t_float(c1 FLOAT, c2 DECIMAL (5,4));
```

 用户要将数据大容量导入 `t_float` 表中。 数据文件 C:\t_float-c.dat 包含以科学记数法表示的 **float** 数据，例如：

```input
8.0000000000000002E-28.0000000000000002E-2
```

但是，BULK INSERT 无法将此数据直接导入 `t_float`，原因是其第二个列 `c2` 使用的是 `decimal` 数据类型。 因此，必须使用格式化文件。 格式化文件必须将以科学记数法表示的 **float** 数据映射到列 `c2` 的小数格式。

以下格式化文件使用 `SQLFLT8` 数据类型将第二个数据字段映射到第二列：

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="30"/>
<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30"/> </RECORD> <ROW>
<COLUMN SOURCE="1" NAME="c1" xsi:type="SQLFLT8"/>
<COLUMN SOURCE="2" NAME="c2" xsi:type="SQLFLT8"/> </ROW> </BCPFORMAT>
```

若要使用此格式化文件（使用文件名 `C:\t_floatformat-c-xml.xml`）将测试数据导入测试表中，请发出下列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：

```sql
BULK INSERT bulktest..t_float
FROM 'C:\t_float-c.dat' WITH (FORMATFILE='C:\t_floatformat-c-xml.xml');
```

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

### <a name="data-types-for-bulk-exporting-or-importing-sqlxml-documents"></a>用于大容量导出或导入 SQLXML 文档的数据类型

若要大容量导出或导入 SQLXML 数据，请在格式化文件中使用下列数据类型之一：

|数据类型|效果|
|---------------|------------|
|SQLCHAR 或 SQLVARCHAR|在客户端代码页或排序规则隐含的代码页中发送数据。 效果与指定 DATAFILETYPE **='char'** 而不指定格式化文件相同。|
|SQLNCHAR 或 SQLNVARCHAR|以 Unicode 格式发送数据。 效果与指定 DATAFILETYPE **= 'widechar'** 而不指定格式化文件相同。|
|SQLBINARY 或 SQLVARBIN|不经任何转换即发送数据。|
| &nbsp; | &nbsp; |

## <a name="general-remarks"></a>一般备注

有关 BULK INSERT 语句、INSERT ...SELECT \* FROM OPENROWSET(BULK...) 语句和 **bcp** 命令，请参阅 [批量导入和导出数据 (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)。

有关准备数据以进行批量导入的信息，请参阅[准备用于批量导出或导入的数据 (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)。

BULK INSERT 语句可在用户定义的事务内执行，以便将数据导入到表或视图中。 或者，为了将多个匹配项用于大容量导入数据，事务可以在 BULK INSERT 语句中指定 BATCHSIZE 子句。 如果回滚某一多批处理事务，则回滚该事务已发送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的每个批处理。

## <a name="interoperability"></a>互操作性

### <a name="importing-data-from-a-csv-file"></a>从 CSV 文件导入数据

从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 开始，BULK INSERT 与 Azure SQL 数据库一样都支持 CSV 格式。
在 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 之前，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 批量导入操作不支持逗号分隔值 (CSV) 文件。 但是，在某些情况下，CSV 文件可在将数据大容量导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]时用作数据文件。 有关从 CSV 数据文件导入数据的要求，请参阅[准备用于批量导出或导入的数据 (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)。

## <a name="logging-behavior"></a>日志记录行为

 有关何时在事务日志中记录由批量导入 SQL Server 执行的行插入操作的信息，请参阅[批量导入的最小日志记录的先决条件](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)。 Azure SQL 数据库中不支持最小日志记录。

## <a name="restrictions"></a><a name="Limitations"></a> 限制

将格式文件用于 BULK INSERT 时，最多只能指定 1024 个字段。 这与表中允许的最大列数相同。 如果将带 BULK INSERT 的格式化文件与包含 1024 个字段以上的数据文件一起使用，BULK INSERT 将生成 4822 错误。 [bcp 实用工具](../../tools/bcp-utility.md)没有此限制，因此，对于包含 1024 个以上字段的数据文件，请使用不带格式化文件 BULK INSERT 或使用 bcp 命令。

## <a name="performance-considerations"></a>性能注意事项

如果要在单次批处理中刷新的页数超过了内部阈值，则可能会对缓冲池执行完全扫描，以识别要在批处理提交时刷新的页面。 此完全扫描可能会降低大容量导入操作的性能。 在将大型缓冲池与较慢的 I/O 子系统结合使用时，就可能出现超过内部阈值的情况。 若要避免大型机上的缓冲区溢出，请不要使用 TABLOCK 提示（将删除大容量优化），也不要使用较小的批大小（将保留大容量优化）。

由于计算机千差万别，因此我们建议在数据加载过程中测试各种批大小，以确定最佳方案。

使用 Azure SQL 数据库，如果要导入大量数据，请考虑在导入之前暂时提高数据库或实例的性能级别。

## <a name="security"></a>安全性

### <a name="security-account-delegation-impersonation"></a>安全帐户委托（模拟）

如果用户使用的是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，则系统将使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程帐户的安全配置文件。 使用 SQL Server 身份验证的登录名不能在数据库引擎外部进行身份验证。 因此，当 BULK INSERT 命令由使用 SQL Server 身份验证的登录名启动时，使用 SQL Server 进程帐户（SQL Server 数据库引擎服务使用的帐户）的安全上下文建立到数据的连接。 要成功读取源数据，您必须授予 SQL Server 数据库引擎使用的帐户访问源数据的权限。与此相反，如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户使用 Windows 身份验证登录，则该用户只能读取用户帐户可以访问的那些文件，而不考虑 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的安全配置文件。

如果在一台计算机上使用 **sqlcmd** 或 **osql** 执行 BULK INSERT 语句，将数据插入第二台计算机上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，同时使用 UNC 路径在第三台计算机上指定 *data_file*，可能会收到 4861 错误。

若要解决此问题，请使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证并指定一个使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程帐户安全配置文件的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，或者配置 Windows 以启用安全帐户委托。 有关如何使用户帐户可信以进行委托的信息，请参阅 Windows 帮助。

有关详细信息和使用 BULK INSERT 的安全注意事项，请参阅[使用 BULK INSERT 或 OPENROWSET (BULK...) 导入批量数据 (SQL Server)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)。

从 Azure Blob 存储导入数据且数据非公共数据（匿名访问）时，请基于使用 [MASTER KEY](create-master-key-transact-sql.md) 加密的 SAS 密钥创建一个 [DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)，然后创建一个[外部数据库源](../../t-sql/statements/create-external-data-source-transact-sql.md)以用于 BULK INSERT 命令。 有关示例，请参阅[从 Azure Blob 存储中的文件导入数据](#f-importing-data-from-a-file-in-azure-blob-storage)。

### <a name="permissions"></a>权限

需要 INSERT 和 ADMINISTER BULK OPERATIONS 权限。 在 Azure SQL 数据库中，需要 INSERT 和 ADMINISTER DATABASE BULK OPERATIONS 权限。 Linux 上的 SQL Server 不支持管理大容量操作权限或 bulkadmin 角色。 只有 `sysadmin` 才能对 Linux 上的 SQL Server 执行批量插入。 

另外，如果存在下列一种或多种情况，则还需要 ALTER TABLE 权限：

- 存在约束但未指定 CHECK_CONSTRAINTS 选项。

   > [!NOTE]
   > 禁用约束是默认行为。 若要显式检查约束，请使用 CHECK_CONSTRAINTS 选项。

- 存在触发器但未指定 FIRE_TRIGGER 选项。

   > [!NOTE]
   > 默认情况下，不激发触发器。 若要显式激发触发器，请使用 FIRE_TRIGGER 选项。

- 使用 KEEPIDENTITY 选项可以从数据文件中导入标识值。

## <a name="examples"></a>示例

### <a name="a-using-pipes-to-import-data-from-a-file"></a>A. 使用竖线从文件导入数据

下面的示例使用竖线 (`AdventureWorks2012.Sales.SalesOrderDetail`) 作为字段终止符，并使用 `|` 作为行终止符，将订单详细信息从指定的数据文件导入 `|\n` 表中。

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
      (  
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR =' |\n'
      );
```

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

### <a name="b-using-the-fire_triggers-argument"></a>B. 使用 FIRE_TRIGGER 参数

下面的示例指定 `FIRE_TRIGGERS` 参数。

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
     (
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR = ':\n'
         , FIRE_TRIGGERS
      );
```

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

### <a name="c-using-line-feed-as-a-row-terminator"></a>C. 使用换行符作为行终止符

 下面的示例将导入使用换行符作为行终止符的文件（如 UNIX 输出）：

```sql
DECLARE @bulk_cmd VARCHAR(1000);
SET @bulk_cmd = 'BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
FROM ''<drive>:\<path>\<filename>''
WITH (ROWTERMINATOR = '''+CHAR(10)+''')';
EXEC(@bulk_cmd);
```

> [!NOTE]
> 由于 Microsoft Windows 处理文本文件的方式，（ **\n** 自动替换为 **\r\n**）。

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

### <a name="d-specifying-a-code-page"></a>D. 指定代码页

以下示例显示如何指定代码页。

```sql
BULK INSERT MyTable
FROM 'D:\data.csv'
WITH
( CODEPAGE = '65001'
   , DATAFILETYPE = 'char'
   , FIELDTERMINATOR = ','
);
```

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

### <a name="e-importing-data-from-a-csv-file"></a>E. 从 CSV 文件导入数据

下面的示例演示如何指定 CSV 文件：跳过标头（第一行），使用 `;` 作为字段终止符，使用 `0x0a` 作为行终止符：

```sql
BULK INSERT Sales.Invoices
FROM '\\share\invoices\inv-2016-07-25.csv'
WITH (FORMAT = 'CSV'
      , FIRSTROW=2
      , FIELDQUOTE = '\'
      , FIELDTERMINATOR = ';'
      , ROWTERMINATOR = '0x0a');
```

> [!IMPORTANT]
> Azure SQL 数据库仅支持从 Azure blob 存储读取内容。

### <a name="f-importing-data-from-a-file-in-azure-blob-storage"></a>F. 从 Azure Blob 存储中的文件导入数据

下面的示例演示如何从已创建 SAS 密钥的 Azure Blob 存储位置中的 csv 文件加载数据。 Azure Blob 存储位置配置为外部数据源。 这需要使用共享访问签名的数据库范围的凭据，该签名通过用户数据库中的主密钥进行加密。

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
          LOCATION = 'https://****************.blob.core.windows.net/invoices'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (DATA_SOURCE = 'MyAzureBlobStorage');
```

> [!IMPORTANT]
> Azure SQL 仅支持从 Azure Blob 存储读取内容。

### <a name="g-importing-data-from-a-file-in-azure-blob-storage-and-specifying-an-error-file"></a>G. 从 Azure Blob 存储中的文件导入数据并指定错误文件

以下示例显示如何从 Azure Blob 存储位置（已配置为外部数据源且指定错误文件）中的 CSV 文件加载数据。 这要求提供使用共享访问签名的数据库作用域凭据。 请注意，如果在 Azure SQL 数据库上运行，则 ERRORFILE 选项应带有 ERRORFILE_DATA_SOURCE，否则导入可能会失败，出现权限错误。 ERRORFILE 中指定的文件不应存在于容器中。

```sql
BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (
         DATA_SOURCE = 'MyAzureInvoices'
         , FORMAT = 'CSV'
         , ERRORFILE = 'MyErrorFile'
         , ERRORFILE_DATA_SOURCE = 'MyAzureInvoices');
```

有关完整的 `BULK INSERT` 示例（包括配置凭据和外部数据源），请参阅[有关批量访问 Azure Blob 存储中数据的示例](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md)。

### <a name="additional-examples"></a>其他示例

 以下主题提供了其他 `BULK INSERT` 示例：

- [批量导入和导出 XML 文档的示例 (SQL Server)](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [批量导入数据时保留标识值 (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [在批量导入期间保留 Null 或使用默认值 (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)
- [使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)
- [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
- [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)
- [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>另请参阅

- [批量导入和导出数据 (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [bcp 实用工具](../../tools/bcp-utility.md)
- [用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)
- [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)
- [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)
- [准备用于批量导出或导入的数据 (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)
- [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)