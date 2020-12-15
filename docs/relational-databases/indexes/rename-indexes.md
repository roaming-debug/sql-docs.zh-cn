---
description: 重命名索引
title: 重命名索引 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- renaming indexes
- index names [SQL Server]
- indexes [SQL Server], renaming
ms.assetid: d3d612a1-ea1b-4d99-85d2-0a2ad54f4b0e
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f073002a08f9f3d07a4d0d6d8af3976c0bfb6dae
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97407086"
---
# <a name="rename-indexes"></a>重命名索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题将说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中重命名索引。 重命名索引将用提供的新名称替换当前的索引名称。 指定的名称在表或视图中必须是唯一的。 例如，两个表可以有一个名为 **XPK_1** 的索引，但同一表中不能有两个名为 **XPK_1** 的索引。 无法创建与现有禁用索引同名的索引。 重命名索引不会导致重新生成索引。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要重命名索引，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 在表中创建 PRIMARY KEY 或 UNIQUE 约束时，会在表中自动创建一个与该约束同名的索引。 因为索引名称在表中必须是唯一的，所以无法通过创建或重命名获得一个与该表的现有 PRIMARY KEY 或 UNIQUE 约束同名的索引。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要具有针对索引的 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-rename-an-index-by-using-the-table-designer"></a>使用表设计器重命名索引  
  
1.  在对象资源管理器中，单击加号以便展开包含您要重命名索引的表的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  右键单击你要重命名索引的表，然后选择“设计”。  
  
4.  在“表设计器”菜单上，单击“索引/键”。  
  
5.  在“选定的主/唯一键或索引”文本框中，选择你要重命名的索引。  
  
6.  在网格中，单击 **“名称”** 并在文本框中键入新名称。  
  
7.  单击“关闭”。  
  
8.  在“文件”菜单上，单击“保存”以保存 _table_name_。  

#### <a name="to-rename-an-index-by-using-object-explorer"></a>通过使用对象资源管理器重命名索引  
  
1.  在对象资源管理器中，单击加号以便展开包含您要重命名索引的表的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以便展开您要重命名索引的表。  
  
4.  单击加号以便展开 **“索引”** 文件夹。  
  
5.  右键单击要重命名的索引，然后选择“重命名”。  
  
6.  键入索引的新名称，再按 Enter。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-rename-an-index"></a>重命名索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    --Renames the IX_ProductVendor_VendorID index on the Purchasing.ProductVendor table to IX_VendorID.   
  
    EXEC sp_rename N'Purchasing.ProductVendor.IX_ProductVendor_VendorID', N'IX_VendorID', N'INDEX';   
    GO  
    ```  
  
 有关详细信息，请参阅 [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)。  
  
  
