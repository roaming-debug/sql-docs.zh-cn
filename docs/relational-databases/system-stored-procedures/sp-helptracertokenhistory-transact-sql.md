---
description: sp_helptracertokenhistory (Transact-SQL)
title: sp_helptracertokenhistory (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helptracertokenhistory_TSQL
- sp_helptracertokenhistory
helpviewer_keywords:
- sp_helptracertokenhistory
ms.assetid: 96910d1c-be76-43eb-9c93-4477e6761749
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f701a1681281bfb63cf6c39ba42869ac83df204e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192876"
---
# <a name="sp_helptracertokenhistory-transact-sql"></a>sp_helptracertokenhistory (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回指定跟踪令牌的详细滞后时间信息，为每个订阅服务器返回一行。 此存储过程在发布服务器上的发布数据库中执行，或者在分发服务器上的分发数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helptracertokenhistory [ @publication = ] 'publication'   
        , [ @tracer_id = ] tracer_id  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 在其中插入跟踪令牌的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @tracer_id = ] tracer_id` 为其返回历史记录信息的 [MStracer_tokens &#40;transact-sql&#41;](../../relational-databases/system-tables/mstracer-tokens-transact-sql.md) 表中的跟踪令牌的 ID。 *tracer_id* 为 **int**，没有默认值。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]
>  只应为非发布服务器指定此参数 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @publisher_db = ] 'publisher_db'` 发布数据库的名称。 *publisher_db* 的值为 **sysname**，默认值为 NULL。 如果在发布服务器上执行该存储过程，将忽略此参数。  
  
## <a name="result-set"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**distributor_latency**|**bigint**|在发布服务器上提交的跟踪令牌记录与在分发服务器上提交的记录之间的秒数。|  
|**订阅服务器**|**sysname**|收到跟踪令牌的订阅服务器的名称。|  
|**subscriber_db**|**sysname**|在其中插入跟踪令牌记录的订阅数据库的名称。|  
|**subscriber_latency**|**bigint**|在分发服务器上提交的跟踪令牌记录和在订阅服务器上提交的记录间隔的秒数。|  
|**overall_latency**|**bigint**|在发布服务器上提交的跟踪令牌记录和在订阅服务器上提交的令牌记录间隔的秒数。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helptracertokenhistory** 用于事务复制。  
  
 执行 [sp_helptracertokens &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) 获取发布的跟踪令牌列表。  
  
 结果集中的 NULL 值意味着无法计算滞后时间统计信息。 这是因为分发服务器或其中一个订阅服务器尚未接收到跟踪令牌。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_tracertokens](../../relational-databases/replication/codesnippet/tsql/sp-helptracertokenhistor_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、发布数据库中 **db_owner** 固定数据库角色的成员或分发数据库中的 **db_owner** 固定数据库角色或 **replmonitor** 角色才能 **sp_helptracertokenhistory** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [为事务复制测量滞后时间并验证连接](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)   
 [sp_deletetracertokenhistory &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md)  
  
  
