---
description: CREATE EXTERNAL FILE FORMAT (Transact-SQL)
title: CREATE EXTERNAL FILE FORMAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/08/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL FILE FORMAT
- CREATE_EXTERNAL_FILE_FORMAT
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, file format
- PolyBase, external file format
ms.assetid: abd5ec8c-1a0e-4d38-a374-8ce3401bc60c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 363f3a4c17ccef494a9de53864dec05a0194c76a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439032"
---
# <a name="create-external-file-format-transact-sql"></a>CREATE EXTERNAL FILE FORMAT (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

创建用于定义存储在 Hadoop、Azure Blob 存储或 Azure Data Lake Store 的外部数据或用于与外部流相关的输入和输出流的外部文件格式对象。 创建外部文件格式是创建外部表的先决条件。 通过创建外部文件格式，可指定外部表引用的数据的实际布局。  
  
支持以下文件格式：
  
- 带分隔符的文本  
  
- Hive RCFile - 不适用于 Azure Synapse Analytics。
  
- Hive ORC
  
- Parquet

- JSON - 仅适用于 Azure SQL Edge。


若要创建外部表，请参阅 [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)。
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法
  
### <a name="delimited-text"></a>[带分隔符的文本](#tab/delimited)
```syntaxsql
-- Create an external file format for DELIMITED (CSV/TSV) files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
        FORMAT_TYPE = DELIMITEDTEXT  
    [ , FORMAT_OPTIONS ( <format_options> [ ,...n  ] ) ]  
    [ , DATA_COMPRESSION = {  
           'org.apache.hadoop.io.compress.GzipCodec'  
         | 'org.apache.hadoop.io.compress.DefaultCodec'  
        }  
     ]);

<format_options> ::=  
{  
    FIELD_TERMINATOR = field_terminator  
    | STRING_DELIMITER = string_delimiter 
    | First_Row = integer -- ONLY AVAILABLE FOR AZURE SYNAPSE ANALYTICS
    | DATE_FORMAT = datetime_format  
    | USE_TYPE_DEFAULT = { TRUE | FALSE } 
    | Encoding = {'UTF8' | 'UTF16'} 
}
```
### <a name="rc"></a>[RC](#tab/rc)
```syntaxsql
--Create an external file format for RC files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = {  
        'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'  
      | 'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'  
    }  
    [ , DATA_COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec' ]);
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

### <a name="orc"></a>[ORC](#tab/orc)
```syntaxsql  
--Create an external file format for ORC file.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (
         FORMAT_TYPE = ORC  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.DefaultCodec'      }  
    ]);  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

