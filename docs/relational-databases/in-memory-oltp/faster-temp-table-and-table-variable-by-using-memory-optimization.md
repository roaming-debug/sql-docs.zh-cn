---
title: 使用内存优化获得更快的临时表和表变量
description: 了解如何将临时表、表变量或表值参数转换为内存优化表和表变量来提高性能。
ms.custom: seo-dt-2019
ms.date: 06/01/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 38512a22-7e63-436f-9c13-dde7cf5c2202
author: kevin-farlee
ms.author: kfarlee
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 09d3e92d2e181264965a1d4525d13f7d13b4504d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460480"
---
# <a name="faster-temp-table-and-table-variable-by-using-memory-optimization"></a>通过使用内存优化获得更快的临时表和表变量
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  
如果使用临时表、表变量或表值参数，请考虑转换它们以使用内存优化的表和表变量，从而提高性能。 此代码的更改通常很小。  
  
本文介绍：  
  
- 支持转换为内存对象的场景。  
- 实现转换为内存对象的技术步骤。  
- 转换为内存对象之前的先决条件。  
- 突出了内存优化的性能优势的代码示例
  
  
## <a name="a-basics-of-memory-optimized-table-variables"></a>A. 内存优化表变量的基础知识  
  
内存优化表变量使用与内存优化表相同的内存优化算法和数据结构，因此具有很高的效率。 从本机编译模块内访问表变量时，效率将最大化。  
  
  
内存优化表变量：  
  
- 仅存储在内存中，在磁盘上没有任何组件。  
- 不涉及 IO 活动。  
- 不涉及 tempdb 使用或争用。  
- 可以作为表值参数 (TVP) 传递到存储过程。  
- 必须具有至少一个索引，哈希或非聚集索引。  
  - 对于哈希索引，理想情况下桶计数应为预期的唯一索引键数的 1-2 倍，但是估计过高的桶计数通常也没有问题（高达 10 倍）。 有关详细信息，请参阅 [《Indexes for Memory-Optimized Tables》](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)（内存优化表的索引）。  

  
  
#### <a name="object-types"></a>对象类型  
  
内存 OLTP 提供以下可用于内存优化临时表和表变量的对象：  
  
- 内存优化表  
  - Durability = SCHEMA_ONLY  
- 内存优化表变量  
  - 必须在两个步骤中（而不是以内联的方式）声明：  
    - `CREATE TYPE my_type AS TABLE ...;` ，然后  
    - `DECLARE @mytablevariable my_type;` 列中的一个值匹配。  
  
  
## <a name="b-scenario-replace-global-tempdb-x23x23table"></a>B. 方案：替换全局 tempdb &#x23;&#x23; 表  
  
将全局临时表替换为内存优化的 SCHEMA_ONLY 表非常简单。 最大的改变是在部署时（而不是运行时）创建该表。 由于采用了编译时优化，创建内存优化表会比创建传统表所用时间更长。 创建和删除联机工作负载中的内存优化表不仅会影响工作负载的性能，而且还会影响 AlwaysOn 辅助副本以及数据库恢复的重做性能。

假设有以下全局临时表。  
  
  
```sql
CREATE TABLE ##tempGlobalB  
    (  
        Column1   INT   NOT NULL ,  
        Column2   NVARCHAR(4000)  
    );  
```
  
  
  
请考虑将全局临时表替换为以下 DURABILITY = SCHEMA_ONLY 的内存优化表。  
  
  
  
```sql
CREATE TABLE dbo.soGlobalB  
(  
    Column1   INT   NOT NULL   INDEX ix1 NONCLUSTERED,  
    Column2   NVARCHAR(4000)  
)  
    WITH  
        (MEMORY_OPTIMIZED = ON,  
        DURABILITY        = SCHEMA_ONLY);  
```  
  
  
  
#### <a name="b1-steps"></a>B.1 步骤  
  
从全局临时转换为 SCHEMA_ONLY 的步骤如下：  
  
  
1. 一次性创建 **dbo.soGlobalB** 表，其方式与创建任意传统的磁盘表一样。  
2. 在 Transact-SQL 中删除 **&#x23;&#x23;tempGlobalB** 表的创建。  请务必在部署时（而不是在运行时）创建内存优化表，以避免创建表时附带的编译开销。
3. 在 T-SQL 中将所有提到的 **&#x23;&#x23;tempGlobalB** 替换为 **dbo.soGlobalB**。  
  
  
## <a name="c-scenario-replace-session-tempdb-x23table"></a>C. 方案：替换会话 tempdb &#x23; 表  
  
