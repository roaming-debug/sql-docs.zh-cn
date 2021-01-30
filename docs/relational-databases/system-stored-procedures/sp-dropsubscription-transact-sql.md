---
title: sp_dropsubscription (Transact-sql) |Microsoft Docs
description: 删除对发布服务器上的项目、发布或订阅的订阅。 此存储过程在发布服务器的发布数据库上运行。
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropsubscription
- sp_dropsubscription_TSQL
helpviewer_keywords:
- sp_dropsubscription
ms.assetid: 7551f345-5510-4684-ab53-f9057249d13a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3097c61204e0a9ae310af87fb83901a252c50e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157951"
---
# <a name="sp_dropsubscription-transact-sql"></a>sp_dropsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  删除对发布服务器上的特殊项目、发布或订阅集的订阅。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dropsubscription [ [ @publication= ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
        , [ @subscriber= ] 'subscriber'  
    [ , [ @destination_db= ] 'destination_db' ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @reserved= ] 'reserved' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 关联发布的名称。 *发布* 为 **sysname**，默认值为 NULL。 如果为 **all**，则取消对指定订阅服务器的所有发布的所有订阅。 *发布* 是必需的参数。  
  
`[ @article = ] 'article'` 项目的名称。 *项目* 的值为 **sysname**，默认值为 NULL。 如果为 **all**，则删除针对每个指定发布和订阅服务器的所有项目的订阅。 对于允许立即更新的发布，请使用 **all** 。  
  
`[ @subscriber = ] 'subscriber'` 将删除其订阅的订阅服务器的名称。 *订阅服务器* 的 **sysname**，无默认值。 如果为 **all**，则删除所有订阅服务器的所有订阅。  
  
`[ @destination_db = ] 'destination_db'` 目标数据库的名称。 *destination_db* 的默认值为 **sysname**，默认值为 NULL。 如果为 NULL，则删除该订阅服务器中的所有订阅。  
  
`[ @ignore_distributor = ] ignore_distributor`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @reserved = ] 'reserved'`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_dropsubscription** 用于快照复制和事务复制。  
  
 如果删除即时同步发布中的一个项目上的订阅，则不能再对其进行重新添加，除非是删除发布中所有项目上的订阅并同时对它们进行重新添加。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_droptransubscription](../../relational-databases/replication/codesnippet/tsql/sp-dropsubscription-tran_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、 **db_owner** 固定数据库角色的成员或创建订阅的用户才能执行 **sp_dropsubscription**。  
  
## <a name="see-also"></a>另请参阅  
 [删除推送订阅](../../relational-databases/replication/delete-a-push-subscription.md)   
 [sp_addsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubstatus &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