### <a name="parquet"></a>[Parquet](#tab/parquet)
```syntaxsql
--Create an external file format for PARQUET files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
         FORMAT_TYPE = PARQUET  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      }  
    ]);    
```
### <a name="json"></a>[JSON](#tab/json)
```syntaxsql
-- Create an external file format for JSON files.
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = JSON  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      
      | 'org.apache.hadoop.io.compress.DefaultCodec'  }  
    ]);  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

---
  
## <a name="arguments"></a>参数  
*file_format_name*  
为外部文件格式指定名称。
 
### <a name="format_type"></a>FORMAT_TYPE 
`FORMAT_TYPE = [ PARQUET | ORC | RCFILE | DELIMITEDTEXT]`

指定外部数据的格式。
  
- PARQUET 指定 Parquet 格式。
  
- ORC  
  指定优化行纵栏表 (ORC) 格式。 此选项需要外部 Hadoop 群集上的 Hive 版本 0.11 或更高版本。 在 Hadoop 中，ORC 文件格式可提供比 RCFILE 文件格式更好的压缩和性能。

- RCFILE（与 SERDE_METHOD = SERDE_method 结合使用）指定记录纵栏表文件格式 (RcFile)。 此选项需要指定 Hive 序列化程序和反序列化程序 (SerDe) 方法。 如果在 Hadoop 中使用 Hive/HiveQL 查询 RC 文件，则此要求是相同的。 请注意，SerDe 方法区分大小写。

  使用 PolyBase 支持的两种 SerDe 方法指定 RCFile 的示例。

  - FORMAT_TYPE = RCFILE, SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'

  - FORMAT_TYPE = RCFILE, SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'

- DELIMITEDTEXT 指定具有列分隔符（也称为字段终止符）的文本格式。
   
- JSON 指定 JSON 格式。 仅适用于 Azure SQL Edge。 

### <a name="data_compression"></a>DATA\_COMPRESSION
 `DATA_COMPRESSION = *data_compression_method*`  
 为外部数据指定数据压缩方法。 未指定 DATA_COMPRESSION 时，默认值是未压缩的数据。
若要正常工作，Gzip 压缩的文件必须具有“.gz”文件扩展名。
 
 #### <a name="delimited-text"></a>[带分隔符的文本](#tab/delimited)
 DELIMITEDTEXT 格式类型支持以下压缩方法：
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'

#### <a name="rc"></a>[RC](#tab/rc)
 RCFILE 格式类型支持以下压缩方法：
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
  
#### <a name="orc"></a>[ORC](#tab/orc)
 ORC 文件格式类型支持以下压缩方法：
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'

#### <a name="parquet"></a>[Parquet](#tab/parquet)
 PARQUET 文件格式类型支持以下压缩方法：
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'

#### <a name="json"></a>[JSON](#tab/json)
 JSON 文件格式类型支持以下压缩方法：
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
---  
### <a name="delimited-text-format-options"></a>“带分隔符的文本格式”选项

本部分中所述的格式选项都是可选的，仅适用于带分隔符的文本文件。

#### <a name="field_terminator"></a>FIELD_TERMINATOR
`FIELD_TERMINATOR = *field_terminator*`  
仅适用于带分隔符的文本文件。 字段终止符指定一个或多个字符，用于在带分隔符的文本文件中标记每个字段（列）的末尾。 默认值为竖线字符“|”。 若要获得有保证的支持，建议使用一个或多个 ascii 字符。
  
  
示例：  
  
-   FIELD_TERMINATOR = '|'  
  
-   FIELD_TERMINATOR = ' '  
  
-   FIELD_TERMINATOR = ꞌ\tꞌ  
  
-   FIELD_TERMINATOR = '~|~'  
  
#### <a name="string_delimiter"></a>STRING_DELIMITER
`STRING_DELIMITER = *string_delimiter*`

为带分隔符的文本文件中的字符串类型数据指定字段终止符。 字符串分隔符的长度是一个或多个字符，并且用单引号括起来。 默认值是空字符串 ""。 若要获得有保证的支持，建议使用一个或多个 ascii 字符。
 
  
 示例：  

-   STRING_DELIMITER = '"'

-   STRING_DELIMITER = '0x22' -- 双引号十六进制
  
-   STRING_DELIMITER = '*'  
  
-   STRING_DELIMITER = ꞌ,ꞌ  
  
-   STRING_DELIMITER = '0x7E0x7E'  -- 两个波形符（例如 ~~）
 
#### <a name="first_row"></a>FIRST_ROW
 `FIRST_ROW = *First_row_int*`  
指定在 PolyBase 加载过程中在所有文件中首先读取的行号。 此参数可以采用值 1-15。 如果值设置为二，则在加载数据时，会跳过每个文件中的第一行（标头行）。 会基于行终止符（/r/n、/r、/n）的存在跳过行。 当此选项用于导出时，行会添加到数据中以确保可以在无数据丢失的情况下读取文件。 如果值设置为 >2，则导出的第一行是外部表的列名。

#### <a name="date_format"></a>DATE\_FORMAT
 `DATE_FORMAT = *datetime_format*`  
为可能出现在带分隔符的文本文件中的所有日期和时间数据指定自定义格式。 如果源文件使用默认日期时间格式，则无需此选项。 每个文件只允许使用一个自定义日期时间格式。 不能对每个文件指定多个自定义日期时间格式。 但是，如果每个日期时间格式是外部表定义中各自数据类型的默认格式，则可以使用多个日期时间格式。

> [!IMPORTANT]
> PolyBase 仅使用自定义日期格式来导入数据。 它不使用自定义格式将数据写入到外部文件中。

 当 DATE_FORMAT 未指定或是空字符串时，PolyBase 会使用以下默认格式：
  
-   DateTime：'yyyy-MM-dd HH:mm:ss'  
  
-   SmallDateTime：'yyyy-MM-dd HH:mm'  
  
-   Date：'yyyy-MM-dd'  
  
-   DateTime2：'yyyy-MM-dd HH:mm:ss'  
  
-   DateTimeOffset：'yyyy-MM-dd HH:mm:ss'  
  
-   时间：'HH:mm:ss'  

> [!IMPORTANT]
> 指定自定义 `DATE_FORMAT` 将覆盖所有默认类型格式。 这意味着，你将需要让文件中的所有日期时间、日期和时间单元中具有相同的日期格式。 使用重写 `DATE_FORMAT`，不能有不同格式的日期和时间值。

下表中显示了 **示例日期格式**：
  
有关该表的说明：  
  
-   年、月和日可以具有各种格式和顺序。 该表仅显示 **ymd** 格式。 月可以具有一位或两位数字，或是三个字符。 天可以具有一位或两位数字。 年可以具有两位或四位数字。
  
-   毫秒 (fffffff) 不是必需的。
  
-   Am、pm (tt) 不是必需的。 默认值为 AM。
  
|日期类型|示例|说明|  
|---------------|-------------|-----------------|  
|DateTime|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fff'|除了年、月和日之外，此日期格式包括 00-24 小时、00-59 分钟、00-59 秒以及用于毫秒的 3 位数字。|  
|DateTime|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffftt'|除了年、月和日之外，此日期格式包括 00-12 小时、00-59 分钟、00-59 秒、用于毫秒的 3 位数字以及 AM、am、PM 或 pm。 |  
|SmallDateTime|DATE_FORMAT =  'yyyy-MM-dd HH:mm'|除了年、月和日之外，此日期格式包括 00-23 小时、00-59 分钟。|  
|SmallDateTime|DATE_FORMAT =  'yyyy-MM-dd hh:mmtt'|除了年、月和日之外，此日期格式包括 00-11 小时、00-59 分钟、无秒以及 AM、am、PM 或 pm。|  
|Date|DATE_FORMAT =  'yyyy-MM-dd'|年、月和日。 不包括任何时间元素。|  
|Date|DATE_FORMAT = 'yyyy-MMM-dd'|年、月和日。 当使用 3 个 M 指定月时，输入值是一，或字符串 Jan、Feb、Mar、Apr、May、Jun、Jul、Aug、Sep、Oct、Nov 或 Dec。|  
|DateTime2|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fffffff'|除了年、月和日之外，此日期格式包括 00-23 小时、00-59 分钟、00-59 秒以及用于毫秒的 7 位数字。|  
|DateTime2|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffffffftt'|除了年、月和日之外，此日期格式包括 00-11 小时、00-59 分钟、00-59 秒、用于毫秒的 7 位数字以及 AM、am、PM 或 pm。|  
|DateTimeOffset|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fffffff zzz'|除了年、月和日之外，此日期格式包括 00-23 小时、00-59 分钟、00-59 秒和用于毫秒的 7 位数字，在输入文件中以 `{+&#124;-}HH:ss` 形式输入的时区偏移。 例如，因为没有夏令时的洛杉矶比 UTC 落后 8 小时，所以输入文件中的值 -08:00 会指定洛杉矶的时区。|  
|DateTimeOffset|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffffffftt zzz'|除了年、月和日之外，此日期格式包括 00-11 小时、00-59 分钟、00-59 秒、用于毫秒的 7 位数字、（AM、am、PM 或 pm）以及时区偏移。 请参阅上一行中的说明。|  
|时间|DATE_FORMAT = 'HH:mm:ss'|没有日期值，只有 00-23 小时、00-59 分钟和 00-59 秒。|  
  
 ##### <a name="supported-date-and-time-formats"></a>支持的日期和时间格式
 
 外部文件格式可以描述大量日期和时间格式：
  
|datetime|smalldatetime|date|datetime2|datetimeoffset|  
|--------------|-------------------|----------|---------------|--------------------|  
|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fff]|[M[M]]M-[d]d-[yy]yy HH:mm[:00]|[M[M]]M-[d]d-[yy]yy|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fffffff]|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fffffff] zzz|  
|[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fff][tt]|[M[M]]M-[d]d-[yy]yy hh:mm[:00][tt]||[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fffffff][tt]|[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fffffff][tt] zzz|  
|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fff]|[M[M]]M-[yy]yy-[d]d HH:mm[:00]|[M[M]]M-[yy]yy-[d]d|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fffffff]|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fffffff] zzz|  
|[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fff][tt]|[M[M]]M-[yy]yy-[d]d hh:mm[:00][tt]||[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fffffff][tt]|[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fffffff][tt] zzz|  
|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fff]|[yy]yy-[M[M]]M-[d]d HH:mm[:00]|[yy]yy-[M[M]]M-[d]d|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fffffff]|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fffffff]  zzz|  
|[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fff][tt]|[yy]yy-[M[M]]M-[d]d hh:mm[:00][tt]||[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fffffff][tt]|[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fffffff][tt] zzz|  
|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fff]|[yy]yy-[d]d-[M[M]]M HH:mm[:00]|[yy]yy-[d]d-[M[M]]M|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fffffff]|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fffffff]  zzz|  
|[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fff][tt]|[yy]yy-[d]d-[M[M]]M hh:mm[:00][tt]||[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fffffff][tt]|[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fffffff][tt] zzz|  
|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fff]|[d]d-[M[M]]M-[yy]yy HH:mm[:00]|[d]d-[M[M]]M-[yy]yy|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fffffff]|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fffffff] zzz|  
|[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fff][tt]|[d]d-[M[M]]M-[yy]yy hh:mm[:00][tt]||[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fffffff][tt]|[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fffffff][tt] zzz|  
|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fff]|[d]d-[yy]yy-[M[M]]M HH:mm[:00]|[d]d-[yy]yy-[M[M]]M|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fffffff]|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fffffff]  zzz|  
|[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fff][tt]|[d]d-[yy]yy-[M[M]]M hh:mm[:00][tt]||[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fffffff][tt]|[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fffffff][tt] zzz|  
  
 详细信息：  
  
