---
description: 查询通知-sys.dm_qn_subscriptions
title: sys.dm_qn_subscriptions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_qn_subscriptions
- dm_qn_subscriptions_TSQL
- sys.dm_qn_subscriptions
- sys.dm_qn_subscriptions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_qn_subscriptions dynamic management view
ms.assetid: a3040ce6-f5af-48fc-8835-c418912f830c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 62953d7f80a8d0fa327f37eadcc7a8dafd6fa734
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100033"
---
# <a name="query-notifications---sysdm_qn_subscriptions"></a>查询通知-sys.dm_qn_subscriptions
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关服务器中的活动查询通知订阅的信息。 可以使用此视图检查服务器或指定数据库中的活动订阅，或者检查指定服务器主体。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|订阅的 ID。|  
|database_id|**int**|执行通知查询所在数据库的 ID。 该数据库存储该订阅的相关信息。|  
|**sid**|**varbinary(85)**|创建并拥有该订阅的服务器主体的安全 ID。|  
|object_id|**int**|存储有关订阅参数信息的内部表的 ID。|  
|**created**|**datetime**|创建订阅的日期和时间。|  
|**timeout**|**int**|订阅超时（以秒为单位）。 在经过这段时间后，通知将标记为激发。<br /><br /> 注意：实际触发时间可能大于指定的超时时间。但是，如果在指定的超时时间之后但在激发订阅之前发生了无效的更改，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可确保在发生更改时进行触发。|  
|**status**|**int**|指示订阅的状态。 有关代码列表，请参阅备注下的表。|  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
|From|目标|开|类型|  
|----------|--------|--------|----------|  
|**sys.dm_qn_subscriptions**|**sys.databases**|database_id|多对一|  
|**sys.dm_qn_subscriptions**|**sys.internal_tables**|object_id|多对一|  
  
## <a name="remarks"></a>备注  
 状态代码为 0 指示未确定的状态。  
  
 下面的状态代码指示由于更改而激发了订阅：  
  
|代码|次要状态|信息|  
|----------|------------------|----------|  
|65798|因为更改数据而激发订阅|由插入触发的订阅|  
|65799|因为更改数据而激发订阅|删除|  
|65800|因为更改数据而激发订阅|更新|  
|65801|因为更改数据而激发订阅|合并|  
|65802|因为更改数据而激发订阅|截断表|  
|66048|因为超时而激发订阅|未确定的信息模式|  
|66315|因为更改对象而激发订阅|删除了对象或用户|  
|66316|因为更改对象而激发订阅|修改了对象|  
|66565|因为分离或删除了数据库而激发订阅|重新启动了服务器或数据库|  
|66571|因为分离或删除了数据库而激发订阅|删除了对象或用户|  
|66572|因为分离或删除了数据库而激发订阅|修改了对象|  
|67341|由于服务器上缺少 od 资源而激发订阅|由于服务器上缺少 od 资源而激发订阅|  
  
 下面的状态代码指示无法创建订阅：  
  
|代码|次要状态|信息|  
|----------|------------------|----------|  
|132609|因为不支持此语句而导致订阅创建失败|查询过于复杂|  
|132610|因为不支持此语句而导致订阅创建失败|用于订阅的语句无效|  
|132611|因为不支持此语句而导致订阅创建失败|用于订阅的设置选项无效|  
|132612|因为不支持此语句而导致订阅创建失败|隔离级别无效|  
|132622|因为不支持此语句而导致订阅创建失败|供内部使用|  
|132623|因为不支持此语句而导致订阅创建失败|超出每个表的模板限制|  
  
 下面的状态代码供内部使用，它们归类为检查终止和初始化模式：  
  
|代码|次要状态|信息|  
|----------|------------------|----------|  
|198656|供内部使用：检查终止和初始化模式|未确定的信息模式|  
|198928|订阅已损坏|因为数据库分离而激发订阅|  
|198929|订阅已损坏|因为删除了用户而激发订阅|  
|198930|订阅已损坏|因为重新订阅而删除了订阅|  
|198931|订阅已损坏|订阅已终止|  
|199168|订阅处于活动状态。|未确定的信息模式|  
|199424|该订阅已初始化，但尚未处于活动状态|未确定的信息模式|  
  
## <a name="permissions"></a>权限  
 需要针对服务器的 VIEW SERVER STATE 权限。  
  
> [!NOTE]  
>  如果用户不具备 VIEW SERVER STATE 权限，该视图将返回当前用户所拥有的订阅的有关信息。  
  
## <a name="examples"></a>示例  
  
### <a name="a-return-active-query-notification-subscriptions-for-the-current-user"></a>A. 返回当前用户的活动查询通知订阅  
 以下示例返回当前用户的活动查询通知订阅。 如果用户拥有 VIEW SERVER STATE 权限，则将返回服务器中的所有活动订阅。  
  
```  
SELECT id, database_id, sid, object_id, created, timeout, status  
FROM sys.dm_qn_subscriptions;  
GO  
```  
  
### <a name="b-returning-active-query-notification-subscriptions-for-a-specified-user"></a>B. 返回指定用户的活动查询通知订阅  
 以下示例返回通过登录 `Ruth0` 进行的活动查询通知订阅。  
  
```  
SELECT id, database_id, sid, object_id, created, timeout, status  
FROM sys.dm_qn_subscriptions  
WHERE sid = SUSER_SID('Ruth0');  
GO  
```  
  
### <a name="c-returning-internal-table-metadata-for-query-notification-subscriptions"></a>C. 返回查询通知订阅的内部表元数据  
 以下示例返回查询通知订阅的内部表元数据。  
  
```  
SELECT qn.id AS query_subscription_id  
    ,it.name AS internal_table_name  
    ,it.object_id AS internal_table_id  
FROM sys.internal_tables AS it  
JOIN sys.dm_qn_subscriptions AS qn ON it.object_id = qn.object_id  
WHERE it.internal_type_desc = 'QUERY_NOTIFICATION';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与查询通知相关的动态管理视图 &#40;Transact-sql&#41;](./system-dynamic-management-views.md)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION (Transact-SQL)](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)  
  
