---
description: syspublications (Transact-SQL)
title: syspublications (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- syspublications
- syspublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspublications system table
ms.assetid: a86eb4f5-1f7b-493e-af55-3d15cf878228
author: cawrites
ms.author: chadam
ms.openlocfilehash: afd2ba410a18aab5acf7bdde290aa498659b8cfb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180391"
---
# <a name="syspublications-transact-sql"></a>syspublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  数据库内定义的每个发布在表中对应一行。 该表存储在发布数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|description|**nvarchar(255)**|发布的说明项。|  
|name|**sysname**|与发布关联的唯一名称。|  
|**pubid**|**int**|为发布提供唯一 ID 的标识列。|  
|**repl_freq**|**tinyint**|复制频率：<br /><br /> **0** = 基于事务。<br /><br /> **1** = 计划的表刷新。|  
|**status**|**tinyint**|状态：<br /><br /> **0** = 非活动。<br /><br /> **1** = 活动。|  
|**sync_method**|**tinyint**|同步方法包括：<br /><br /> **0** = 本机模式大容量复制程序实用工具 (**BCP**) 。<br /><br /> **1** = 字符模式 BCP。<br /><br /> **3** = 并发，表示使用纯模式 BCP，但在快照期间不锁定表。<br /><br /> **4** = Concurrent_c，这意味着将使用字符模式 BCP，但在快照过程中不锁定表。|  
|**snapshot_jobid**|**binary(16)**|预定任务 ID。|  
|**independent_agent**|**bit**|指定此发布是否有独立的分发代理。<br /><br /> **0** = 发布使用共享分发代理，每个发布服务器数据库/订阅服务器数据库对都有一个共享代理。<br /><br /> **1** = 此发布有独立的分发代理。|  
|**immediate_sync**|**bit**|指示每次运行快照代理时是创建还是重新创建同步文件，其中 **1** 表示每次运行代理时都创建同步文件。|  
|**enabled_for_internet**|**bit**|指示是否通过文件传输协议向 Internet 向 Internet 公开发布的同步文件 (FTP) 和其他服务，其中， **1** 表示可以从 Internet 访问它们。|  
|**allow_push**|**bit**|指示是否允许对发布使用推送订阅，其中 **1** 表示允许使用推送订阅。|  
|**allow_pull**|**bit**|指示是否允许对发布使用请求订阅，其中 **1** 表示允许使用请求订阅。|  
|**allow_anonymous**|**bit**|指示是否允许对发布使用匿名订阅，其中 **1** 表示允许使用匿名订阅。|  
|**immediate_sync_ready**|**bit**|指示快照代理是否已生成快照且该快照是否准备好用于新的订阅。 仅对于立即更新发布才有意义。 **1** 表示快照已准备就绪。|  
|**allow_sync_tran**|**bit**|指定是否允许对发布使用立即更新订阅。 **1** 表示允许立即更新订阅。|  
|**autogen_sync_procs**|**bit**|指定是否在发布服务器中为立即更新订阅生成同步存储过程。 **1** 表示在发布服务器上生成它。|  
|**保留**|**int**|为给定发布保存的更改数量（小时）。|  
|**allowed_queued_tran**|**bit**|指定是否启用在订阅服务器上对更改进行排队，直到更改可以在发布服务器上应用为止。 如果为 **1**，则订阅服务器上的更改将排队。|  
|**snapshot_in_defaultfolder**|**bit**|指定是否在默认文件夹中存储快照文件。<br /><br /> **0** = 快照文件存储在 *alternate_snapshot_folder* 指定的备用位置。<br /><br /> **1** = 可以在默认文件夹中找到快照文件。|  
|**alt_snapshot_folder**|**nvarchar(255)**|指定快照的备用文件夹的位置。|  
|**pre_snapshot_script**|**nvarchar(255)**|指定指向 **.sql** 文件位置的指针。 在订阅服务器上应用快照时，分发代理将在运行任何复制的对象脚本之前运行快照前脚本。|  
|**post_snapshot_script**|**nvarchar(255)**|指定指向 **.sql** 文件位置的指针。 在初始同步过程中，分发代理将在应用所有其他复制的对象脚本和数据之后运行快照后脚本。|  
|**compress_snapshot**|**bit**|指定写入 *alt_snapshot_folder* 位置的快照将压缩为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] CAB 格式。**1** 表示将压缩快照。|  
|**ftp_address**|**sysname**|分发服务器的 FTP 服务的网络地址。 指定发布快照文件所在的位置以供分发代理拾取。|  
|**ftp_port**|**int**|分发服务器的 FTP 服务的端口号。 指定发布快照文件的位置，以便分发代理选择|  
|**ftp_subdirectory**|**nvarchar(255)**|指定当发布支持使用 FTP 传播快照时的快照文件的位置以供分发代理获取。|  
|**ftp_login**|**sysname**|用于连接到 FTP 服务的用户名。|  
|**ftp_password**|**nvarchar (524)**|用于连接到 FTP 服务的用户密码。|  
|**allow_dts**|**bit**|指定发布是否允许数据转换。 **1** 指定允许 DTS 转换。|  
|**allow_subscription_copy**|**bit**|指定是否已启用复制订阅该发布的订阅数据库的功能。 **1** 表示允许复制。|  
|**centralized_conflicts**|**bit**|指定冲突记录是否存储在发布服务器上：<br /><br /> **0** = 在导致冲突的发布服务器和订阅服务器上存储冲突记录。<br /><br /> **1** = 冲突记录存储在发布服务器上。|  
|**conflict_retention**|**int**|指定冲突保持期（天）。|  
|**conflict_policy**|**int**|指定使用排队更新订阅服务器选项时遵循的冲突解决策略。 可以是下列值之一：<br /><br /> **1** = 发布服务器入选冲突。<br /><br /> **2** = 订阅服务器入选冲突。<br /><br /> **3** = 重新初始化订阅。|  
|**queue_type**|**int**|指定所使用的队列类型。 可以是下列值之一：<br /><br /> **1** = msmq，使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 消息队列存储事务。<br /><br /> **2** = 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 来存储事务的 sql。<br /><br /> 注意：使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 消息队列已弃用，不再可用。|  
|**ad_guidname**|**sysname**|指定是否在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory 中发布该发布。 有效的全局唯一标识符 (GUID) 指定发布在 Active Directory 中发布，GUID 是对应的 Active Directory 发布对象 **objectGUID**。 如果为 NULL，则将不在 Active Directory 中发布该发布。|  
|**backward_comp_level**|**int**|数据库兼容性级别，可以是以下值之一：<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 。<br /><br /> **110**  =  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 。<br /><br /> **120**  =  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 。|  
|**allow_initialize_from_backup**|**bit**|指示订户是否可以从备份而非初始快照中初始化对此发布的订阅。 **1** 表示可以从备份中初始化订阅， **0** 表示不能。 有关详细信息，请参阅 [初始化事务订阅（不使用快照）](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。|  
|**min_autonosync_lsn**|**binary**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**replicate_ddl**|**int**|指示发布是否支持架构复制。 **1** 指示复制在发布服务器上执行的数据定义语言 (DDL) 语句， **0** 指示不复制 ddl 语句。 有关详细信息，请参阅[对发布数据库进行架构更改](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。|  
|**options**|**int**|指定其他发布选项的位图，其中的位选项值如下所示：<br /><br /> **0x1** -已启用对等复制。<br /><br /> **0x2** -仅发布对等复制的本地更改。<br /><br /> **0x4** -为非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器启用。<br /><br /> **0x8** -启用对等冲突检测。|  
|**originator_id**|**smallint**|为进行冲突检测标识对等复制拓扑中的每个节点。 有关详细信息，请参阅 [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addpublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)  
  
  
