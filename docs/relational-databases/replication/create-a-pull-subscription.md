---
title: 创建请求订阅 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio、Transact-SQL 或复制管理对象在 SQL Server 中创建请求订阅。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- pull subscriptions [SQL Server replication], creating
- merge replication subscribing [SQL Server replication], pull subscriptions
- subscriptions [SQL Server replication], pull
- snapshot replication [SQL Server], subscribing
- transactional replication, subscribing
ms.assetid: 41d1886d-59c9-41fc-9bd6-a59b40e0af6e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a84517056baaf5cf4738add3e85a9297b82ec408
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480028"
---
# <a name="create-a-pull-subscription"></a>创建请求订阅

<!--
2019-10-24 , GeneMi:
This article .md file exists in the so-called "2016+" section of repo 'sql-docs-pr'.
No article in 2016+ should ever have the moniker 'sql-server-2014' on its metadata line 'monikerRange:', I think.
Presently 'sql-server-2014' moniker is on this 'monikerRange'. This situation deserves further investigation.
-->

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建请求订阅。  
  
 可以通过脚本设置 P2P 复制的请求订阅，但是不能通过向导这样做。  
 
  ##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 使用新建订阅向导在发布服务器或订阅服务器中创建请求订阅。 按照向导中的页的指示执行下列操作：  
  
-   指定发布服务器和发布。  
  
-   选择运行复制代理的位置。 对于请求订阅，根据发布类型的不同，请在 **“分发代理位置”** 页或 **“合并代理位置”** 页上选择 **“在其订阅服务器上运行每个代理(请求订阅)”** 。  
  
-   指定订阅服务器和订阅数据库。  
  
-   指定复制代理建立连接所用的登录名和密码：  
  
    -   对于快照发布和事务发布的订阅，在 **“分发代理安全性”** 页上指定凭据。  
  
    -   对于合并发布的订阅，在 **“合并代理安全性”** 页上指定凭据。  
  
     有关每个代理所需权限的信息，请参阅 [R复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
-   指定同步计划和初始化订阅服务器的时间。  
  
-   指定合并发布的其他选项：订阅类型；参数化筛选值；如果发布启用了 Web 同步，则还需指定要通过 HTTPS 同步的信息。  
  
-   指定允许更新订阅的事务发布的其他选项：订阅服务器是立即在发布服务器上提交更改还是将它们写入队列、用于从订阅服务器连接到发布服务器的凭据。  
  
-   还可以编写订阅的脚本（可选）。  
  
#### <a name="to-create-a-pull-subscription-from-the-publisher"></a>从发布服务器创建请求订阅  
  
1.  在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。  
  
3.  右键单击要为其创建一个或多个订阅的发布，然后单击 **“新建订阅”** 。  
  
4.  完成新建订阅向导中的页。  

#### <a name="to-create-a-pull-subscription-from-the-subscriber"></a>从订阅服务器创建请求订阅  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中连接到订阅服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹。  
  
3.  右键单击 **“本地订阅”** 文件夹，再单击 **“新建订阅”** 。  
  
4.  在“新建订阅向导”的“发布”页上，从“发布服务器”下拉列表中选择 \<Find SQL Server Publisher> 或 \<Find Oracle Publisher>   。  
  
5.  在 **“连接到服务器”** 对话框中连接到发布服务器。  
  
6.  在 **“发布”** 页上，选择一个发布。  
  
7.  完成新建订阅向导中的页。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程以编程方式创建请求订阅。 所用的存储过程取决于订阅所属的发布的类型。  
  
#### <a name="to-create-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>创建快照或事务发布的请求订阅  
  
1.  在发布服务器上，通过执行 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) 验证发布是否支持请求订阅。  
  
    -   如果结果集中 **allow_pull** 的值为 **1**，则发布支持请求订阅。  
  
    -   如果 allow_pull 的值为 0，则执行 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)，将 \@property 指定为 allow_pull，将 \@value 指定为 true。  
  
2.  在订阅服务器上，执行 [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)。 指定 \@publisher 和 \@publication。 有关更新订阅的信息，请参阅 [创建事务发布的可更新订阅](publish/create-an-updatable-subscription-to-a-transactional-publication.md)。   
  
