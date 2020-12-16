---
title: 创建推送订阅 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio、Transact-SQL 或复制管理对象在 SQL Server 中创建推送订阅。
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- push subscriptions [SQL Server replication], creating
- merge replication subscribing [SQL Server replication], push subscriptions
- subscriptions [SQL Server replication], push
- snapshot replication [SQL Server], subscribing
- transactional replication, subscribing
ms.assetid: adfbbc61-58d1-4330-9ad6-b14ab1142e2b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: bb4684cf3a50259338122fbadfbe6d3f5ff796be
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480018"
---
# <a name="create-a-push-subscription"></a>创建推送订阅
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建推送订阅。 有关为非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器创建推送订阅的信息，请参阅[为非 SQL Server 订阅服务器创建订阅](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)。  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
 
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
使用新建订阅向导，在发布服务器或订阅服务器上创建推送订阅。 按照向导中的页的指示执行下列操作：  
  
- 指定发布服务器和发布。  
  
- 选择运行复制代理的位置。 对于推送订阅，根据发布类型的不同，在 **“分发代理位置”** 页或 **“合并代理位置”** 页上选择 **“在分发服务器上运行所有代理(推送订阅)”** 。  
  
- 指定订阅服务器和订阅数据库。  
  
- 指定复制代理建立连接所用的登录名和密码：  
  
  - 对于快照发布和事务发布的订阅，在 **“分发代理安全性”** 页上指定凭据。  
  
  - 对于合并发布的订阅，在 **“合并代理安全性”** 页上指定凭据。  
  
    有关每个代理所需权限的信息，请参阅 [复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
- 指定同步计划和初始化订阅服务器的时间。  
  
- 指定合并发布的其他选项：订阅类型以及用于参数化筛选的值。  
  
- 指定允许更新订阅的事务性发布的其他选项。 一个选项是确定订阅服务器是应立即在发布服务器上提交更改还是将更改写入队列。 另一个选项是设置用于从订阅服务器连接到发布服务器的凭据。  
  
- 此外，还可以选择编写订阅的脚本。  
  
#### <a name="to-create-a-push-subscription-from-the-publisher"></a>从发布服务器创建推送订阅  
  
1. 在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后展开服务器节点。  
  
2. 展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。  
  
3. 右键单击要为其创建一个或多个订阅的发布，然后选择“新建订阅”。  
  
4. 完成新建订阅向导中的页。  
  
#### <a name="to-create-a-push-subscription-from-the-subscriber"></a>从订阅服务器创建推送订阅  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中连接到订阅服务器，然后展开服务器节点。  
  
2. 展开 **“复制”** 文件夹。  
  
3. 右键单击“本地订阅”文件夹，然后选择“新建订阅” 。  
  
4. 在“新建订阅向导”的“发布”页上，从“发布服务器”下拉列表中选择 \<Find SQL Server Publisher> 或 \<Find Oracle Publisher>   。  
  
5. 在 **“连接到服务器”** 对话框中连接到发布服务器。  
  
6. 在 **“发布”** 页上，选择一个发布。  
  
7. 完成新建订阅向导中的页。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
可以使用复制存储过程以编程方式创建推送订阅。 所用的存储过程取决于订阅所属的发布的类型。  
  
> [!IMPORTANT]
> 如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>创建快照或事务发布的推送订阅  
  
1. 在发布服务器上的发布数据库中，通过运行[sp_helppublication](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) 验证该发布是否支持推送订阅。  
  
   - 如果 **allow_push** 的值为 **1**，则支持推送订阅。  
  
   - 如果 allow_push 的值为 0，则运行 [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) 。 为 \@property 指定 allow_push，为 \@value 指定 true   。  
  
2. 在发布服务器上的发布数据库中，运行 [sp_addsubscription](../system-stored-procedures/sp-addsubscription-transact-sql.md)。 指定 \@publication、\@subscriber 和 \@destination_db。 将 \@subscription_type 的值指定为 push。 有关如何更新订阅的信息，请参阅[创建事务性发布的可更新订阅](publish/create-an-updatable-subscription-to-a-transactional-publication.md)。  
  
3. 在发布服务器上的发布数据库中，运行 [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)。 指定下列各项：  
  
   - \@subscriber、\@subscriber_db 和 \@publication 参数。  
  
   - 分发服务器中的分发代理运行时所使用的 \@job_login 和 \@job_password 指定的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 凭据。  
  
     > [!NOTE]
     > 使用 Windows 集成身份验证进行的连接始终使用由 \@job_login 和 \@job_password 指定的 Windows 凭据 。 分发代理始终使用 Windows 集成身份验证与分发服务器建立本地连接。 默认情况下，该代理将使用 Windows 集成身份验证连接到订阅服务器。  
  
   - （可选）subscriber_security_mode 的 0 值以及 \@subscriber_login 和 \@subscriber_password 的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录信息 **\@**  。 如果您需要在连接到订阅服务器时使用 SQL Server 身份验证，则指定这些参数。  
  
   - 该订阅的分发代理作业计划。 有关详细信息，请参阅[指定同步计划](../../relational-databases/replication/specify-synchronization-schedules.md)。  
  
> [!IMPORTANT]
> 使用远程分发服务器在发布服务器上创建推送订阅时，为所有参数（包括 job_login 和 job_password）提供的值将以纯文本格式发送到分发服务器 。 在运行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>创建合并发布的推送订阅  
  
1. 在发布服务器上的发布数据库中，通过运行 [sp_helpmergepublication](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) 验证发布是否支持推送订阅。  
  
   - 如果 **allow_push** 的值为 **1**，则发布支持推送订阅。  
  
   - 如果 allow_push 的值不为 1，则运行 [sp_changemergepublication](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) 。 为 \@property 指定 allow_push，为 \@value 指定 true   。  
  
2. 在发布服务器上的发布数据库中，运行 [sp_addmergesubscription](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)。 指定下列参数：  
  
   - **\@publication**。 这是发布的名称。  
  
   - **\@subscriber_type**。 对于客户端订阅，请指定“本地”。 对于服务器订阅，请指定“全局”。  
  
   - **\@subscription_priority**。 对于服务器订阅，请指定订阅的优先级（从 **0.00** 到 **99.99**）。  
  
   有关详细信息，请参阅[高级合并复制冲突检测和解决](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)。  
  
3. 在发布服务器上的发布数据库中，运行 [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md)。 指定下列各项：  
  
   - \@subscriber、\@subscriber_db 和 \@publication 参数。  
  
   - 分发服务器中的合并代理运行时所使用的 \@job_login 和 \@job_password 指定的 Windows 凭据。  
  
     > [!NOTE]
     > 使用 Windows 集成身份验证进行的连接始终使用由 \@job_login 和 \@job_password 指定的 Windows 凭据 。 合并代理程序始终使用 Windows 集成身份验证与分发服务器建立本地连接。 默认情况下，该代理将使用 Windows 集成身份验证连接到订阅服务器。  
  
   - （可选）\@subscriber_security_mode 的 0 值以及 \@subscriber_login 和 \@subscriber_password 指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录信息。 如果您需要在连接到订阅服务器时使用 SQL Server 身份验证，则指定这些参数。  
  
   - （可选）\@publisher_security_mode 的 0 值以及 \@publisher_login 和 \@publisher_password 指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录信息。 如果您需要在连接到发布服务器时使用 SQL Server 身份验证，则指定这些值。  
  
   - 该订阅的合并代理作业计划。 有关详细信息，请参阅[指定同步计划](../../relational-databases/replication/specify-synchronization-schedules.md)。  
  
> [!IMPORTANT]
> 使用远程分发服务器在发布服务器上创建推送订阅时，为所有参数（包括 job_login 和 job_password）提供的值将以纯文本格式发送到分发服务器 。 在运行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 以下示例创建事务发布的推送订阅。 登录名和密码在运行时使用 sqlcmd 脚本变量提供。  
  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_1.sql)]  
  
 以下示例创建合并发布的推送订阅。 登录名和密码在运行时使用 sqlcmd 脚本变量提供。  
  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_2.sql)]  
  
