---
description: 创建带有包含列的索引
title: 创建带有包含列的索引 | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index size [SQL Server]
- index keys [SQL Server]
- index columns [SQL Server]
- size [SQL Server], indexes
- key columns [SQL Server]
- included columns
- nonclustered indexes [SQL Server], included columns
- designing indexes [SQL Server], included columns
- nonkey columns
ms.assetid: d198648d-fea5-416d-9f30-f9d4aebbf4ec
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ce010e0093e234c9095c440f3283b8c7942c78bc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480048"
---
# <a name="create-indexes-with-included-columns"></a>创建带有包含列的索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题说明如何通过使用 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，添加包含列（或非键列）以便在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中扩展非聚集索引的功能。 通过包含非键列，可以创建覆盖更多查询的非聚集索引。 这是因为非键列具有下列优点：  
  
-   它们可以是不允许作为索引键列的数据类型。  
-   在计算索引键列数或索引键大小时， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 不考虑它们。  
  
 当查询中的所有列都作为键列或非键列包含在索引中时，带有包含性非键列的索引可以显著提高查询性能。 这样可以实现性能提升，因为查询优化器可以在索引中找到所有列值；不访问表或聚集索引数据，从而减少磁盘 I/O 操作。  
  
> [!NOTE]  
> 当索引包含查询引用的所有列时，它通常称为“覆盖查询”。  
   
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="design-recommendations"></a><a name="DesignRecs"></a> 设计建议  
  
-   重新设计索引键大小较大的非聚集索引，以便只有用于搜索和查找的列为键列。 使覆盖查询的所有其他列成为非键列。 这样，将具有覆盖查询所需的所有列，但索引键本身较小，而且效率高。  
  
-   将非键列包含在非聚集索引中，以避免超过当前索引大小的限制（最大键列数为 32，最大索引键大小为 1,700 字节，而在 [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] 以前，最大键列数为 16，最大索引键大小为 900 字节）。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 计算索引键列数或索引键大小时，不考虑非键列。  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   只能对非聚集索引定义非键列。  
  
-   除了 **text**、 **ntext** 和 **image** 之外的所有数据类型都可以用作非键列。  
  
-   精确或不精确的确定性计算列都可以是非键列。 有关详细信息，请参阅 [计算列上的索引](../../relational-databases/indexes/indexes-on-computed-columns.md)。  
  
-   只要允许将计算列数据类型作为非键索引列，从 **image**、 **ntext** 和 **text** 数据类型派生的计算列就可以作为非键索引列。  
  
-   除非先删除某一表的索引，否则无法从该表中删除非键列。  
  
-   除进行下列更改外，不能对非键列进行其他更改：  
  
    -   将列的为空性从 NOT NULL 改为 NULL。  
  
    -   增加 **varchar**、 **nvarchar** 或 **varbinary** 列的长度。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求对表或视图具有 ALTER 权限。 用户必须是 **sysadmin** 固定服务器角色的成员，或者是 **db_ddladmin** 和 **db_owner** 固定数据库角色的成员。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-an-index-with-nonkey-columns"></a>创建带有非键列的索引  
  
1.  在对象资源管理器中，单击加号以便展开包含您要创建带有非键列的索引的表的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以便展开您要创建带有非键列的索引的表。  
  
4.  右键单击“索引”文件夹，指向“新建索引”，然后选择“非群集索引…”  。  
  
5.  在 **“新建索引”** 对话框的 **“常规”** 页中，在 **“索引名称”** 框中输入新索引的名称。  
  
6.  在“索引键列”选项卡下，单击“添加…” 。  
  
7.  在“从 _table\_name_ 中选择列”对话框中，选中要添加到索引的一个或多个表列的复选框。  
  
8.  单击“确定”。  
  
9. 在“包含性列”选项卡下，单击“添加…” 。  
  
10. 在“从 _table\_name_ 中选择列”对话框中，选中要作为非键列添加到索引的一个或多个表列的复选框。  
  
11. 单击“确定”。  
  
12. 在 **“新建索引”** 对话框中，单击 **“确定”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-an-index-with-nonkey-columns"></a>创建带有非键列的索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    -- Creates a nonclustered index on the Person.Address table with four included (nonkey) columns.   
    -- index key column is PostalCode and the nonkey columns are  
    -- AddressLine1, AddressLine2, City, and StateProvinceID.  
    CREATE NONCLUSTERED INDEX IX_Address_PostalCode  
    ON Person.Address (PostalCode)  
    INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);  
    GO  
    ```  

## <a name="related-content"></a>相关内容  
[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)    
[SQL Server 索引设计指南](../../relational-databases/sql-server-index-design-guide.md)   
