---
description: sp_dropmergesubscription (Transact-SQL)
title: sp_dropmergesubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergesubscription_TSQL
- sp_dropmergesubscription
helpviewer_keywords:
- sp_dropmergesubscription
ms.assetid: 34244ae6-bd98-4a6a-bbd3-85f50edfcdc0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 29cf6541b12f49194c1d1a6562d3fb80975a5591
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204800"
---
# <a name="sp_dropmergesubscription-transact-sql"></a>sp_dropmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除对合并发布的订阅及其关联的合并代理。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dropmergesubscription [ [ @publication= ] 'publication' ]   
    [ , [ @subscriber= ] 'subscriber'    
    [ , [ @subscriber_db= ] 'subscriber_db' ]   
    [ , [ @subscription_type= ] 'subscription_type' ]   
    [ , [ @ignore_distributor = ] ignore_distributor ]   
    [ , [ @reserved = ] reserved ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布名称。 *发布* 为 **sysname**，默认值为 NULL。 该发布必须已经存在，并符合标识符的相关规则。  
  
`[ @subscriber = ] 'subscriber'` 订阅服务器的名称。 *订阅服务器* 的值为 **sysname**，默认值为 NULL。  
  
`[ @subscriber_db = ] 'subscriber_db'` 订阅数据库的名称。 *subscription_database* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @subscription_type = ] 'subscription_type'` 订阅的类型。 *subscription_type* 为 **nvarchar (15)**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**一切**|推送订阅、请求订阅和匿名订阅|  
|**匿名**|匿名订阅。|  
|**push**|推送订阅。|  
|**请求**|请求订阅。|  
|**(默认** 值) |推送订阅和请求订阅。|  
  
`[ @ignore_distributor = ] ignore_distributor` 指示是否在未连接到分发服务器的情况下执行此存储过程。 *ignore_distributor* 为 **bit**，默认值为 **0**。 该参数可用于删除订阅，但不清除分发服务器上的任务。 在必须重新安装分发服务器的情况下，该参数也很有用。  
  
`[ @reserved = ] reserved` 保留供将来使用。 *reserved* 为 **bit**，默认值为 **0**。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_dropmergesubscription** 用于合并复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_dropmergesubscription](../../relational-databases/replication/codesnippet/tsql/sp-dropmergesubscription_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_dropmergesubscription**。  
  
## <a name="see-also"></a>另请参阅  
 [删除推送订阅](../../relational-databases/replication/delete-a-push-subscription.md)   
 [删除请求订阅](../../relational-databases/replication/delete-a-pull-subscription.md)   
 [sp_addmergesubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_changemergesubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)  
  
  