3.  在订阅服务器上，执行 [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)。 指定下列各项：  
  
    -   \@publisher、\@publisher_db 和 \@publication 参数。  
  
    -   订阅服务器中的分发代理运行时所使用的 \@job_login 和 \@job_password 指定的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 凭据。  
  
        > [!NOTE]  
        >  使用 Windows 集成身份验证进行的连接始终使用由 \@job_login 和 \@job_password 指定的 Windows 凭据。 分发代理始终使用 Windows 集成身份验证与订阅服务器建立本地连接。 默认情况下，该代理将使用 Windows 集成身份验证连接到分发服务器。  
  
    -   （可选）\@distributor_security_mode 的 0 值以及 \@distributor_login 和 \@distributor_password 的 SQL Server 登录信息，如果需要在连接到分发服务器时使用 SQL Server 身份验证，请指定这些参数。  
  
    -   该订阅的分发代理作业计划。 有关详细信息，请参阅 [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md)。  
  
4.  在发布服务器上，执行 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) 以注册请求订阅。 指定 \@publication、\@subscriber 和 \@destination_db。 将 \@subscription_type 的值指定为 pull。  
  
#### <a name="to-create-a-pull-subscription-to-a-merge-publication"></a>创建合并发布的请求订阅  
  
1.  在发布服务器上，通过执行 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) 验证发布是否支持请求订阅。  
  
    -   如果结果集中 **allow_pull** 的值为 **1**，则发布支持请求订阅。  
  
    -   如果 allow_pull 的值为 0，则执行 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)，将 \@property 指定为 allow_pull，将 \@value 指定为 true。  
  
2.  在订阅服务器上，执行 [sp_addmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)。 指定 \@publisher、\@publisher_db、\@publication 以及下列参数：  
  
    -   \@subscriber_type - 对于客户端订阅指定 local，对于服务器订阅指定 global。  
  
    -   \@subscription_priority – 指定订阅的优先级（从 0.00 到 99.99）。 只有服务器订阅要求指定优先级。  
  
         有关详细信息，请参阅 [高级合并复制冲突的检测和解决](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)。  
  
3.  在订阅服务器上，执行 [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)。 指定下列参数：  
  
    -   \@publisher、\@publisher_db 和 \@publication。  
  
    -   订阅服务器中的合并代理运行时所使用的 \@job_login 和 \@job_password 指定的 Windows 凭据。  
  
        > [!NOTE]  
        >  使用 Windows 集成身份验证进行的连接始终使用由 \@job_login 和 \@job_password 指定的 Windows 凭据。 合并代理始终使用 Windows 集成身份验证与订阅服务器进行本地连接。 默认情况下，该代理将使用 Windows 集成身份验证连接到分发服务器和发布服务器。  
  
    -   （可选）\@distributor_security_mode 的 0 值以及 \@distributor_login 和 \@distributor_password 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录信息，如果需要在连接到分发服务器时使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，请指定这些参数。  
  
    -   （可选）\@publisher_security_mode 的 0 值以及 \@publisher_login 和 \@publisher_password 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录信息，如果需要在连接到发布服务器时使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，请指定这些参数。  
  
    -   该订阅的合并代理作业计划。 有关详细信息，请参阅 [创建事务发布的可更新订阅](publish/create-an-updatable-subscription-to-a-transactional-publication.md)。  
  
4.  在发布服务器上，执行 [sp_addmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)。 指定 \@publication、\@subscriber 和 \@subscriber_db，并将 \@subscription_type 的值指定为 pull    。 这样便可注册请求订阅。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 以下示例创建事务发布的请求订阅。 第一个批处理在订阅服务器中执行，第二个批处理在发布服务器中执行。 登录名和密码在运行时使用 sqlcmd 脚本变量进行提供。  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Execute this batch at the Subscriber.  
DECLARE @publication AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @publicationDB AS sysname;  
SET @publication = N'AdvWorksProductTran';  
SET @publisher = $(PubServer);  
SET @publicationDB = N'AdventureWorks';  
  
