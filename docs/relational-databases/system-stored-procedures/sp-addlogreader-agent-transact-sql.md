---
description: sp_addlogreader_agent (Transact-SQL)
title: sp_addlogreader_agent (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addlogreader_agent
- sp_addlogreader_agent_TSQL
helpviewer_keywords:
- sp_addlogreader_agent
ms.assetid: d83096b9-96ee-4789-bde0-940d4765b9ed
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: a511c4f47f84a8f083dbab86bf1d851762142d90
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202365"
---
# <a name="sp_addlogreader_agent-transact-sql"></a>sp_addlogreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  为给定数据库添加日志读取器代理。 此存储过程在发布服务器上对发布数据库执行。  
  
> [!IMPORTANT]  
>  使用远程分发服务器配置发布服务器时，为所有参数提供的值（包括 *job_login* 和 *job_password*）都会以纯文本方式发送到该分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addlogreader_agent [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @publisher_security_mode = ] publisher_security_mode ]  
    [ , [ @publisher_login = ] 'publisher_login' ]  
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @job_login = ] 'job_login'` 用于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 运行代理的 Windows 帐户的登录名。 *job_login* 为 **nvarchar (257)**，默认值为 NULL。 此 Windows 帐户总是用于与分发服务器建立代理连接。  
  
> [!NOTE]
>  对于非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器，此登录名必须与[&#40;transact-sql&#41;sp_adddistpublisher](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)中指定的相同。  
  
`[ @job_password = ] 'job_password'` 运行代理所用的 Windows 帐户的密码。 *job_password* 的值为 **sysname**，默认值为 NULL。  
  
> [!IMPORTANT]  
>  请不要将身份验证信息存储在脚本文件中。 为保证安全性，应当在运行时再提供登录名和密码。  
  
`[ @job_name = ] 'job_name'` 现有代理作业的名称。 *job_name* 的值为 **sysname**，默认值为 NULL。 只有在使用现有作业而不是使用新创建的作业（默认值）来启动代理时，才需要指定此参数。  
  
`[ @publisher_security_mode = ] publisher_security_mode` 连接到发布服务器时代理所使用的安全模式。 *publisher_security_mode* 为 **smallint**，默认值为 **1**。 **0** 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证， **1** 指定 Windows 身份验证。 对于非发布服务器，必须指定 **0** 值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @publisher_login = ] 'publisher_login'` 连接到发布服务器时使用的登录名。 *publisher_login* 的默认值为 **sysname**，默认值为 NULL。 当 *publisher_security_mode* 为 **0** 时，必须指定 *publisher_login* 。 如果 *publisher_login* 为 NULL 且 *publisher_security_mode* 为 **1**，则在连接到发布服务器时将使用 *job_login* 中指定的 Windows 帐户。  
  
`[ @publisher_password = ] 'publisher_password'` 连接到发布服务器时使用的密码。 *publisher_password* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!IMPORTANT]  
>  请不要将身份验证信息存储在脚本文件中。 为保证安全性，应当在运行时再提供登录名和密码。  
  
`[ @publisher = ] 'publisher'` 非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器，无需指定此参数。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addlogreader_agent** 用于事务复制。  
  
 如果在创建使用数据库的发布之前将已启用复制的数据库升级到此版本的，则必须执行 **sp_addlogreader_agent** 才能添加日志读取器代理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addlogreader_agent**。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addlogreader-agent-tr_1.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [sp_addpublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changelogreader_agent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