-   若要分隔月、日和年值，可以使用“-”、“/”或“.”。 为简单起见，该表仅使用“-”分隔符。
  
-   若要将月指定为文本，请使用三个或更多字符。 使用一个或两个字符的月会解释为数字。
  
-   若要分隔时间值，请使用“:”符号。
  
-   括在方括号中的字母是可选的。
  
-   字母“tt”指定 [AM|PM|am|pm]。 AM 是默认值。 当指定“tt”时，小时值 (hh) 必须在 0 到 12 范围内。
  
-   字母“zzz”以格式 {+|-}HH:ss] 为系统的当前时区指定时区偏移。
 
#### <a name="use_type_default"></a>USE_TYPE_DEFAULT 
 `USE_TYPE_DEFAULT = { TRUE | FALSE }`
 
 指定在 PolyBase 从文本文件中检索数据时如何处理带分隔符的文本文件中的缺失值。
  
 TRUE  
 从文本文件中检索数据时，使用外部表定义中对应列的数据类型的默认值来存储每个缺失值。 例如，将缺失值替换为：  
  
-   如果列定义为数字列，则替换为 0。 不支持十进制列，容易出错。
  
-   如果列是字符串列，则替换为空字符串。
  
-   如果列是日期列，则替换为 1900-01-01。
  
 **FALSE**  
 将所有缺失值存储为 NULL。 在带分隔符的文本文件中使用 NULL 一词存储的任何 NULL 值都会作为字符串“NULL”导入。
 