-- At the subscription database, create a pull subscription   
-- to a transactional publication.  
USE [AdventureWorksReplica]  
EXEC sp_addpullsubscription   
  @publisher = @publisher,   
  @publication = @publication,   
  @publisher_db = @publicationDB;  
  
-- Add an agent job to synchronize the pull subscription.  
EXEC sp_addpullsubscription_agent   
  @publisher = @publisher,   
  @publisher_db = @publicationDB,   
  @publication = @publication,   
  @distributor = @publisher,   
  @job_login = $(Login),   
  @job_password = $(Password);  
GO  
```  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Execute this batch at the Publisher.  
DECLARE @publication AS sysname;  
DECLARE @subscriber AS sysname;  
DECLARE @subscriptionDB AS sysname;  
SET @publication = N'AdvWorksProductTran';  
SET @subscriber = $(SubServer);  
SET @subscriptionDB = N'AdventureWorksReplica';  
  
-- At the Publisher, register the subscription, using the defaults.  
EXEC sp_addsubscription   
  @publication = @publication,   
  @subscriber = @subscriber,   
  @destination_db = @subscriptionDB,   
  @subscription_type = N'pull',  
  @status = N'subscribed';  
GO  
  
```  
  
 以下示例创建合并发布的请求订阅。 第一个批处理在订阅服务器中执行，第二个批处理在发布服务器中执行。 在运行时使用 **sqlcmd** 脚本变量提供登录名和密码值。  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Execute this batch at the Subscriber.  
DECLARE @publication AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @publicationDB AS sysname;  
DECLARE @hostname AS sysname;  
SET @publication = N'AdvWorksSalesOrdersMerge';  
SET @publisher = $(PubServer);  
SET @publicationDB = N'AdventureWorks';  
SET @hostname = N'adventure-works\david8';  
  
-- At the subscription database, create a pull subscription   
-- to a merge publication.  
USE [AdventureWorksReplica]  
EXEC sp_addmergepullsubscription   
  @publisher = @publisher,   
  @publication = @publication,   
  @publisher_db = @publicationDB;  
  
-- Add an agent job to synchronize the pull subscription.   
EXEC sp_addmergepullsubscription_agent   
  @publisher = @publisher,   
  @publisher_db = @publicationDB,   
  @publication = @publication,   
  @distributor = @publisher,   
  @job_login = $(Login),   
  @job_password = $(Password),  
  @hostname = @hostname;  
GO  
```  
  
```  
-- Execute this batch at the Publisher.  
DECLARE @myMergePub  AS sysname;  
DECLARE @mySub       AS sysname;  
DECLARE @mySubDB     AS sysname;  
  
SET @myMergePub = N'AdvWorksSalesOrdersMerge';  
SET @mySub = N'MYSUBSERVER';  
SET @mySubDB = N'AdventureWorksReplica';  
  
-- At the Publisher, register the subscription, using the defaults.  
USE [AdventureWorks]  
EXEC sp_addmergesubscription @publication = @myMergePub,   
@subscriber = @mySub, @subscriber_db = @mySubDB,   
@subscription_type = N'pull';  
GO  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 用于创建请求订阅的 RMO 类取决于订阅所属的发布的类型。  
  
#### <a name="to-create-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>创建快照或事务发布的请求订阅  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与订阅服务器和发布服务器的连接。  
  
2.  使用步骤 1 中的发布服务器连接，创建 <xref:Microsoft.SqlServer.Replication.TransPublication> 类的实例。 指定 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 和 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果该方法返回 **false**，则表示步骤 2 中指定的属性不正确，或者服务器中不存在发布。  
  
4.  在 **&** 属性和 **And** 之间执行逻辑位与（在 Visual C# 中为 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> ，在 Visual Basic 中为 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>。 如果结果为 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>，则将 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 设置为 **|** 属性和 **Or** 之间的逻辑位或（在 Visual C# 中为 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> ，将 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>。 然后，调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 以启用请求订阅。  
  
5.  如果订阅数据库不存在，则使用 <xref:Microsoft.SqlServer.Management.Smo.Database> 类创建该数据库。 有关详细信息，请参阅[创建、更改和删除数据库](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md)。  
  
