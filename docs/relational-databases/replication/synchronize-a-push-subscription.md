---
title: 同步推送订阅 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio、复制代理或复制管理对象在 SQL Server 中同步推送订阅。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- synchronization [SQL Server replication], push subscriptions
- subscriptions [SQL Server replication], push
- push subscriptions [SQL Server replication], synchronizing
ms.assetid: 0cfa7ae5-91d3-4a4f-9edf-a852d45783b5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 14240f40dcd00f439c0aa5f368ebe4f331641414
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250556"
---
# <a name="synchronize-a-push-subscription"></a>同步推送订阅
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]复制代理 [或复制管理对象 (RMO) 在](../../relational-databases/replication/agents/replication-agents-overview.md)中同步推送订阅。  
  
[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]

##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 订阅由分发代理（对于快照复制和事务复制）或合并代理（对于合并复制）进行同步。 代理可以连续运行、按需运行或按计划运行。 有关如何指定同步计划的详细信息，请参阅[指定同步计划](../../relational-databases/replication/specify-synchronization-schedules.md)。  
  
 从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 的“本地发布”和“本地订阅”文件夹及复制监视器的“所有订阅”选项卡中，按需同步订阅  。 不能从订阅服务器按需同步对 Oracle 发布的订阅。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
#### <a name="to-synchronize-a-push-subscription-on-demand-in-management-studio-at-the-publisher"></a>在 Management Studio 中按需同步推送订阅（在发布服务器中）  
  
1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中连接到发布服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。  
  
3.  展开要同步其订阅的发布。  
  
4.  右键单击要同步的订阅，然后单击 **“查看同步状态”** 。  
  
5.  在“查看同步状态 - \<Subscriber>:\<SubscriptionDatabase>”对话框中，单击“启动” 。 完成同步后，将显示消息 **“同步完成”** 。  
  
6.  单击“关闭”。  

#### <a name="to-synchronize-a-push-subscription-on-demand-in-management-studio-at-the-subscriber"></a>在 Management Studio 中按需同步推送订阅（在订阅服务器中）  
  
1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中连接到订阅服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地订阅”** 文件夹。  
  
3.  右键单击要同步的订阅，然后单击 **“查看同步状态”** 。  
  
4.  将显示一条消息，指示建立与分发服务器的连接。 单击“确定”。  
  
5.  在“查看同步状态 - \<Subscriber>:\<SubscriptionDatabase>”对话框中，单击“启动” 。 完成同步后，将显示消息 **“同步完成”** 。  
  
6.  单击“关闭”。  
  
#### <a name="to-synchronize-a-push-subscription-on-demand-in-replication-monitor"></a>在复制监视器中按需同步推送订阅  
  
1.  在复制监视器的左窗格中依次展开发布服务器组、发布服务器，再单击一个发布。  
  
2.  单击 **“所有订阅”** 选项卡。  
  
3.  右键单击要同步的订阅，然后单击 **“开始同步”** 。  
  
4.  若要查看同步进度，请右键单击该订阅，然后单击 **“查看详细信息”** 。  
  
##  <a name="using-replication-agents"></a><a name="ReplProg"></a> 使用复制代理  
 可通过在命令提示符下调用相应的复制代理可执行文件，以编程方式按需同步推送订阅。 被调用的复制代理可执行文件将取决于推送订阅所属的发布的类型。  
  
#### <a name="to-start-the-distribution-agent-to-synchronize-a-push-subscription-to-a-transactional-publication"></a>启动分发代理以将推送订阅与事务发布进行同步  
  
1.  通过命令提示符或分发服务器中的批处理文件，执行 **distrib.exe**。 指定下列命令行参数：  
  
    -   **-Publisher**  
  
    -   **-PublisherDB**  
  
    -   **-Distributor**  
  
    -   **-Subscriber**  
  
    -   **-SubscriberDB**  
  
    -   **-SubscriptionType = 0**  
  
     如果您使用的是 SQL Server 身份验证，则还必须指定下列参数：  
  
    -   **-DistributorLogin**  
  
    -   **-DistributorPassword**  
  
    -   **-DistributorSecurityMode = 0**  
  
    -   **-PublisherLogin**  
  
    -   **-PublisherPassword**  
  
    -   **-PublisherSecurityMode = 0**  
  
    -   **-SubscriberLogin**  
  
    -   **-SubscriberPassword**  
  
    -   **-SubscriberSecurityMode = 0**  
  
        > [!IMPORTANT]  
        >  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
