---
title: sp_droppublication (Transact-sql) |Microsoft Docs
description: 删除发布及其关联的快照代理。 此存储过程在发布服务器的发布数据库上运行。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_droppublication_TSQL
- sp_droppublication
helpviewer_keywords:
- sp_droppublication
ms.assetid: b52b37e6-4fec-40cf-abba-7dce4ff395fd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fcad06d60e5770490d8f4a619d7483c8b29be2d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187047"
---
# <a name="sp_droppublication-transact-sql"></a>sp_droppublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  删除发布及其关联的快照代理。 删除发布之前必须删除全部的订阅。 发布中的项目将自动删除。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_droppublication [ @publication= ] 'publication'   
    [ , [ @ignore_distributor = ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要删除的发布的名称。 *发布* 为 **sysname**，无默认值。 如果指定 **all** ，则将从发布数据库中删除所有发布，但具有订阅的发布除外。  
  
`[ @ignore_distributor = ] ignore_distributor` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_droppublication** 用于快照复制和事务复制。  
  
 **sp_droppublication** 以递归方式删除与发布关联的所有项目，然后删除该发布本身。 如果发布包含一个或更多对它的订阅，则不能删除。 有关如何删除订阅的信息，请参阅 [删除推送订阅](../../relational-databases/replication/delete-a-push-subscription.md) 和 [删除请求订阅](../../relational-databases/replication/delete-a-pull-subscription.md)。  
  
 执行删除发布的 **sp_droppublication** 不会删除发布数据库中的已发布对象，也不会删除订阅数据库中的相应对象。 \<object>如有必要，请使用 DROP 手动删除这些对象。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_droppublication** 执行。  
  
## <a name="examples"></a>示例  
 [!code-sql[HowTo#sp_droppublication](../../relational-databases/replication/codesnippet/tsql/sp-droppublication-trans_1.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [删除发布](../../relational-databases/replication/publish/delete-a-publication.md)   
 [sp_addpublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
