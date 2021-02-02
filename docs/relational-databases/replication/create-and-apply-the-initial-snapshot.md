---
title: 创建和应用初始快照 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio、Transact-SQL 或复制管理对象在 SQL Server 中创建和应用初始快照。
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server replication], creating
- snapshot replication [SQL Server], initial snapshots
ms.assetid: 742727a1-5189-44ec-b3ae-6fd7aa1f5347
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: e1ed0fd0126f63032fe7d041bfc4cd949561c14f
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076339"
---
# <a name="create-and-apply-the-initial-snapshot"></a>创建并应用初始快照
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建和应用初始快照。 使用参数化筛选器的合并发布需要由两部分组成的快照。 有关详细信息，请参阅 [为包含参数化筛选器的合并发布创建快照](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  快照由快照代理在创建发布后生成。 按以下方式生成：  
  
-   立即。 默认情况下，在新建发布向导中创建合并发布后会立即生成此发布的快照。    
-   在计划时间。 在新建发布向导的 **“快照代理”** 页面上指定计划时间，或者在使用存储过程或复制管理对象 (RMO) 时指定计划时间。    
-   手动。 从命令提示或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]运行快照代理。 有关运行代理的详细信息，请参阅[复制代理可执行文件概念](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)或[启动和停止复制代理 (SQL Server Management Studio)](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)。  
  
对于合并复制，每当运行快照代理时都会生成快照。 对于事务复制，是否生成快照取决于发布属性 **immediate_sync** 的设置。 如果该属性设置为 TRUE（使用新建发布向导时的默认设置），则每当运行快照代理时都会生成快照，而且可以随时将其应用到订阅服务器。 如果该属性设置为 FALSE（使用 **sp_addpublication** 时的默认设置），则仅当自上次快照代理运行以来添加了新订阅时，才会生成快照；订阅服务器必须等待快照代理完成，才能实现同步。  
  
默认情况下，快照生成后，它们将保存在位于分发服务器上的默认快照文件夹中。 还可以将快照文件保存在可移动介质（例如可移动磁盘、CD-ROM）上，或者保存在默认快照文件夹以外的位置。 另外，可以压缩文件，以便它们更容易存储和传输以及在订阅服务器上应用快照前后执行脚本。 有关这些选项的详细信息，请参阅 [Snapshot Options](../../relational-databases/replication/snapshot-options.md)。  
  
如果快照用于使用参数化筛选器的合并发布，则创建快照的过程包括两部分。 首先创建包含复制脚本和已发布对象的架构（但不包含数据）的架构快照。 然后，使用包括脚本、从架构快照复制的架构以及属于订阅分区的数据的快照初始化每个订阅。 有关详细信息，请参阅 [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
在发布服务器上创建快照并将其存储在默认位置或其他快照位置后，可以将快照传输到订阅服务器并应用。 分发代理（用于快照复制或事务复制）或合并代理（用于合并复制）在初始同步期间将快照传输到订阅服务器上的订阅数据库中并将架构和数据文件应用到此数据库。 默认情况下，如果使用新建订阅向导，在创建订阅后会立即发生初始同步。 此行为由该向导的 **“初始化订阅”** 页面上的 **“初始化时间”** 选项控制。 当初始化订阅后生成快照时，除非订阅标记为重新初始化，否则快照不会应用到订阅服务器。 有关详细信息，请参阅 [重新初始化订阅](../../relational-databases/replication/reinitialize-subscriptions.md)。  
  
在分发代理或合并代理应用初始快照后，该代理将传播后续更新和其他数据修改。 在向订阅服务器分发并应用快照时，只有那些正在等待初始快照或新建快照的订阅服务器会受到影响。 该发布的其他订阅服务器（即那些已经收到对已发布数据的插入、更新、删除或其他修改内容的订阅服务器）不受影响。  

若要查看或修改默认快照文件夹位置，请参阅  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]：[修改快照选项](../../relational-databases/replication/snapshot-options.md)  
  
-   复制编程和 RMO 编程：[配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)  