#### <a name="to-start-the-merge-agent-to-synchronize-a-push-subscription-to-a-merge-publication"></a>启动合并代理以将推送订阅与合并发布进行同步  
  
1.  通过命令提示符或分发服务器中的批处理文件，执行 **replmerg.exe**。 指定下列命令行参数：  
  
    -   **-Publisher**  
  
    -   **-PublisherDB**  
  
    -   **-Publication**  
  
    -   **-Distributor**  
  
    -   **-Subscriber**  
  
    -   **-SubscriberDB**  
  
    -   **-SubscriptionType = 0**  
  
     如果您使用的是 SQL Server 身份验证，则还必须指定下列参数：  
  
    -   **-DistributorLogin**  
  
    -   **-DistributorPassword**  
  
    -   **-DistributorSecurityMode = 0**  
  
    -   **-PublisherLogin**  
  
    -   **-PublisherPassword**  
  
    -   **-PublisherSecurityMode = 0**  
  
    -   **-SubscriberLogin**  
  
    -   **-SubscriberPassword**  
  
    -   **-SubscriberSecurityMode = 0**  
  
        > [!IMPORTANT]  
        >  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
###  <a name="examples-replication-agents"></a><a name="TsqlExample"></a> 示例（复制代理）  
 以下示例启动分发代理以同步推送订阅。  
  
```  
  
REM -- Declare the variables.  
SET Publisher=%instancename%  
SET Subscriber=%instancename%  
SET PublicationDB=AdventureWorks2012  
SET SubscriptionDB=AdventureWorks2012Replica   
SET Publication=AdvWorksProductsTran  
  
REM -- Start the Distribution Agent with four subscription streams.  
REM -- The following command must be supplied without line breaks.  
"C:\Program Files\Microsoft SQL Server\120\COM\DISTRIB.EXE" -Subscriber %Subscriber%   
-SubscriberDB %SubscriptionDB% -SubscriberSecurityMode 1 -Publication %Publication%   
-Publisher %Publisher% -PublisherDB %PublicationDB% -Distributor %Publisher%   
-DistributorSecurityMode 1 -Continuous -SubscriptionType 0 -SubscriptionStreams 4  
  
```  
  
 以下示例启动合并代理以同步推送订阅。  
  