6.  创建的 <xref:Microsoft.SqlServer.Replication.TransPullSubscription> 类的实例。  
  
7.  设置下列订阅属性：  
  
    -   在步骤 1 中为 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 创建的订阅服务器的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>的订阅数据库的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>的发布服务器的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>的发布数据库的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>的发布的名称。  
  
    -   订阅服务器中的分发代理运行时所使用的 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> ，将 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> 或 <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> 字段，用于提供分发代理在订阅服务器中运行时所使用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户凭据。 该帐户用于与订阅服务器进行本地连接，同时还用于使用 Windows 身份验证进行远程连接。  
  
        > [!NOTE]  
        >  当 <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> 固定服务器角色的成员创建订阅时，不需要设置 **P:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity** ，尽管建议这样做。 在这种情况下，代理会模拟 SQL Server Agent 帐户。 有关详细信息，请参阅 [复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
    -   （可选） **\@value** 指定为 <xref:Microsoft.SqlServer.Replication.PullSubscription.CreateSyncAgentByDefault%2A> 值，用于创建用来同步订阅的代理作业。 如果您指定了 **false** （默认值），则只能以编程的方式同步订阅，如果您通过 <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent> 属性访问该对象，则必须指定 <xref:Microsoft.SqlServer.Replication.TransPullSubscription.SynchronizationAgent%2A> 的其他属性。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md)。  
  
        > [!NOTE]  
        >  并不是所有版本的 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 都提供 SQL Server 代理。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。 当您将 Express Edition 订阅服务器的值指定为 **true** 时，便不会创建代理作业。 但是，与订阅相关的重要元数据存储在订阅服务器中。  
  
    -   （可选）在使用 SQL Server 身份验证连接到分发服务器时设置 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> 或 <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> 字段。  
  
8.  调用 <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> 方法。  
  
9. 使用步骤 2 中的 <xref:Microsoft.SqlServer.Replication.TransPublication> 类的实例调用 <xref:Microsoft.SqlServer.Replication.TransPublication.MakePullSubscriptionWellKnown%2A> 方法以向发布服务器注册请求订阅。 如果此注册已经存在，则会发生异常。  
  
#### <a name="to-create-a-pull-subscription-to-a-merge-publication"></a>创建合并发布的请求订阅  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与订阅服务器和发布服务器的连接。  
  
2.  使用步骤 1 中的发布服务器连接，创建 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例。 指定 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>和 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果该方法返回 **false**，则表示步骤 2 中指定的属性不正确，或者服务器中不存在发布。  
  
4.  在 **&** 属性和 **And** 之间执行逻辑位与（在 Visual C# 中为 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> ，在 Visual Basic 中为 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>。 如果结果为 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>，则将 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 设置为 **|** 属性和 **Or** 之间的逻辑位或（在 Visual C# 中为 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> ，将 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>。 然后，调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 以启用请求订阅。  
  
5.  如果订阅数据库不存在，则使用 <xref:Microsoft.SqlServer.Management.Smo.Database> 类创建该数据库。 有关详细信息，请参阅[创建、更改和删除数据库](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md)。  
  
6.  创建的 <xref:Microsoft.SqlServer.Replication.MergePullSubscription> 类的实例。  
  
7.  设置下列订阅属性：  
  
    -   在步骤 1 中为 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 创建的订阅服务器的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>的订阅数据库的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>的发布服务器的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>的发布数据库的名称。  
  
    -   用于 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>的发布的名称。  
  
    -   订阅服务器中的分发代理运行时所使用的 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> ，将 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> 或 <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> 字段，用于提供分发代理在订阅服务器中运行时所使用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户的凭据。 该帐户用于与订阅服务器进行本地连接，同时还用于使用 Windows 身份验证进行远程连接。  
  
        > [!NOTE]  
        >  当 <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> 固定服务器角色的成员创建订阅时，不需要设置 **P:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity** ，尽管建议这样做。 在这种情况下，代理会模拟 SQL Server Agent 帐户。 有关详细信息，请参阅 [复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
    -   （可选） **\@value** 指定为 <xref:Microsoft.SqlServer.Replication.PullSubscription.CreateSyncAgentByDefault%2A> 值，用于创建用来同步订阅的代理作业。 如果您指定了 **false** （默认值），则只能以编程的方式同步订阅，如果您通过 <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent> 属性访问该对象，则必须指定 <xref:Microsoft.SqlServer.Replication.MergePullSubscription.SynchronizationAgent%2A> 的其他属性。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md)。  
  
    -   （可选）在使用 SQL Server 身份验证连接到分发服务器时设置 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> 或 <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> 字段。  
  
    -   （可选）在使用 SQL Server 身份验证连接到发布服务器时设置 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> 或 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> 字段。  
  
