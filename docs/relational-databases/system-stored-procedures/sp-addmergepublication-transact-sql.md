---
description: sp_addmergepublication (Transact-SQL)
title: sp_addmergepublication (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepublication
- sp_addmergepublication_TSQL
helpviewer_keywords:
- sp_addmergepublication
ms.assetid: 28a629a1-7374-4614-9b04-279d290a942a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0c132ffc9315bafc28530afeef83f779c2975e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209436"
---
# <a name="sp_addmergepublication-transact-sql"></a>sp_addmergepublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建新合并发布。 此存储过程针对发布服务器上要发布的数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addmergepublication [ @publication = ] 'publication'   
    [ , [ @description = ] 'description'   
    [ , [ @retention = ] retention ]   
    [ , [ @sync_mode = ] 'sync_mode' ]   
    [ , [ @allow_push = ] 'allow_push' ]   
    [ , [ @allow_pull = ] 'allow_pull' ]   
    [ , [ @allow_anonymous = ] 'allow_anonymous' ]   
    [ , [ @enabled_for_internet = ] 'enabled_for_internet' ]   
    [ , [ @centralized_conflicts = ] 'centralized_conflicts' ]   
    [ , [ @dynamic_filters = ] 'dynamic_filters' ]   
    [ , [ @snapshot_in_defaultfolder = ] 'snapshot_in_default_folder' ]   
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]   
    [ , [ @pre_snapshot_script = ] 'pre_snapshot_script' ]   
    [ , [ @post_snapshot_script = ] 'post_snapshot_script' ]   
    [ , [ @compress_snapshot = ] 'compress_snapshot' ]   
    [ , [ @ftp_address = ] 'ftp_address' ]   
    [ , [ @ftp_port = ] ftp_port ]   
    [ , [ @ftp_subdirectory = ] 'ftp_subdirectory' ]   
    [ , [ @ftp_login = ] 'ftp_login' ]   
    [ , [ @ftp_password = ] 'ftp_password' ]   
    [ , [ @conflict_retention = ] conflict_retention ]   
    [ , [ @keep_partition_changes = ] 'keep_partition_changes' ]   
    [ , [ @allow_subscription_copy = ] 'allow_subscription_copy' ]   
    [ , [ @allow_synctoalternate = ] 'allow_synctoalternate' ]   
    [ , [ @validate_subscriber_info = ] 'validate_subscriber_info' ]   
    [ , [ @add_to_active_directory = ] 'add_to_active_directory' ]   
    [ , [ @max_concurrent_merge = ] maximum_concurrent_merge ]   
    [ , [ @max_concurrent_dynamic_snapshots = ] max_concurrent_dynamic_snapshots ]  
    [ , [ @use_partition_groups = ] 'use_partition_groups' ]  
    [ , [ @publication_compatibility_level = ] 'backward_comp_level' ]  
    [ , [ @replicate_ddl = ] replicate_ddl ]  
    [ , [ @allow_subscriber_initiated_snapshot = ] 'allow_subscriber_initiated_snapshot' ]   
    [ , [ @allow_web_synchronization = ] 'allow_web_synchronization' ]   
    [ , [ @web_synchronization_url = ] 'web_synchronization_url' ]  
    [ , [ @allow_partition_realignment = ] 'allow_partition_realignment' ]  
    [ , [ @retention_period_unit = ] 'retention_period_unit' ]  
    [ , [ @generation_leveling_threshold = ] generation_leveling_threshold ]  
    [ , [ @automatic_reinitialization_policy = ] automatic_reinitialization_policy ]  
    [ , [ @conflict_logging = ] 'conflict_logging' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要创建的合并发布的名称。 *发布* 为 **sysname**，无默认值，且不能为关键字 ALL。 发布的名称在数据库内必须唯一。  
  
`[ @description = ] 'description'` 发布说明。 *描述* 为 **nvarchar (255)**，默认值为 NULL。  
  
`[ @retention = ] retention` 保存给定 *发布* 的更改的保留期（以保持期单位表示）。 *保留期* 为 **int**，默认值为14个单位。 保持期单位由 *retention_period_unit* 定义。 如果在保持期内没有同步该订阅，并在分发服务器上使用清除操作删除了该订阅本应接收到的挂起更改，则该订阅将过期，必须重新初始化。 所允许的最大保持期为当前日期到 9999 年 12 月 31 日之间的天数。  
  
> [!NOTE]  
>  合并发布的保持期具有24小时的宽限期，以适应不同时区中的订阅者。 例如，如果将保持期设置为 1 天，则实际的保持期为 48 小时。  
  
`[ @sync_mode = ] 'sync_mode'` 发布服务器的初始同步模式。 *sync_mode* 为 **nvarchar (10)**，可以是以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**本机** (默认值) |生成所有表的本机模式大容量复制程序输出。|  
|**character**|生成所有表的字符模式大容量复制程序输出。 需要支持 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEW](../../includes/ssew-md.md)] 和非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。|  
  
