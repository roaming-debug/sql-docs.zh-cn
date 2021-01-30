---
description: sp_link_publication (Transact-SQL)
title: sp_link_publication (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_link_publication_TSQL
- sp_link_publication
helpviewer_keywords:
- sp_link_publication
ms.assetid: 1945ed24-f9f1-4af6-94ca-16d8e864706e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b6a6e0b057f0d01c838aa5ac7b57143221fec13c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210780"
---
# <a name="sp_link_publication-transact-sql"></a>sp_link_publication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  设置在连接到发布服务器时立即更新订阅的同步触发器所使用的配置和安全信息。 此存储过程在订阅服务器的订阅数据库中执行。  
  
> [!IMPORTANT]
>  使用远程分发服务器配置发布服务器时，为所有参数提供的值（包括 *job_login* 和 *job_password*）都会以纯文本方式发送到该分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
> 
> [!IMPORTANT]
>  在某些情况下，如果订阅服务器正在运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 1 或更高版本，并且发布服务器运行的是早期版本，则此存储过程可能会失败。 如果存储过程在上述情况下失败，请将发布服务器升级至 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 1 或更高版本。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_link_publication [ @publisher = ] 'publisher'   
        , [ @publisher_db = ] 'publisher_db'   
        , [ @publication = ] 'publication'   
        , [ @security_mode = ] security_mode  
    [ , [ @login = ] 'login' ]  
    [ , [ @password = ]'password' ]  
    [ , [ @distributor = ] 'distributor' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 要链接到的发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 要链接到的发布服务器数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 要链接到的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @security_mode = ] security_mode` 订阅服务器用于连接到远程发布服务器以立即更新的安全模式。 *security_mode* 为 **int**，可以是下列值之一。 [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
|值|说明|  
|-----------|-----------------|  
|**0**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]对此存储过程中指定的登录名使用身份验证作为 *登录名* 和 *密码*。<br /><br /> 注意：在以前版本的中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，此选项用于指定 (RPC) 的动态远程过程调用。|  
|**1**|使用在订阅服务器上进行更改的用户的安全上下文（[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证或 Windows 身份验证）。<br /><br /> 注意：此帐户还必须存在于具有足够权限的发布服务器上。 使用 Windows 身份验证时，必须支持安全策略帐户委托。|  
|**2**|使用使用 **sp_link_publication** 创建的现有用户定义的链接服务器登录名。|  
  
`[ @login = ] 'login'` 登录名。 login 的数据类型为 sysname，默认值为 NULL。 当 *security_mode* 为 **0** 时，必须指定此参数。  
  
`[ @password = ] 'password'` 密码。 *password* 的值为 **sysname**，默认值为 NULL。 当 *security_mode* 为 **0** 时，必须指定此参数。  
  
`[ @distributor = ] 'distributor'` 分发服务器的名称。 *分发服务器* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_link_publication** 由事务复制中的立即更新订阅使用。  
  
 **sp_link_publication** 可用于推送订阅和请求订阅。 在创建订阅之前或之后都可以调用此过程。 在 [MSsubscription_properties &#40;transact-sql&#41;](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) 系统表中插入或更新了一个条目。  
  
 对于推送订阅，可通过 [sp_subscription_cleanup &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md)来清理条目。 对于请求订阅，可以通过 [sp_droppullsubscription &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md) 或 [sp_subscription_cleanup &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md)来清理条目。 你还可以使用 NULL 密码调用 **sp_link_publication** 以清除 [MSsubscription_properties &#40;transact-sql&#41;](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) 系统表中的条目，以了解安全问题。  
  
 当立即更新订阅服务器连接到发布服务器时，它使用的默认模式不允许使用 Windows 身份验证进行连接。 若要用 Windows 身份验证模式进行连接，则必须设置到发布服务器的链接服务器，且立即更新订阅服务器在更新订阅服务器时应使用该连接。 这要求 **sp_link_publication** 与 *security_mode*  =  **2** 一起运行。 使用 Windows 身份验证时，必须支持安全策略帐户委托。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent_failover](../../relational-databases/replication/codesnippet/tsql/sp-link-publication-tran_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_link_publication** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [sp_droppullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)   
 [sp_subscription_cleanup &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
