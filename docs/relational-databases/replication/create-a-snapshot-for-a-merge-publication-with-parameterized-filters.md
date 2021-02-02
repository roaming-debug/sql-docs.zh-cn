---
title: 使用参数化筛选器创建快照（合并）
description: 了解如何使用参数化筛选器为合并发布创建快照。
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- parameterized filters [SQL Server replication], snapshots
- snapshots [SQL Server replication], parameterized filters and
- filters [SQL Server replication], parameterized
ms.assetid: 00dfb229-f1de-4d33-90b0-d7c99ab52dcb
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 89a470964db8b0475a6c5d4f20a5007337645cb1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076149"
---
# <a name="create-a-snapshot-for-a-merge-publication-with-parameterized-filters"></a>为包含参数化筛选器的合并发布创建快照
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中通过参数化筛选器为合并发布创建快照。  

在合并发布中使用参数化行筛选器时，复制将使用由两部分构成的快照初始化各个订阅。 首先，创建一个架构快照，该快照包含复制所需的所有对象和已发布对象的架构，但不包含数据。 然后，使用快照初始化每个订阅，该快照包含架构快照中的对象和架构以及属于订阅分区的数据。 如果多个订阅接收某个给定分区（即这些订阅接收相同的架构和数据），则该分区的快照只创建一次；多个订阅通过使用相同的快照来初始化。 有关参数化行筛选器的详细信息，请参阅 [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。  
  
 可以采用下列三种方法之一为包含参数化筛选器的发布创建快照：  
  
-   **为每个分区预生成快照。** 使用此选项可控制快照生成时间。    
     您也可以选择按计划刷新快照。 订阅创建了快照的分区的新订阅服务器将接收最新的快照。   
-   允许订阅服务器在第一次同步时请求快照生成和应用。 使用此选项允许新订阅服务器无需请求管理员干预即可进行同步（[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理必须在发布服务器上运行以便能够生成快照）。  
  
    > [!NOTE]  
    >  如果发布中对一个或多个项目的筛选生成了对每个订阅具有唯一性的非重叠分区，则每当运行合并代理时都会清除元数据。 这意味着分区快照会过期得更快。 使用此选项时，应考虑允许订阅服务器启动快照的生成和传递。 有关筛选选项的详细信息，请参阅 [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。  
  
-   使用快照代理为每台订阅服务器手动生成一个快照。 然后，订阅服务器必须为合并代理提供快照位置，使之可检索和应用正确的快照。  
  
    > [!NOTE]  
    >  支持此选项是为了向后兼容，此选项不允许 FTP 快照共享。  
  
 最灵活的方法是组合使用预生成的快照选项和订阅服务器请求的快照选项：按计划（通常在非高峰时段）预生成快照和刷新快照，但如果创建了需要新分区的订阅，则订阅服务器可以生成自己的快照。  
  
 例如 [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)]，它完成将库存情况传递到各个店铺的行动任务。 每个销售人员都接收到依据各自登录帐户的订阅（检索销售人员所工作店铺的数据）。 管理员选择预生成快照并在每个星期日刷新快照。 偶而会有新用户添加到系统中，并需要尚无快照的分区的数据。 管理员也选择允许订阅服务器启动的快照，以避免出现因为尚无快照而使订阅服务器无法订阅发布的情况。 当新订阅服务器第一次连接时，将为指定分区生成快照，并将快照应用于订阅服务器上（[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理必须在发布服务器上运行以便能够生成快照）。  
  
 若要为包含参数化筛选器的发布创建快照，请参阅 [为包含参数化筛选器的合并发布创建快照](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
## <a name="security-settings-for-the-snapshot-agent"></a>快照代理的安全设置  
 快照代理为每个分区创建快照。 对于预生成的快照和订阅服务器请求的快照，代理使用在创建发布的快照代理作业时（该作业通过新建发布向导或 **sp_addpublication_snapshot** 创建）指定的凭据运行和进行连接。 若要更改凭据，请使用 **sp_changedynamicsnapshot_job**。 有关详细信息，请参阅 [sp_changedynamicsnapshot_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedynamicsnapshot-job-transact-sql.md)。  

  
##  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   在使用参数化筛选器为合并发布生成快照时，必须先生成一个包含所有已发布数据和订阅的订阅服务器元数据的标准（架构）快照。 有关详细信息，请参阅 [创建并应用初始快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。 创建完架构快照后，便可生成包含特定于订阅服务器的已发布数据分区的快照。  
  
-   如果发布中对一个或多个项目的筛选生成了对每个订阅具有唯一性的非重叠分区，则每当运行合并代理时都会清除元数据。 这意味着分区快照会过期得更快。 使用此选项时，应考虑允许订阅服务器启动快照的生成和传递。 
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 在“发布属性 - \<Publication>”对话框的“数据分区”页上为分区生成快照。  有关访问此对话框的详细信息，请参阅 [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。 可以允许订阅服务器启动快照生成及传送和/或生成快照。  
  
 生成一个或多个分区的快照之前，必须：  
  
1.  使用新建发布向导创建合并发布，并在该向导的 **“添加筛选器”** 页上指定一个或多个参数化行筛选器。 有关详细信息，请参阅 [定义和修改合并项目的参数化行筛选器](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)。  
  
2.  生成发布的架构快照。 默认情况下，架构快照在完成新建发布向导时生成；您也可以从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中生成架构快照。  

#### <a name="to-generate-a-schema-snapshot"></a>生成架构快照  
  
1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中连接到发布服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“发布”** 文件夹。  
  
3.  右键单击要为其创建快照的发布，然后单击 **“查看快照代理状态”** 。  
  
4.  在“查看快照代理状态 - \<Publication>”对话框中，单击“启动” 。  
  
     快照代理生成快照后，将显示一条消息，例如“[100%] 已生成 17 个项目的快照”。  
  
#### <a name="to-allow-subscribers-to-initiate-snapshot-generation-and-delivery"></a>允许订阅服务器启动快照的生成和传递  
  
1.  在“发布属性 - \<Publication>”对话框的“数据分区”页上，选择“在新订阅服务器尝试同步时，根据需要自动定义分区并生成快照”。    
  
2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-generate-and-refresh-snapshots"></a>生成和刷新快照  
  
1.  在“发布属性 - \<Publication>”对话框的“数据分区”页上，单击“添加”。    
  
2.  为与要为其创建快照的分区关联的 **HOST_NAME()** 和/或 **SUSER_SNAME()** 输入一个值。  
  
3.  还可以指定快照刷新计划：  
  
    1.  选择 **“安排此分区的快照代理在以下时间运行”**  
  
    2.  接受默认的快照刷新计划，或者单击 **“更改”** 以指定其他计划。  
  
4.  单击“确定”，这会使你返回“发布属性 - \<Publication>”对话框。   
  
5.  在属性网格中选择分区，然后单击 **“立即生成所选快照”** 。  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 使用存储过程和快照代理，您可以执行以下操作：  
  
-   允许订阅服务器在第一次同步时请求快照生成和应用。  
  
-   为每个分区预生成快照。  
  
-   手动为每个订阅服务器生成快照。  
  
    > [!IMPORTANT]  
    >  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
#### <a name="to-create-a-publication-that-allows-subscribers-to-initiate-snapshot-generation-and-delivery"></a>创建允许订阅服务器启动快照生成和传递的发布  
  
1.  在发布服务器上，对发布数据库执行 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)。 指定下列参数：  
  
    -   将 \@publication 指定为发布的名称。  
  
    -   将 true 值指定为 \@allow_subscriber_initiated_snapshot，这样可使订阅服务器启动快照进程 。  
  
    -   （可选）将 \@max_concurrent_dynamic_snapshots 指定为可并发运行的动态快照进程数。 如果正在运行的进程数达到了最大值，并且订阅服务器尝试生成快照，则该进程将被置于队列中。 默认情况下，并发进程的数量不受限制。  
  
2.  在发布服务器上，执行[sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)。 指定在步骤 1 中对 \@publication 使用的发布名称以及[复制快照代理](../../relational-databases/replication/agents/replication-snapshot-agent.md)针对 \@job_login和 \@password 运行的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 凭据  。 如果代理在连接到发布服务器时将使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，则还必须将 \@publisher_security_mode 的值指定为 0，并为 \@publisher_login 和 \@publisher_password 指定 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录信息   。 此操作将为发布创建一个快照代理作业。 有关生成初始快照和为快照代理定义自定义计划的详细信息，请参阅 [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
    > [!IMPORTANT]  
    >  使用远程分发服务器配置发布服务器时，为所有参数提供的值（包括 *job_login* 和 *job_password*）都会以纯文本方式发送到该分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
3.  执行 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)，将项目添加到发布。 必须对发布中的每个项目执行一次此存储过程。 使用参数化筛选器时，必须使用 \@subset_filterclause 参数为一个或多个项目指定参数化行筛选器。 有关详细信息，请参阅 [定义和修改合并项目的参数化行筛选器](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)。  
  
4.  如果将基于该参数化行筛选器筛选其他项目，请执行 [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) 定义项目间的联接或逻辑记录关系。 必须对要定义的每个关系执行一次此存储过程。 有关详细信息，请参阅 [定义和修改合并项目间的联接筛选器](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)。  
  
5.  当合并代理请求快照初始化订阅服务器时，将自动生成请求订阅的分区的快照。  
  
#### <a name="to-create-a-publication-and-pre-generate-or-automatically-refresh-snapshots"></a>创建发布并预生成或自动刷新快照  
  
1.  执行 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) 创建发布。 有关详细信息，请参阅 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)。  
  
2.  在发布服务器上，执行[sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)。 指定在步骤 1 中对 \@publication 使用的发布名称以及快照代理针对 \@job_login 和 \@password运行的 Windows 凭据  。 如果代理在连接到发布服务器时将使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，则还必须将 \@publisher_security_mode 的值指定为 0，并为 \@publisher_login 和\@publisher_password 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录信息   。 此操作将为发布创建一个快照代理作业。 有关生成初始快照和为快照代理定义自定义计划的详细信息，请参阅 [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
    > [!IMPORTANT]  
    >  使用远程分发服务器配置发布服务器时，为所有参数提供的值（包括 *job_login* 和 *job_password*）都会以纯文本方式发送到该分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
3.  执行 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)，将项目添加到发布。 必须对发布中的每个项目执行一次此存储过程。 使用参数化筛选器时，必须使用 \@subset_filterclause 参数为一个项目指定参数化行筛选器。 有关详细信息，请参阅 [定义和修改合并项目的参数化行筛选器](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)。  
  
4.  如果将基于该参数化行筛选器筛选其他项目，请执行 [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) 定义项目间的联接或逻辑记录关系。 必须对要定义的每个关系执行一次此存储过程。 有关详细信息，请参阅 [定义和修改合并项目间的联接筛选器](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)。  
  
5.  在发布服务器上，对发布数据库执行 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)，并指定步骤 1 中 \@publication 的值。 请注意结果集中的 **snapshot_jobid** 值。  
  
6.  将步骤 5 中得到的 **snapshot_jobid** 的值转换为 **uniqueidentifier**。  
  
7.  在 msdb 数据库的发布服务器上，执行 [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)，将 \@job_id 指定为在步骤 6 中得到的转换后的值 。  
  
8.  在发布服务器上，对发布数据库执行 [sp_addmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)。 为 \@publication 指定步骤 1 中发布的名称，并为 \@suser_sname（如果在筛选子句中使用 [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md)）或 \@host_name（如果在筛选子句中使用 [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md)）指定用于定义分区的值  。  
  
9. 在发布服务器上，对发布数据库执行 [sp_adddynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddynamicsnapshot-job-transact-sql.md)。 为 \@publication 指定步骤 1 中发布的名称，并为 \@suser_sname 或 \@host_name 指定步骤 8 中的值，同时为作业指定一个计划  。 此操作将创建为指定分区生成参数化快照的作业。 有关详细信息，请参阅 [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md)。  
  
    > [!NOTE]  
    >  使用在步骤 2 中定义的初始快照作业的 Windows 帐户运行此作业。 若要删除参数化快照作业及其相关的数据分区，请执行 [sp_dropdynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdynamicsnapshot-job-transact-sql.md)。  
  
10. 在发布服务器上的发布数据库中执行 [sp_helpmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md)，指定步骤 1 中的 \@publication 值和步骤 8 中的 \@suser_sname 或 \@host_name 值  。 请注意结果集中的 **dynamic_snapshot_jobid** 值。  
  
11. 在分发服务器上，对 msdb 数据库执行 [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)，并为 \@job_id 指定在步骤 9 中获取的值 。 此操作将启动分区的参数化快照作业。  
  
12. 重复步骤 8-11，分别为每个订阅生成一个分区快照。  
  
#### <a name="to-create-a-publication-and-manually-create-snapshots-for-each-partition"></a>创建发布并为每个分区手动创建快照  
  
1.  执行 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) 创建发布。 有关详细信息，请参阅 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)。  
  
