---
description: CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)
title: CREATE EXTERNAL TABLE AS SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.subservice: design
ms.topic: conceptual
f1_keywords:
- CREATE EXTERNAL TABLE AS SELECT
- CREATE_EXTERNAL_TABLE_AS_SELECT
- CREATE EXTERNAL TABLE AS SELECT_TSQL
helpviewer_keywords:
- External, table
- PolyBase, external table
- External, table create as select
- PolyBase, create table as select
ms.assetid: 32dfe254-6df7-4437-bfd6-ca7d37557b0a
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 1085686f4c83198a043855e701ec2ef38d17541f
ms.sourcegitcommit: d35d0901296580bfceda6e0ab2e14cf2b7e99a0f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92496940"
---
# <a name="create-external-table-as-select-transact-sql"></a>CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  创建一个外部表，然后将 [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT 语句的结果并行导出到 Hadoop 或 Azure Blob 存储。

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql 
CREATE EXTERNAL TABLE [ [database_name  . [ schema_name ] . ] | schema_name . ] table_name   
    WITH (   
        LOCATION = 'hdfs_folder',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
    AS <select_statement>  
[;]  
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage  
    | REJECT_VALUE = reject_value  
    | REJECT_SAMPLE_VALUE = reject_sample_value  
}  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```

## <a name="arguments"></a>参数
 [ [ database_name . [ schema_name ] . ] | schema_name . ] table_name 是要在数据库中创建的表的一部分到三部分的名称。 对于外部表，只有表元数据存储在关系数据库中。 

 LOCATION =  'hdfs_folder' 在外部数据源上指定写入 SELECT 语句结果的位置。 位置是文件夹名称，可选择性地包含相对于 Hadoop 群集或 Blob 存储的根文件夹的路径。 如果尚未存在，PolyBase 将创建路径和文件夹。

外部文件写入 hdfs_folder 并命名为 QueryID_date_time_ID.format，其中 ID 是增量标识符，format 是导出的数据格式   。 例如，QID776_20160130_182739_0.orc。

 DATA_SOURCE = external_data_source_name 指定包含外部数据存储或将存储的位置的外部数据源对象的名称。 位置是 Hadoop 群集或 Azure Blob 存储。 若要创建外部数据源，请使用 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)。

 FILE_FORMAT = external_file_format_name 指定包含外部数据文件格式的外部文件格式对象的名称。 若要创建外部文件格式，请使用 [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](../../t-sql/statements/create-external-file-format-transact-sql.md)。

 运行 CREATE EXTERNAL TABLE AS SELECT 语句时，REJECT 选项不适用。 而是在此处指定，以便数据库稍后可从外部表导入数据时使用它们。 稍后，当 CREATE TABLE AS SELECT 语句从外部表中选择数据时，数据库将使用拒绝选项以确定在停止导入前无法导入的行的数量或百分比。

   - REJECT_VALUE = reject_value 指定在数据库暂停导入前无法导入的行的值或百分比。

   - REJECT_TYPE = value | percentage 说明 REJECT_VALUE 选项是指定为文本值还是百分比。

      - 如果 REJECT_VALUE 是文本值而不是百分比，则使用 Value 。 当失败行数超过 reject_value 时，数据库将停止从外部数据文件导入行。

        例如，如果 REJECT_VALUE = 5 且 REJECT_TYPE = value，数据库将在导入五行失败后停止导入行。

      - 如果 REJECT_VALUE 为百分比而不是文本值，则使用 percentage。 当失败行的百分比超过 reject_value 时，数据库将停止从外部数据文件导入行 。 每隔一段时间计算失败行的百分比。

   - 当 REJECT_TYPE = percentage 时，需要 REJECT_SAMPLE_VALUE = reject_sample_value，它指定在数据库重新计算失败行的百分比之前尝试导入的行数。

      例如，如果 REJECT_SAMPLE_VALUE = 1000，数据库将在尝试从外部数据文件导入 1000 行后计算失败行的百分比。 如果失败行的百分比小于 reject_value，数据库将尝试加载另外 1000 行。 数据库在尝试导入每个其他 1000 行后继续重新计算失败行的百分比。

     > [!NOTE]
     >  由于数据库按间隔计算失败行的百分比，因此失败行的实际百分比可能会超过 reject_value。

     **示例：**

     此示例演示三个 REJECT 选项相互之间如何交互。 例如，如果 REJECT_TYPE = percentage、REJECT_VALUE = 30、REJECT_SAMPLE_VALUE = 100，可能出现以下情况：

     - 数据库尝试加载前 100 行，其中 25 行失败，75 行成功。
     - 失败行的百分比计算结果为 25%，小于 30% 的拒绝值。 因此，无需暂停加载。
     - 数据库尝试加载下一个 100 行。 这次 25 行成功，75 行失败。
     - 重新计算的失败行的百分比为 50%。 失败行的百分比已超过 30% 的拒绝值。
     - 尝试加载 200 行后，加载失败的行数为 50%，大于指定的 30% 限制。

 WITH common_table_expression 指定临时命名的结果集，这些结果集称为公用表表达式 (CTE)。 有关详细信息，请参阅 [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md) 

 SELECT \<select_criteria> 使用 SELECT 语句的结果填充新表。 select_criteria 是 SELECT 语句的主体，用于确定将哪些数据复制到新表中。 有关 SELECT 语句的信息，请参阅 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)。

## <a name="permissions"></a>权限

 若要运行此命令，数据库用户需要所有这些权限或成员身份：

- 本地架构上的 ALTER SCHEMA 权限，将包含 db_ddladmin 固定数据库角色中的新表或成员身份 。
- CREATE TABLE 权限或 db_ddladmin 固定数据库角色的成员身份 。
- select_criteria 中引用的任何对象的 SELECT 权限。

 登录名需要所有这些权限：

- **ADMINISTER BULK OPERATIONS**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- 写入权限能读写 Hadoop 集群上或 Blob 存储中的外部文件夹。

 > [!IMPORTANT]
 >  ALTER ANY EXTERNAL DATA SOURCE 权限授予任何主体创建和修改任何外部数据源对象的能力，因此，它还授予访问数据库上所有数据库作用域凭据的能力。 必须将此权限视为高度特权，并且必须仅授予系统中受信任的主体。

## <a name="error-handling"></a>错误处理。
 当 CREATE EXTERNAL TABLE AS SELECT 将数据导出到文本分隔文件时，没有无法导出的行的拒绝文件。

 创建外部表时，数据库将尝试连接到外部 Hadoop 群集或 Blob 存储。 如果连接失败，该命令将失败且不会创建外部表。 由于数据库重新尝试连接至少三次，因此需要一分钟或更多时间命令才会失败。

 如果 CREATE EXTERNAL TABLE AS SELECT 取消或失败，数据库将一次性尝试删除已在外部数据源上创建的任何新文件和文件夹。

 在数据导出期间，数据库将报告外部数据源上发生的任何 Java 错误。

##  <a name="general-remarks"></a><a name="GeneralRemarks"></a> 一般备注
 在 CREATE EXTERNAL TABLE AS SELECT 语句结束后，可在外部表上运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询。 除非使用 CREATE TABLE AS SELECT 语句进行导入，否则这些操作将在查询期间将数据导入数据库。

 外部表名和定义存储在数据库元数据中。 数据存储在外部数据源中。

 外部文件被命名为 *QueryID_date_time_ID.format* ，其中 *ID* 是增量标识符， *format* 是导出的数据格式。 例如，QID776_20160130_182739_0.orc。

 即使已对源表进行分区，CREATE EXTERNAL TABLE AS SELECT 语句也会始终创建一个非分区表。

 对于使用 EXPLAIN 创建的查询计划，数据库将这些查询计划操作用于外部表：

- 外部随机移动
- 外部广播移动
- 外部分区移动

 **适用于：** 并行数据仓库

作为创建外部表的先决条件，设备管理员需要配置 Hadoop 连接。 有关详细信息，请参阅 Analytics Platform System 文档中的“配置与外部数据的连接 (Analytics Platform System)”，可从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=48241)下载该文档。

## <a name="limitations-and-restrictions"></a>限制和局限
 由于外部表数据驻留在数据库之外，所以备份和还原操作将仅对存储在数据库中的数据进行操作。 因此，仅备份和还原元数据。

 还原包含外部表的数据库备份时，数据库不会验证与外部数据源的连接。 如果原始源不可访问，外部表的元数据仍会还原成功，但外部表上的 SELECT 操作将失败。

 数据库不保证数据库与外部数据之间的数据一致性。 客户全权负责维护外部数据和数据库之间的一致性。

 外部表上不支持数据操作语言 (DML) 操作。 例如，不能使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 更新、插入或删除 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句来修改外部数据。

 CREATE TABLE、DROP TABLE、CREATE STATISTICS、DROP STATISTICS、CREATE VIEW 和 DROP VIEW 是外部表中允许的唯一数据定义语言 (DDL) 操作。

 运行 32 个并发 PolyBase 查询时，每个文件夹中 PolyBase 最多可使用 33,000 个文件。 此最大数量包括每个 HDFS 文件夹中的文件和子文件夹。 如果并发度小于 32，用户可以针对 HDFS 中包含超过 33,000 个文件的文件夹运行 PolyBase 查询。 我们建议 Hadoop 和 PolyBase 的用户保持文件路径简短，并且每个 HDFS 文件夹不超过 30,000 个文件。 当引用太多文件时，会发生 JVM 内存不足异常。

 [SET ROWCOUNT (Transact-SQL)](../../t-sql/statements/set-rowcount-transact-sql.md) 对此 CREATE EXTERNAL TABLE AS SELECT 没有影响。 要实现类似的行为，请使用 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)。

 当 CREATE EXTERNAL TABLE AS SELECT 从 RCFile 中选择时，RCFile 中的列值不能包含管道“|”字符。

使用 CREATE EXTERNAL TABLE AS SELECT 导出到 Parquet 或 ORC 文件时将导致错误，其中包括数据中存在以下字符时已拒绝的记录：

- |
- “（引号字符）
- /r/n
- /r
- /n

若要使用包含这些字符的 CREATE EXTERNAL TABLE AS SELECT，必须先运行 CREATE EXTERNAL TABLE AS SELECT 语句，将数据导出到带分隔符的文本文件，然后可使用外部工具将其转换为 Parquet 或 ORC。

## <a name="locking"></a>锁定
 采用 SCHEMARESOLUTION 对象上的共享锁。

##  <a name="examples"></a><a name="Examples"></a> 示例

### <a name="a-create-a-hadoop-table-by-using-create-external-table-as-select"></a>A. 使用 CREATE EXTERNAL TABLE AS SELECT 创建 Hadoop 表

 以下示例使用源表 `dimCustomer` 中的列定义和数据创建一个名为 `hdfsCustomer` 的新外部表。

 表定义存储在数据库中，并且 SELECT 语句的结果将导出到 Hadoop 外部数据源 customer_ds 上的“/pdwdata/customer.tbl”文件。 该文件根据外部文件格式 customer_ff 设置格式。

 文件名由数据库生成，并包含查询 ID，便于保持文件与生成该文件的查询的一致性。

 “客户”目录前的路径 `hdfs://xxx.xxx.xxx.xxx:5000/files/` 必须已经存在。 如果 Customer 目录不存在，数据库将创建该目录。