`[ @allow_push = ] 'allow_push'` 指定是否可以为给定发布创建推送订阅。 *allow_push* 为 **nvarchar (5)**，默认值为 TRUE，表示允许对发布使用推送订阅。  
  
`[ @allow_pull = ] 'allow_pull'` 指定是否可以为给定发布创建请求订阅。 *allow_pull* 为 **nvarchar (5)**，默认值为 TRUE，表示允许对发布使用请求订阅。 您必须指定 true 以支持 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器。  
  
`[ @allow_anonymous = ] 'allow_anonymous'` 指定是否可以为给定发布创建匿名订阅。 *allow_anonymous* 为 **nvarchar (5)**，默认值为 TRUE，这允许对发布进行匿名订阅。 若要支持 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器，必须指定 **true**。  
  
`[ @enabled_for_internet = ] 'enabled_for_internet'` 指定是否为 Internet 启用发布，并确定是否可以使用文件传输协议 (FTP) 将快照文件传输到订阅服务器。 *enabled_for_internet* 为 **nvarchar (5)**，默认值为 FALSE。 如果 **为 true**，则将发布的同步文件放在 C:\PROGRAM Files\Microsoft SQL Server\MSSQL\MSSQL.x\Repldata\Ftp 目录中。 用户必须创建 Ftp 目录。 如果 **为 false**，则不启用对 Internet 访问的发布。  
  
`[ @centralized_conflicts = ] 'centralized_conflicts'` 此参数已弃用，仅支持脚本的后向兼容性。 使用 *conflict_logging* 指定存储冲突记录的位置。  
  
`[ @dynamic_filters = ] 'dynamic_filters'` 允许合并发布使用参数化行筛选器。 *dynamic_filters* 为 **nvarchar (5)**，默认值为 FALSE。  
  
> [!NOTE]  
>  如果正在使用参数化行筛选器，请勿指定此参数，而应由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自行确定。 如果将 *dynamic_filters* 的值指定为 **true** ，则必须为项目定义参数化行筛选器。 有关详细信息，请参阅 [定义和修改合并项目的参数化行筛选器](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)。  
  
`[ @snapshot_in_defaultfolder = ] 'snapshot_in_default_folder'` 指定快照文件是否存储在默认文件夹中。 *snapshot_in_default_folder* 为 **nvarchar (5)**，默认值为 TRUE。 如果 **为 true**，则可以在默认文件夹中找到快照文件。 如果 **为 false**，则快照文件将存储在 *alternate_snapshot_folder* 指定的备用位置。 备用位置可以在另一台服务器、网络驱动器或可移动介质 (如 cd-rom 或可移动磁盘) 上。 也可以将快照文件保存到文件传输协议 (FTP) 站点以供订阅方以后检索。 请注意，此参数可以为 true，并且仍有 *alt_snapshot_folder* 指定的位置。 该组合指定将快照文件同时存储在默认位置和备用位置。  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'` 指定快照的备用文件夹的位置。 *alternate_snapshot_folder* 为 **nvarchar (255)**，默认值为 NULL。  
  
`[ @pre_snapshot_script = ] 'pre_snapshot_script'` 指定指向 **.sql** 文件位置的指针。 *pre_snapshot_script* 为 **nvarchar (255)**，默认值为 NULL。 在订阅服务器上应用快照时，合并代理将在运行任何复制的对象脚本之前运行快照前脚本。 该脚本将在合并代理连接到订阅数据库时使用的安全上下文中执行。 快照前脚本不在 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器上运行。  
  
`[ @post_snapshot_script = ] 'post_snapshot_script'` 指定指向 **.sql** 文件位置的指针。 *post_snapshot_script* 为 **nvarchar (255)**，默认值为 NULL。 当所有其他复制的对象脚本和数据均已在初始同步过程中应用之后，合并代理将运行快照后脚本。 该脚本将在合并代理连接到订阅数据库时使用的安全上下文中执行。 快照后脚本不在 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器上运行。  
  