#### <a name="encoding"></a>ENCODING
   `Encoding = {'UTF8' | 'UTF16'}`
   
 在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] (APS CU7.4) 中，PolyBase 可以读取 UTF8 和 UTF16-LE 编码的带分隔符的文本文件。 在 SQL Server 中，PolyBase 不支持读取 UTF16 编码的文件。

## <a name="permissions"></a>权限  
 需要 ALTER ANY EXTERNAL FILE FORMAT 权限。
  
## <a name="general-remarks"></a>一般备注
 外部文件格式在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]中适用于数据库范围。 它在[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]中适用于服务器范围。  
  
 格式选项都是可选的，仅适用于带分隔符的文本文件。  
  
 当数据采用压缩格式之一进行存储时，PolyBase 会在返回数据记录之前先解压缩数据。
  
## <a name="limitations-and-restrictions"></a>限制和局限
  
 带分隔符的文本文件中的行分隔符必须受 Hadoop 的 LineRecordReader 支持。 也就是说，它必须是“\r”、“\n”或“\r\n”。 这些分隔符不可由用户配置。
  
 受支持的 SerDe 方法与 RCFile 的组合以及受支持的数据压缩方法已在本文前面列出。 并非所有组合都受支持。
  
 并发 PolyBase 查询的最大数量是 32。 当运行 32 个并发查询时，每个查询可以从外部文件位置最多读取 33,000 个文件。 根文件夹和每个子文件夹也计为文件。 如果并发度小于 32，则外部文件位置可以包含 33,000 个以上的文件。
  
 由于外部表中文件数方面的限制，因此建议将 30,000 个以下的文件存储在外部文件位置的根和子文件夹中。 此外，建议使根目录下的子文件夹数保留为较小数字。 当引用太多文件时，可能会发生 Java 虚拟机内存不足异常。
  
  通过 PolyBase 将数据导出到 Hadoop 或 Azure Blob 存储时，只会导出数据，而不会导出 CREATE EXTERNAL TABLE 命令中定义的列名（元数据）。

## <a name="locking"></a>锁定  
 在 EXTERNAL FILE FORMAT 对象上采用共享锁。
  
