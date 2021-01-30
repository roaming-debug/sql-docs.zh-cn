---
description: sp_addqreader_agent (Transact-SQL)
title: sp_addqreader_agent (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addqreader_agent_TSQL
- sp_addqreader_agent
helpviewer_keywords:
- sp_addqreader_agent
ms.assetid: dc9f591a-e67e-4ba8-bf47-defd5eda0822
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f148297e264a0ca097671234655f9854b7e79aba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206643"
---
# <a name="sp_addqreader_agent-transact-sql"></a>sp_addqreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  为给定分发服务器添加队列读取器代理。 此存储过程在分发服务器上针对分发数据库执行，或在发布服务器上针对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addqreader_agent [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
    [ , [ @job_name = ] 'job_name'  
    [ , [ @frompublisher = ] frompublisher   
```  
  
## <a name="arguments"></a>参数  
`[ @job_login = ] 'job_login'` 用于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 运行代理的 Windows 帐户的登录名。 *job_login* 为 **nvarchar (257)**，无默认值。 此 Windows 帐户总是用于与分发服务器建立代理连接。  
  
`[ @job_password = ] 'job_password'` 运行代理所用的 Windows 帐户的密码。 *job_password* **sysname**，无默认值。  
  
> [!IMPORTANT]  
>  请不要将身份验证信息存储在脚本文件中。 为保证安全性，应当在运行时再提供登录名和密码。  
  
`[ @job_name = ] 'job_name'` 现有代理作业的名称。 *job_name* 的值为 **sysname**，默认值为 NULL。 只有在使用现有作业而不是新创建作业（默认值）创建代理时，才需要指定此参数。  
  
`[ @frompublisher = ] frompublisher` 指定是否在发布服务器上执行该过程。 *frompublisher* 的值为 bit，默认值为 **0**。 值为 **1** 表示在发布服务器上对发布数据库执行此过程。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_addqreader_agent** 用于事务复制。  
  
 在 [sp_adddistributiondb](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)之后但 [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)之前，必须在支持排队更新的分发服务器上至少执行一次 **sp_addqreader_agent** 。  
  
 当您执行 [sp_dropdistributiondb](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md)时，队列读取器代理作业将被删除。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_addqreader_agent** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [为事务发布启用更新订阅](../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)   
 [升级复制脚本（复制 Transact-SQL 编程）](../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md)   
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_changeqreader_agent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changeqreader-agent-transact-sql.md)   
 [sp_helpqreader_agent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpqreader-agent-transact-sql.md)  
  
  