`[ @compress_snapshot = ] 'compress_snapshot'`指定写入 **\@ alt_snapshot_folder** 位置的快照将压缩为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] CAB 格式。 *compress_snapshot* 为 **nvarchar (5)**，默认值为 FALSE。 **false** 指定不压缩快照; **如果为 true** ，则指定要压缩快照。 无法压缩大于 2GB 的快照文件。 压缩的快照文件被解压缩到合并代理所在的位置；一般对压缩的快照使用请求订阅，以便在订阅服务器上解压缩文件。 不能压缩默认文件夹中的快照。 若要支持 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器，必须指定 **false**。  
  
`[ @ftp_address = ] 'ftp_address'` 分发服务器的 FTP 服务的网络地址。 *ftp_address* 的默认值为 **sysname**，默认值为 NULL。 指定发布快照文件的位置，以便订阅服务器合并代理获取该文件。 由于为每个发布都存储此属性，因此每个发布可以具有不同的 *ftp_address*。 该发布必须支持使用 FTP 来传播快照。  
  
`[ @ftp_port = ] ftp_port` 分发服务器的 FTP 服务的端口号。 *ftp_port* 的值为 **int**，默认值为21。 指定发布快照文件所在的位置以供订阅服务器的合并代理挑选。 由于为每个发布都存储了此属性，因此每个发布都可以有自己的 *ftp_port*。  
  
`[ @ftp_subdirectory = ] 'ftp_subdirectory'` 指定在发布支持使用 FTP 传播快照时，快照文件将可供订阅服务器合并代理使用的位置。 *ftp_subdirectory* 为 **nvarchar (255)**，默认值为 NULL。 由于为每个发布都存储此属性，因此每个发布都可以有自己的 *ftp_subdirctory* 或选择不包含任何子目录，用 NULL 值指示。  
  
 在使用参数化筛选器为发布预先生成快照时，需要将每个订阅服务器分区的数据快照置于自己的文件夹中。 使用 FTP 的预先生成快照的目录结构必须为以下结构：  
  
 *alternate_snapshot_folder*\ftp \\ *publisher_publicationDB_publication* \\ *partitionID*。  
  
> [!NOTE]  
>  上面以斜体显示的值取决于发布和订阅服务器分区的具体情况。  
  
`[ @ftp_login = ] 'ftp_login'` 用于连接到 FTP 服务的用户名。 *ftp_login* 的值为 **sysname**，默认值为 "anonymous"。  
  
`[ @ftp_password = ] 'ftp_password'` 用于连接到 FTP 服务的用户密码。 *ftp_password* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。  
  
`[ @conflict_retention = ] conflict_retention` 指定保留冲突的保持期（天）。 *conflict_retention* 的值为 **int**，默认值为14天，然后将从冲突表中清除冲突行。  
  
`[ @keep_partition_changes = ] 'keep_partition_changes'` 指定当无法使用预计算分区时是否启用分区更改优化。 *keep_partition_changes* 为 **nvarchar (5)**，默认值为 TRUE。 **false** 表示未优化分区更改，如果未使用预计算分区，则在分区中的数据更改时，将验证发送到所有订阅服务器的分区。 **如果为 true，则** 表示对分区更改进行优化，并且只有在更改的分区中包含行的订阅服务器才会受到影响。 使用预计算分区时，将 *use_partition_groups* 设置为 **true** ，并将 *keep_partition_changes* 设置为 **false**。 有关详细信息，请参阅[使用预计算分区优化参数化筛选器性能](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)。  
  
> [!NOTE]  
>  如果将 " **true** " 的值指定为 " *keep_partition_changes*"，请将快照代理参数 **MaxNetworkOptimization** 的值指定为 **1** 。 有关此参数的详细信息，请参阅 [Replication 快照代理](../../relational-databases/replication/agents/replication-snapshot-agent.md)。 有关如何指定代理参数的信息，请参阅 [复制代理管理](../../relational-databases/replication/agents/replication-agent-administration.md)。  
  
 对于 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器， *keep_partition_changes* 必须设置为 true 以确保正确传播删除操作。 设置为 false 时，订阅服务器可能有比预期更多的行。  
  
`[ @allow_subscription_copy = ] 'allow_subscription_copy'` 启用或禁用复制订阅此发布的订阅数据库的功能。 *allow_subscription_copy* 为 **nvarchar (5)**，默认值为 FALSE。 要复制的订阅数据库的大小必须小于 2 GB。  
  
`[ @allow_synctoalternate = ] 'allow_synctoalternate'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @validate_subscriber_info = ] 'validate_subscriber_info'` 列出在使用参数化行筛选器时用于定义已发布数据的订阅服务器分区的函数。 *validate_subscriber_info* 为 **nvarchar (500)**，默认值为 NULL。 合并代理利用该信息来验证订阅服务器分区。 例如，如果在参数化行筛选器中使用 [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) ，则参数应为 `@validate_subscriber_info=N'SUSER_SNAME()'` 。  
  
