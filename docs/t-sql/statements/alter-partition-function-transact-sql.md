---
description: ALTER PARTITION FUNCTION (Transact-SQL)
title: ALTER PARTITION FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER PARTITION FUNCTION
- ALTER_PARTITION_FUNCTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- splitting partitions [SQL Server]
- partitioned tables [SQL Server], splitting
- ALTER PARTITION FUNCTION statement
- merging partitions [SQL Server]
- partitioned indexes [SQL Server], merging
- partitioned indexes [SQL Server], splitting
- modifying partition functions
- partition functions [SQL Server], modifying
- partitioned tables [SQL Server], merging
ms.assetid: 70866dac-0a8f-4235-8108-51547949ada4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c3ae0c860bc31a228ce3a842d7e1ba8a861ea1e5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092102"
---
# <a name="alter-partition-function-transact-sql"></a>ALTER PARTITION FUNCTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

通过拆分或合并边界值更改分区函数。 运行 ALTER PARTITION FUNCTION 语句可以将使用分区函数的一个表分区或索引拆分为两个分区。 此外，该语句还可以将两个分区合并为一个较小的分区。  
  
> [!CAUTION]  
>  多个表或索引可以使用同一分区函数。 ALTER PARTITION FUNCTION 在单个事务中影响所有这些表或索引。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
ALTER PARTITION FUNCTION partition_function_name()  
{   
    SPLIT RANGE ( boundary_value )  
  | MERGE RANGE ( boundary_value )   
} [ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
partition_function_name  
要修改的分区函数的名称。  
  
SPLIT RANGE ( boundary_value )  
在分区函数中添加一个分区。 boundary_value 确定新分区的范围，因此它必须不同于分区函数的现有边界范围。 根据 boundary_value，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将某个现有范围拆分为两个范围。 在这两个范围中，具有新 boundary_value 的范围是新分区。  
  
文件组必须处于联机状态， 并且使用分区函数作为 NEXT USED 来保存新分区的分区方案必须标记该文件组。 CREATE PARTITION SCHEME 语句将文件组分配给分区。 CREATE PARTITION FUNCTION 语句创建的分区数少于用于保存分区的文件组。 CREATE PARTITION SCHEME 语句可能会留出比所需文件组数量更多的文件组。 如果发生这种情况，最终就会有未分配的文件组。 此外，分区方案会将其中一个文件组标记为 NEXT USED。 该文件组用于保存新的分区。 如果分区方案未将任何文件组标记为 NEXT USED，则必须使用 ALTER PARTITION SCHEME 语句。 

ALTER PARTITION SCHEME 语句可以添加文件组，也可以选择现有文件组来保存新分区。 你可以指定已保存分区的文件组来保存附加分区。 一个分区函数可以参与多个分区方案。 出于此原因，使用要添加分区的分区函数的所有分区方案都必须具有 NEXT USED 文件组。 否则，ALTER PARTITION FUNCTION 语句将失败并出现错误，显示缺少 NEXT USED 文件组的一个或多个分区方案。  
  
如果在同一文件组创建所有分区，则最初自动将该文件组分配为 NEXT USED 文件组。 但是，在运行拆分操作后，将不再有选定的 NEXT USED 文件组。 可以使用 ALTER PARTITION SCHEME 将文件组显式分配为 NEXT USED 文件组，否则后续的拆分操作将失败。  
  
> [!NOTE]  
>  列存储索引限制：当表上存在列存储索引时，仅可拆分空分区。 需要先删除或禁用此列存储索引才能执行此操作。  
  
MERGE [ RANGE ( boundary_value) ]  
删除一个分区并将该分区中存在的所有值都合并到某个剩余分区中。 RANGE (*boundary_value*) 必须是要删除的分区的现有边界值。 如果最初保存 boundary_value 的文件组没有被剩余分区使用，也没有使用 NEXT USED 属性进行标记，此参数将从分区方案中删除该文件组。 合并的分区位于最初不保存 boundary_value 的文件组中。 boundary_value 是一个可以引用变量（包括用户定义类型变量）或函数（包括用户定义函数）的常量表达式。 它无法引用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式。 boundary_value 必须匹配或可隐式转换为其对应分区列的数据类型。 另外，在隐式转换期间，如果 boundary_value 值的大小和比例与其对应的 input_parameter_type 不匹配，也不能截断它。  
  
> [!NOTE]  
>  列存储索引限制：不能合并包含列存储索引的两个非空分区。 需要先删除或禁用此列存储索引才能执行此操作  
  
## <a name="best-practices"></a>最佳实践  
始终在分区范围的两端保留空分区。 在两端保留分区可保证分区拆分和分区合并不会导致任何数据移动。 在开始时执行分区拆分，在结束时执行分区合并。 避免拆分或合并填充的分区。 拆分或合并填充的分区可能效率低下。 之所以效率低下，是因为拆分或合并可能导致日志生成次数比原来多四倍，并且还可能导致严重的锁定问题。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
ALTER PARTITION FUNCTION 在单个原子操作中对使用该函数的任何表和索引进行重新分区。 但该操作在脱机状态下进行，并且根据重新分区的范围，可能会消耗大量资源。  
  
仅使用 ALTER PARTITION FUNCTION 将一个分区拆分为两个分区，或将两个分区合并为一个分区。 若要更改其他情况下对表进行分区的方法（例如，将 10 个分区合并为 5 个分区），可以尝试使用以下任何选项。 根据系统配置，这些选项可能在资源消耗方面有所不同：  
  
-   使用必要的分区函数创建新的分区表。 然后，使用 INSERT INTO...SELECT FROM 语句将旧表中的数据插入到新表中。  
  
-   为堆创建分区聚集索引。  
  
    > [!NOTE]  
    >  删除已分区的聚集索引将产生分区堆。  
  
-   通过将 [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE INDEX 语句与 DROP EXISTING = ON 子句一起使用来删除并重新生成现有的已分区索引。  
  
-   运行一系列 ALTER PARTITION FUNCTION 语句。  
  
ALTER PARTITION FUNCTION 所影响的全部文件组都必须处于联机状态。  
  
如果使用分区函数的任何表中存在已禁用的聚集索引，ALTER PARTITION FUNCTION 将失败。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不对修改分区函数提供复制支持。 必须在订阅数据库中手动应用对发布数据库中的分区函数的更改。  
  
## <a name="permissions"></a>权限  
可以使用以下任意权限执行 ALTER PARTITION FUNCTION：  
  
-   ALTER ANY DATASPACE 权限。 默认情况下，此权限授予 **sysadmin** 固定服务器角色和 **db_owner** 及 **db_ddladmin** 固定数据库角色的成员。  
  
-   对创建分区函数时所在数据库的 CONTROL 或 ALTER 权限。  
  
-   对包含创建分区函数所在的数据库的服务器具有 CONTROL SERVER 或 ALTER ANY DATABASE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-splitting-a-partition-of-a-partitioned-table-or-index-into-two-partitions"></a>A. 将已分区表或索引的一个分区拆分为两个分区  
以下示例创建一个分区函数，将表或索引分为四个分区。 `ALTER PARTITION FUNCTION` 将某个分区拆分为两个分区，从而总共创建五个分区。  
  
```sql  
IF EXISTS (SELECT * FROM sys.partition_functions  
    WHERE name = 'myRangePF1')  
DROP PARTITION FUNCTION myRangePF1;  
GO  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
GO  
--Split the partition between boundary_values 100 and 1000  
--to create two partitions between boundary_values 100 and 500  
--and between boundary_values 500 and 1000.  
ALTER PARTITION FUNCTION myRangePF1 ()  
SPLIT RANGE (500);  
```  
  
### <a name="b-merging-two-partitions-of-a-partitioned-table-into-one-partition"></a>B. 将已分区表的两个分区合并为一个分区  
以下示例与上例创建同一分区函数，然后将两个分区合并为一个分区，从而总共创建了三个分区。  
  
```sql  
IF EXISTS (SELECT * FROM sys.partition_functions  
    WHERE name = 'myRangePF1')  
DROP PARTITION FUNCTION myRangePF1;  
GO  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
GO  
--Merge the partitions between boundary_values 1 and 100  
--and between boundary_values 100 and 1000 to create one partition  
--between boundary_values 1 and 1000.  
ALTER PARTITION FUNCTION myRangePF1 ()  
MERGE RANGE (100);  
```  
  
## <a name="see-also"></a>另请参阅  
[已分区表和已分区索引](../../relational-databases/partitions/partitioned-tables-and-indexes.md)   
[CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md)   
[DROP PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/drop-partition-function-transact-sql.md)   
[CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
[ALTER PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
[DROP PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
[ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)   
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
[sys.partition_functions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
[sys.partition_parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md) [sys.partition_range_values (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
[sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
[sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
[sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
[sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
