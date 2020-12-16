---
description: 创建非聚集索引
title: 创建非聚集索引 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index creation [SQL Server], nonclustered indexes
- nonclustered indexes [SQL Server], creating
- nonclustered indexes [SQL Server], UNIQUE constraint
- indexes [SQL Server], nonclustered
- nonclustered indexes [SQL Server], PRIMARY KEY constraint
ms.assetid: 9402029a-1227-46c4-93aa-c2122eb1b943
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7c1831a5b16577b608629a09beb3c8d6e12c3634
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481178"
---
# <a name="create-nonclustered-indexes"></a>创建非聚集索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  您可以使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建非聚集索引。 非聚集索引是一种与存储在表中的数据相分离的索引结构，可对一个或多个选定列重新排序。 非聚集索引通常可帮助您通过比搜索基础表更快的速度查找数据；有时可以完全由非聚集索引中的数据回答查询，或非聚集索引可将 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 指向基础表中的行。 一般来说，创建非聚集索引是为了提高聚集索引不涵盖的频繁使用的查询的性能，或在没有聚集索引的表（称为堆）中查找行。 可以对表或索引视图创建多个非聚集索引。  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="typical-implementations"></a><a name="Implementations"></a> 典型实现  
 可以通过下列方法实现非聚集索引：  
  
-   **UNIQUE 约束**  
  
     在创建 UNIQUE 约束时，默认情况下将创建唯一非聚集索引，以便强制 UNIQUE 约束。 如果不存在该表的聚集索引，则可以指定唯一聚集索引。 有关详细信息，请参阅 [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md)。  
  
-   **独立于约束的索引**  
  
     默认情况下，如果未指定聚集，将创建非聚集索引。 对于每个表可创建的最大非聚集索引数为 999。 这包括使用 PRIMARY KEY 或 UNIQUE 约束创建的任何索引，但不包括 XML 索引。  
  
-   **索引视图的非聚集索引**  
  
     对视图创建唯一的聚集索引后，便可以创建非聚集索引。 有关详细信息，请参阅 [创建索引视图](../../relational-databases/views/create-indexed-views.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求对表或视图具有 ALTER 权限。 用户必须是 **sysadmin** 固定服务器角色的成员，或者是 **db_ddladmin** 和 **db_owner** 固定数据库角色的成员。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-nonclustered-index-by-using-the-table-designer"></a>使用表设计器创建非聚集索引  
  
1.  在“对象资源管理器”中，展开其中包含您要创建非聚集索引的表的数据库。  
  
2.  展开 **“表”** 文件夹。  
  
3.  右键单击你要创建非聚集索引的表，然后选择“设计”。  
  
4.  在“表设计器”菜单上，单击“索引/键”。  
  
5.  在“索引/键”对话框中，单击“添加”。  
  
6.  从“选定的主/唯一键或索引”文本框中选择新索引。  
  
7.  在网格中，选择“创建为聚集”，然后从该属性右侧的下拉列表中选择“否”。  
  
8.  单击“关闭”。  
  
9. 在“文件”菜单上，单击“保存”以保存 _table_name_。  

#### <a name="to-create-a-nonclustered-index-by-using-object-explorer"></a>使用对象资源管理器创建非聚集索引  
  
1.  在“对象资源管理器”中，展开其中包含您要创建非聚集索引的表的数据库。  
  
2.  展开 **“表”** 文件夹。  
  
3.  展开要为其创建非聚集索引的表。  
  
4.  右键单击“索引”文件夹，指向“新建索引”，然后选择“非群集索引…”  。  
  
5.  在 **“新建索引”** 对话框的 **“常规”** 页中，在 **“索引名称”** 框中输入新索引的名称。  
  
6.  在“索引键列”下，单击“添加…” 。  
  
7.  在“从 table_name 中选择列”对话框中，选中要添加到非聚集索引的一个或多个表列的复选框。  
  
8.  单击“确定”。  
  
9. 在 **“新建索引”** 对话框中，单击 **“确定”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-nonclustered-index-on-a-table"></a>对表创建非聚集索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    -- Find an existing index named IX_ProductVendor_VendorID and delete it if found.   
    IF EXISTS (SELECT name FROM sys.indexes  
                WHERE name = N'IX_ProductVendor_VendorID')   
        DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;   
    GO  
    -- Create a nonclustered index called IX_ProductVendor_VendorID   
    -- on the Purchasing.ProductVendor table using the BusinessEntityID column.   
    CREATE NONCLUSTERED INDEX IX_ProductVendor_VendorID   
        ON Purchasing.ProductVendor (BusinessEntityID);   
    GO  
    ```  
  
## <a name="related-content"></a>相关内容  
[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
[SQL Server 索引设计指南](../../relational-databases/sql-server-index-design-guide.md) 
