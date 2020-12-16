---
description: 删除索引
title: 删除索引 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- removing indexes
- deleting indexes
- dropping indexes
- indexes [SQL Server], dropping
- index deletions [SQL Server]
ms.assetid: fd38a0ed-26c4-4c76-9ef7-e0a16147329d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: b76d22d712327cac381a7a615d0170f76d476f34
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479998"
---
# <a name="delete-an-index"></a>删除索引
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  本主题将说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中删除索引。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要删除索引，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 不能使用此方法删除作为 PRIMARY KEY 或 UNIQUE 约束的结果而创建的索引。 而必须删除该约束。 若要删除该约束和相应的索引，请在 [中使用带有 DROP CONSTRAINT 子句的](../../t-sql/statements/alter-table-transact-sql.md) ALTER TABLE [!INCLUDE[tsql](../../includes/tsql-md.md)]。 有关详细信息，请参阅 [Delete Primary Keys](../../relational-databases/tables/delete-primary-keys.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求对表或视图具有 ALTER 权限。 默认情况下，将向 **sysadmin** 固定服务器角色以及 **db_ddladmin** 和 **db_owner** 固定数据库角色授予此权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-delete-an-index-by-using-object-explorer"></a>通过使用对象资源管理器删除索引  
  
1.  在“对象资源管理器”中，展开包含您要删除索引的表的数据库。  
  
2.  展开 **“表”** 文件夹。  
  
3.  展开包含您要删除的索引的表。  
  
4.  展开 **“索引”** 文件夹。  
  
5.  右键单击要删除的索引，然后选择“删除”。  
  
6.  在 **“删除对象”** 对话框中，确认正确的索引位于 **“要重删除的对象”** 网格中，然后单击 **“确定”**。  
  
#### <a name="to-delete-an-index-using-table-designer"></a>使用表设计器删除索引  
  
1.  在“对象资源管理器”中，展开包含您要删除索引的表的数据库。  
  
2.  展开 **“表”** 文件夹。  
  
3.  右键单击包含您要删除的索引的表，然后单击“设计”。  
  
4.  在“表设计器”菜单上，单击“索引/键”。  
  
5.  在“索引/键”对话框中，选择要删除的索引。  
  
6.  单击 **“删除”** 。  
  
7.  单击“关闭”。  
  
8.  在“文件”菜单上，选择“保存”以保存 _table_name_。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-delete-an-index"></a>删除索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- delete the IX_ProductVendor_BusinessEntityID index  
    -- from the Purchasing.ProductVendor table  
    DROP INDEX IX_ProductVendor_BusinessEntityID   
        ON Purchasing.ProductVendor;  
    GO  
    ```  
  
 有关详细信息，请参阅 [DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)  
  
  
