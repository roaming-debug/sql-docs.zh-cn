---
description: 创建聚集索引
title: 创建聚集索引 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index creation [SQL Server], clustered indexes
- clustered indexes, creating
- clustered indexes, PRIMARY KEY constraint
- clustered indexes, UNIQUE constraint
- indexes [SQL Server], clustered
ms.assetid: 47148383-c2c7-4f08-a9e4-7016bf2d1d13
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5a343bbd9686856d6b8bdb27808673178336ee84
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130264"
---
# <a name="create-clustered-indexes"></a>创建聚集索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在表中创建聚集索引。 除了个别表之外，每个表都应该有聚集索引。 聚集索引除了可以提高查询性能之外，还可以按需重新生成或重新组织来控制表碎片。 也可以对视图创建聚集索引。 （ [描述的聚集索引和非聚集索引](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)主题中定义了聚集索引。）  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [典型实现](#Implementations)  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要创建表的聚集索引，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="typical-implementations"></a><a name="Implementations"></a> 典型实现  
 聚集索引按下列方式实现：  
  
-   **PRIMARY KEY 和 UNIQUE 约束**  
  
     在创建 PRIMARY KEY 约束时，如果不存在该表的聚集索引且未指定唯一非聚集索引，则将自动对一列或多列创建唯一聚集索引。 主键列不允许空值。  
  
     在创建 UNIQUE 约束时，默认情况下将创建唯一非聚集索引，以便强制 UNIQUE 约束。 如果不存在该表的聚集索引，则可以指定唯一聚集索引。  
  
     将索引创建为约束的一部分后，会自动将索引命名为与约束名称相同的名称。 有关详细信息，请参阅 [Primary and Foreign Key Constraints](../../relational-databases/tables/primary-and-foreign-key-constraints.md) 和 [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md)。  
  
-   **独立于约束的索引**  
  
     指定非聚集主键约束后，您可以对非主键列的列创建聚集索引。  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   创建聚集索引结构后，旧（源）结构和新（目标）结构的各自的文件和文件组都需要磁盘空间。 在完成事务提交后，才会释放旧结构。 排序也需要其他临时磁盘空间。 有关详细信息，请参阅 [Disk Space Requirements for Index DDL Operations](../../relational-databases/indexes/disk-space-requirements-for-index-ddl-operations.md)。  
  
-   如果对具有多个现有非聚集索引的堆创建聚集索引，则必须重新生成所有非聚集索引，以使它们包含聚集键值而非行标识符 (RID)。 同样，如果删除具有多个非聚集索引的表的聚集索引，在 DROP 操作过程中，将重新生成非聚集索引。 对于大型表，这可能需要很长时间。  
  
     对大型表创建索引的首选方法是先创建聚集索引，然后创建任何非聚集索引。 在对现有表创建索引时，请考虑将 ONLINE 选项设置为 ON。 如果设置为 ON，则不会持有长期表锁。 这使对基础表的查询或更新可以继续进行。 有关详细信息，请参阅 [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md)。  
  
-   聚集索引的索引键不能包含在 ROW_OVERFLOW_DATA 分配单元中具有现有数据的 **varcharr** 列。 如果对 **varchar** 列创建了聚集索引，并且 IN_ROW_DATA 分配单元中存在现有数据，则对该列执行的将数据推送到行外的后续插入或更新操作将会失败。 若要获得有关可能包含行溢出数据的表的信息，请使用 [sys.dm_db_index_physical_stats (Transact-SQL) ](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) 动态管理函数。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求对表或视图具有 ALTER 权限。 用户必须是 **sysadmin** 固定服务器角色的成员，或者是 **db_ddladmin** 和 **db_owner** 固定数据库角色的成员。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-clustered-index-by-using-object-explorer"></a>使用对象资源管理器创建聚集索引  
  
1.  在“对象资源管理器”中，展开要创建聚集索引的表。  
  
2.  右键单击“索引”文件夹，指向“新建索引”，然后选择“聚集索引…”。  
  
3.  在 **“新建索引”** 对话框的 **“常规”** 页中，在 **“索引名称”** 框中输入新索引的名称。  
  
4.  在“索引键列”下，单击“添加…” 。  
  
5.  在“从表名称中选择列”对话框中，选中要添加到聚集索引的表列的复选框 _\__。  
  
6.  单击“确定”。  
  
7.  在 **“新建索引”** 对话框中，单击 **“确定”** 。  
  
#### <a name="to-create-a-clustered-index-by-using-the-table-designer"></a>使用表设计器创建聚集索引  
  
1.  在“对象资源管理器”中，展开要使用聚集索引创建表的数据库。  
  
2.  右键单击“表”文件夹，然后单击“新建表…”。  
  
3.  按常规方式创建新表。 有关详细信息，请参阅[创建表（数据库引擎）](../../relational-databases/tables/create-tables-database-engine.md)。  
  
4.  右键单击上面创建的新表，然后单击“设计”。  
  
5.  在“表设计器”菜单上，单击“索引/键”。  
  
6.  在“索引/键”对话框中，单击“添加”。  
  
7.  从“选定的主/唯一键或索引”文本框中选择新索引。  
  
8.  在网格中，选择“创建为聚集”，然后从该属性右侧的下拉列表中选择“是”。  
  
9. 单击“关闭”。  
  
10. 在“文件”菜单上，单击“保存 _table\_name_”。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-clustered-index"></a>创建聚集索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Create a new table with three columns.  
    CREATE TABLE dbo.TestTable  
        (TestCol1 int NOT NULL,  
         TestCol2 nchar(10) NULL,  
         TestCol3 nvarchar(50) NULL);  
    GO  
    -- Create a clustered index called IX_TestTable_TestCol1  
    -- on the dbo.TestTable table using the TestCol1 column.  
    CREATE CLUSTERED INDEX IX_TestTable_TestCol1   
        ON dbo.TestTable (TestCol1);   
    GO  
    ```  
  
 有关详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [创建主键](../../relational-databases/tables/create-primary-keys.md)   
 [创建唯一约束](../../relational-databases/tables/create-unique-constraints.md)  
  
  