## <a name="default-snapshot-location"></a>默认快照位置

 可以在配置分发向导的 **“快照文件夹”** 页上指定默认快照位置。 有关使用此向导的详细信息，请参阅[配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)。 如果在未配置为分发服务器的服务器上创建发布，请在新建发布向导的 **“快照文件夹”** 页上指定默认快照位置。 有关使用此向导的详细信息，请参阅[创建发布](../../relational-databases/replication/publish/create-a-publication.md)。  
  
 在“分发服务器属性 - \<Distributor>”对话框的“发布服务器”页上，修改默认快照位置 。 有关详细信息，请参阅[查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)。 在“发布属性 - \<Publication>”对话框中设置每个发布的快照文件夹。 有关详细信息，请参阅 [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
### <a name="modify-the-default-snapshot-location"></a>修改默认快照位置  
  
1.  在“分发服务器属性 - \<Distributor>”对话框的“发布服务器”页上，单击要更改其默认快照位置的发布服务器的属性按钮 (...)  。  
  
2.  在“分发服务器属性 - \<Publisher>”对话框中，为“默认快照文件夹”属性输入一个值 。  
  
    > [!NOTE]  
    >  快照代理必须对指定的目录具有写权限，而分发代理或合并代理必须具有读权限。 如果使用的是请求订阅，则必须指定一个共享目录作为通用命名约定 (UNC) 路径，如 \\\computername\snapshot。 有关详细信息，请参阅[保护快照文件夹](../../relational-databases/replication/security/secure-the-snapshot-folder.md)。  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

## <a name="create-snapshot"></a>创建快照
默认情况下，如果运行 SQL Server 代理，在使用“新建发布向导”创建发布后，快照代理将立即生成快照。 然后，默认情况下将由分发代理（对于快照复制和事务复制）或合并代理（对于合并订阅）把此快照应用于所有订阅。 还可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 和复制监视器生成快照。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  

### <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio

1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中连接到发布服务器，然后展开服务器节点。    
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。    
3.  右键单击要为其创建快照的发布，然后单击 **“查看快照代理状态”** 。    
4.  在“查看快照代理状态 - \<Publication>”对话框中，单击“启动” 。    
 快照代理生成快照后，将显示一条消息，例如“[100%] 已生成 17 个项目的快照”。  
  
### <a name="in-replication-monitor"></a>在复制监视器中  
  
1.  在复制监视器的左窗格中依次展开发布服务器组、发布服务器。    
2.  右键单击要为其生成快照的发布，再单击 **“生成快照”** 。    
3.  若要查看快照代理的状态，请单击 **“代理”** 选项卡。有关更多详细信息，请右键单击网格中的快照代理，再单击 **“查看详细信息”** 。  

## <a name="using-transact-sql"></a>“使用 Transact-SQL”
可通过创建并运行快照代理作业或通过批处理文件运行快照代理可执行文件，以编程方式创建初始快照。 初始快照生成后，该快照将在订阅首次同步时传输并应用到订阅服务器。 如果您在命令提示符处或通过批处理文件运行快照代理，则只要现有快照变为无效，您就需要重新运行此代理。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  

1.  创建快照发布、事务发布或合并发布。 有关详细信息，请参阅 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)。  
  
2.  执行 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)。 指定 \@publication 以及下列参数：  
  
    -   **\@job_login**，用于指定快照代理在分发服务器上运行时所用的 Windows 身份验证凭据。  
  
    -   **\@job_password**，为提供的 Windows 凭据的密码。  
  
    -   （可选）如果代理在连接到发布服务器时将使用 SQL Server 身份验证，请将 \@publisher_security_mode 的值指定为 0 。 在这种情况下，还必须为 \@publisher_login 和 \@publisher_password 指定 SQL Server 身份验证的登录信息 。  
  
    -   （可选）快照代理作业的同步计划。 有关详细信息，请参阅 [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md)。  
  
    > [!IMPORTANT]  
    >  使用远程分发服务器配置发布服务器时，为所有参数提供的值（包括 *job_login* 和 *job_password*）都会以纯文本方式发送到该分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
3.  向发布添加项目。 有关详细信息，请参阅 [定义项目](../../relational-databases/replication/publish/define-an-article.md)。  
  