替换会话临时表的准备工作包含的 T-SQL 比之前的全局临时表场景要多。 值得庆幸的是额外的 T-SQL 并不意味着需要执行更多操作来完成转换。  

和全局临时表方案一样，最大的改变就是在部署时（而不是运行时）创建表，从而避免编译开销。
  
假设有以下会话临时表。  
  
  
  
```sql  
CREATE TABLE #tempSessionC  
(  
    Column1   INT   NOT NULL ,  
    Column2   NVARCHAR(4000)  
);  
```
  
  
  
首先，创建以下表值函数来筛选 \@\@spid。 该函数可供所有从会话临时表转换而来的 SCHEMA_ONLY 表使用。  
  
  
  
```sql  
CREATE FUNCTION dbo.fn_SpidFilter(@SpidFilter smallint)  
    RETURNS TABLE  
    WITH SCHEMABINDING , NATIVE_COMPILATION  
AS  
    RETURN  
        SELECT 1 AS fn_SpidFilter  
            WHERE @SpidFilter = @@spid;  
```
  
  
  
其次，创建 SCHEMA_ONLY 表，并在该表上添加安全策略。  
  
  
请注意每个内存优化表都必须具有至少一个索引。  
  
- 对于表 dbo.soSessionC，如果我们要计算合适的 BUCKET_COUNT，则使用哈希索引可能更好。 不过在本示例中我们简化为使用非聚集索引。  
  
  
  
```sql
CREATE TABLE dbo.soSessionC  
(  
    Column1     INT         NOT NULL,  
    Column2     NVARCHAR(4000)  NULL,  

    SpidFilter  SMALLINT    NOT NULL   DEFAULT (@@spid),  

    INDEX ix_SpidFiler NONCLUSTERED (SpidFilter),  
    --INDEX ix_SpidFilter HASH  
    --    (SpidFilter) WITH (BUCKET_COUNT = 64),  
        
    CONSTRAINT CHK_soSessionC_SpidFilter  
        CHECK ( SpidFilter = @@spid ),  
)  
    WITH  
        (MEMORY_OPTIMIZED = ON,  
            DURABILITY = SCHEMA_ONLY);  
go  
  
  
CREATE SECURITY POLICY dbo.soSessionC_SpidFilter_Policy  
    ADD FILTER PREDICATE dbo.fn_SpidFilter(SpidFilter)  
    ON dbo.soSessionC  
    WITH (STATE = ON);  
go  
```  
  
  
  
再次，在常规的 T-SQL 代码中：  
  
1. 将 Transact-SQL 语句中对临时表的所有引用替换为新的内存优化表：
    - _旧表名：_ &#x23;tempSessionC  
    - _新表名：_ dbo.soSessionC  
2. 将代码中的 `CREATE TABLE #tempSessionC` 语句替换为 `DELETE FROM dbo.soSessionC`，以确保会话不会公开到由具有同一 session_id 的以前会话插入的表内容。 请务必在部署时（而不是在运行时）创建内存优化表，以避免创建表时附带的编译开销。
3. 从代码中删除 `DROP TABLE #tempSessionC` 语句 - 如果内存大小是一个潜在的忧患，则还可以插入一个 `DELETE FROM dbo.soSessionC` 语句
  
  
  
  
## <a name="d-scenario-table-variable-can-be-memory_optimizedon"></a>D. 方案：表变量可以将 MEMORY_OPTIMIZED 设置为 ON  
  
  
传统的表变量表示 tempdb 数据库中的一个表。 为了获得更快的性能，可以对表变量进行内存优化。  
  
以下是针对传统的表变量的 T-SQL。 其作用域在批处理或会话结束时结束。  
  
  
  
```sql
DECLARE @tvTableD TABLE  
    ( Column1   INT   NOT NULL ,  
      Column2   CHAR(10) );  
```
  
  
  
#### <a name="d1-convert-inline-to-explicit"></a>D.1 从内联转换为显式  
  
前面的语法是要以 *内联* 方式创建表变量。 内联语法不支持内存优化。 因此，让我们针对 TYPE 将内联语法转换为显式语法。  
  
作用域：在由 go 关键字分隔的批处理语句中第一组语句创建的 TYPE 定义即使在服务器关闭并重新启动之后仍然有效。 但是在第一个 go 分隔符之后，声明的表 @tvTableC 仅会保留到到达下一个 go 分隔符，并且批处理将结束。  
  
  
  
