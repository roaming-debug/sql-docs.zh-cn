---
description: sp_subscription_cleanup (Transact-SQL)
title: sp_subscription_cleanup (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_subscription_cleanup_TSQL
- sp_subscription_cleanup
helpviewer_keywords:
- sp_subscription_cleanup
ms.assetid: bdc8aaa0-ff2d-40c2-84b2-4ba513ced279
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 79e44839da9a2df185a125007e7491b6d269d02e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186214"
---
# <a name="sp_subscription_cleanup-transact-sql"></a>sp_subscription_cleanup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在从订阅服务器中删除订阅时同时删除元数据。 对于同步事务订阅，还包括立即更新触发器。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_subscription_cleanup [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
    [ , [ @publication = ] 'publication']  
    [ , [ @reserved = ] 'reserved']  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 NULL。 如果为 NULL，则将删除使用发布数据库中的共享代理发布的订阅。  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_subscription_cleanup** 用于事务复制和快照复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_subscription_cleanup**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_expired_subscription_cleanup &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-expired-subscription-cleanup-transact-sql.md)   
 [sp_mergesubscription_cleanup &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