8.  调用 <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> 方法。  
  
9. 使用步骤 2 中的 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例调用 <xref:Microsoft.SqlServer.Replication.MergePublication.MakePullSubscriptionWellKnown%2A> 方法以向发布服务器注册请求订阅。 如果此注册已经存在，则会发生异常。  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 该示例创建事务发布的请求订阅。 用于创建分发代理作业的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户凭据在运行时通过。  
  
```csharp  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksProductTran";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
TransPublication publication;  
TransPullSubscription subscription;  
  
try  
{  
    // Connect to the Publisher and Subscriber.  
    subscriberConn.Connect();  
    publisherConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions.  
    publication = new TransPublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.IsExistingObject)  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
  
        // Define the pull subscription.  
        subscription = new TransPullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
  
        // Specify the Windows login credentials for the Distribution Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin;  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword;  
  
        // Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = true;  
  
        // By default, subscriptions to transactional publications are synchronized   
        // continuously, but in this case we only want to synchronize on demand.  
        subscription.AgentSchedule.FrequencyType = ScheduleFrequencyType.OnDemand;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (TransSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                TransSubscriberType.ReadOnly);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksProductTran"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As TransPublication  
Dim subscription As TransPullSubscription  
  
Try  
    ' Connect to the Publisher and Subscriber.  
    subscriberConn.Connect()  
    publisherConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions.  
    publication = New TransPublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.IsExistingObject Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
  
        ' Define the pull subscription.  
        subscription = New TransPullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
        subscription.Description = "Pull subscription to " + publicationDbName _  
        + " on " + subscriberName + "."  
  
        ' Specify the Windows login credentials for the Distribution Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword  
  
        ' Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = True  
  
        ' By default, subscriptions to transactional publications are synchronized   
        ' continuously, but in this case we only want to synchronize on demand.  
        subscription.AgentSchedule.FrequencyType = ScheduleFrequencyType.OnDemand  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As TransSubscription In publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName And _  
                existing.SubscriptionDBName = subscriptionDbName Then  
                registered = True  
            End If  
        Next existing  
        If Not registered Then  
            ' Register the subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             TransSubscriberType.ReadOnly)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
        "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
  
```  
  
 该示例创建合并发布的请求订阅。 用于创建合并代理作业的 Windows 帐户凭据在运行时通过。  
  
```cpp#  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksSalesOrdersMerge";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
string hostname = @"adventure-works\garrett1";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
MergePublication publication;  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    subscriberConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions.  
    publication = new MergePublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.LoadProperties())  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
  
        // Define the pull subscription.  
        subscription = new MergePullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
        subscription.HostName = hostname;  
  
        // Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin;  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword;  
  
        // Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = true;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (MergeSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName  
                && existing.SubscriptionType == SubscriptionOption.Pull)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                MergeSubscriberType.Local, 0);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
Dim hostname As String = "adventure-works\garrett1"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As MergePublication  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    subscriberConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions.  
    publication = New MergePublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.LoadProperties() Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
  
        ' Define the pull subscription.  
        subscription = New MergePullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
        subscription.HostName = hostname  
  
        ' Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword  
  
        ' Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = True  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As MergeSubscription In _  
        publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName Then  
                registered = True  
            End If  
        Next  
        If Not registered Then  
            ' Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             MergeSubscriberType.Local, 0)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
        "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
```  
  
 该示例不在 [MSsubscription_properties](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md)中创建关联的代理作业和订阅元数据，而直接创建合并发布的请求订阅。 用于创建合并代理作业的 Windows 帐户凭据在运行时通过。  
  