```  
  
REM -- Declare the variables.  
SET Publisher=%instancename%  
SET Subscriber=%instancename%  
SET PublicationDB=AdventureWorks2012  
SET SubscriptionDB=AdventureWorks2012Replica   
SET Publication=AdvWorksSalesOrdersMerge  
  
REM -- Start the Merge Agent.  
REM -- The following command must be supplied without line breaks.  
"C:\Program Files\Microsoft SQL Server\120\COM\REPLMERG.EXE"  -Publisher %Publisher%   
-Subscriber  %Subscriber%  -Distributor %Publisher% -PublisherDB  %PublicationDB%   
-SubscriberDB %SubscriptionDB% -Publication %Publication% -PublisherSecurityMode 1   
-OutputVerboseLevel 3  -Output -SubscriberSecurityMode 1  -SubscriptionType 0   
-DistributorSecurityMode 1  
  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 可以使用复制管理对象 (RMO) 和托管代码的复制代理功能访问权限以编程方式同步推送订阅。 用于同步推送订阅的类取决于订阅所属的发布的类型。  
  
> [!NOTE]
>  如果您要启动一个自主运行而不影响应用程序的订阅，请异步启动代理。 但是，如果您要监视同步的结果并在同步进程期间从代理处接收回调（例如，您要显示进度栏），则应当同步启动代理。 对于 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] 订阅服务器，必须同步启动代理。  
  
#### <a name="to-synchronize-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>将推送订阅与快照发布或事务发布进行同步  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与分发服务器的连接。  
  
2.  创建 <xref:Microsoft.SqlServer.Replication.TransSubscription> 类的实例并设置下列属性：  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>的发布数据库的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>的订阅所属的发布的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>的订阅数据库的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>的订阅服务器的名称。  
  
    -   在步骤 1 中为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>创建的连接。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法以获取其他订阅属性。 如果该方法返回 **false**，则确保订阅存在。  
  
4.  使用下列方法之一在分发服务器中启动分发代理：  
  
    -   在步骤 2 中的 <xref:Microsoft.SqlServer.Replication.TransSubscription.SynchronizeWithJob%2A> 实例上调用 <xref:Microsoft.SqlServer.Replication.TransSubscription> 方法。 该方法异步启动分发代理，并在代理作业运行时立即将控制权返回给您的应用程序。 如果创建的订阅的 **false** for <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A>。  
  
    -   获取 <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent> 属性中 <xref:Microsoft.SqlServer.Replication.TransSubscription.SynchronizationAgent%2A> 类的实例，然后调用 <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.Synchronize%2A> 方法。 该方法可以同步启动代理，并且控制权仍属于运行代理作业。 在执行同步期间，您可以在代理仍旧运行的情况下处理 <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.Status> 事件。  
  
#### <a name="to-synchronize-a-push-subscription-to-a-merge-publication"></a>将推送订阅与合并发布进行同步  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与分发服务器的连接。  
  
2.  创建 <xref:Microsoft.SqlServer.Replication.MergeSubscription> 类的实例并设置下列属性：  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>的发布数据库的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>的订阅所属的发布的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>的订阅数据库的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>的订阅服务器的名称。  
  
    -   在步骤 1 中为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>创建的连接。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法以获取其他订阅属性。 如果该方法返回 **false**，则确保订阅存在。  
  
4.  使用下列方法之一在分发服务器中启动合并代理：  
  
    -   在步骤 2 中的 <xref:Microsoft.SqlServer.Replication.MergeSubscription.SynchronizeWithJob%2A> 实例上调用 <xref:Microsoft.SqlServer.Replication.MergeSubscription> 方法。 该方法异步启动合并代理，并在代理作业运行时立即将控制权返回给您的应用程序。 如果创建的订阅的 **false** for <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A>。  
  
    -   获取 <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent> 属性中 <xref:Microsoft.SqlServer.Replication.MergeSubscription.SynchronizationAgent%2A> 类的实例，然后调用 <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.Synchronize%2A> 方法。 该方法可以同步启动合并代理，并且控件仍属于运行代理作业。 在执行同步期间，您可以在代理仍旧运行的情况下处理 <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.Status> 事件。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 该示例将推送订阅与事务发布进行同步，其中，代理使用代理作业进行异步启动。  
  
 [!code-cs[HowTo#rmo_SyncTranPushSub_WithJob](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_synctranpushsub_withjob)]  
  
 [!code-vb[HowTo#rmo_vb_SyncTranPushSub_WithJob](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_synctranpushsub_withjob)]  
  
 该示例将推送订阅与事务发布进行同步，其中，代理进行同步启动。  
  
 [!code-cs[HowTo#rmo_SyncTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_synctranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_SyncTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_synctranpushsub)]  
  
 该示例将推送订阅与合并发布进行同步，其中，代理使用代理作业进行异步启动。  
  
 [!code-cs[HowTo#rmo_SyncMergePushSub_WithJob](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_syncmergepushsub_withjob)]  
  
 [!code-vb[HowTo#rmo_vb_SyncMergePushSub_WithJob](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_syncmergepushsub_withjob)]  
  
 该示例将推送订阅与合并发布进行同步，其中，代理进行同步启动。  
  
 [!code-cs[HowTo#rmo_SyncMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_syncmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_SyncMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_syncmergepushsub)]  
  
## <a name="see-also"></a>另请参阅  
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [同步数据](../../relational-databases/replication/synchronize-data.md)   
 [复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
