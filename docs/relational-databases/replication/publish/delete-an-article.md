---
description: 删除项目
title: 删除项目 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- articles [SQL Server replication], dropping
- sp_droparticle
- sp_dropmergearticle
- deleting articles
- removing articles
- dropping articles
ms.assetid: 185b58fc-38c0-4abe-822e-6ec20066c863
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d29a89fb25fef977973e2353eed864e49d2808c9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477738"
---
# <a name="delete-an-article"></a>删除项目
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 中删除项目。 有关删除项目时使用的条件以及删除项目是否需要新的快照或重新初始化订阅的信息，请参阅[向现有发布添加项目和从中删除项目](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。  
  
 **本主题内容**  
  
-   **删除项目，使用：**  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程以编程方式删除项目。 使用的存储过程取决于项目所属的发布的类型。  
  
#### <a name="to-delete-an-article-from-a-snapshot-or-transactional-publication"></a>从快照或事务发布中删除项目  
  
1.  执行 [sp_droparticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md) 以从由 \@publication 指定的发布中删除由 \@article 指定的项目。 将 \@force_invalidate_snapshot 的值指定为 1 。  
  
2.  （可选）若要从数据库完全删除已发布的对象，请在发布服务器上对发布数据库执行 `DROP <objectname>` 命令。  

#### <a name="to-delete-an-article-from-a-merge-publication"></a>从合并发布删除项目  
  
1.  执行 [sp_dropmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md) 以从由 \@publication 指定的发布中删除由 \@article 指定的项目。 如有必要，可将 \@force_invalidate_snapshot 的值指定为 1，并将 \@force_reinit_subscription 的值指定为 1   。  
  
2.  （可选）若要从数据库完全删除已发布的对象，请在发布服务器上对发布数据库执行 `DROP <objectname>` 命令。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 下面的示例将从事务发布中删除项目。 因为此更改会使现有快照失效，所以 \@force_invalidate_snapshot 参数的值将会指定为 1 。  
  
```  
DECLARE @publication AS sysname;  
DECLARE @article AS sysname;  
SET @publication = N'AdvWorksProductTran';   
SET @article = N'Product';   
  
-- Drop the transactional article.  
USE [AdventureWorks]  
EXEC sp_droparticle   
  @publication = @publication,   
  @article = @article,  
  @force_invalidate_snapshot = 1;  
GO  
```  
  
 下面的示例将从合并发布中删除两个项目。 因为这些更改会使现有快照失效，所以 \@force_invalidate_snapshot 参数的值将会指定为 1 。  
  
```  
DECLARE @publication AS sysname;  
DECLARE @article1 AS sysname;  
DECLARE @article2 AS sysname;  
SET @publication = N'AdvWorksSalesOrdersMerge';  
SET @article1 = N'SalesOrderDetail';   
SET @article2 = N'SalesOrderHeader';   
  
-- Remove articles from a merge publication.  
USE [AdventureWorks]  
EXEC sp_dropmergearticle   
  @publication = @publication,   
  @article = @article1,  
  @force_invalidate_snapshot = 1;  
EXEC sp_dropmergearticle   
  @publication = @publication,   
  @article = @article2,  
  @force_invalidate_snapshot = 1;  
GO  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 可以使用复制管理对象 (RMO) 以编程方式删除项目。 用于删除项目的 RMO 类取决于该项目所属的发布的类型。  
  
#### <a name="to-delete-an-article-that-belongs-to-a-snapshot-or-transactional-publication"></a>删除属于快照发布或事务发布的项目  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.TransArticle> 类的实例。  
  
3.  设置 <xref:Microsoft.SqlServer.Replication.Article.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>和 <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> 属性。  
  
4.  为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置步骤 1 中的连接。  
  
5.  检查 <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> 属性以验证该项目是否存在。 如果此属性的值为 **false**，则步骤 3 中的项目属性定义不正确或此项目不存在。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.Article.Remove%2A> 方法。  
  
7.  关闭所有连接。  
  
#### <a name="to-delete-an-article-that-belongs-to-a-merge-publication"></a>删除属于合并发布的项目  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.MergeArticle> 类的实例。  
  
3.  设置 <xref:Microsoft.SqlServer.Replication.Article.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>和 <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> 属性。  
  
4.  为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置步骤 1 中的连接。  
  
5.  检查 <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> 属性以验证该项目是否存在。 如果此属性的值为 **false**，则步骤 3 中的项目属性定义不正确或此项目不存在。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.Article.Remove%2A> 方法。  
  
7.  关闭所有连接。  
  
## <a name="see-also"></a>另请参阅  
 [向现有发布添加项目和从中删除项目](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
  
