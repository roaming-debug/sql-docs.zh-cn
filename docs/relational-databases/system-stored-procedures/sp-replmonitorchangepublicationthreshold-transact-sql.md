---
title: 'sp_replmonitorchangepublicationthreshold (T-sql) '
description: 描述为发布更改监视阈值指标的 sp_replmonitorchangepublicationthreshold 存储过程。
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorchangepublicationthreshold_TSQL
- sp_replmonitorchangepublicationthreshold
helpviewer_keywords:
- sp_replmonitorchangepublicationthreshold
ms.assetid: 2c3615d8-4a1a-4162-b096-97aefe6ddc16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 62f360416f46eee0bbe685f6ebd1af7520526ee6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204390"
---
# <a name="sp_replmonitorchangepublicationthreshold-transact-sql"></a>sp_replmonitorchangepublicationthreshold (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  更改发布的监视阈值标准。 在分发服务器的分发数据库上执行此存储过程，用于监视复制。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replmonitorchangepublicationthreshold [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @metric_id = ] metric_id ]   
    [ , [ @thresholdmetricname = ] 'thresholdmetricname'   
    [ , [ @value = ] value ]   
    [ , [ @shouldalert = ] shouldalert ]   
    [ , [ @mode = ] mode ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 已发布数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 正在更改其监视阈值属性的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @publication_type = ] publication_type` 如果发布的类型为。 *publication_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|事务发布。|  
|**1**|快照发布。|  
|**2**|合并发布。|  
|NULL（默认值）|复制尝试确定发布类型。|  
  
`[ @metric_id = ] metric_id` 正在更改的发布阈值指标的 ID。 *metric_id* 为 **int**，默认值为 NULL，可以是下列值之一。  
  
|值|标准名称|  
|-----------|-----------------|  
|**1**|**expiration** - 监视对事务发布的订阅是否即将过期。|  
|**2**|**latency** - 监视对事务发布的订阅的性能。|  
|**4**|**mergeexpiration** - 监视对合并发布的订阅是否即将过期。|  
|**5**|**mergeslowrunduration** -监视通过低带宽 (拨号) 连接进行的合并同步的持续时间。|  
|**6**|**mergefastrunduration** -监视通过高带宽局域网 (LAN) 连接进行的合并同步的持续时间。|  
|**7**|**mergefastrunspeed** - 监视通过高带宽 (LAN) 连接进行的合并同步的同步速率。|  
|**8**|**mergeslowrunspeed** -监视通过低带宽 (拨号) 连接进行的合并同步的同步速率。|  
  
 必须指定 *metric_id* 或 *thresholdmetricname*。 如果指定了 *thresholdmetricname* ，则 *METRIC_ID* 应为 NULL。  
  
`[ @thresholdmetricname = ] 'thresholdmetricname'` 正在更改的发布阈值指标的名称。 *thresholdmetricname* 的值为 **sysname**，默认值为 NULL。 必须指定 *thresholdmetricname* 或 *metric_id*。 如果指定 *metric_id* ，则 *THRESHOLDMETRICNAME* 应为 NULL。  
  
`[ @value = ] value` 发布阈值指标的新值。 *值* 为 **int**，默认值为 NULL。 如果 **为 null**，则不更新指标值。  
  
`[ @shouldalert = ] shouldalert` 当达到发布阈值指标时是否生成警报。 *shouldalert* 的值为 **bit**，默认值为 NULL。 值为 **1** 表示生成警报，值为 **0** 表示不生成警报。  
  
`[ @mode = ] mode` 如果已启用发布阈值指标，则为。 *mode* 的值为 **tinyint**，默认值为 **1**。 如果值为 **1** ，则表示启用了此指标的监视，值为 **2** 表示禁用了此指标的监视。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_replmonitorchangepublicationthreshold** 用于所有类型的复制。  
  
## <a name="permissions"></a>权限  
 只有分发数据库中 **db_owner** 或 **replmonitor** 固定数据库角色的成员才能执行 **sp_replmonitorchangepublicationthreshold**。  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