2.  在发布服务器上，执行[sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)。 指定在步骤 1 中对 \@publication 使用的发布名称以及快照代理针对 \@job_login 和 \@password运行的 Windows 凭据  。 如果代理在连接到发布服务器时将使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，则还必须将 \@publisher_security_mode 的值指定为 0，并为 \@publisher_login 和\@publisher_password 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录信息   。 此操作将为发布创建一个快照代理作业。 有关生成初始快照和为快照代理定义自定义计划的详细信息，请参阅 [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
    > [!IMPORTANT]  
    >  使用远程分发服务器配置发布服务器时，为所有参数提供的值（包括 *job_login* 和 *job_password*）都会以纯文本方式发送到该分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
3.  执行 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)，将项目添加到发布。 必须对发布中的每个项目执行一次此存储过程。 使用参数化筛选器时，必须使用 \@subset_filterclause 参数为至少一个项目指定参数化行筛选器。 有关详细信息，请参阅 [定义和修改合并项目的参数化行筛选器](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)。  
  
4.  如果将基于该参数化行筛选器筛选其他项目，请执行 [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) 定义项目间的联接或逻辑记录关系。 必须对要定义的每个关系执行一次此存储过程。 有关详细信息，请参阅 [定义和修改合并项目间的联接筛选器](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)。  
  
