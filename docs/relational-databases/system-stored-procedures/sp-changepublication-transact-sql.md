---
description: sp_changepublication (Transact-SQL)
title: sp_changepublication (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changepublication
- sp_changepublication_TSQL
helpviewer_keywords:
- sp_changepublication
ms.assetid: c36e5865-25d5-42b7-b045-dc5036225081
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a0e183d97b17936d5d7e1b414c2d492910a8a8b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207078"
---
# <a name="sp_changepublication-transact-sql"></a>sp_changepublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  更改发布的属性。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_changepublication [ [ @publication = ] 'publication' ]  
    [ , [ @property = ] 'property' ]  
    [ , [ @value = ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 NULL。  
  
`[ @property = ] 'property'` 要更改的发布属性。 *属性* 为 **nvarchar (255)**。  
  
`[ @value = ] 'value'` 新属性值。 *值* 为 **nvarchar (255)**，默认值为 NULL。  
  
 下表说明了可以更改的发布属性以及对这些属性值的限制。  
  
|properties|值|说明|  
|--------------|-----------|-----------------|  
|**allow_anonymous**|true|可以为给定发布创建匿名订阅，还必须为 **true** *immediate_sync* 。 对于对等发布，无法更改此属性。|  
||**false**|不能为给定发布创建匿名订阅。 对于对等发布，无法更改此属性。|  
|**allow_initialize_from_backup**|true|订阅服务器可以从备份而非初始快照中初始化对此发布的订阅。 对于非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
||**false**|订阅服务器必须使用初始快照。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**allow_partition_switch**|true|ALTER TABLE .。。SWITCH 语句可针对已发布的数据库执行。 有关详细信息，请参阅[复制已分区表和索引](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md)。|  
||**false**|ALTER TABLE .。。不能对已发布数据库执行 SWITCH 语句。|  
|**allow_pull**|true|给定的发布允许请求订阅。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
||**false**|给定的发布不允许进行请求订阅。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**allow_push**|true|给定的发布允许推送订阅。|  
||**false**|给定的发布不允许进行推送订阅。|  
|**allow_subscription_copy**|true|启用复制订阅该发布的数据库的功能。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
||**false**|禁用复制订阅该发布的数据库的功能。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**alt_snapshot_folder**||快照的备用文件夹的位置。|  
|**centralized_conflicts**|true|在发布服务器上存储冲突记录。 只有在没有任何活动订阅的情况下才能更改此属性。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
||**false**|冲突记录同时存储在导致冲突的发布服务器和订阅服务器上。 只有在没有任何活动订阅的情况下才能更改此属性。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**compress_snapshot**|true|备用快照文件夹中的快照压缩为 .cab 文件格式。 不能压缩默认快照文件夹中的快照。|  
||**false**|不压缩快照，这是复制的默认行为。|  
|**conflict_policy**|**pub wins**|更新订阅服务器的冲突解决策略，此时发布服务器入选冲突。 只有在没有活动订阅时才能更改该属性。 Oracle 发布服务器不支持。|  
||**sub reinit**|要更新订阅服务器，如果出现冲突，则必须重新初始化订阅。 只有在没有活动订阅时才能更改该属性。 Oracle 发布服务器不支持。|  
||**sub wins**|更新订阅服务器的冲突解决策略，此时订阅服务器入选冲突。 只有在没有活动订阅时才能更改该属性。 Oracle 发布服务器不支持。|  
|**conflict_retention**||**int** ，它指定冲突保持期（天）。 默认保持期为 14 天。 **0** 表示不需要清除冲突。 Oracle 发布服务器不支持。|  
|description||用于说明发布的可选项。|  
|**enabled_for_het_sub**|true|启用发布以支持非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。 当存在对发布的订阅时，不能更改 **enabled_for_het_sub** 。 在将 **enabled_for_het_sub** 设置为 true 之前，您可能需要 [ (Transact-sql) 执行复制存储过程](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)才能符合以下要求：<br /> - **allow_queued_tran** 必须为 **false**。<br /> - **allow_sync_tran** 必须为 **false**。<br /> 将 **enabled_for_het_sub** 更改为 **true** 可能会更改现有的发布设置。 有关详细信息，请参阅 [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
||**false**|发布不支持非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**enabled_for_internet**|true|为 Internet 启用发布，此时可以使用文件传输协议 (FTP) 向订阅服务器传输快照文件。 发布的同步文件将放入以下目录中：C:\Program Files\Microsoft SQL Server\MSSQL\Repldata\ftp。 *ftp_address* 不能为 NULL。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
||**false**|不为 Internet 启用发布。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**enabled_for_p2p**|true|发布支持对等复制。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。<br /> 若要将 **enabled_for_p2p** 设置为 **true**，请遵循以下限制：<br /> - **allow_anonymous** 必须为 **false**<br /> - **allow_dts** 必须为 **false**。<br /> - **allow_initialize_from_backup** 必须为 **true**<br /> - **allow_queued_tran** 必须为 **false**。<br /> - **allow_sync_tran** 必须为 **false**。<br /> - **enabled_for_het_sub** 必须为 **false**。<br /> - **independent_agent** 必须为 **true**。<br /> - **repl_freq** 必须是 **连续** 的。<br /> - **replicate_ddl** 必须为 **1**。|  
||**false**|发布不支持对等复制。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**ftp_address**||发布快照文件的可访问 FTP 地址。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**ftp_login**||用于连接到 FTP 服务的用户名，允许使用值 ANONYMOUS。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**ftp_password**||用于连接到 FTP 服务的用户名的密码。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**ftp_port**||分发服务器的 FTP 服务端口号。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**ftp_subdirectory**||指定在发布支持使用 FTP 传播快照时创建快照文件的位置。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
|**immediate_sync**|true|每次运行快照代理时创建或重新创建发布的同步文件。 如果订阅前已完成一次快照代理，则订阅服务器可以在订阅后立即收到同步文件。 新订阅将获取最近一次执行快照代理所生成的最新同步文件。 *independent_agent* 也必须为 **true**。 有关 **immediate_sync** 的其他信息，请参阅下面的备注。|  
||**false**|仅当有新订阅时，才创建同步文件。 订阅服务器无法在订阅后收到同步文件，直到开始快照代理并完成。|  
|**independent_agent**|true|发布具有专用的分发代理。|  
||**false**|发布使用共享分发代理，每一对发布/订阅数据库共享一个代理。|  
|**p2p_continue_onconflict**|true|检测到冲突时，分发代理继续处理更改。<br /> **警告：** 建议使用默认值 `FALSE` 。 如果将此选项设置为 `TRUE` ，则分发代理尝试通过应用具有最高发起方 ID 的节点的冲突行来聚合拓扑中的数据。 此方法不保证将会收敛。 您应确保检测到冲突之后拓扑保持一致。 有关详细信息，请参阅 [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)中的“处理冲突”。|  
||**false**|检测到冲突时，分发代理将停止处理更改。|  
|**post_snapshot_script**||指定在初始同步过程中应用了其他所有复制对象脚本和数据后，分发代理所运行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本文件的位置。|  
|**pre_snapshot_script**||指定在初始同步过程中应用其他所有复制对象脚本和数据之前，分发代理所运行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本文件的位置。|  
|**publish_to_ActiveDirectory**|true|已不推荐使用该参数，支持该参数只是为了让脚本能够向后兼容。 不能再向 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory 中添加发布信息。|  
||**false**|将发布信息从 Active Directory 上删除。|  
|**queue_type**|**sql**|使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储事务。 只有在没有活动订阅时才能更改该属性。<br /><br /> 注意：不再支持使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 消息队列。 为 "*值*" 指定 **msmq** 值将导致错误。|  
|**repl_freq**|**持续**|发布所有基于日志的事务的输出。|  
||**概述**|仅发布计划的同步事件。|  
|**replicate_ddl**|**1**|复制在发布服务器上执行的数据定义语言 (DDL) 语句。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。|  
||**0**|不复制 DDL 语句。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布，无法更改此属性。 使用对等复制时，无法禁用架构更改复制功能。|  
|**replicate_partition_switch**|true|ALTER TABLE .。。应将对已发布数据库执行的 SWITCH 语句复制到订阅服务器。 仅当 *allow_partition_switch* 设置为 TRUE 时，此选项才有效。 有关详细信息，请参阅[复制已分区表和索引](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md)。|  
||**false**|ALTER TABLE .。。不应将 SWITCH 语句复制到订阅服务器。|  
|**保留**||**整数** ，表示订阅活动的保持期（小时）。 如果订阅在保持期内不活动，则将其删除。|  
|**snapshot_in_defaultfolder**|true|在默认快照文件夹中存储快照文件。 如果同时指定了 *alt_snapshot_folder*，则快照文件将同时存储在默认位置和备用位置。|  
||**false**|快照文件存储在 *alt_snapshot_folder* 指定的备用位置。|  
|**status**|**active**|发布创建后，发布数据立即可用于订阅服务器。 Oracle 发布服务器不支持。|  
||**inactive**|发布创建后，发布数据不可用于订阅服务器。 Oracle 发布服务器不支持。|  
|**sync_method**|**native**|同步订阅时，使用所有表的本机模式大容量复制输出。|  
||**character**|同步订阅时，使用所有表的字符模式大容量复制输出。|  
||**发出**|在快照生成期间生成所有表的本机模式大容量复制程序输出，但不锁定表。 对快照复制无效。|  
||**concurrent_c**|在快照生成期间生成所有表的字符模式大容量复制程序输出，但不锁定表。 对快照复制无效。|  
|**taskid**||不推荐使用该属性，也不再支持该属性。|  
|**allow_drop**|true|`DROP TABLE`为属于事务复制的项目启用 DLL 支持。 支持的最低版本： [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Service pack 2 或更高版本以及 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] service pack 1 或更高版本。 附加参考： [KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactional-replication-in-sql-server-2014-or-in-sql-server-2016-sp1)|
||**false**|`DROP TABLE`对于属于事务复制的项目，禁用 DLL 支持。 这是此属性的 **默认** 值。|
|**NULL** (默认值) ||返回 *属性* 的支持值的列表。|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 为一个 **位**，默认值为 **0**。  
  - **0** 指定对项目所做的更改不会导致快照无效。 如果该存储过程检测到更改确实需要新的快照，则会发生错误，并且不进行任何更改。  
  - **1** 指定对项目的更改可能导致快照无效。 如果有现有订阅需要新快照，该值授予将现有快照标记为过时快照的权限，并生成新快照。   
有关在更改时需要生成新快照的属性，请参阅“备注”部分。  
  
[**@force_reinit_subscription =** ] *force_reinit_subscription*  
 确认此存储过程所执行的操作可能需要重新初始化现有订阅。 *force_reinit_subscription* 是一 **位** ，默认值为 **0**。  
  - **0** 指定对项目所做的更改不会导致重新初始化订阅。 如果该存储过程检测到更改将需要重新初始化现有订阅，则会发生错误，并且不进行任何更改。  
  - **1** 指定对项目所做的更改会导致重新初始化现有订阅，并授予重新初始化订阅的权限。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
  > [!NOTE]  
  >  更改发布服务器上的项目属性时，不应使用 *publisher* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changepublication** 用于快照复制和事务复制。  
  
 更改以下任何属性之后，必须生成新的快照，并且必须将 *force_invalidate_snapshot* 参数的值指定为 **1** 。  
-   **alt_snapshot_folder**  
-   **compress_snapshot**  
-   **enabled_for_het_sub**  
-   **ftp_address**  
-   **ftp_login**  
-   **ftp_password**  
-   **ftp_port**  
-   **ftp_subdirectory**  
-   **post_snapshot_script**  
-   **pre_snapshot_script**  
-   **snapshot_in_defaultfolder**  
-   **sync_mode**  
  
若要使用 **publish_to_active_directory** 参数列出 Active Directory 中的发布对象， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须已经在 Active Directory 中创建了对象。  
  
## <a name="impact-of-immediate-sync"></a>立即同步的影响  
 如果打开立即同步，则在生成初始快照后，即使没有订阅，也会立即跟踪日志中的所有更改。 当客户使用备份来添加新的对等节点时，会使用记录的更改。 还原备份后，对等机将与创建备份后发生的任何其他更改同步。 由于在分发数据库中跟踪命令，同步逻辑可以查看上次备份的 LSN，并将其用作起点，知道如果在最大保持期内执行备份，则该命令可用。  (最小保持期的默认值为0小时，最大保持期为24小时。 )   
  
 关闭立即同步时，将至少在最小保持期内保留更改，并对已复制的所有事务立即清除更改。 如果已关闭立即同步并为其配置了默认保持期，很可能会清理在进行备份后所需要的更改，并且不会正确初始化新的对等节点。 剩下的唯一选项是使拓扑静止。 将立即同步设置为打开可提供更大的灵活性，并且是用于 P2P 复制的推荐设置。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_changepublication](../../relational-databases/replication/codesnippet/tsql/sp-changepublication-tra_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_changepublication**。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addpublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_droppublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [sp_helppublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
