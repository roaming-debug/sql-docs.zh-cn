---
description: sysmergesubscriptions (Transact-SQL)
title: sysmergesubscriptions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergesubscriptions_TSQL
- sysmergesubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergesubscriptions system table
ms.assetid: 6adc78da-991d-4c08-98c3-ecb4762e0e99
author: cawrites
ms.author: chadam
ms.openlocfilehash: e3caa66b5e8e938a0e54f7212b37e0442e29ebfe
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98090615"
---
# <a name="sysmergesubscriptions-transact-sql"></a>sysmergesubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个已知订阅服务器在表中对应一行，并且该表是发布服务器的本地表。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|subscriber_server|**sysname**|服务器的 ID。 将订阅数据库的复本迁移到其他服务器时，用于将 srvid 字段映射到服务器特定的值。|  
|db_name|**sysname**|订阅数据库的名称。|  
|pubid|**uniqueidentifier**|从中创建当前订阅的发布 ID。|  
|datasource_type|**int**|数据源的类型：<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br /> **2** = Jet OLE DB。|  
|subid|**uniqueidentifier**|订阅的唯一标识号。|  
|replnickname|**binary**|复制的压缩别名。|  
|replicastate|**uniqueidentifier**|一个唯一标识符，它通过将发布服务器中的值与订阅服务器中的值进行比较来确定以前的同步是否成功。|  
|status|**tinyint**|订阅的状态：<br /><br /> **0** = 非活动。<br /><br /> **1** = 活动。<br /><br /> **2** = 已删除。|  
|subscriber_type|**int**|订阅服务器的类型：<br /><br /> **1** = 全局。<br /><br /> **2** = 本地。<br /><br /> **3** = 匿名。|  
|subscription_type|**int**|订阅的类型：<br /><br /> **0** = 推送。<br /><br /> **1** = 请求。<br /><br /> **2** = 匿名。|  
|sync_type|**tinyint**|同步类型：<br /><br /> **1** = 自动。<br /><br /> **2** = 不同步。|  
|说明|**nvarchar(255)**|对订阅的简短说明。|  
|priority|**real**|指定订阅优先级并允许实现基于优先级的冲突解决。 对于所有本地或匿名订阅，等于 **0.00** 。|  
|recgen|**bigint**|上次接收的生成的编号。|  
|recguid|**uniqueidentifier**|上次接收的生成的唯一 ID。|  
|sentgen|**bigint**|上次发送的生成的编号。|  
|sentguid|**uniqueidentifier**|上次发送的生成的唯一 ID。|  
|schemaversion|**int**|上次接收的架构的编号。|  
|schemaguid|**uniqueidentifier**|上次接收的架构的唯一 ID。|  
|last_validated|**datetime**|上次成功验证订阅服务器数据的 **日期时间** 。|  
|attempted_validate|**datetime**|尝试对订阅进行验证的最后一个 **日期时间** 。|  
|last_sync_date|**datetime**|同步的 **日期时间** 。|  
|last_sync_status|**int**|订阅状态：<br /><br /> **0** = 所有作业都在等待启动。<br /><br /> **1** = 一个或多个作业正在启动。<br /><br /> **2** = 所有作业都已成功执行。<br /><br /> **3** = 至少有一个作业正在执行。<br /><br /> **4** = 所有作业都已计划且处于空闲状态。<br /><br /> **5** = 在上次失败后至少有一个作业正在尝试执行。<br /><br /> **6** = 至少有一个作业无法成功执行。|  
|last_sync_summary|**sysname**|上一个同步结果的说明。|  
|metadatacleanuptime|**datetime**|从合并复制系统表中删除过期元数据的最后一个 **日期时间** 。|  
|partition_id|**int**|标识订阅所属的预先计算的分区。|  
|cleanedup_unsent_changes|**bit**|标识已在订阅服务器上清除了未发送更改的元数据。|  
|replica_version|**int**|标识订阅所属的订阅服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本，可以为以下值之一：<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|supportability_mode|**int**|仅限内部使用。|  
|application_name|**nvarchar(128)**|仅限内部使用。|  
|subscriber_number|**int**|仅限内部使用。|  
|last_makegeneration_datetime|**datetime**|Makegeneration 进程为发布服务器运行的最后一个 **日期时间** 。 有关详细信息，请参阅 MakeGenerationInterval 中的-参数 [合并代理](../../relational-databases/replication/agents/replication-merge-agent.md)。|  
  
## <a name="see-also"></a>另请参阅  
 [复制表 (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
