---
description: MSmerge_subscriptions (Transact-SQL)
title: MSmerge_subscriptions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_subscriptions
- MSmerge_subscriptions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_subscriptions system table
ms.assetid: cafd954a-92f8-44cb-a5d0-dce9aafa5ee1
author: cawrites
ms.author: chadam
ms.openlocfilehash: be426f69b1566aa3565396cb53dbb78c17fdbcc5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098598"
---
# <a name="msmerge_subscriptions-transact-sql"></a>MSmerge_subscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  订阅服务器上的合并代理服务的每个订阅在 **MSmerge_subscriptions** 表中各占一行。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|发布服务器的 ID。|  
|**publisher_db**|**sysname**|发布服务器数据库的名称。|  
|**publication_id**|**int**|发布 ID。|  
|**subscriber_id**|**smallint**|订阅服务器 ID。|  
|**subscriber_db**|**sysname**|订阅数据库的名称。|  
|**subscription_type**|**int**|订阅的类型：<br /><br /> 0 = 推送。<br /><br /> 1 = 请求。<br /><br /> 2 = 匿名。|  
|**sync_type**|**tinyint**|同步类型：<br /><br /> 1 = 自动。<br /><br /> 2 = 不同步。|  
|**status**|**tinyint**|订阅的状态。|  
|**subscription_time**|**datetime**|添加订阅的时间。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