```sql  
CREATE TYPE dbo.typeTableD  
    AS TABLE  
    (  
        Column1  INT   NOT NULL ,  
        Column2  CHAR(10)  
    );  
go  
        
SET NoCount ON;  
DECLARE @tvTableD dbo.typeTableD  
;  
INSERT INTO @tvTableD (Column1) values (1), (2)  
;  
SELECT * from @tvTableD;  
go  
```
  
  
  
#### <a name="d2-convert-explicit-on-disk-to-memory-optimized"></a>D.2 将显式磁盘转换为内存优化  
  
内存优化表变量不驻留在 tempdb 中。 内存优化会使速度变快，通常快 10 倍或更多。  
  
转换为内存优化只要一个步骤就可以完成。 增强 TYPE 的显式创建，如下所示，在其中添加：  
  
- 一个索引。 再次提醒，每个内存优化表都必须具有至少一个索引。  
- MEMORY_OPTIMIZED = ON。  
  
  
  
```sql
CREATE TYPE dbo.typeTableD  
    AS TABLE  
    (  
        Column1  INT   NOT NULL   INDEX ix1,  
        Column2  CHAR(10)  
    )  
    WITH  
        (MEMORY_OPTIMIZED = ON);  
```  
  
  
  
完成。  
  
  
## <a name="e-prerequisite-filegroup-for-sql-server"></a>E. SQL Server 的先决条件 FILEGROUP  
  
在 Microsoft SQL Server 上，若要使用内存优化功能，数据库必须具有使用 **MEMORY_OPTIMIZED_DATA** 声明的 FILEGROUP。  
  
- Azure SQL 数据库不需要创建此 FILEGROUP。  
  
  
先决条件：下面的针对 FILEGROUP 的 Transact-SQL 代码是本文后面小节中较长的 T-SQL 代码示例的先决条件。  
  
1. 必须使用可提交 T-SQL 的 SSMS.exe 或另一种工具。  
2. 将示例 FILEGROUP T-SQL 代码粘贴到 SSMS。  
3. 编辑 T-SQL，根据自己的喜好更改其特定名称和目录路径。  
  - FILENAME 值中的所有目录必须预先存在，最后一个目录则不能预先存在。  
4. 运行已编辑的 T-SQL。  
  - 即使在下一小节中重复调整并重新运行速度比较 T-SQL 语句，也无需多次运行 FILEGROUP T-SQL。  
  
  
  
 
```sql
ALTER DATABASE InMemTest2  
    ADD FILEGROUP FgMemOptim3  
        CONTAINS MEMORY_OPTIMIZED_DATA;  
go  
ALTER DATABASE InMemTest2  
    ADD FILE  
    (  
        NAME = N'FileMemOptim3a',  
        FILENAME = N'C:\DATA\FileMemOptim3a'  
                    --  C:\DATA\    preexisted.  
    )  
    TO FILEGROUP FgMemOptim3;  
go  
```  


以下脚本为你创建文件组，并配置建议的数据库设置： [enable-in-memory-oltp.sql](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql)
  
有关针对 FILE 和 FILEGROUP 的 `ALTER DATABASE ... ADD` 的详细信息，请参阅：  
  
- [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)  
- [内存优化的文件组](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)    
  
  
## <a name="f-quick-test-to-prove-speed-improvement"></a>F. 证明速度提高的快速测试  
  
  
本节提供的 Transact-SQL 代码用于测试并比较 INSERT-DELETE 从使用内存优化表变量中的速度提升效果。 代码由几乎一样的两部分组成，除了第一部分的表类型为内存优化。  
  
比较测试的持续时间大约为 7 秒。 若要运行该示例：  
  
1. 先决条件：必须在上一节中已运行 FILEGROUP T-SQL。  
2. 运行以下 T-SQL INSERT-DELETE 脚本。  
  - 请注意“GO 5001”语句，该语句将重新提交 T-SQL 5001 次。 你可以调整该数字，然后重新运行。  
  
