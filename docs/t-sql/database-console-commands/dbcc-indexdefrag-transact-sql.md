---
description: DBCC INDEXDEFRAG (Transact-SQL)
title: DBCC INDEXDEFRAG (Transact-SQL)
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- INDEXDEFRAG
- DBCC INDEXDEFRAG
- DBCC_INDEXDEFRAG_TSQL
- INDEXDEFRAG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- defragmenting indexes
- DBCC INDEXDEFRAG statement
- leaf level defragmenting
- fragmentation [SQL Server]
- index defragmenting [SQL Server]
ms.assetid: 3c7df676-4843-44d0-8c1c-a9ab7e593b70
author: pmasl
ms.author: umajay
ms.openlocfilehash: 7bc27698a8ddb0cffe294a9caf07ec2b307961fe
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596639"
---
# <a name="dbcc-indexdefrag-transact-sql"></a>DBCC INDEXDEFRAG (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

指定表或视图的索引碎片整理。
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]请改用 [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)。  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到[当前版本](../../sql-server/what-s-new-in-sql-server-2016.md)）
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC INDEXDEFRAG  
(  
    { database_name | database_id | 0 }   
    , { table_name | table_id | view_name | view_id }   
    [ , { index_name | index_id } [ , { partition_number | 0 } ] ]  
)  
    [ WITH NO_INFOMSGS ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 database_name \| database_id \| 0   
 包含要进行碎片整理的索引的数据库。 如果指定 0，则使用当前数据库。 数据库名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。  
  
 table_name \| table_id \| view_name \| view_id     
 包含要进行碎片整理的索引的表或视图。 表和视图的名称必须符合有关标识符的规则。  
  
 index_name \| index_id   
 要进行碎片整理的索引的名称或 ID。 如果未指定，该语句将针对指定表或视图的所有索引进行碎片整理。 索引名称必须符合标识符规则。  
  
 partition_number \| 0  
 要进行碎片整理的索引的分区号。 如果未指定或指定 0，该语句将对指定索引的所有分区进行碎片整理。  
  
 WITH NO_INFOMSGS  
 取消严重级别从 0 到 10 的所有信息性消息。  
  
## <a name="remarks"></a>备注  
DBCC INDEXDEFRAG 对索引的叶级进行碎片整理，以便页的物理顺序与叶节点从左到右的逻辑顺序相匹配，因此可提高索引扫描性能。
  
> [!NOTE]  
>  运行 DBCC INDEXDEFRAG 时，索引碎片整理是串行进行的。 这表示对单个索引的操作是使用单个线程执行的。 没有发生并行操作。 同样，在同一个 DBCC INDEXDEFRAG 语句中对多个索引的操作是一次对一个索引执行的。  
  
DBCC INDEXDEFRAG 还压缩索引页，并考虑创建索引时指定的填充因子。 任何因这种压缩而创建的空页将被删除。 有关详细信息，请参阅 [为索引指定填充因子](../../relational-databases/indexes/specify-fill-factor-for-an-index.md)。
  
如果索引跨越多个文件，则 DBCC INDEXDEFRAG 一次对一个文件进行碎片整理。 不会在文件之间迁移页。
  
DBCC INDEXDEFRAG 每五分钟就报告完成的估计百分比。 可在进程中的任一点停止 DBCC INDEXDEFRAG，任何已完成的工作都将保留。
  
与 DBCC DBREINDEX（或通常的索引生成操作）不同，DBCC INDEXDEFRAG 是联机操作。 它不长期保持锁。 因此，DBCC INDEXDEFRAG 不会阻塞运行查询或更新。 因为碎片整理所需的时间与碎片整理的级别相关，若索引的碎片相对较少，则该索引的碎片整理速度比生成一个新索引要快。 对碎片太多的索引进行整理可能要比重建索引花更多的时间。
  
始终对碎片整理进行完整的日志记录，与数据库恢复模式设置无关。 有关详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。 对碎片太多的索引进行整理所生成的日志可能比完全记录的索引创建多。 但是，碎片整理是作为一系列短事务执行的，因此如果经常进行日志备份或恢复模式设置为 SIMPLE，则不需要大日志。
  
## <a name="restrictions"></a>限制  
DBCC INDEXDEFRAG 打乱了已有的索引叶级页。 因此，如果索引与磁盘上的其他索引交叉，则针对该索引运行 DBCC INDEXDEFRAG 不使索引中的所有叶级页连续。 若要改善页的聚集，请重建索引。
DBCC INDEXDEFRAG 不能用于对以下索引进行碎片整理：
-   已禁用的索引。  
-   页锁定设置为 OFF 的索引。  
-   空间索引。  
  
不支持对系统表使用 DBCC INDEXDEFRAG。
  
## <a name="result-sets"></a>结果集  
如果在语句中指定索引（除非指定了 WITH NO_INFOMSGS），DBCC INDEXDEFRAG 返回以下结果集（值可能不同）：
  
```
Pages Scanned Pages Moved Pages Removed  
------------- ----------- -------------  
359           346         8  
  
(1 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>权限  
调用方必须拥有此表，或是 sysadmin 固定服务器角色、db_owner 固定数据库角色或 db_ddladmin 固定数据库角色的成员    。
  
## <a name="examples"></a>示例  
### <a name="a-using-dbcc-indexdefrag-to-defragment-an-index"></a>A. 使用 DBCC INDEXDEFRAG 对索引进行碎片整理  
下面的示例对 `AdventureWorks` 数据库的 `Production.Product` 表中的 `PK_Product_ProductID` 索引的所有分区进行碎片整理。
  
```sql  
DBCC INDEXDEFRAG (AdventureWorks2012, 'Production.Product', PK_Product_ProductID);  
GO  
```  
  
### <a name="b-using-dbcc-showcontig-and-dbcc-indexdefrag-to-defragment-the-indexes-in-a-database"></a>B. 使用 DBCC SHOWCONTIG 和 DBCC INDEXDEFRAG 对数据库中的索引进行碎片整理  
 以下示例将展示一种简单的方法，该方法可用于对数据库中碎片数量在声明的阈值之上的所有索引进行碎片整理。  
  
```sql  
/*Perform a 'USE <database name>' to select the database in which to run the script.*/  
-- Declare variables  
SET NOCOUNT ON;  
DECLARE @tablename VARCHAR(255);  
DECLARE @execstr   VARCHAR(400);  
DECLARE @objectid  INT;  
DECLARE @indexid   INT;  
DECLARE @frag      DECIMAL;  
DECLARE @maxfrag   DECIMAL;  
  
-- Decide on the maximum fragmentation to allow for.  
SELECT @maxfrag = 30.0;  
  
-- Declare a cursor.  
DECLARE tables CURSOR FOR  
   SELECT TABLE_SCHEMA + '.' + TABLE_NAME  
   FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_TYPE = 'BASE TABLE';  
  
-- Create the table.  
CREATE TABLE #fraglist (  
   ObjectName CHAR(255),  
   ObjectId INT,  
   IndexName CHAR(255),  
   IndexId INT,  
   Lvl INT,  
   CountPages INT,  
   CountRows INT,  
   MinRecSize INT,  
   MaxRecSize INT,  
   AvgRecSize INT,  
   ForRecCount INT,  
   Extents INT,  
   ExtentSwitches INT,  
   AvgFreeBytes INT,  
   AvgPageDensity INT,  
   ScanDensity DECIMAL,  
   BestCount INT,  
   ActualCount INT,  
   LogicalFrag DECIMAL,  
   ExtentFrag DECIMAL);  
  