> [!NOTE]  
>  该参数不应由用户指定，而应由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 来自动确定筛选准则。  
  
`[ @add_to_active_directory = ] 'add_to_active_directory'` 此参数已弃用，仅支持脚本的后向兼容性。 不能再向 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory 中添加发布信息。  
  
`[ @max_concurrent_merge = ] maximum_concurrent_merge` 并发合并进程的最大数量。 *maximum_concurrent_merge* 的 **整数为 int** ，默认值为0。 如果此属性的值为 **0** ，则表示在任何给定时间运行的并发合并进程数没有限制。 该属性对可以同时在合并发布上运行的并发合并进程数设置限制。 如果同时调度的合并进程数大于所允许的数目，则将多出的作业放置在队列中等待，直到当前正在运行的合并进程结束。  
  
`[ @max_concurrent_dynamic_snapshots = ] max_concurrent_dynamic_snapshots` 可同时运行以生成订阅服务器分区的筛选数据快照的最大快照代理会话数。 *maximum_concurrent_dynamic_snapshots* 的 **整数为 int** ，默认值为0。 如果为 **0**，则快照会话数没有限制。 如果同时调度的快照进程数比允许运行的数多，则多出的作业将放置在队列中等待，直到当前正在运行的快照进程完成。  
  
`[ @use_partition_groups = ] 'use_partition_groups'` 指定应使用预计算分区来优化同步过程。 *use_partition_groups* 为 **nvarchar (5)**，可以是以下值之一：  
  
|值|说明|  
|-----------|-----------------|  
|true|发布使用预计算分区。|  
|**false**|发布不使用预计算分区。|  
|NULL（默认值）|由系统确定分区策略。|  
  
 默认情况下，使用预计算分区。 若要避免使用预计算分区， *use_partition_groups* 必须设置为 **false**。 如果设置为 NULL，则系统会确定预计算分区是否可用。 如果无法使用预计算分区，则此值将有效地变成 **false** ，而不会生成任何错误。 在这种情况下，可以将 *keep_partition_changes* 设置为 **true** 以提供一些优化。 有关详细信息，请参阅 [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md) 和 [用预计算分区优化参数化筛选器性能](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)。  
  
`[ @publication_compatibility_level = ] backward_comp_level` 指示发布的向后兼容性。 *backward_comp_level* 为 **nvarchar (6)**，可以是以下值之一：  
  
|值|版本|  
|-----------|-------------|  
|**90RTM**|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|  
|**100RTM**|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
  