> [!NOTE]
>  此示例指定为 5000。 如果未指定端口，数据库将使用 8020 作为默认端口。

 生成的 Hadoop 位置和文件名将为 `hdfs:// xxx.xxx.xxx.xxx:5000/files/Customer/ QueryID_YearMonthDay_HourMinutesSeconds_FileIndex.txt.`。

```sql  
-- Example is based on AdventureWorks   
CREATE EXTERNAL TABLE hdfsCustomer  
WITH (  
        LOCATION='/pdwdata/customer.tbl',  
        DATA_SOURCE = customer_ds,  
        FILE_FORMAT = customer_ff  
) AS SELECT * FROM dimCustomer;  
```

### <a name="b-use-a-query-hint-with-create-external-table-as-select"></a>B. 通过 CREATE EXTERNAL TABLE AS SELECT 使用查询提示

 此查询显示在 CREATE EXTERNAL TABLE AS SELECT 语句中使用查询联接提示的基本语法。 提交查询后，数据库使用哈希联接策略生成查询计划。 有关联接提示以及如何使用 OPTION 子句的详细信息，请参阅 [OPTION 子句 (Transact-SQL)](../../t-sql/queries/option-clause-transact-sql.md)。

> [!NOTE]
>  此示例指定为 5000。 如果未指定端口，数据库将使用 8020 作为默认端口。

```sql  
-- Example is based on AdventureWorks  
CREATE EXTERNAL TABLE dbo.FactInternetSalesNew  
WITH   
    (   
        LOCATION = '/files/Customer',  
        DATA_SOURCE = customer_ds,  
        FILE_FORMAT = customer_ff  
    )  
AS SELECT T1.* FROM dbo.FactInternetSales T1 JOIN dbo.DimCustomer T2  
ON ( T1.CustomerKey = T2.CustomerKey )  
OPTION ( HASH JOIN );  
```

## <a name="see-also"></a>请参阅
 - [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)
 - [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](../../t-sql/statements/create-external-file-format-transact-sql.md)
 - [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)
 - [CREATE TABLE（Azure Synapse Analytics 和并行数据仓库）](~/t-sql/statements/create-table-azure-sql-data-warehouse.md)
 - [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)
 - [DROP TABLE (Transact-SQL)](../../t-sql/statements/drop-table-transact-sql.md)
 - [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)



