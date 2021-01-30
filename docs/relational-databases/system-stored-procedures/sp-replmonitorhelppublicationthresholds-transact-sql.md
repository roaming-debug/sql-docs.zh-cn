---
title: 'sp_replmonitorhelppublicationthresholds (T-sql) '
description: 描述返回监视的发布的阈值度量值集的 sp_replmonitorhelppublicationthresholds 存储过程。
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelppublicationthresholds
- sp_replmonitorhelppublicationthresholds_TSQL
helpviewer_keywords:
- sp_replmonitorhelppublicationthresholds
ms.assetid: d6b1aa4b-3369-4255-a892-c0e5cc9cb693
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1cfd6db6845702b624e98a31980a46f2fc6fd4f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193921"
---
# <a name="sp_replmonitorhelppublicationthresholds-transact-sql"></a>sp_replmonitorhelppublicationthresholds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回为所监视发布设置的阈值度量指标。 在分发服务器的分发数据库上执行此存储过程，用于监视复制。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replmonitorhelppublicationthresholds [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @thresholdmetricname = ] 'thresholdmetricname'  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 已发布数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @publication_type = ] publication_type` 如果发布的类型为。 *publication_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|事务发布。|  
|**1**|快照发布。|  
|**2**|合并发布。|  
|NULL（默认值）|由复制来确定发布类型。|  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**metric_id**|**int**|复制性能度量指标的 ID，可为下列值之一。<br /><br /> **1expiration** -监视对事务发布的订阅是否即将过期。<br /><br /> **2latency** -监视对事务发布的订阅的性能。<br /><br /> **4mergeexpiration** -监视对合并发布的订阅是否即将过期。<br /><br /> **5mergeslowrunduration** -监视通过低带宽 (拨号) 连接进行的合并同步的持续时间。<br /><br /> **6mergefastrunduration** -监视通过高带宽 (LAN) 连接进行的合并同步的持续时间。<br /><br /> **7mergefastrunspeed** -监视通过高带宽 (LAN) 连接进行的合并同步的同步速率。<br /><br /> **8mergeslowrunspeed** -监视通过低带宽 (拨号) 连接进行的合并同步的同步速率。|  
|**title**|**sysname**|复制性能度量指标的名称。|  
|**value**|**int**|性能度量指标的阈值。|  
|**shouldalert**|**bit**|如果在指标超过此发布定义的阈值时是否应生成警报，则为;如果值为 **1** ，则表示应引发警报。|  
|**isenabled**|**bit**|如果为此发布的此复制性能指标启用了监视，则为;值为 **1** 表示启用监视。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_replmonitorhelppublicationthresholds** 用于所有类型的复制。  
  
## <a name="permissions"></a>权限  
 只有分发数据库上的 **db_owner** 或 **replmonitor** 固定数据库角色的成员才能执行 **sp_replmonitorhelppublicationthresholds**。  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
