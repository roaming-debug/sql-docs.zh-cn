---
description: sp_replqueuemonitor (Transact-SQL)
title: sp_replqueuemonitor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replqueuemonitor
- sp_replqueuemonitor_TSQL
helpviewer_keywords:
- sp_replqueuemonitor
ms.assetid: 6909a3f1-43a2-4df5-a6a5-9e6f347ac841
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8fa6635b4f31692e5f45e848043e44496e2d702f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211663"
---
# <a name="sp_replqueuemonitor-transact-sql"></a>sp_replqueuemonitor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  列出对 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] 指定发布的排队更新订阅的队列或消息队列的队列消息。 如果使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 队列，则将在订阅服务器上的订阅数据库上执行此存储过程。 如果使用消息队列，则将在分发服务器上的分发数据库上执行此存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replqueuemonitor [ @publisher = ] 'publisher'  
    [ , [ @publisherdb = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @tranid = ] 'tranid' ]  
    [ , [ @queuetype = ] 'queuetype' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。 必须将该服务器配置为用于发布。 NULL 表示所有发布服务器。  
  
`[ @publisherdb = ] 'publisher_db' ]` 发布数据库的名称。 *publisher_db* 的默认值为 **sysname**，默认值为 NULL。 NULL 表示所有发布数据库。  
  
`[ @publication = ] 'publication' ]` 发布的名称。 *发布* 为 **sysname**，默认值为 NULL。 NULL 表示所有发布。   
  
`[ @tranid = ] 'tranid' ]` 事务 ID。 *tranid* 的值为 **sysname**，默认值为 NULL。 NULL 表示所有事务。  
  
`[ @queuetype = ] 'queuetype' ]` 存储事务的队列类型。 *queuetype* 的数据值为 **tinyint** ，默认值为 **0**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|所有队列类型|  
|**1**|消息队列|  
|**2**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_replqueuemonitor** 用于快照复制或带有排队更新订阅的事务复制。 不显示不包含 SQL 命令的队列消息，也不显示作为跨越式 SQL 命令的一部分的队列消息。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_replqueuemonitor**。  
  
## <a name="see-also"></a>另请参阅  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