`[ @replicate_ddl = ] replicate_ddl` 指示发布是否支持架构复制。 *replicate_ddl* 的值为 **int**，默认值为1。 **1** 指示复制在发布服务器上执行的数据定义语言 (DDL) 语句， **0** 指示不复制 ddl 语句。 有关详细信息，请参阅[对发布数据库进行架构更改](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。  
  
 当 DDL 语句添加列时， *\@ replicate_ddl* 参数。 当 DDL 语句出于以下原因更改或删除列时，将忽略 *\@ replicate_ddl* 参数。  
  
-   删除列时，必须更新 sysarticlecolumns 以防止新的 DML 语句包含删除的列，如果包含可能导致分发代理失败。 *\@ Replicate_ddl* 参数将被忽略，因为复制必须始终复制架构更改。  
  
-   更改列时，源数据类型或为 Null 性可能已更改，这导致 DML 语句包含可能与订阅服务器上的表不兼容的值。 这种 DML 语句可能导致分发代理失败。 *\@ Replicate_ddl* 参数将被忽略，因为复制必须始终复制架构更改。  
  
-   DDL 语句添加新列时，sysarticlecolumns 不包含新列。 DML 语句将不尝试复制新列的数据。 采用该参数，因为复制或不复制 DDL 均可接受。  
  
`[ @allow_subscriber_initiated_snapshot = ] 'allow_subscriber_initiated_snapshot'` 指示此发布的订阅服务器是否可以启动快照进程来为其数据分区生成筛选快照。 *allow_subscriber_initiated_snapshot* 为 **nvarchar (5)**，默认值为 FALSE。 **true** 指示订阅服务器可以启动快照进程。  
  
`[ @allow_web_synchronization = ] 'allow_web_synchronization'` 指定是否为 Web 同步启用发布。 *allow_web_synchronization* 为 **nvarchar (5)**，默认值为 FALSE。 **true** 指定可以通过 HTTPS 对此发布的订阅进行同步。 有关详细信息，请参阅 [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md)。 若要支持 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器，必须指定 **true**。  
  
`[ @web_synchronization_url = ] 'web_synchronization_url'` 指定用于 Web 同步的 Internet URL 的默认值。 *web_synchronization_url i* s **nvarchar (500)**，默认值为 NULL。 定义默认 Internet URL （如果执行 [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) 时未显式设置 Internet URL）。  
  
`[ @allow_partition_realignment = ] 'allow_partition_realignment'` 确定当修改发布服务器上的行时，是否将删除操作发送到订阅服务器，以使其更改其分区。 *allow_partition_realignment* 为 **nvarchar (5)**，默认值为 TRUE。 **如果为 true，则** 将删除操作发送到订阅服务器，以通过删除不再属于订阅服务器分区的数据来反映分区更改的结果。 **如果** 设置为 false，则会将旧分区中的数据保留在订阅服务器上，对发布服务器上的此数据所做的更改将不会复制到此订阅服务器，但订阅服务器上所做的更改将复制到发布服务器。 将 *allow_partition_realignment* 设置为 **false** 时，会使用将数据保留在旧分区的订阅中，以便在历史记录时需要访问数据。  
  
> [!NOTE]  
>  将 *allow_partition_realignment* 设置为 **False** 时，订阅服务器上保留的数据应视为只读数据;但是，复制系统不会强制执行此方法。  
  
`[ @retention_period_unit = ] 'retention_period_unit'`指定保持期设置的保持期的单位 *。* *retention_period_unit* 为 **nvarchar (10)**，可以是以下值之一。  
  
|值|版本|  
|-----------|-------------|  
|**日期** (默认值) |按天指定保持期。|  
|week |按周指定保持期。|  
|**month**|按月指定保持期。|  
|**year**|按年指定保持期。|  
  
`[ @generation_leveling_threshold = ] generation_leveling_threshold` 指定代中包含的更改的数目。 代是传递给发布服务器或订阅服务器的更改的集合。 *generation_leveling_threshold* 的值为 **int**，默认值为1000。  
  
`[ @automatic_reinitialization_policy = ] automatic_reinitialization_policy`指定在对发布的更改所需的自动重新初始化之前，是否从订阅服务器上载更改，其中，为 **\@ force_reinit_subscription** 指定了值 **1** 。 *automatic_reinitialization_policy* 为 bit，默认值为0。 **1** 表示在进行自动重新初始化之前，从订阅服务器上载更改。  
  
> [!IMPORTANT]  
>  如果添加、删除或更改参数化筛选器，则订阅服务器上挂起的更改在重新初始化期间将无法上载到发布服务器。 若要上载挂起的更改，请在更改筛选器前同步所有订阅。  
  
`[ @conflict_logging = ] 'conflict_logging'` 指定存储冲突记录的位置。 *conflict_logging* 为 **nvarchar (15)**，可以为以下值之一：  
  
|值|说明|  
|-----------|-----------------|  
|**publisher**|在发布服务器上存储冲突记录。|  
|**订阅服务器**|在导致冲突的订阅服务器上存储冲突记录。 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器不支持此值。|  
|**全部**|在发布服务器和订阅服务器上都存储冲突记录。|  
|NULL（默认值）|在所有其他情况下 **，如果值** *backward_comp_level* 为 **90rtm** ，则自动将 *conflict_logging* **设置为。**|  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_addmergepublication** 用于合并复制。  
  
 若要使用 **\@ add_to_active_directory** 参数将发布对象列出到 Active Directory，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须已在 Active Directory 中创建对象。  
  
 如果存在发布相同数据库对象的多个发布，则只有 *replicate_ddl* 值为 **1** 的发布才会复制 ALTER TABLE、ALTER VIEW、ALTER PROCEDURE、ALTER FUNCTION 和 alter TRIGGER ddl 语句。 但是，发布已删除列的所有发布都将复制 ALTER TABLE DROP COLUMN DDL 语句。  
  
 对于 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器，仅当 *snapshot_in_default_folder* 的值为 **false** 时，才使用 *alternate_snapshot_folder* 的值。  
  
 启用 DDL 复制后 (_replicate_ddl_**= 1**) 发布时，若要对发布进行非复制 DDL 更改，必须首先执行 [sp_changemergepublication &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) ，将 *replicate_ddl* 设置为 **0**。 在发出非复制 DDL 语句后，可以再次运行 **sp_changemergepublication** 以重新打开 DDL 复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_AddMergePub](../../relational-databases/replication/codesnippet/tsql/sp-addmergepublication-t_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addmergepublication**。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [sp_changemergepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_helpmergepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
