---
description: IHpublications (Transact-SQL)
title: IHpublications (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHpublications_TSQL
- IHpublications
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublications system table
ms.assetid: b519a101-fa53-44be-bd55-6ea79245b5d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: cb4402e34f11463761dda8ce872568dffb029906
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201691"
---
# <a name="ihpublications-transact-sql"></a>IHpublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个使用当前分发服务器的非 SQL Server 发布在 **IHpublications** 系统表中各占一行。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**pubid**|**int**|为发布提供唯一 ID 的标识列。|  
|name|**sysname**|与发布关联的唯一名称。|  
|**repl_freq**|**tinyint**|复制频率：<br /><br /> **0** = 基于事务。<br /><br /> **1** = 计划的表刷新。|  
|**status**|**tinyint**|发布的状态，可以是以下状态之一。<br /><br /> **0** = 非活动。<br /><br /> **1** = 活动。|  
|**sync_method**|**tinyint**|同步方法包括：<br /><br /> **1** = 字符大容量复制。<br /><br /> **4** = Concurrent_c，这意味着将使用字符大容量复制，但在快照过程中不锁定表。|  
|**snapshot_jobid**|**binary**|预定任务 ID。|  
|**enabled_for_internet**|**bit**|指示是否通过 FTP 和其他服务向 Internet 公开发布的同步文件，其中， **1** 表示可以从 internet 访问。|  
|**immediate_sync_ready**|**bit**|指示同步文件是否可用，其中 **1** 表示可用。 *非 SQL 发布服务器不支持此列。*|  
|**allow_queued_tran**|**bit**|指定是否启用在订阅服务器上对更改进行排队，直到更改可以在发布服务器上应用为止。 如果为 **1**，则订阅服务器上的更改将排队。 *非 SQL 发布服务器不支持此列。*|  
|**allow_sync_tran**|**bit**|指定是否允许对发布使用立即更新订阅。 **1** 表示允许立即更新订阅。 *非 SQL 发布服务器不支持此列。*|  
|**autogen_sync_procs**|**bit**|指定是否在发布服务器中为立即更新订阅生成同步存储过程。 **1** 表示在发布服务器上生成它。 *非 SQL 发布服务器不支持此列。*|  
|**snapshot_in_defaultfolder**|**bit**|指定是否在默认文件夹中存储快照文件。 如果为 **0**，则将快照文件存储在 *alternate_snapshot_folder* 指定的备用位置中。 如果为 **1**，则可以在默认文件夹中找到快照文件。|  
|**alt_snapshot_folder**|**nvarchar (510)**|指定快照的备用文件夹的位置。|  
|**pre_snapshot_script**|**nvarchar (510)**|指定指向 **.sql** 文件位置的指针。 在订阅服务器上应用快照时，分发代理将在运行任何复制的对象脚本之前运行快照前脚本。|  
|**post_snapshot_script**|**nvarchar (510)**|指定指向 **.sql** 文件位置的指针。 在初始同步过程中，分发代理将在应用所有其他复制的对象脚本和数据之后运行快照后脚本。|  
|**compress_snapshot**|**bit**|指定写入 *alt_snapshot_folder* 位置的快照将压缩为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] CAB 格式。 **0** 指定不压缩快照。|  
|**ftp_address**|**sysname**|分发服务器的 FTP 服务的网络地址。 指定发布快照文件所在的位置以供分发代理拾取。|  
|**ftp_port**|**int**|分发服务器的 FTP 服务的端口号。 指定发布快照文件的位置，以便分发代理选择|  
|**ftp_subdirectory**|**nvarchar (510)**|指定如果发布支持使用 FTP 传播快照，分发代理应从何处拾取快照文件。|  
|**ftp_login**|**nvarchar(256)**|用于连接到 FTP 服务的用户名。|  
|**ftp_password**|**nvarchar (1048)**|用于连接到 FTP 服务的用户密码。|  
|**allow_dts**|**bit**|指定发布允许数据转换。 **1** 指定允许 DTS 转换。 *非 SQL 发布服务器不支持此列。*|  
|**allow_anonymous**|**bit**|指示是否允许对发布使用匿名订阅，其中 **1** 表示允许使用匿名订阅。|  
|**centralized_conflicts**|**bit**|指定冲突记录是否存储在发布服务器上：<br /><br /> **0** = 在导致冲突的发布服务器和订阅服务器上存储冲突记录。<br /><br /> **1** = 冲突记录存储在发布服务器上。<br /><br /> *非 SQL 发布服务器不支持此列。*|  
|**conflict_retention**|**int**|指定冲突保持期（天）。 *非 SQL 发布服务器不支持此列。*|  
|**conflict_policy**|**int**|指定使用排队更新订阅服务器选项时遵循的冲突解决策略。 可以是下列值之一：<br /><br /> **1** = 发布服务器入选冲突。<br /><br /> **2** = 订阅服务器入选冲突。<br /><br /> **3** = 重新初始化订阅。<br /><br /> *非 SQL 发布服务器不支持此列。*|  
|**queue_type**|**int**|指定所使用的队列类型。 可以是下列值之一：<br /><br /> **1** = msmq，使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 消息队列存储事务。<br /><br /> **2** = 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 来存储事务的 sql。<br /><br /> 非发布服务器不使用此列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br /> 注意：使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 消息队列已弃用，不再受支持。<br /><br /> *非 SQL 发布服务器不支持此列。*|  
|**ad_guidname**|**sysname**|指定是否在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory 中发布该发布。 有效的全局唯一标识符 (GUID) 指定发布在 Active Directory 中发布 [!INCLUDE[msCoName](../../includes/msconame-md.md)] ，GUID 是对应的 Active Directory 发布对象 **objectGUID**。 如果为 NULL，则不在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory 中发布该发布。 *非 SQL 发布服务器不支持此列。*|  
|**backward_comp_level**|**int**|数据库兼容性级别，可以是以下值之一：<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 。<br /><br /> *非 SQL 发布服务器不支持此列。*|  
|description|**nvarchar(255)**|发布的说明性条目。|  
|**independent_agent**|**bit**|指定此发布是否有独立的分发代理。<br /><br /> **0** = 发布使用共享分发代理，每个发布服务器数据库/订阅服务器数据库对都有一个共享代理。<br /><br /> **1** = 此发布有独立的分发代理。|  
|**immediate_sync**|**bit**|指示每次运行快照代理时是创建还是重新创建同步文件，其中 **1** 表示每次运行代理时都创建同步文件。|  
|**allow_push**|**bit**|指示是否允许对发布使用推送订阅，其中 **1** 表示允许使用推送订阅。|  
|**allow_pull**|**bit**|指示是否允许对发布使用请求订阅，其中 **1** 表示允许使用请求订阅。|  
|**保留**|**int**|为给定发布保存的更改数量（小时）。|  
|**allow_subscription_copy**|**bit**|指定是否已启用复制订阅该发布的订阅数据库的功能。 **1** 表示允许复制。|  
|**allow_initialize_from_backup**|**bit**|指示订阅服务器是否能够从备份而不是从初始快照来初始化对此发布的订阅。 **1** 表示可以从备份中初始化订阅， **0** 表示不能。 有关详细信息，请参阅 [初始化事务订阅（不使用快照）](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。 *非 SQL 发布服务器不支持此列。*|  
|**min_autonosync_lsn**|**二进制 (1)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**replicate_ddl**|**int**|指示发布是否支持架构复制。 **1** 指示复制在发布服务器上执行的 ddl 语句， **0** 指示不复制 ddl 语句。 有关详细信息，请参阅[对发布数据库进行架构更改](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。 *非 SQL 发布服务器不支持此列。*|  
|**options**|**int**|指定其他发布选项的位图，其中位选项值包括：<br /><br /> **0x1** -已启用对等复制。<br /><br /> **0x2** -仅发布本地更改。<br /><br /> **0x4** -为非 SQL Server 订阅服务器启用。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addpublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [syspublications &#40;系统视图&#41; &#40;Transact-sql&#41;](../../relational-databases/system-views/syspublications-system-view-transact-sql.md)   
 [syspublications &#40;Transact-sql&#41;](../../relational-databases/system-tables/syspublications-transact-sql.md)  
  
  
