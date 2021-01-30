---
description: sp_changemergepublication (Transact-SQL)
title: sp_changemergepublication (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergepublication_TSQL
- sp_changemergepublication
helpviewer_keywords:
- sp_changemergepublication
ms.assetid: 81fe1994-7678-4852-980b-e02fedf1e796
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e7807a445de3ddf919679b09002646c87b313c5d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159856"
---
# <a name="sp_changemergepublication-transact-sql"></a>sp_changemergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改合并发布的属性。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changemergepublication [ @publication= ] 'publication'  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @property = ] 'property'` 给定发布的要更改的属性。 *属性* 为 **sysname**，可以是下表中列出的值之一。  
  
`[ @value = ] 'value'` 指定属性的新值。 *值* 为 **nvarchar (255)**，可以是下表中列出的值之一。  
  
 下表描述了可以更改的发布属性，并描述了这些属性的值的限制。  
  
|properties|值|说明|  
|--------------|-----------|-----------------|  
|**allow_anonymous**|true|允许匿名订阅。|  
||**false**|不允许匿名订阅。|  
|**allow_partition_realignment**|true|将删除指令发送到订阅服务器，删除不再属于订阅服务器分区的数据以反映分区更改结果。 这是默认行为。|  
||**false**|旧分区中的数据保留在订阅服务器上，发布服务器上对此数据所做的更改不会复制到此订阅服务器。 相反，在订阅服务器上进行的更改将复制到发布服务器。 此属性用于保留旧分区中的订阅数据，以便满足用户访问历史数据的需要。|  
|**allow_pull**|true|给定的发布允许请求订阅。|  
||**false**|给定的发布不允许进行请求订阅。|  
|**allow_push**|true|给定的发布允许推送订阅。|  
||**false**|给定的发布不允许进行推送订阅。|  
|**allow_subscriber_initiated_snapshot**|true|订阅服务器可以启动快照进程。|  
||**false**|订阅服务器无法启动快照进程。|  
|**allow_subscription_copy**|true|可以复制订阅此发布的订阅数据库。|  
||**false**|不可以复制订阅此发布的订阅数据库。|  
|**allow_synctoalternate**|true|允许备用同步伙伴与此发布服务器进行同步。|  
||**false**|禁止备用同步伙伴与此发布服务器进行同步。|  
|**allow_web_synchronization**|true|可以通过 HTTPS 同步订阅。|  
||**false**|不能通过 HTTPS 同步订阅。|  
|**alt_snapshot_folder**||指定快照的备用文件夹的位置。|  
|**automatic_reinitialization_policy**|**1**|在重新初始化订阅之前，从订阅服务器上载更改。|  
||**0**|不先上载更改就重新初始化订阅。|  
|**centralized_conflicts**|true|所有冲突记录都存储在发布服务器上。 如果更改此属性，则必须重新初始化现有订阅服务器。|  
||**false**|冲突记录存储在冲突解决中落选的服务器上。 如果更改此属性，则必须重新初始化现有订阅服务器。|  
|**compress_snapshot**|true|备用快照文件夹中的快照将压缩为 CAB 格式。 不能压缩默认快照文件夹中的快照。 若要更改此属性，则需要新的快照。|  
||**false**|默认情况下，不压缩快照。 若要更改此属性，则需要新的快照。|  
|**conflict_logging**|**publisher**|在发布服务器上存储冲突记录。|  
||**订阅服务器**|在导致冲突的订阅服务器上存储冲突记录。 [!INCLUDE[ssEW](../../includes/ssew-md.md)]订阅服务器不支持 *。*|  
||**全部**|在发布服务器和订阅服务器上都存储冲突记录。|  
|**conflict_retention**||一个 **int** ，指定保留冲突的保持期（天）。 将 *conflict_retention* 设置为 **0** 表示不需要进行冲突清除。|  
|description||对发布的说明。|  
|**dynamic_filters**|true|根据动态子句筛选发布。|  
||**false**|不对发布进行动态筛选。|  
|**enabled_for_internet**|true|为 Internet 启用发布。 可以使用文件传输协议 (FTP) 将快照文件传输到订阅服务器。 发布的同步文件放在 C:\Program Files\Microsoft SQL Server\MSSQL\Repldata\ftp 目录中。|  
||**false**|不为 Internet 启用发布。|  
|**ftp_address**||分发服务器的 FTP 服务的网络地址。 指定存储发布快照文件的位置。|  
|**ftp_login**||用于连接到 FTP 服务的用户名。|  
|**ftp_password**||用于连接到 FTP 服务的用户密码。|  
|**ftp_port**||分发服务器的 FTP 服务的端口号。 指定存储发布快照文件的 FTP 站点的 TCP 端口号。|  
|**ftp_subdirectory**||指定当发布支持使用 FTP 传播快照时用于创建快照文件的位置。|  
|**generation_leveling_threshold**|**int**|指定代中包含的更改的数目。 代是传递给发布服务器或订阅服务器的更改的集合。|  
|**keep_partition_changes**|true|对同步进行优化，只有所包含的一些行位于已更改分区中的订阅服务器才会受影响。 若要更改此属性，则需要新的快照。|  
||**false**|不优化同步，分区中的数据发生更改时，对发送到订阅服务器的分区进行验证。 若要更改此属性，则需要新的快照。|  
|**max_concurrent_merge**||这是一个 **int** ，它表示可对发布运行的最大并发合并进程数。 如果为 0，则没有限制。如果计划同时运行的合并进程数超出这一数值，则将多出的作业放在队列中等待，直到当前的合并进程完成。|  
|**max_concurrent_dynamic_snapshots**||这是一个 **int** ，它表示要生成筛选的数据快照的最大快照会话数，该快照可以同时对使用参数化行筛选器的合并发布运行。 如果为 **0**，则没有限制。 如果计划同时执行的快照进程数超出这一数值，则多出的作业将被放到队列中等待，直到当前运行的合并进程完成。|  
|**post_snapshot_script**||指定指向 **.sql** 文件位置的指针。 在初始同步过程中应用了所有其他复制的对象脚本和数据之后，分发代理或合并代理才运行快照后脚本。 若要更改此属性，则需要新的快照。|  
|**pre_snapshot_script**||指定指向 **.sql** 文件位置的指针。 合并代理在订阅服务器上应用快照时，在任何复制的对象脚本之前运行快照前脚本。 若要更改此属性，则需要新的快照。|  
|**publication_compatibility_level**|**100RTM**|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
||**90RTM**|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|  
|**publish_to_activedirectory**|true|已不推荐使用该参数，支持该参数只是为了让脚本能够向后兼容。 您不再能够向 Active Directory 中添加发布信息。|  
||**false**|将发布信息从 Active Directory 上删除。|  
|**replicate_ddl**|**1**|复制在发布服务器上执行的数据定义语言 (DDL) 语句。|  
||**0**|不复制 DDL 语句。|  
|**保留**||这是一个 **int** ，它表示为给定发布保存更改的 *retention_period_unit* 单元数。 如果在保持期内没有同步该订阅，并在分发服务器上使用清除操作删除了该订阅本应接收到的挂起更改，则该订阅将过期，必须重新初始化。 允许的最大保持期为当前日期到 9999 年 12 月 31 日之间的天数。<br /><br /> 注意：合并发布的保持期具有24小时的宽限期，以适应不同时区中的订阅者。|  
|**retention_period_unit**|**day**|按天指定保持期。|  
||week |按周指定保持期。|  
||**month**|按月指定保持期。|  
||**year**|按年指定保持期。|  
|**snapshot_in_defaultfolder**|true|在默认快照文件夹中存储快照文件。|  
||**false**|快照文件存储在 *alt_snapshot_folder* 指定的备用位置。 此组合指定将快照文件同时存储在默认位置和备用位置中。|  
|**snapshot_ready**|true|用于发布的快照已准备就绪。|  
||**false**|用于发布的快照尚未准备就绪。|  
|**status**|**active**|发布处于活动状态。|  
||**inactive**|发布处于非活动状态。|  
|**sync_mode**|**本机** 或<br /><br /> **bcp 本机**|将所有表的本机模式大容量复制程序输出用于初始快照。|  
||**character**<br /><br /> 或 **bcp 字符**|将所有表的字符模式大容量复制程序输出用于初始快照，所有非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器必须执行此操作。|  
|**use_partition_groups**<br /><br /> 注意：使用 partition_groups 后，如果要还原为使用 **setupbelongs**，并在 **changemergearticle** 中设置 **use_partition_groups = false** ，则在拍摄快照后，这可能不会正确反映。 快照生成的触发器符合分区组的要求。<br /><br /> 此方案的解决方法是将状态设置为 "非活动"，修改 **use_partition_groups**，然后将 "状态" 设置为 "活动"。|true|发布使用预计算分区。|  
||**false**|发布不使用预计算分区。|  
|**validate_subscriber_info**||列出用于检索订阅服务器信息的函数。 然后，验证要用于订阅服务器的动态筛选条件以验证是否对信息进行了一致的分区。|  
|**web_synchronization_url**||用于 Web 同步的 Internet URL 的默认值。|  
|NULL（默认值）||返回 *属性* 的支持值的列表。|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作可能会使现有快照失效。 *force_invalidate_snapshot* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定更改发布不会使快照无效。 如果该存储过程检测到更改确实需要新的快照，则会发生错误，并且不进行任何更改。  
  
 **1** 指定更改发布可能 invvalidate 快照。 如果现有订阅需要新的快照，则授予将现有快照标记为过时快照并生成新快照的权限。  
  
 请参阅 "备注" 部分，了解在更改时需要生成新快照的属性。  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` 确认此存储过程所执行的操作可能需要重新初始化现有订阅。 *force_reinit_subscription* 是一 **位** ，默认值为 **0**。  
  
 **0** 指定更改发布不需要重新初始化订阅。 如果该存储过程检测到更改将需要重新初始化现有订阅，则会发生错误，并且不进行任何更改。  
  
 **1** 指定更改发布将导致重新初始化现有订阅，并授予重新初始化订阅的权限。  
  
 有关在更改时需要重新初始化所有现有订阅的属性，请参阅“备注”部分。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changemergepublication** 用于合并复制。  
  
 若要更改下列属性，需要生成新的快照。 必须将 *force_invalidate_snapshot* 参数的值指定为 **1** 。  
  
-   **alt_snapshot_folder**  
  
-   **compress_snapshot**  
  
-   **dynamic_filters**  
  
-   **ftp_address**  
  
-   **ftp_login**  
  
-   **ftp_password**  
  
-   **ftp_port**  
  
-   **ftp_subdirectory**  
  
-   **post_snapshot_script**  
  
-   **publication_compatibility_level** 仅 (到 **80SP3**)   
  
-   **pre_snapshot_script**  
  
-   **snapshot_in_defaultfolder**  
  
-   **sync_mode**  
  
-   **use_partition_groups**  
  
 若要更改下列属性，需要重新初始化现有订阅。 必须将 *force_reinit_subscription* 参数的值指定为 **1** 。  
  
-   **dynamic_filters**  
  
-   **validate_subscriber_info**  
  
 若要通过使用 *publish_to_active_directory* 列出要 Active Directory 的发布对象， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须已经在 Active Directory 中创建了对象。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_changemergepublication](../../relational-databases/replication/codesnippet/tsql/sp-changemergepublicatio_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_changemergepublication**。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergepublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_helpmergepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