## <a name="performance"></a>性能
 使用压缩文件时始终需要在传输更少的数据（在外部数据源与 SQL Server 之间）与提高 CPU 使用率来压缩和解压缩数据之间进行权衡。
  
 Gzip 压缩文本文件不可拆分。 若要提高 Gzip 压缩文本文件的性能，建议生成全部存储在外部数据源中同一目录中的多个文件。 此文件结构使 PolyBase 可以使用多个读取器和解压缩进程更快地读取和解压缩数据。 理想的压缩文件数是每个计算节点的最大数据读取器进程数。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中，最大数据读取器进程数为每节点 8 个，但 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] Gen2 除外，它的每个节点为 20 个读取器。 在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]中，每个节点的最大数据读取器进程数因 SLO 而异。 有关详细信息，请参阅 [[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 加载模式和策略](https://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-create-a-delimitedtext-external-file-format"></a>A. 创建 DELIMITEDTEXT 外部文件格式  
 此示例为带分隔符的文本文件创建名为 textdelimited1 的外部文件格式。 为 FORMAT\_OPTIONS 列出的选项指定应使用竖线字符“|”分隔文件中的字段。 文本文件也使用 Gzip 编解码器进行压缩。 如果未指定 DATA\_COMPRESSION，则文本文件未压缩。
  
 对于带分隔符的文本文件，数据压缩方法可以是默认编解码器“org.apache.hadoop.io.compress.DefaultCodec”或 Gzip 编解码器“org.apache.hadoop.io.compress.GzipCodec”。
  
```sql  
CREATE EXTERNAL FILE FORMAT textdelimited1  
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT,  
    FORMAT_OPTIONS (  
        FIELD_TERMINATOR = '|',  
        DATE_FORMAT = 'MM/dd/yyyy' ),  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
);  
```  
  
### <a name="b-create-an-rcfile-external-file-format"></a>B. 创建 RCFile 外部文件格式  
 此示例为使用序列化/反序列化方法 org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe 的 RCFile 创建外部文件格式。 它还指定将默认编解码器用于数据压缩方法。 如果未指定 DATA_COMPRESSION，则默认为无压缩。
  
```sql  
CREATE EXTERNAL FILE FORMAT rcfile1  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe',  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'  
);  
```  
  
### <a name="c-create-an-orc-external-file-format"></a>C. 创建 ORC 外部文件格式  
 此示例为使用 org.apache.io.compress.SnappyCodec 数据压缩方法压缩数据的 ORC 文件创建外部文件格式。 如果未指定 DATA_COMPRESSION，则默认为无压缩。
  
```sql 
CREATE EXTERNAL FILE FORMAT orcfile1  
WITH (  
    FORMAT_TYPE = ORC,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  
  
### <a name="d-create-a-parquet-external-file-format"></a>D. 创建 PARQUET 外部文件格式  
 此示例为使用 org.apache.io.compress.SnappyCodec 数据压缩方法压缩数据的 Parquet 文件创建外部文件格式。 如果未指定 DATA_COMPRESSION，则默认为无压缩。  
  
```sql  
CREATE EXTERNAL FILE FORMAT parquetfile1  
WITH (  
    FORMAT_TYPE = PARQUET,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  
### <a name="e-create-a-delimited-text-file-skipping-header-row-azure-synapse-analytics-only"></a>E. 创建带分隔符的文本文件，跳过标头行（仅限 Azure Synapse Analytics）
 此示例为具有单个标头行的 CSV 文件创建外部文件格式。 
  
```sql  
CREATE EXTERNAL FILE FORMAT skipHeader_CSV
WITH (FORMAT_TYPE = DELIMITEDTEXT,
      FORMAT_OPTIONS(
          FIELD_TERMINATOR = ',',
          STRING_DELIMITER = '"',
          FIRST_ROW = 2, 
          USE_TYPE_DEFAULT = True)
)
```   
### <a name="f-create-a-json-external-file-format"></a>F. 创建 JSON 外部文件格式  
 此示例为使用 org.apache.io.compress.SnappyCodec 数据压缩方法压缩数据的 JSON 文件创建外部文件格式。 如果未指定 DATA_COMPRESSION，则默认为无压缩。 此示例适用于 Azure SQL Edge，目前不支持其他 SQL 产品。 
  
```sql  
CREATE EXTERNAL FILE FORMAT jsonFileFormat  
WITH (  
    FORMAT_TYPE = JSON,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  

## <a name="see-also"></a>另请参阅
 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
 [sys.external_file_formats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)  