-- Open the cursor.  
OPEN tables;  
  
-- Loop through all the tables in the database.  
FETCH NEXT  
   FROM tables  
   INTO @tablename;  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
-- Do the showcontig of all indexes of the table  
   INSERT INTO #fraglist   
   EXEC ('DBCC SHOWCONTIG (''' + @tablename + ''')   
      WITH FAST, TABLERESULTS, ALL_INDEXES, NO_INFOMSGS');  
   FETCH NEXT  
      FROM tables  
      INTO @tablename;  
END;  
  
-- Close and deallocate the cursor.  
CLOSE tables;  
DEALLOCATE tables;  
  
-- Declare the cursor for the list of indexes to be defragged.  
DECLARE indexes CURSOR FOR  
   SELECT ObjectName, ObjectId, IndexId, LogicalFrag  
   FROM #fraglist  
   WHERE LogicalFrag >= @maxfrag  
      AND INDEXPROPERTY (ObjectId, IndexName, 'IndexDepth') > 0;  
  
-- Open the cursor.  
OPEN indexes;  
  
-- Loop through the indexes.  
FETCH NEXT  
   FROM indexes  
   INTO @tablename, @objectid, @indexid, @frag;  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
   PRINT 'Executing DBCC INDEXDEFRAG (0, ' + RTRIM(@tablename) + ',  
      ' + RTRIM(@indexid) + ') - fragmentation currently '  
       + RTRIM(CONVERT(varchar(15),@frag)) + '%';  
   SELECT @execstr = 'DBCC INDEXDEFRAG (0, ' + RTRIM(@objectid) + ',  
       ' + RTRIM(@indexid) + ')';  
   EXEC (@execstr);  
  
   FETCH NEXT  
      FROM indexes  
      INTO @tablename, @objectid, @indexid, @frag;  
END;  
  
-- Close and deallocate the cursor.  
CLOSE indexes;  
DEALLOCATE indexes;  
  
-- Delete the temporary table.  
DROP TABLE #fraglist;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)
  