##  <a name="using-replication-management-objects"></a><a name="RMOProcedure"></a> 使用复制管理对象  
 可以使用复制管理对象 (RMO) 以编程方式创建推送订阅。 用于创建推送订阅的 RMO 类取决于要为其创建订阅的发布类型。  
  
> [!IMPORTANT]
> 如果可能，请在运行时提示用户输入安全凭据。 如果必须存储凭据，请使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework 提供的[加密服务](/previous-versions/aa719848(v=vs.71))。  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>创建快照或事务发布的推送订阅  
  
1. 使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2. 使用步骤 1 中的发布服务器连接，创建 <xref:Microsoft.SqlServer.Replication.TransPublication> 类的实例。 指定 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>和 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
3. 调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果该方法返回 **false**，则表示步骤 2 中指定的属性不正确，或者服务器中不存在发布。  
  
4. 在 **&** 属性和 **And** 之间执行逻辑位与（在 Visual C# 中为 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> ，在 Visual Basic 中为 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>。 如果结果为 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>，则将 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 设置为 **|** 属性和 **Or** 之间的逻辑位或（在 Visual C# 中为 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> ，将 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>。 然后，调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 以启用推送订阅。  
  
5. 如果订阅数据库不存在，则使用 <xref:Microsoft.SqlServer.Management.Smo.Database> 类创建该数据库。 有关详细信息，请参阅[创建、修改和删除数据库](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md)。  
  
