---
title: PolyBase 的类型映射 | Microsoft Docs
description: 有关 PolyBase 外部数据源与 SQL Server 之间的映射，请参阅这些表。 使用 Transact-SQL CREATE EXTERNAL TABLE 定义外部表。
ms.date: 09/24/2018
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
ms.openlocfilehash: ddb0c0ac0c1afd4e4a3f94504256fa787dceaf9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079998"
---
# <a name="type-mapping-with-polybase"></a>PolyBase 的类型映射

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

本文介绍 PolyBase 外部数据源与 SQL Server 之间的映射。 可以使用此信息通过 [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) Transact-SQL 命令正确定义外部表。

## <a name="overview"></a>概述

使用 PolyBase 创建外部表时，列定义（包括数据类型和列数）必须与外部文件中的数据匹配。 如果存在不匹配，则在查询实际数据时会拒绝文件行。  
  
对于引用外部数据源中的文件的外部表，列和类型定义必须映射到外部文件的确切架构。 定义引用 Hadoop/Hive 中存储的数据的数据类型时，可在 SQL 与 Hive 数据类型之间使用以下映射，并在从中进行选择时将类型强制转换为 SQL 数据类型。 除非另有说明，否则类型包括 Hive 的所有版本。

> [!NOTE]  
> 在任何转换中，SQL Server 都不支持 Hive 无穷大  数据值。 PolyBase 会失败，并出现数据类型转换错误。

## <a name="hadoop-type-mapping-reference"></a>Hadoop 类型映射引用

| SQL 数据类型 | .NET 数据类型            | Hive 数据类型 | Hadoop/Java 数据类型 | 注释                       |
| ------------- | ------------------------- | -------------- | --------------------- | ------------------------------ |
| tinyint       | Byte                      | tinyint        | ByteWritable          | 仅用于无符号数字。     |
| smallint      | Int16                     | smallint       | ShortWritable         |
| int           | Int32                     | int            | IntWritable           |
| bigint        | Int64                     | bigint         | LongWritable          |
| bit           | Boolean                   | boolean        | BooleanWritable       |
| FLOAT         | Double                    | double         | DoubleWritable        |
| real          | Single                    | FLOAT          | FloatWritable         |
| money         | Decimal                   | double         | DoubleWritable        |
| smallmoney    | Decimal                   | double         | DoubleWritable        |
| nchar         | String<br /><br /> Char[] | 字符串         | Varchar               |
| nvarchar      | String<br /><br /> Char[] | 字符串         | Varchar               |
| char          | String<br /><br /> Char[] | 字符串         | Varchar               |
| varchar       | String<br /><br /> Char[] | 字符串         | Varchar               |
| binary        | Byte[]                    | binary         | BytesWritable         | 适用于 Hive 0.8 及更高版本。 |
| varbinary     | Byte[]                    | binary         | BytesWritable         | 适用于 Hive 0.8 及更高版本。 |
| date          | DateTime                  | timestamp      | TimestampWritable     |
| smalldatetime | DateTime                  | timestamp      | TimestampWritable     |
| datetime2     | DateTime                  | timestamp      | TimestampWritable     |
| datetime      | DateTime                  | timestamp      | TimestampWritable     |
| time          | TimeSpan                  | timestamp      | TimestampWritable     |
| Decimal       | Decimal                   | Decimal        | BigDecimalWritable    | 适用于 Hive 0.11 及更高版本。 |

<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

## <a name="oracle-type-mapping-reference"></a>Oracle 类型映射引用

| Oracle 数据类型 | SQL Server 类型 | 
| -------------    | --------------- |
|Float             |Float            |
|NUMBER            |Float            |
|NUMBER (p,s)      |Decimal (p, s)   |
|LONG              |nvarchar         |
|BINARY_FLOAT      |Real             | 
|BINARY_DOUBLE     |Float            | 
|CHAR              |Char             |
|VARCHAR2          |Varchar          | 
|NVARCHAR2         |nvarchar         | 
|RAW               |Varbinary        |
|LONG RAW          |Varbinary        | 
|BLOB              |Varbinary        |
|CLOB              |Varchar          |
|NCLOB             | nvarchar        | 
|ROWID             |Varchar          |
|UROWID            |Varchar          | 
|DATE              |Datetime2        |
|TIMESTAMP         |Datetime2        | 

类型不匹配 

Float：Oracle 支持的浮点精度为 126，低于 SQL Server 支持的精度 (53)。 因此，可以直接映射 Float (1-53)，但除此之外，截断会导致数据丢失。

时间戳：Oracle 中的时间戳和具有当地时区的时间戳支持 0.9 秒的精度，而 SQL Server DateTime2 仅支持 0.7 秒的精度。 




## <a name="mongodb-type-mapping"></a>MongoDB 类型映射

| BSON 数据类型     | SQL Server 类型 |
| ------------------ | --------------- |
| Double             | Float           |
| String             | nvarchar        |
| Binary data        | nvarchar        |
| 对象 ID          | nvarchar        |
| 布尔            | bit             |
| Date               | Datetime2       |
| 32-bit integer     | int             |
| Timestamp          | nvarchar        |
| 64 位整数     | BigInt          |
|Decimal 128         | Decimal         | 
| DBPointer          | nvarchar        |
| Javascript         | nvarchar        |
| Max Key            | nvarchar        |
| Min Key            | nvarchar        |
| 符号             | nvarchar        |
| Regular Expression | nvarchar        |
| Undefined/NULL     | nvarchar        |


MongoDB 使用 BSON 文档来存储数据记录。 与之前的方案不同，BSON 是无架构的，并且支持在其他文档中嵌入文档和数组。 这为用户提供了灵活性。 


## <a name="teradata-type-mapping-reference"></a>Teradata 类型映射引用

| Teradata 数据类型 | SQL Server 类型 | 
| -------------      | -------------   |
|INTEGER             |int              |
|SMALLINT            |SmallInt         |
|BIGINT              |BigInt           |
|BYTEINT             |SmallInt         |
|DECIMAL             |Decimal          |
|FLOAT               |Decimal          |
|BYTE                |二进制           |
|VARBYTE             |Varbinary        |
|BLOB                |varbinary        |
|CHAR                |Nchar            |
|CLOB                |nvarchar         |
|VARCHAR             |nvarchar         |
|Graphic             |Nchar            |
|JSON                |nvarchar         |
|VARGRAPHIC          |nvarchar         |
|DATE                |Date             |
|TIMESTAMP           |Datetime2        |
|TIME                |时间             |
|TIME WITH TIME ZONE |时间             |
|TIMESTAMP WITH TIME ZONE|时间         |

::: moniker-end

## <a name="next-steps"></a>后续步骤

有关如何使用它的更多信息，请参阅 [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) 的 Transact-SQL 参考文章。