5.  启动快照作业或从命令提示符下运行复制快照代理，以生成标准快照架构及其他文件。 有关详细信息，请参阅 [创建并应用初始快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
6.  再次从命令提示符下运行复制快照代理以生成大容量复制 (.bcp) 文件，为 **-DynamicSnapshotLocation** 指定分区快照的位置，同时指定用于定义分区的以下一个或两个属性：  
  
    -   **-DynamicFilterHostName** - 使用 [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) 时的值。  
  
    -   **-DynamicFilterLogin** - 使用 [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md) 时的值。  
  
7.  重复步骤 6，分别为每个订阅生成一个分区快照。  
  
8.  对每个订阅运行合并代理以在订阅服务器上应用初始分区快照，并指定以下属性：  
  
    -   **-Hostname** - 在 HOST_NAME 的实际值被覆盖时用于定义分区的值。  
  
    -   **-DynamicSnapshotLocation** - 此分区的动态快照的位置。  
  
> [!NOTE]  
>  有关复制代理编程的详细信息，请参阅[复制代理可执行文件概念](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此示例使用参数化筛选器创建合并发布，其中由订阅服务器启动快照生成过程。 \@job_login 和 \@job_password 的值通过脚本变量进行传递 。  
  
 [!code-sql[HowTo#sp_MergeDynamicPub1](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_1.sql)]  
  
 此示例使用参数筛选器创建发布，通过传递分区信息，其中的每个订阅服务器均有自己的分区（通过执行 [sp_addmergepartition](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md) 进行定义）和经过筛选的快照作业（通过执行 [sp_adddynamicsnapshot_job](../../relational-databases/system-stored-procedures/sp-adddynamicsnapshot-job-transact-sql.md) 来创建）。 \@job_login 和 \@job_password 的值通过脚本变量进行传递 。  
  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_2.sql)]  
  
 此示例使用参数化筛选器创建发布，其中每个订阅服务器都必须通过提供分区信息来创建它自己的数据分区和已筛选的快照作业。 手动运行复制代理时，订阅服务器使用命令行参数提供分区信息。 此示例假设还对发布创建了订阅。  
  
 [!code-sql[HowTo#sp_MergeDynamicPubPartitionManual](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_3.sql)]  
  
```  
  
REM Line breaks are added to improve readability.   
REM In a batch file, commands must be made in a single line.  
REM Run the Snapshot agent from the command line to generate the standard snapshot   
REM schema and other files.   
SET DistPub=%computername%  
SET PubDB=AdventureWorks2012   
SET PubName=AdvWorksSalesPersonMerge  
  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %PubName%    
-Publisher %DistPub% -Distributor  %DistPub%  -PublisherDB %PubDB%  -ReplicationType 2    
-OutputVerboseLevel 1  -DistributorSecurityMode 1  
  
PAUSE  
  
```  
  
```  
  
REM Run the Snapshot agent from the command line, this time to generate   
REM the bulk copy (.bcp) data for each Subscriber partition.    
SET DistPub=%computername%  
SET PubDB=AdventureWorks2012   
SET PubName=AdvWorksSalesPersonMerge  
SET SnapshotDir=\\%DistPub%\repldata\unc\fernando  
  
MD %SnapshotDir%  
  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %PubName%    
-Publisher %DistPub%  -Distributor  %DistPub%  -PublisherDB %PubDB%  -ReplicationType 2    
-OutputVerboseLevel 1  -DistributorSecurityMode 1  -DynamicFilterHostName "adventure-works\Fernando"    
-DynamicSnapshotLocation %SnapshotDir%  
  
PAUSE  
  
```  
  
```  
  
REM Run the Merge Agent for each subscription to apply the partitioned   
REM snapshot for each Subscriber.    
SET Publisher = %computername%  
SET Subscriber = %computername%  
SET PubDB = AdventureWorks2012   
SET SubDB = AdventureWorks2012Replica   
SET PubName = AdvWorksSalesPersonMerge   
SET SnapshotDir=\\%DistPub%\repldata\unc\fernando  
  
"C:\Program Files\Microsoft SQL Server\120\COM\REPLMERG.EXE" -Publisher  %Publisher%    
-Subscriber  %Subscriber%  -Distributor %Publisher%  -PublisherDB %PubDB%    
-SubscriberDB %SubDB% -Publication %PubName%  -PublisherSecurityMode 1  -OutputVerboseLevel 3    
-Output -SubscriberSecurityMode 1  -SubscriptionType 3 -DistributorSecurityMode 1    
-Hostname "adventure-works\Fernando"  -DynamicSnapshotLocation %SnapshotDir%  
  
PAUSE  
  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 可以使用复制管理对象 (RMO) 通过以下方法以编程的方式生成分区快照：  
  
-   允许订阅服务器在第一次同步时请求快照生成和应用。  
  
-   为每个分区预生成快照。  
  
-   通过运行快照代理为每台订阅服务器手动生成一个快照。  
  
> [!NOTE]  
>  如果对项目进行筛选后，生成对于每个订阅都唯一的非重叠分区（通过在创建合并项目时将 F:Microsoft.SqlServer.Replication.PartitionOptions.NonOverlappingSingleSubscription 的值指定为 P:Microsoft.SqlServer.Replication.MergeArticle.PartitionOption ），则只要合并代理运行，就会清除元数据。 这意味着分区快照会过期得更快。 使用此选项时，应考虑允许订阅服务器请求快照生成。 有关详细信息，请参阅主题 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的“使用适当的筛选选项”部分。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须存储凭据，请使用 [Windows .NET Framework 提供的](/previous-versions/aa719848(v=vs.71)) Cryptographic Services [!INCLUDE[msCoName](../../includes/msconame-md.md)] （加密服务）。  
  
#### <a name="to-create-a-publication-that-allows-subscribers-to-initiate-snapshot-generation-and-delivery"></a>创建允许订阅服务器启动快照生成和传递的发布  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  为发布数据库创建 <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> 类的实例，将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 的实例，然后调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 If <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 返回 **false**，请确认该数据库是否存在。  
  
3.  If <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> 属性为 **false**，则将其设置为 **true** ，然后调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A>。  
  
4.  创建 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例，并设置此对象的以下属性：  
  
    -   将 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 设置为步骤 1 中的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>设置为已发布的数据库的名称。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>属性设置为发布的名称。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.MergePublication.MaxConcurrentDynamicSnapshots%2A>设置为要运行的动态快照作业的最大数目。 因为订阅服务器启动的快照请求随时都可以发生，所以在多个订阅服务器同时请求其分区快照时，此属性会限制可以同时运行的快照代理作业的数目。 运行的作业达到最大数目时，其他分区快照请求会进行排队，直到其前面的一个作业运行完才开始运行。  
  
    -   使用逻辑“位或”（在 Visual C# 中为 **|** ，在 Visual Basic 中为 **Or** ）运算符将值 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowSubscriberInitiatedSnapshot> 添加到 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A>。  
  
    -   <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> 的 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> 字段和 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> 字段，用于提供快照代理作业运行时所用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户的凭据。  
  
        > [!NOTE]  
        >  如果发布是由 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> 固定服务器角色的成员创建的，则建议设置 **P:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity** 。 有关详细信息，请参阅 [复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> 方法来创建发布。  
  
    > [!IMPORTANT]  
    >  在使用远程分发服务器配置发布服务器时，为所有属性提供的值（包括 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>）都会以纯文本形式发送到该分发服务器。 调用 <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> 方法之前，应先对发布服务器与其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
6.  使用 <xref:Microsoft.SqlServer.Replication.MergeArticle> 属性将项目添加到发布。 至少为一个项目指定 <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> 属性以定义参数化筛选器。 （可选）创建 <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> 对象以定义项目之间的联接筛选器。 有关详细信息，请参阅 [定义项目](../../relational-databases/replication/publish/define-an-article.md)。  
  
7.  如果 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> 的值为 **false**，请调用 <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> 为此发布创建初始快照代理作业。  
  
8.  调用在步骤 4 中创建的 <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> 对象的 <xref:Microsoft.SqlServer.Replication.MergePublication> 方法。 这将启动生成初始快照的代理作业。 有关生成初始快照和为快照代理定义自定义计划的详细信息，请参阅 [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
9. （可选）检查 **true** 属性的值是否为 <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> ，以确定初始快照何时可以使用。  
  
10. 如果订阅服务器的合并代理是第一次连接，则会自动生成一个分区快照。  
  
#### <a name="to-create-a-publication-and-pregenerate-or-automatically-refresh-snapshots"></a>创建发布并预生成快照或自动刷新快照  
  
1.  使用 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例定义一个合并发布。 有关详细信息，请参阅 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)。  
  
2.  使用 <xref:Microsoft.SqlServer.Replication.MergeArticle> 属性将项目添加到发布。 至少为一个项目指定 <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> 属性以定义参数化筛选器，然后创建任何 <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> 对象以定义项目之间的联接筛选器。 有关详细信息，请参阅 [定义项目](../../relational-databases/replication/publish/define-an-article.md)。  
  
3.  如果 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> 的值为 **false**，请调用 <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> 为此发布创建快照代理作业。  
  
4.  调用在步骤 1 中创建的 <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> 对象的 <xref:Microsoft.SqlServer.Replication.MergePublication> 方法。 此方法启动生成初始快照的代理作业。 有关生成初始快照和为快照代理定义自定义计划的详细信息，请参阅 [创建并应用初始快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
5.  检查 **true** 属性的值是否为 <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> ，以确定初始快照何时可以使用。  
  
6.  创建 <xref:Microsoft.SqlServer.Replication.MergePartition> 类的实例，然后使用下列属性的一个或两个设置订阅服务器的参数化筛选条件：  
  
    -   如果订阅服务器的分区是通过 [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md) 的结果定义的，请使用 <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterLogin%2A>。  
  
    -   如果订阅服务器的分区是通过 [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) 的结果或此函数的重载定义的，请使用 <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterHostName%2A>。  
  
7.  创建 <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> 类的实例，然后设置与步骤 6 相同的属性。  
  
8.  使用 <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule> 类定义用于生成订阅服务器分区的筛选快照的计划。  
  
9. 使用步骤 1 中的 <xref:Microsoft.SqlServer.Replication.MergePublication> 实例，调用 <xref:Microsoft.SqlServer.Replication.MergePublication.AddMergePartition%2A>。 传递步骤 6 中的 <xref:Microsoft.SqlServer.Replication.MergePartition> 对象。  
  
10. 使用步骤 1 中的 <xref:Microsoft.SqlServer.Replication.MergePublication> 实例，调用 <xref:Microsoft.SqlServer.Replication.MergePublication.AddMergeDynamicSnapshotJob%2A> 方法。 传递步骤 7 中的 <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> 对象和步骤 8 中的 <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule> 对象。  
  
11. 调用 <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergeDynamicSnapshotJobs%2A>，然后在返回的数组中为新添加的分区快照作业找到 <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> 对象。  
  
12. 为此作业获取 <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob.Name%2A> 属性。  
  
13. 使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与分发服务器的连接。  
  
14. 创建 SQL Server 管理对象 (SMO) <xref:Microsoft.SqlServer.Management.Smo.Server> 类的实例，并传递步骤 13 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
15. 创建 <xref:Microsoft.SqlServer.Management.Smo.Agent.Job> 类的实例，并传递步骤 14 中的 <xref:Microsoft.SqlServer.Management.Smo.Server.JobServer%2A> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Server> 属性和步骤 12 中的作业名称。  
  
16. 调用 <xref:Microsoft.SqlServer.Management.Smo.Agent.Job.Start%2A> 方法以启动分区快照作业。  
  
17. 为每个订阅服务器重复步骤 6-16。  
  
#### <a name="to-create-a-publication-and-manually-create-snapshots-for-each-partition"></a>创建发布并为每个分区手动创建快照  
  
1.  使用 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例定义一个合并发布。 有关详细信息，请参阅 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)。  
  
2.  使用 <xref:Microsoft.SqlServer.Replication.MergeArticle> 属性将项目添加到发布。为至少一个项目指定 <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> 属性以定义参数化筛选器，然后创建任何 <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> 对象以定义项目之间的联接筛选器。 有关详细信息，请参阅 [定义项目](../../relational-databases/replication/publish/define-an-article.md)。  
  
3.  生成初始快照。 有关详细信息，请参阅 [创建并应用初始快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
4.  创建 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> 类的实例，并设置下列所需属性：  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> - 发布服务器的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> - 发布数据库的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> - 发布的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> - 分发服务器的名称  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> - 若使用 Windows 集成身份验证，则值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> ；若使用 SQL Server 身份验证，则值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> 。  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> - 若使用 Windows 集成身份验证，则值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> ；若使用 SQL Server 身份验证，则值为 <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> 。  
  
5.  将 <xref:Microsoft.SqlServer.Replication.ReplicationType.Merge> 的值设置为 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>。  
  
6.  设置一个或多个下列属性以定义分区参数：  
  
    -   如果订阅服务器的分区是通过 [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md) 的结果定义的，请使用 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DynamicFilterLogin%2A>。  
  
    -   如果订阅服务器的分区是通过 [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) 的结果或此函数的重载定义的，请使用 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DynamicFilterHostName%2A>。  
  
7.  调用 <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> 方法。  
  
8.  为每个订阅服务器重复步骤 4-7。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 此示例创建一个允许订阅服务器请求快照生成的合并发布。  
  
 [!code-cs[HowTo#rmo_CreateMergePub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepub)]  
  
 此示例为带有参数化行筛选器的合并发布手动创建订阅服务器分区和筛选快照。  
  
 [!code-cs[HowTo#rmo_CreateMergePartition](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepartition)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePartition](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepartition)]  
  
 此示例手动启动快照代理，为带有参数化行筛选器的合并发布的订阅服务器生成筛选数据快照。  
  
 [!code-cs[HowTo#rmo_GenerateFilteredSnapshot](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatefilteredsnapshot)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateFilteredSnapshot](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatefilteredsnapshot)]  
  
## <a name="see-also"></a>另请参阅  
 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