4.  在发布服务器上，对发布数据库执行 [sp_startpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-startpublication-snapshot-transact-sql.md)，并指定步骤 1 中 \@publication 的值。  
  
## <a name="apply-a-snapshot"></a>应用快照  

### <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio
  
1.  快照生成后，通过用分发代理或合并代理同步订阅来应用此快照：   
    -   如果代理设置为连续运行（事务复制下的默认设置），则快照生成后将自动应用。   
    -   如果代理设置为根据计划运行，则在安排代理下次运行时应用快照。    
    -   如果代理设置为按需运行，则在您下次运行代理时应用快照。  
  
     有关同步订阅的详细信息，请参阅 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) 和 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md)文件夹中打开。  
  
###   <a name="use-transact-sql"></a>使用 Transact-SQL  
 
1.  创建快照发布、事务发布或合并发布。 有关详细信息，请参阅 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)。  
  
2.  向发布添加项目。 有关详细信息，请参阅 [定义项目](../../relational-databases/replication/publish/define-an-article.md)。  
  
3.  在命令提示符处或批处理文件中，通过运行 [snapshot.exe](../../relational-databases/replication/agents/replication-snapshot-agent.md) 并指定下列命令行参数，启动 **复制合并代理**：  
  
    -   **-Publication**  
    -   **-Publisher**  
    -   **-Distributor**   
    -   **-PublisherDB**   
    -   **-ReplicationType**  
  
     如果您使用的是 SQL Server 身份验证，则还必须指定下列参数：  
  
    -   **-DistributorLogin**    
    -   **-DistributorPassword**   
    -   **-DistributorSecurityMode** =  **\@publisher_security_mode**    
    -   **-PublisherLogin**    
    -   **-PublisherPassword**    
    -   **-PublisherSecurityMode** =  **\@publisher_security_mode**  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此示例演示如何创建事务发布，并为新的发布添加快照代理作业（使用 **sqlcmd** 脚本变量）。 此示例还启动该作业。  
  
 [!code-sql[HowTo#sp_trangenerate_snapshot](../../relational-databases/replication/codesnippet/tsql/create-and-apply-the-ini_1.sql)]  
  
 此示例创建一个合并发布，并为此发布添加一个快照代理作业（使用 **sqlcmd** 变量）。 此示例还启动该作业。  
  
 [!code-sql[HowTo#sp_mergegenerate_snapshot](../../relational-databases/replication/codesnippet/tsql/create-and-apply-the-ini_2.sql)]  
  
 以下命令行参数启动快照代理，以为合并发布生成快照。  
  
> [!NOTE]  
>  为便于阅读，添加了换行符。 但在批处理文件中，命令必须位于一行中。  
  
```  
  
REM -- Declare variables  
SET Publisher=%InstanceName%  
SET PublicationDB=AdventureWorks2012   
SET Publication=AdvWorksSalesOrdersMerge   
  
REM --Start the Snapshot Agent to generate the snapshot for AdvWorksSalesOrdersMerge.  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %Publication%   
-Publisher %Publisher% -Distributor %Publisher% -PublisherDB %PublicationDB%   
-ReplicationType 2 -OutputVerboseLevel 1 -DistributorSecurityMode 1  
  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 快照代理将在创建发布后生成快照。 可以使用复制管理对象 (RMO) 和直接托管代码对复制代理功能的访问权限以编程的方式生成这些快照。 所使用的对象取决于复制的类型。 可以使用 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> 对象同步启动快照代理，也可以使用代理作业异步启动快照代理。 初始快照生成后，该快照将在订阅首次同步时传输并应用到订阅服务器。 只要现有快照不再包含有效的最新数据，您就需要重新运行代理。 有关详细信息，请参阅[维护发布](../../relational-databases/replication/publish/maintain-publications.md)。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须存储凭据，请使用 [Windows .NET Framework 提供的](/previous-versions/aa719848(v=vs.71)) Cryptographic Services [!INCLUDE[msCoName](../../includes/msconame-md.md)] （加密服务）。  
  
#### <a name="to-generate-the-initial-snapshot-for-a-snapshot-or-transactional-publication-by-starting-the-snapshot-agent-job-asynchronous"></a>通过启动快照代理作业（异步）为快照发布或事务发布生成初始快照  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.TransPublication> 类的实例。 设置发布的 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性，并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的连接。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法以加载该对象的其余属性。 如果此方法返回 **false**，则说明步骤 2 中的发布属性定义不正确，或者此发布不存在。  
  
4.  如果 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> 的值为 **false**，请调用 <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> 为此发布创建快照代理作业。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> 方法以启动为此发布生成快照的代理作业。  
  
6.  （可选） <xref:Microsoft.SqlServer.Replication.TransPublication.SnapshotAvailable%2A> 的值为 **true** 时，订阅服务器具有快照。  
  
#### <a name="to-generate-the-initial-snapshot-for-a-snapshot-or-transactional-publication-by-running-the-snapshot-agent-synchronous"></a>通过运行快照代理（同步）为快照发布或事务发布生成初始快照  
  
1.  创建 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> 类的实例，并设置下列所需属性：  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> - 发布服务器的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> - 发布数据库的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> - 发布的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> - 分发服务器的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> - 值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> 表示连接到发布服务器时使用 Windows 身份验证，值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> 以及值为 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherLogin%2A> 和 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherPassword%2A> 表示连接到发布服务器时使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 建议使用 Windows 身份验证。  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> - 值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> 表示连接到分发服务器时使用 Windows 身份验证，值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> 以及值为 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorLogin%2A> 和 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorPassword%2A> 表示连接到分发服务器时使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 建议使用 Windows 身份验证。  
  
2.  将 <xref:Microsoft.SqlServer.Replication.ReplicationType.Transactional> 的值设置为 <xref:Microsoft.SqlServer.Replication.ReplicationType.Snapshot> 或 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> 方法。  
  
#### <a name="to-generate-the-initial-snapshot-for-a-merge-publication-by-starting-the-snapshot-agent-job-asynchronous"></a>通过启动快照代理作业（异步）为合并发布生成初始快照  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例。 设置发布的 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性，并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的连接。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法以加载该对象的其余属性。 如果此方法返回 **false**，则说明步骤 2 中的发布属性定义不正确，或者此发布不存在。  
  
4.  如果 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> 的值为 **false**，请调用 <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> 为此发布创建快照代理作业。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> 方法以启动为此发布生成快照的代理作业。  
  
6.  （可选） <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> 的值为 **true** 时，订阅服务器具有快照。  
  
#### <a name="to-generate-the-initial-snapshot-for-a-merge-publication-by-running-the-snapshot-agent-synchronous"></a>通过运行快照代理（同步）为合并发布生成初始快照  
  
1.  创建 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> 类的实例，并设置下列所需属性：  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> - 发布服务器的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> - 发布数据库的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> - 发布的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> - 分发服务器的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> - 值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> 表示连接到发布服务器时使用 Windows 身份验证，值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> 以及值为 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherLogin%2A> 和 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherPassword%2A> 表示连接到发布服务器时使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 建议使用 Windows 身份验证。  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> - 值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> 表示连接到分发服务器时使用 Windows 身份验证，值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> 以及值为 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorLogin%2A> 和 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorPassword%2A> 表示连接到分发服务器时使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 建议使用 Windows 身份验证。  
  
2.  将 <xref:Microsoft.SqlServer.Replication.ReplicationType.Merge> 的值设置为 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> 方法。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 此示例同步运行快照代理，为事务发布生成初始快照。  
  
 [!code-cs[HowTo#rmo_GenerateSnapshot](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatesnapshot)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateSnapshot](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatesnapshot)]  
  
 此示例异步启动代理作业，为事务发布生成初始快照。  
  
 [!code-cs[HowTo#rmo_GenerateSnapshot_WithJob](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatesnapshot_withjob)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateSnapshot_WithJob](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatesnapshot_withjob)]  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [ssSDSFull](../../relational-databases/replication/create-a-push-subscription.md)   
 [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md)   
 [使用快照初始化订阅](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [将 sqlcmd 与脚本变量结合使用](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  
