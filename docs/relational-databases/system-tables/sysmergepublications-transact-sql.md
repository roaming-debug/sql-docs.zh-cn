---
description: sysmergepublications (Transact-SQL)
title: sysmergepublications (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergepublications
- sysmergepublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepublications system table
ms.assetid: 7f82c6c3-22d1-47c0-a92b-4d64b98cc455
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0b2a7e7783ffb67688041a6da39ea99f945a607f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097323"
---
# <a name="sysmergepublications-transact-sql"></a>sysmergepublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  数据库中定义的每个合并发布在表中各占一行。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|默认服务器的名称。|  
|**publisher_db**|**sysname**|默认发布服务器数据库的名称。|  
|name|**sysname**|发布的名称。|  
|description|**nvarchar(255)**|对发布的简短说明。|  
|**保留**|**int**|整个发布集的保持期，其中单位由 **retention_period_unit** 列的值指示。|  
|**publication_type**|**tinyint**|指示发布是否经过筛选：<br /><br /> **0** = 未筛选。<br /><br /> **1** = 已筛选。|  
|**pubid**|**uniqueidentifier**|此发布的唯一标识号。 它是在添加发布时生成的。|  
|**designmasterid**|**uniqueidentifier**|留待将来使用。|  
|**parentid**|**uniqueidentifier**|指示据以创建当前对等发布或子集发布的父发布（用于层次结构发布拓朴）。|  
|**sync_mode**|**tinyint**|此发布的同步模式：<br /><br /> **0** = 本机。<br /><br /> **1** = 字符。|  
|**allow_push**|**int**|指示发布是否允许推送订阅。<br /><br /> **0** = 不允许推送订阅。<br /><br /> **1** = 允许推送订阅。|  
|**allow_pull**|**int**|指示发布是否允许请求订阅。<br /><br /> **0** = 不允许使用请求订阅。<br /><br /> **1** = 允许使用请求订阅。|  
|**allow_anonymous**|**int**|指示发布是否允许匿名订阅。<br /><br /> **0** = 不允许匿名订阅。<br /><br /> **1** = 允许匿名订阅。|  
|**centralized_conflicts**|**int**|指示是否在发布服务器上存储冲突记录：<br /><br /> **0** = 不在发布服务器上存储冲突记录。<br /><br /> **1** = 冲突记录存储在发布服务器上。|  
|**status**|**tinyint**|留待将来使用。|  
|**snapshot_ready**|**tinyint**|指示发布的快照的状态：<br /><br /> **0** = 快照尚不可用。<br /><br /> **1** = 快照已准备就绪，可供使用。<br /><br /> **2** = 必须为此发布创建新快照。|  
|**enabled_for_internet**|**bit**|指示是否通过 FTP 和其他服务将用于发布的同步文件在 Internet 上公开。<br /><br /> **0** = 可以从 Internet 访问同步文件。<br /><br /> **1** = 不能从 Internet 访问同步文件。|  
|**dynamic_filters**|**bit**|指示是否使用参数化行筛选器筛选了发布。<br /><br /> **0** = 不会对发布进行行筛选。<br /><br /> **1** = 发布是行筛选的。|  
|**snapshot_in_defaultfolder**|**bit**|指定快照文件是否存储在默认文件夹中：<br /><br /> **0** = 快照文件位于默认文件夹中。<br /><br /> **1** = 快照文件存储在 **alt_snapshot_folder** 指定的位置。|  
|**alt_snapshot_folder**|**nvarchar(255)**|快照的备用文件夹位置。|  
|**pre_snapshot_script**|**nvarchar(255)**|指向的指针。在订阅服务器上应用快照时，合并代理在任何复制对象脚本之前运行的 **sql** 文件。|  
|**post_snapshot_script**|**nvarchar(255)**|指向的指针。在初始同步过程中应用了其他所有复制对象脚本和数据之后合并代理运行的 **sql** 文件。|  
|**compress_snapshot**|**bit**|指定是否将写入 **alt_snapshot_folder** 位置的快照压缩为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] CAB 格式。 **0** 指定不压缩文件。|  
|**ftp_address**|**sysname**|分发服务器的文件传输协议 (FTP) 服务的网络地址。 如果启用了 FTP，则指定合并代理要拾取的发布快照文件所在的位置。|  
|**ftp_port**|**int**|分发服务器的 FTP 服务的端口号。|  
|**ftp_subdirectory**|**nvarchar(255)**|合并代理可拾取的快照文件所在的子目录。|  
|**ftp_login**|**sysname**|用于连接到 FTP 服务的用户名。|  
|**ftp_password**|**nvarchar (524)**|用于连接到 FTP 服务的用户密码。|  
|**conflict_retention**|**int**|指定保留冲突的保持期（天）。 保持期过后，将从冲突表中清除冲突行。|  
|**keep_before_values**|**int**|指定是否对此发布的同步进行优化：<br /><br /> **0** = 不优化同步，分区中的数据发生更改时，将验证发送到所有订阅服务器的分区。<br /><br /> **1** = 优化了同步，并且只有具有更改分区中的行的订阅服务器才会受到影响。|  
|**allow_subscription_copy**|**bit**|指定是否启用了复制订阅数据库的功能。 **0** 表示不允许复制。|  
|**allow_synctoalternate**|**bit**|指定是否允许备用同步伙伴与该发布服务器同步。 **0** 表示不允许同步伙伴。|  
|**validate_subscriber_info**|**nvarchar (500)**|列出用于检索订阅服务器信息和验证订阅服务器上的参数化行筛选条件的函数。|  
|**ad_guidname**|**sysname**|指定是否在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory 中发布该发布。 有效的 GUID 指定发布在 Active Directory 中发布，GUID 是对应的 Active Directory 发布对象 **objectGUID**。 如果为 NULL，则将不在 Active Directory 中发布该发布。|  
|**backward_comp_level**|**int**|数据库兼容级别。 可以是以下其中一个值：<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 。|  
|**max_concurrent_merge**|**int**|允许的最大并发合并进程数。 如果此属性的值为 **0** ，则表示在任何给定时间运行的并发合并进程数没有限制。 此属性设置一次可以对合并发布运行的并发合并进程数的限制。 如果同时调度的快照进程数比允许运行的进程数多，则多出的作业将放置在队列中等待，直到当前正在运行的合并进程完成。|  
|**max_concurrent_dynamic_snapshots**|**int**|允许针对合并发布运行的最大并发筛选数据快照会话数。 如果为 **0**，则在任意给定时间，可同时对发布运行的并发筛选数据快照会话的最大数量没有限制。 此属性对可以同时对合并发布运行的最大并发快照进程数设置限制。 如果同时调度的快照进程数比允许运行的进程数多，则多出的作业将放置在队列中等待，直到当前正在运行的合并进程完成。|  
|**use_partition_groups**|**smallint**|指定发布是否使用预计算分区。|  
|**dynamic_filters_function_list**|**nvarchar (500)**|一组分号分隔的函数，用于发布的参数化行筛选器。|  
|**partition_id_eval_proc**|**sysname**|指定由订阅服务器的合并代理运行的过程名称，以便确定为其分配的分区 ID。|  
|**publication_number**|**smallint**|指定标识列，该列提供到 **pubid** 的2字节映射。 **pubid** 是发布的全局唯一标识符，而发布号只在指定数据库中是唯一的。|  
|**replicate_ddl**|**int**|指示发布是否支持架构复制。<br /><br /> **0** = 不复制 DDL 语句。<br /><br /> **1** = 复制在发布服务器上执行的 DDL 语句。<br /><br /> 有关详细信息，请参阅[对发布数据库进行架构更改](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。|  
|**allow_subscriber_initiated_snapshot**|**bit**|指示订阅服务器是否可以启动使用参数化筛选器为发布生成快照的进程。 **1** 指示订阅服务器可以启动快照进程。|  
|**dynamic_snapshot_queue_timeout**|**int**|指定使用参数化筛选器时，订阅服务器必须在队列中等待快照生成进程开始的分钟数。|  
|**dynamic_snapshot_ready_timeout**|**int**|指定使用参数化筛选器时，订阅服务器在队列中等待快照生成进程完成的分钟数。|  
|**发行人**|**sysname**|发布的分发服务器的名称。|  
|**snapshot_jobid**|**binary(16)**|订阅服务器可以启动快照生成进程时，标识生成快照的代理作业。|  
|**allow_web_synchronization**|**bit**|指定是否为 Web 同步启用发布，其中 **1** 表示为发布启用了 web 同步。|  
|**web_synchronization_url**|**nvarchar (500)**|指定用于 Web 同步的 Internet URL 的默认值。|  
|**allow_partition_realignment**|**bit**|指示对发布服务器上的行所做的修改导致了更改其分区时，是否向订阅服务器发送删除指令。<br /><br /> **0** = 旧分区中的数据将保留在订阅服务器上，对发布服务器上的此数据所做的更改将不会复制到此订阅服务器，但订阅服务器上所做的更改将复制到发布服务器。<br /><br /> **1** = 删除到订阅服务器以反映分区更改的结果，方法是删除不属于订阅服务器分区的数据。<br /><br /> 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_addmergepublication ](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)。<br /><br /> 注意：如果此值为 **0** ，则在订阅服务器上保留的数据应视为只读。但是，复制系统并不严格地强制执行此方法。|  
|**retention_period_unit**|**tinyint**|定义在定义 *保持期* 时使用的单位，可以是以下值之一：<br /><br /> **0** = 天。<br /><br /> **1** = 周。<br /><br /> **2** = 月份。<br /><br /> **3** = 年。|  
|**decentralized_conflicts**|**int**|指示是否在导致冲突的订阅服务器上存储冲突记录：<br /><br /> **0** = 冲突记录不存储在订阅服务器上。<br /><br /> **1** = 冲突记录存储在订阅服务器上。|  
|**generation_leveling_threshold**|**int**|指定生成中包含的更改次数。 代是传递给发布服务器或订阅服务器的更改的集合。|  
|**automatic_reinitialization_policy**|**bit**|指示是否在进行自动重新初始化之前从订阅服务器上载更改。<br /><br /> **1** = 在自动重新初始化之前，从订阅服务器上载更改。<br /><br /> **0** = 在自动重新初始化之前不上载更改。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergepublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)  
  
  
