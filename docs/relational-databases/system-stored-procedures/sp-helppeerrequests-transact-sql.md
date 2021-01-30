---
description: sp_helppeerrequests (Transact-SQL)
title: sp_helppeerrequests (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helppeerrequests_TSQL
- sp_helppeerrequests
helpviewer_keywords:
- sp_helppeerrequests
ms.assetid: 37bd503e-46c4-47c6-996e-be7ffe636fe8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d728d168ec84d27cbe5c4316eec1e5e0c9f61221
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210862"
---
# <a name="sp_helppeerrequests-transact-sql"></a>sp_helppeerrequests (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回对等复制拓扑中参与者收到的所有状态请求的信息，这些请求是通过在拓扑中的任何已发布数据库上执行 [sp_requestpeerresponse &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md) 启动的。 此存储过程将对参与对等复制拓扑的发布服务器上的发布数据库执行。 有关详细信息，请参阅 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helppeerrequests [ @publication = ] 'publication'  
    [ , [ @description = ] 'description'  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发送了状态请求的对等拓扑中的发布名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @description = ] 'description'` 可用于标识各个状态请求的值，这使你能够根据在调用 [sp_requestpeerresponse &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md)时提供的用户定义的信息筛选返回的响应。 *描述* 为 **nvarchar (4000)**，默认值为 **%** 。 默认情况下，返回发布的所有状态请求。 此参数用于仅返回其说明与 *说明* 中提供的值匹配的状态请求，其中使用 [类似的 &#40;transact-sql&#41;](../../t-sql/language-elements/like-transact-sql.md) 子句来匹配字符串。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|标识一个请求。|  
|**发布**|**sysname**|为之发送状态请求的发布的名称。|  
|**sent_date**|**datetime**|发送状态请求的日期和时间。|  
|description|**nvarchar(4000)**|可用于标识各个状态请求的用户定义的信息。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helppeerrequests** 用于对等事务复制。  
  
 还原在对等拓扑中发布的数据库时，将使用 **sp_helppeerrequests** 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_helppeerrequests**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_deletepeerrequesthistory &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-deletepeerrequesthistory-transact-sql.md)   
 [sp_helppeerresponses &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md)  
  
  