在 Azure SQL 数据库中运行该脚本时，请确保从同一区域中的 VM 上运行。

  
```sql
PRINT ' ';  
PRINT '---- Next, memory-optimized, faster. ----';  

DROP TYPE IF EXISTS dbo.typeTableC_mem;  
go  
CREATE TYPE dbo.typeTableC_mem  -- !!  Memory-optimized.  
        AS TABLE  
        (  
            Column1  INT NOT NULL INDEX ix1,  
            Column2  CHAR(10)  
        )  
        WITH  
            (MEMORY_OPTIMIZED = ON);  
go  
DECLARE @dateString_Begin nvarchar(64) =  
    Convert(nvarchar(64), GetUtcDate(), 121);  
PRINT Concat(@dateString_Begin, '  = Begin time, _mem.');  
go  
SET NoCount ON;  
DECLARE @tvTableC dbo.typeTableC_mem;  -- !!  

INSERT INTO @tvTableC (Column1) values (1), (2);  
INSERT INTO @tvTableC (Column1) values (3), (4);  
DELETE @tvTableC;  

GO 5001  

DECLARE @dateString_End nvarchar(64) =  
    Convert(nvarchar(64), GetUtcDate(), 121);  
PRINT Concat(@dateString_End, '  = End time, _mem.');  
go  
DROP TYPE IF EXISTS dbo.typeTableC_mem;  
go  

---- End memory-optimized.  
-------------------------------------------------  
---- Start traditional on-disk.  

PRINT ' ';  
PRINT '---- Next, tempdb based, slower. ----';  

DROP TYPE IF EXISTS dbo.typeTableC_tempdb;  
go  
CREATE TYPE dbo.typeTableC_tempdb  -- !!  Traditional tempdb.  
    AS TABLE  
    (  
        Column1  INT NOT NULL ,  
        Column2  CHAR(10)  
    );  
go  
DECLARE @dateString_Begin nvarchar(64) =  
    Convert(nvarchar(64), GetUtcDate(), 121);  
PRINT Concat(@dateString_Begin, '  = Begin time, _tempdb.');  
go  
SET NoCount ON;  
DECLARE @tvTableC dbo.typeTableC_tempdb;  -- !!  

INSERT INTO @tvTableC (Column1) values (1), (2);  
INSERT INTO @tvTableC (Column1) values (3), (4);  
DELETE @tvTableC;  

GO 5001  

DECLARE @dateString_End nvarchar(64) =  
    Convert(nvarchar(64), GetUtcDate(), 121);  
PRINT Concat(@dateString_End, '  = End time, _tempdb.');  
go  
DROP TYPE IF EXISTS dbo.typeTableC_tempdb;  
go  
----  

PRINT '---- Tests done. ----';  

go  

/*** Actual output, SQL Server 2016:  

---- Next, memory-optimized, faster. ----  
2016-04-20 00:26:58.033  = Begin time, _mem.  
Beginning execution loop  
Batch execution completed 5001 times.  
2016-04-20 00:26:58.733  = End time, _mem.  

---- Next, tempdb based, slower. ----  
2016-04-20 00:26:58.750  = Begin time, _tempdb.  
Beginning execution loop  
Batch execution completed 5001 times.  
2016-04-20 00:27:05.440  = End time, _tempdb.  
---- Tests done. ----  
**_/
```
  
  
  
## <a name="g-predict-active-memory-consumption"></a>G. 预测活动内存使用  
  
你可以通过以下资源预测内存优化表的活动内存需求：  
  
- [估算内存优化表的内存需求](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md)  
- [内存优化表中的表和行大小：示例计算](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)  
  
对于较大的表变量，非聚集索引所使用的内存大于它们对内存优化表所使用的内存。 行数和索引键越大，这种差别就越大。  
  
如果每次访问内存优化表变量时只使用一个准确的键值，那么哈希索引是比非聚集索引更好的选择。 但是，如果不能估计出合适的 BUCKET_COUNT，非聚集索引是一个不错的第二选择。  
  
## <a name="h-see-also"></a>H. 另请参阅  
  
- [内存优化表。](./sample-database-for-in-memory-oltp.md)

- [为内存优化对象定义持续性。](../../relational-databases/in-memory-oltp/defining-durability-for-memory-optimized-objects.md)

- [2017 年 9 月在博客中宣布的累积更新，可消除不当内存不足错误出现的可能。](https://support.microsoft.com/help/4025208/fix-memory-leak-occurs-when-you-use-memory-optimized-tables-in-microso)
    - [SQL Server 2016 生成版本](https://support.microsoft.com/help/3177312/sql-server-2016-build-versions)提供发布、Service Pack 和累积更新的完整详细信息。
    - 这些偶尔出现的不当错误未在 SQL Server 的 Enterprise Edition 中出现。