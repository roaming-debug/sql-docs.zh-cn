---
description: 删除请求订阅
title: 删除请求订阅 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- removing subscriptions
- deleting subscriptions
- pull subscriptions [SQL Server replication], deleting
- subscriptions [SQL Server replication], pull
ms.assetid: 997c0b8e-d8d9-4eed-85b1-6baa1f8594ce
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 531790cbcf7bfcc903daaf101be6a7cc42836155
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479968"
---
# <a name="delete-a-pull-subscription"></a>删除请求订阅
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中删除请求订阅。  
  
 **本主题内容**  
  
-   **删除请求订阅，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 在发布服务器上删除请求订阅（从 **的** “本地发布” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]文件夹中），或在订阅服务器上删除请求订阅（从 **“本地订阅”** 文件夹中）。 删除订阅不会从订阅中删除对象或数据，必须对其手动删除。  
  
#### <a name="to-delete-a-pull-subscription-at-the-publisher"></a>在发布服务器上删除请求订阅  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中连接到发布服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。  
  
3.  展开与要删除的订阅关联的发布。  
  
4.  右键单击该订阅，再单击 **“删除”** 。  
  
5.  在确认对话框中，选择是否连接到订阅服务器以删除订阅信息。 如果清除 **“连接到订阅服务器”** 复选框，则应在以后连接到订阅服务器以删除订阅信息。  
  
#### <a name="to-delete-a-pull-subscription-at-the-subscriber"></a>在订阅服务器上删除请求订阅  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中连接到订阅服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地订阅”** 文件夹。  
  
3.  右键单击要删除的订阅，再单击 **“删除”** 。  
  
4.  在确认对话框中，选择是否连接到发布服务器以删除订阅信息。 如果清除 **“连接到发布服务器”** 复选框，则应在以后连接到发布服务器以删除订阅信息。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程以编程方式删除请求订阅。 所用的存储过程取决于订阅所属的发布的类型。  
  
#### <a name="to-delete-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>删除对快照发布或事务发布的请求订阅  
  
1.  在订阅服务器上，对订阅数据库执行 [sp_droppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)。 指定 \@publication、\@publisher 和 \@publisher_db  。  
  
2.  在发布服务器上，对发布数据库执行 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)。 指定 \@publication 和 \@subscriber 。 将 \@article 的值指定为 all 。 （可选）如果无法访问分发服务器，请将 \@ignore_distributor 的值指定为 1，以便在不删除分发服务器上相关对象的情况下删除订阅 。  
  
#### <a name="to-delete-a-pull-subscription-to-a-merge-publication"></a>删除对合并发布的请求订阅  
  
1.  在订阅服务器上，对订阅数据库执行 [sp_dropmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)。 指定 \@publication、\@publisher 和 \@publisher_db  。  
  
2.  在发布服务器上，对发布数据库执行 [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)。 指定 \@publication、\@subscriber 和 \@subscriber_db  。 将 \@subscription_type 的值指定为 pull。 （可选）如果无法访问分发服务器，请将 \@ignore_distributor 的值指定为 1，以便在不删除分发服务器上相关对象的情况下删除订阅 。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 以下示例删除对事务发布的请求订阅。 第一个批处理在订阅服务器上执行，第二个批处理在发布服务器上执行。  
  
 [!code-sql[HowTo#sp_droptranpullsubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_1.sql)]  
  
 [!code-sql[HowTo#sp_droptransubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_2.sql)]  
  
 以下示例删除对合并发布的请求订阅。 第一个批处理在订阅服务器上执行，第二个批处理在发布服务器上执行。  
  
 [!code-sql[HowTo#sp_dropmergepullsubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_3.sql)]  
  
 [!code-sql[HowTo#sp_dropmergesubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_4.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 可通过使用复制管理对象 (RMO) 以编程方式删除请求订阅。 用于删除请求订阅的 RMO 类由该请求订阅所订阅的发布类型决定。  
  
#### <a name="to-delete-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>删除对快照发布或事务发布的请求订阅  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与订阅服务器和发布服务器的连接。  
  
2.  创建 <xref:Microsoft.SqlServer.Replication.TransPullSubscription> 类的一个实例，并设置 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>、 <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>、 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>以及 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> 属性。 使用步骤 1 中的订阅服务器连接来设置 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性。  
  
3.  检查 <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> 属性以确保该订阅存在。 如果此属性的值为 **false**，则步骤 2 中所定义的订阅属性不正确或者该订阅不存在。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.PullSubscription.Remove%2A> 方法。  
  
5.  使用步骤 1 中的发布服务器连接，创建 <xref:Microsoft.SqlServer.Replication.TransPublication> 类的实例。 指定 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 和 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果该方法返回 **false**，则表示步骤 5 中指定的属性不正确，或者服务器中不存在发布。  
  
7.  调用 <xref:Microsoft.SqlServer.Replication.TransPublication.RemovePullSubscription%2A> 方法。 将订阅服务器和订阅数据库的名称分别指定给 *subscriber* 和 *subscriberDB* 参数。  
  
#### <a name="to-delete-a-pull-subscription-to-a-merge-publication"></a>删除对合并发布的请求订阅  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与订阅服务器和发布服务器的连接。  
  
2.  创建 <xref:Microsoft.SqlServer.Replication.MergePullSubscription> 类的一个实例，并设置 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>、 <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>、 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>以及 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> 属性。 使用步骤 1 中的连接来设置 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性。  
  
3.  检查 <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> 属性以确保该订阅存在。 如果此属性的值为 **false**，则步骤 2 中所定义的订阅属性不正确或者该订阅不存在。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.PullSubscription.Remove%2A> 方法。  
  
5.  使用步骤 1 中的发布服务器连接，创建 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例。 指定 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 和 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果该方法返回 **false**，则表示步骤 5 中指定的属性不正确，或者服务器中不存在发布。  
  
7.  调用 <xref:Microsoft.SqlServer.Replication.MergePublication.RemovePullSubscription%2A> 方法。 将订阅服务器和订阅数据库的名称分别指定给 *subscriber* 和 *subscriberDB* 参数。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 本示例将删除对事务发布的请求订阅并删除发布服务器上的订阅注册。  
  
 [!code-cs[HowTo#rmo_DropTranPullSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_droptranpullsub)]  
  
 [!code-vb[HowTo#rmo_vb_DropTranPullSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_droptranpullsub)]  
  
 本示例将删除对合并发布的请求订阅并删除发布服务器上的订阅注册。  
  
 [!code-cs[HowTo#rmo_DropMergePullSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropmergepullsub)]  
  
 [!code-vb[HowTo#rmo_vb_DropMergePullSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropmergepullsub)]  
  
## <a name="see-also"></a>另请参阅  
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
