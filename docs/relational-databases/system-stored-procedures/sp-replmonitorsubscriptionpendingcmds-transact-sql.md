---
title: 'sp_replmonitorsubscriptionpendingcmds (T-sql) '
description: 介绍 sp_replmonitorsubscriptionpendingcmds 存储过程，该存储过程返回有关对事务发布的订阅的挂起命令的数目的信息。
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorsubscriptionpendingcmds_TSQL
- sp_replmonitorsubscriptionpendingcmds
helpviewer_keywords:
- sp_replmonitorsubscriptionpendingcmds
ms.assetid: df5b955a-feb0-4863-9b3b-7f71e9653b3d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b5c2737f891d100ac724f051f2d14c1c6a7e7692
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204360"
---
# <a name="sp_replmonitorsubscriptionpendingcmds-transact-sql"></a>sp_replmonitorsubscriptionpendingcmds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回有关对事务发布的订阅的等待命令数以及处理这些命令的粗略估计时间的信息。 此存储过程针对每个返回的订阅返回一行。 在分发服务器的分发数据库上执行此存储过程，用于监视复制。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replmonitorsubscriptionpendingcmds [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @subscriber = ] 'subscriber'  
        , [ @subscriber_db = ] 'subscriber_db'   
        , [ @subscription_type = ] subscription_type  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 已发布数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @subscriber = ] 'subscriber'` 订阅服务器的名称。 *订阅服务器* 的 **sysname**，无默认值。  
  
`[ @subscriber_db = ] 'subscriber_db'` 订阅数据库的名称。 *subscriber_db* **sysname**，无默认值。  
  
`[ @subscription_type = ] subscription_type` 如果订阅的类型为。 *publication_type* 是 **int**，没有默认值，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|推送订阅|  
|**1**|请求订阅|  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**pendingcmdcount**|**int**|等待订阅的命令数。|  
|**estimatedprocesstime**|**int**|将所有等待的命令传递到订阅服务器所需的估计秒数。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_replmonitorsubscriptionpendingcmds** 用于事务复制。  
  
## <a name="permissions"></a>权限  
 只有分发服务器上 **sysadmin** 固定服务器角色的成员或分发数据库中 **db_owner** 固定数据库角色的成员才能执行 **sp_replmonitorsubscriptionpendingcmds**。 使用分发数据库的发布的发布访问列表的成员可以执行 **sp_replmonitorsubscriptionpendingcmds** 以返回该发布的挂起命令。  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