```csharp  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksSalesOrdersMerge";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
MergePublication publication;  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    subscriberConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions.  
    publication = new MergePublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.LoadProperties())  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
  
        // Define the pull subscription.  
        subscription = new MergePullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
  
        // Specify that an agent job not be created for this subscription. The  
        // subscription can only be synchronized by running the Merge Agent directly.  
        // Subscripition metadata stored in MSsubscription_properties will not  
        // be available and must be specified at run time.  
        subscription.CreateSyncAgentByDefault = false;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (MergeSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName  
                && existing.SubscriptionType == SubscriptionOption.Pull)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                MergeSubscriberType.Local, 0);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As MergePublication  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    subscriberConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions.  
    publication = New MergePublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.LoadProperties() Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
  
        ' Define the pull subscription.  
        subscription = New MergePullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
  
        ' Specify that an agent job not be created for this subscription. The  
        ' subscription can only be synchronized by running the Merge Agent directly.  
        ' Subscripition metadata stored in MSsubscription_properties will not  
        ' be available and must be specified at run time.  
        subscription.CreateSyncAgentByDefault = False  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As MergeSubscription In _  
        publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName Then  
                registered = True  
            End If  
        Next  
        If Not registered Then  
            ' Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             MergeSubscriberType.Local, 0)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
     "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
```  
  
 该示例创建可使用 Web 同步通过 Internet 进行同步的合并发布的请求订阅。 用于创建合并代理作业的 Windows 帐户凭据在运行时通过。 有关详细信息，请参阅 [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md)。  
  
```csharp  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksSalesOrdersMerge";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
string hostname = @"adventure-works\garrett1";  
string webSyncUrl = "https://" + publisherInstance + "/WebSync/replisapi.dll";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
MergePublication publication;  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    subscriberConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions and Web synchronization.  
    publication = new MergePublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.LoadProperties())  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
        if ((publication.Attributes & PublicationAttributes.AllowWebSynchronization) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowWebSynchronization;  
        }  
  
        // Define the pull subscription.  
        subscription = new MergePullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
        subscription.HostName = hostname;  
  
        // Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin;  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword;  
  
        // Enable Web synchronization.  
        subscription.UseWebSynchronization = true;  
        subscription.InternetUrl = webSyncUrl;  
  
        // Specify the same Windows credentials to use when connecting to the  
        // Web server using HTTPS Basic Authentication.  
        subscription.InternetSecurityMode = AuthenticationMethod.BasicAuthentication;  
        subscription.InternetLogin = winLogin;  
        subscription.InternetPassword = winPassword;  
  
        // Ensure that we create a job for this subscription.  
        subscription.CreateSyncAgentByDefault = true;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (MergeSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName  
                && existing.SubscriptionType == SubscriptionOption.Pull)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                MergeSubscriberType.Local, 0);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
Dim hostname As String = "adventure-works\garrett1"  
Dim webSyncUrl As String = "https://" + publisherInstance + "/WebSync/replisapi.dll"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As MergePublication  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    subscriberConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions and Web synchronization.  
    publication = New MergePublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.LoadProperties() Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
        If (publication.Attributes And PublicationAttributes.AllowWebSynchronization) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowWebSynchronization  
        End If  
  
        ' Define the pull subscription.  
        subscription = New MergePullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
        subscription.HostName = hostname  
        subscription.CreateSyncAgentByDefault = True  
  
        ' Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword  
  
        ' Enable Web synchronization.  
        subscription.UseWebSynchronization = True  
        subscription.InternetUrl = webSyncUrl  
  
        ' Specify the same Windows credentials to use when connecting to the  
        ' Web server using HTTPS Basic Authentication.  
        subscription.InternetSecurityMode = AuthenticationMethod.BasicAuthentication  
        subscription.InternetLogin = winLogin  
        subscription.InternetPassword = winPassword  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As MergeSubscription In _  
        publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName Then  
                registered = True  
            End If  
        Next  
        If Not registered Then  
            ' Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             MergeSubscriberType.Local, 0)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
     "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
```  
  
## <a name="see-also"></a>另请参阅  
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [查看和修改请求订阅属性](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
