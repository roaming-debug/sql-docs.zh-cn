---
title: 禁用发布和分发 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio、Transact-SQL 或复制管理对象在 SQL Server 中禁用发布和分发。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- disabling publishing
- publishing [SQL Server replication], disabling
- distribution disabling [SQL Server replication]
- removing replication
- replication [SQL Server], removing
- disabling replication
- disabling distribution
ms.assetid: 6d4a1474-4d13-4826-8be2-80050fafa8a5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: c5ac700e65cc251f43afecaa910c3af67fcbdbdd
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076479"
---
# <a name="disable-publishing-and-distribution"></a>禁用发布和分发
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中禁用发布和分发。  
  
 您可以执行下列操作：  
  
-   删除分发服务器上的所有分发数据库。  
  
-   禁用使用分发服务器的所有发布服务器并删除这些发布服务器上的所有发布。  
  
-   删除对这些发布的所有订阅。 发布和订阅数据库中的数据不会被删除，但它将失去与任何发布数据库的同步关系。 如果要删除订阅服务器上的数据，则必须手动删除。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [先决条件](#Prerequisites)  
  
-   **禁用发布和分发，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   若要禁用发布和分发，所有分发数据库和发布数据库都必须联机。 如果存在分发数据库或发布数据库的“数据库快照”  ，则在禁用发布和分发前，必须先删除这些数据库快照。 数据库快照是数据库的只读脱机副本，与复制快照无关。 有关详细信息，请参阅[数据库快照 (SQL Server)](../../relational-databases/databases/database-snapshots-sql-server.md)。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 使用禁用发布和分发向导禁用发布和分发。  
  
#### <a name="to-disable-publishing-and-distribution"></a>禁用发布和分发  
  
1.  在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到要禁用的发布服务器或分发服务器，然后展开该服务器节点。  
  
2.  右键单击 **“复制”** 文件夹，再单击 **“禁用发布和分发”** 。  
  
3.  完成禁用发布和分发向导中的步骤。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程以编程方式禁用发布和分发。  
  
#### <a name="to-disable-publishing-and-distribution"></a>禁用发布和分发  
  
1.  停止所有与复制相关的作业。 有关作业名称列表，请参阅 [复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)的“SQL Server 代理下的代理安全性”部分。  
  
2.  在每个订阅服务器上，对订阅数据库执行 [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) 以从该数据库删除复制对象。 此存储过程不会删除分发服务器上的复制作业。  
  
3.  在发布服务器上，对发布数据库执行 [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) 以从该数据库删除复制对象。  
  
4.  如果发布服务器使用远程分发服务器，则执行 [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)。  
  
5.  在分发服务器上，执行 [sp_dropdistpublisher](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)。 应为在分发服务器上注册的每个发布服务器运行一次此存储过程。  
  
6.  在分发服务器上，执行 [sp_dropdistributiondb](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md) 以删除分发数据库。 应为在分发服务器上注册的每个分发数据库运行一次此存储过程。 此操作还将删除与分发数据库关联的任何队列读取器代理作业。  
  
7.  在分发服务器上，执行 [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md) 以从该服务器删除分发服务器指定。  
  
    > [!NOTE]  
    > 如果在您执行 [sp_dropdistpublisher](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md) 和 [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)之前并未删除所有复制发布和分发对象，这些过程将返回错误。 若要在删除发布服务器或分发服务器时删除所有与复制相关的对象，必须将 `@no_checks` 参数设置为 1。 如果发布服务器或分发服务器脱机或无法访问，则可以将 `@ignore_distributor` 参数设置为 1，以便能够删除它们；不过，必须手动删除任何保留的发布和分发对象。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此示例脚本从订阅数据库删除复制对象。  
  
 [!code-sql[HowTo#sp_removedbreplication](../../relational-databases/replication/codesnippet/tsql/disable-publishing-and-d_1.sql)]  
  
 此示例脚本在既是发布服务器又是分发服务器的服务器上禁用发布和分发，并删除分发数据库。  
  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/disable-publishing-and-d_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
  
#### <a name="to-disable-publishing-and-distribution"></a>禁用发布和分发  
  
1.  删除所有使用分发服务器的发布订阅。 有关详细信息，请参阅 [Delete a Pull Subscription](../../relational-databases/replication/delete-a-pull-subscription.md) 和 [Delete a Push Subscription](../../relational-databases/replication/delete-a-push-subscription.md)。  
  
2.  如果发布服务器和分发服务器在相同的服务器上，则删除所有使用分发服务器的发布，并禁用发布所有数据库。 有关详细信息，请参阅 [Delete a Publication](../../relational-databases/replication/publish/delete-a-publication.md)。  
  
3.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与分发服务器的连接。  
  
4.  创建的 <xref:Microsoft.SqlServer.Replication.DistributionPublisher> 类的实例。 指定 <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> 属性，并传递步骤 3 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
5.  （可选）调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法以获取对象的属性，并验证发布服务器是否存在。 如果此方法返回 **false**，则步骤 4 中设置的发布服务器名称不正确，或者此分发服务器未使用该发布服务器。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Remove%2A> 方法。 如果发布服务器和分发服务器在不同的服务器上，并且应在分发服务器上卸载发布服务器而没有首先验证发布服务器上是否不再存在发布时，将值 **true** 传递给 *force* 。  
  
7.  创建的 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 类的实例。 传递步骤 3 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
8.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationServer.UninstallDistributor%2A> 方法。 在没有首先验证所有本地发布数据库是否已禁用以及分发数据库是否已卸载的情况下，将 **true** 值传递给 *force* 以删除分发服务器上的所有复制对象。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 此示例将删除分发服务器上的发布服务器注册，删除分发数据库并卸载分发服务器。  
  
 [!code-cs[HowTo#rmo_DropDistPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropdistpub)]  
  
 [!code-vb[HowTo#rmo_vb_DropDistPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropdistpub)]  
  
 此示例在没有首先禁用本地发布数据库或删除分发数据库的情况下，卸载分发服务器。  
  
 [!code-cs[HowTo#rmo_DropDistPubForce](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropdistpubforce)]  
  
 [!code-vb[HowTo#rmo_vb_DropDistPubForce](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropdistpubforce)]  
  
## <a name="see-also"></a>另请参阅  
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
  