6. 创建的 <xref:Microsoft.SqlServer.Replication.TransSubscription> 类的实例。  
  
7. 设置下列订阅属性：  
  
   - 在步骤 1 中为 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 创建的与发布服务器的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
   - 用于 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>的订阅数据库的名称。  
  
   - 用于 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>的订阅服务器的名称。 
  
   - 用于 <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>的发布数据库的名称。  
  
   - 用于 <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>的发布的名称。
  
   - 分发服务器中的分发代理运行时所使用的 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> ，将 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> 或 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> 字段，用于提供分发代理在分发服务器中运行所使用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户的凭据。 此帐户用于与分发服务器进行本地连接，以及通过使用 Windows 身份验证进行远程连接。  
  
     > [!NOTE]
     > 当 sysadmin 固定服务器角色的成员创建订阅时，不需要设置 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>，但建议设置。 在这种情况下，代理会模拟 SQL Server Agent 帐户。 有关详细信息，请参阅[复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
   - （可选） **@value** 的 <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> 值（默认值），用于创建用来同步订阅的代理作业。 如果您指定了 **false**，则只能以编程的方式同步订阅。  
  
   - （可选）在使用 SQL Server 身份验证连接到订阅服务器时设置 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> 或 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> 字段。  
  
8. 调用 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> 方法。  
  
> [!IMPORTANT]
> 使用远程分发服务器在发布服务器上创建推送订阅时，为所有属性（包括 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>）提供的值将以纯文本形式发送到分发服务器。 调用 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> 方法之前，应先对发布服务器与其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>创建合并发布的推送订阅  
  
1. 使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2. 使用步骤 1 中的发布服务器连接，创建 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例。 指定 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>和 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
3. 调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果该方法返回 **false**，则表示步骤 2 中指定的属性不正确，或者服务器中不存在发布。  
  
4. 在 **&** 属性和 **And** 之间执行逻辑位与（在 Visual C# 中为 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> ，在 Visual Basic 中为 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>。 如果结果为 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>，则将 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 设置为 **|** 属性和 **Or** 之间的逻辑位或（在 Visual C# 中为 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> ，将 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>。 然后，调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 以启用推送订阅。  
  
5. 如果订阅数据库不存在，则使用 <xref:Microsoft.SqlServer.Management.Smo.Database> 类创建该数据库。 有关详细信息，请参阅[创建、修改和删除数据库](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md)。  
  
6. 创建的 <xref:Microsoft.SqlServer.Replication.MergeSubscription> 类的实例。  
  
7. 设置下列订阅属性：  
  
   - 在步骤 1 中为 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 创建的与发布服务器的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
   - 用于 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>的订阅数据库的名称。  
  
   - 用于 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>的订阅服务器的名称。    
   - 用于 <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>的发布数据库的名称。  
  
   - 用于 <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>的发布的名称。    
   - 分发服务器中的分发代理运行时所使用的 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> ，将 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> 或 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> 字段，用于提供分发代理在分发服务器中运行所使用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户的凭据。 该帐户用于与分发服务器进行本地连接，同时还用于使用 Windows 身份验证进行远程连接。  
  
     > [!NOTE]
     > 当 sysadmin 固定服务器角色的成员创建订阅时，不需要设置 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>，但建议设置。 在这种情况下，代理会模拟 SQL Server Agent 帐户。 有关详细信息，请参阅[复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
   - （可选） **@value** 的 <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> 值（默认值），用于创建用来同步订阅的代理作业。 如果您指定了 **false**，则只能以编程的方式同步订阅。  
  
   - （可选）在使用 SQL Server 身份验证连接到订阅服务器时设置 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> 或 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> 字段。  
  
   - （可选）在使用 SQL Server 身份验证连接到发布服务器时设置 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> 或 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> 字段。  
  
8. 调用 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> 方法。  
  
> [!IMPORTANT]  
> 使用远程分发服务器在发布服务器上创建推送订阅时，为所有属性（包括 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>）提供的值将以纯文本形式发送到分发服务器。 调用 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> 方法之前，应先对发布服务器与其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 该示例创建事务发布的新推送订阅。 用于运行分发代理作业的 Windows 帐户凭据在运行时传递。  
  
 [!code-cs[HowTo#rmo_CreateTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranpushsub)]  
  
 该示例创建合并发布的新推送订阅。 用于运行合并代理作业的 Windows 帐户凭据在运行时传递。  
  
 [!code-cs[HowTo#rmo_CreateMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepushsub)]  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改推送订阅属性](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [创建发布](../../relational-databases/replication/publish/create-a-publication.md)   
 [复制管理对象概念](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [同步推送订阅](../../relational-databases/replication/synchronize-a-push-subscription.md)   
 [订阅发布](../../relational-databases/replication/subscribe-to-publications.md)   
 [将 sqlcmd 与脚本变量结合使用](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  
