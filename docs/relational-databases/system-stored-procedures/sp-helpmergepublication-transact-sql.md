---
description: sp_helpmergepublication (Transact-SQL)
title: sp_helpmergepublication (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepublication
- sp_helpmergepublication_TSQL
helpviewer_keywords:
- sp_helpmergepublication
ms.assetid: dfe1e1e1-9a65-406a-aced-6385a078e135
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8001e98b207ca9493ff71b1f874b12b0d5227306
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160610"
---
# <a name="sp_helpmergepublication-transact-sql"></a>sp_helpmergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关合并发布的信息。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpmergepublication [ [ @publication = ] 'publication' ]  
    [ , [ @found = ] 'found' OUTPUT ]  
    [ , [ @publication_id = ] 'publication_id' OUTPUT ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
```  
  
## <a name="arguments"></a>参数  
 [ @publication **=** ] **"**_发布_**"**  
 发布的名称。 *发布* 为 **sysname**，默认值为 **%** ，它返回当前数据库中所有合并发布的相关信息。  
  
 [ @found **=** ] **"**_找到_*_"_* 输出  
 指示返回行的标志。 *找到* **int** 和 OUTPUT 参数，默认值为 NULL。 **1** 指示已找到发布。 **0** 表示找不到发布。  
  
 [ @publication_id **=** ] **"**_publication_id_*_"_* 输出  
 发布的标识号。 *publication_id* 是 **uniqueidentifier** 和 OUTPUT 参数，默认值为 NULL。  
  
 [ @reserved **=** ] **"**_reserved_*_"_*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]*reserved* 为 **nvarchar (20)**，默认值为 NULL。  
  
 [ @publisher **=** ] **'**_发布服务器_*_'_*  
 发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
 [ @publisher_db **=** ] **'**_publisher_db_*_'_*  
 发布数据库的名称。 *publisher_db* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|id|**int**|发布在结果集列表中的连续顺序。|  
|name|**sysname**|发布的名称。|  
|description|**nvarchar(255)**|对发布的说明。|  
|status|**tinyint**|指示发布数据何时可用。|  
|retention|**int**|用于保存对发布中项目所做更改的元数据的时间量。 此时间段单位可以为天、星期、月或年。 有关单位的信息，请参阅 retention_period_unit 列。|  
|sync_mode|**tinyint**|该发布的同步模式：<br /><br /> **0** = 本机大容量复制程序 (**bcp** 实用工具) <br /><br /> **1** = 字符大容量复制|  
|allow_push|**int**|确定是否可以为给定发布创建推送订阅。 **0** 表示不允许推送订阅。|  
|allow_pull|**int**|确定是否可以为给定发布创建请求订阅。 **0** 表示不允许请求订阅。|  
|allow_anonymous|**int**|确定是否可以为给定发布创建匿名订阅。 **0** 表示不允许匿名订阅。|  
|centralized_conflicts|**int**|确定是否将冲突记录存储在给定发布服务器上：<br /><br /> **0** = 在导致冲突的发布服务器和订阅服务器上存储冲突记录。<br /><br /> **1** = 所有冲突记录都存储在发布服务器上。|  
|priority|**float (8)**|环回订阅的优先级。|  
|snapshot_ready|**tinyint**|指示该发布的快照是否可以使用：<br /><br /> **0** = 快照已准备就绪，可供使用。<br /><br /> **1** = 快照尚不可用。|  
|publication_type|**int**|发布的类型：<br /><br /> **0** = Snapshot。<br /><br /> **1** = 事务性。<br /><br /> **2** = 合并。|  
|pubid|**uniqueidentifier**|该发布的唯一标识符。|  
|snapshot_jobid|**binary(16)**|快照代理的作业 ID。 若要获取 [sysjobs](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md) 系统表中快照作业的条目，必须将此十六进制值转换为 **uniqueidentifier**。|  
|enabled_for_internet|**int**|确定是否为 Internet 启用发布。 如果为 **1**，则将发布的同步文件放在 `C:\Program Files\Microsoft SQL Server\MSSQL\Repldata\Ftp` 目录中。 用户必须创建文件传输协议 (FTP) 目录。 如果为 **0**，则不启用发布以进行 Internet 访问。|  
|dynamic_filter|**int**|指示是否使用参数化行筛选器。 **0** 表示不使用参数化行筛选器。|  
|has_subscription|**bit**|指示发布是否具有任何订阅。 **0** 表示当前没有对此发布的订阅。|  
|snapshot_in_default_folder|**bit**|指定快照文件是否存储在默认文件夹中。<br /><br /> 如果为 **1**，则可以在默认文件夹中找到快照文件。<br /><br /> 如果为 **0**，则将快照文件存储在 **alt_snapshot_folder** 指定的备用位置。 备用位置可以在另一台服务器、网络驱动器或可移动介质 (如 cd-rom 或可移动磁盘) 上。 也可以将快照文件保存到 FTP 站点以供订阅方以后检索。<br /><br /> 注意：此参数可以为 true，并且在 **alt_snapshot_folder** 参数中仍有一个位置。 这个组合指定将快照文件存储在默认位置和替代位置。|  
|alt_snapshot_folder|**nvarchar(255)**|指定快照的备用文件夹的位置。|  
|pre_snapshot_script|**nvarchar(255)**|指定指向 .sql 文件的指针，在订阅服务器上应用快照时，合并代理在任何复制的对象脚本之前运行该文件 **。**|  
|post_snapshot_script|**nvarchar(255)**|指定一个指向 **.sql** 文件的指针，该文件在初始同步过程中应用了其他所有复制对象脚本和数据之后运行合并代理。|  
|compress_snapshot|**bit**|指定写入 **alt_snapshot_folder** 位置的快照压缩为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] CAB 格式。|  
|ftp_address|**sysname**|分发服务器的 FTP 服务网络地址。 指定发布快照文件的位置，以便合并代理选取。|  
|ftp_port|**int**|分发服务器 FTP 服务的端口号。 **ftp_port** 的默认值为 **21**。 指定供合并代理拾取的发布快照文件所在的位置。|  
|ftp_subdirectory|**nvarchar(255)**|指定当使用 FTP 传递快照时，快照文件供合并代理拾取的位置。|  
|ftp_login|**sysname**|用于连接到 FTP 服务的用户名。|  
|conflict_retention|**int**|指定保留冲突的保持期（天）。 指定的天数过后，冲突行将从冲突表中清除掉。|  
|keep_partition_changes|**int**|指定是否对此发布的同步进行优化。 **keep_partition_changes** 的默认值为 **0**。 值 **0** 表示不优化同步，在分区中的数据更改时，将验证发送到所有订阅服务器的分区。<br /><br /> **1** 表示同步经过优化，并且只有具有更改的分区中的行的订阅服务器才会受到影响。<br /><br /> 注意：默认情况下，合并发布使用预计算分区，这比此选项提供更高的优化度。 有关详细信息，请参阅 [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md) 和 [用预计算分区优化参数化筛选器性能](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)。|  
|allow_subscription_copy|**int**|指定是否已启用复制订阅该发布的订阅数据库的功能。 如果值为 **0** ，则表示不允许复制。|  
|allow_synctoalternate|**int**|指定是否允许备用同步伙伴与该发布服务器同步。 值 **0** 表示不允许同步伙伴。|  
|validate_subscriber_info|**nvarchar (500)**|列出用于检索订阅服务器信息和验证订阅服务器上的参数化行筛选条件的函数。 有助于验证信息分区是否与每个合并一致。|  
|backward_comp_level|**int**|数据库兼容级别，可以为下列值之一：<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP1<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP2<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|publish_to_activedirectory|**bit**|指定是否将发布信息发布到 Active Directory。 值为 **0** 表示无法从 Active Directory 获取发布信息。<br /><br /> 已不推荐使用该参数，支持该参数只是为了让脚本能够向后兼容。 您不再能够向 Active Directory 中添加发布信息。|  
|max_concurrent_merge|**int**|并发合并进程数。 如果为 **0**，则在任意给定时间运行的并发合并进程数没有限制。|  
|max_concurrent_dynamic_snapshots|**int**|针对合并发布可以运行的最大并发已筛选数据快照会话数。 如果为 **0**，则在任意给定时间，可同时对发布运行的并发筛选数据快照会话的最大数量没有限制。|  
|use_partition_groups|**int**|确定是否使用预计算分区。 如果值为 **1** ，则表示使用的是预计算分区。|  
|num_of_articles|**int**|发布中的项目个数。|  
|replicate_ddl|**int**|表示是否复制对已发布表的架构更改。 如果值为 **1** ，则表示复制架构更改。|  
|publication_number|**smallint**|分配给该发布的编号。|  
|allow_subscriber_initiated_snapshot|**bit**|确定订阅服务器是否可以启动已筛选数据快照生成进程。 如果值为 **1** ，则表示订阅服务器可以启动快照进程。|  
|allow_web_synchronization|**bit**|确定是否为 Web 同步启用发布。 如果值为 **1** ，则表示启用了 Web 同步。|  
|web_synchronization_url|**nvarchar (500)**|用于 Web 同步的 Internet URL。|  
|allow_partition_realignment|**bit**|确定在发布服务器上的行修改导致该发布服务器更改其分区时是否将删除指令发送到订阅服务器。 如果值为 **1** ，则表示将删除发送到订阅服务器。  有关详细信息，请参阅 [&#40;transact-sql&#41;sp_addmergepublication ](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)。|  
|retention_period_unit|**tinyint**|指定用于定义保持期的单位。 这可以是以下值之一：<br /><br /> **0** = 天<br /><br /> **1** = 周<br /><br /> **2** = 月<br /><br /> **3** = 年|  
|has_downloadonly_articles|**bit**|指示属于发布的所有项目是否为仅用于下载的项目。 如果值为 **1** ，则表示存在仅限下载的项目。|  
|decentralized_conflicts|**int**|指示是否在导致冲突的订阅服务器上存储冲突记录。 值 **0** 表示不将冲突记录存储在订阅服务器上。 值为 1 表示在订阅服务器上存储冲突记录。|  
|generation_leveling_threshold|**int**|指定代中包含的更改的数目。 生成是指已传递给发布服务器或订阅服务器的更改集合|  
|automatic_reinitialization_policy|**bit**|指示是否在进行自动重新初始化之前从订阅服务器上载更改。 值为 **1** 表示在进行自动重新初始化之前，从订阅服务器上载更改。 值为 0 表示在自动重新初始化之前不上载更改。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 sp_helpmergepublication 用于合并复制。  
  
## <a name="permissions"></a>权限  
 发布的发布访问列表的成员可以针对该发布执行 sp_helpmergepublication。 发布数据库中的 db_owner 固定数据库角色成员可以针对所有发布的信息执行 sp_helpmergepublication。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_helpmergepublication](../../relational-databases/replication/codesnippet/tsql/sp-helpmergepublication-_1.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addmergepublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
