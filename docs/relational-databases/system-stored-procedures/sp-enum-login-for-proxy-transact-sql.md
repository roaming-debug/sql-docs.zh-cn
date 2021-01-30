---
description: sp_enum_login_for_proxy (Transact-SQL)
title: sp_enum_login_for_proxy (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_login_for_proxy_TSQL
- sp_enum_login_for_proxy
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_login_for_proxy
ms.assetid: 62a75019-248a-44c8-a5cc-c79f55ea3acf
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: b51d02a66833df04a82a970319bab4e62f462a33
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186968"
---
# <a name="sp_enum_login_for_proxy-transact-sql"></a>sp_enum_login_for_proxy (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  列出安全主体服务器和代理服务器之间的关联。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_enum_login_for_proxy  
    [ @name = ] 'name'  
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @name = ] 'name'`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]要列出其代理的主体、登录名、服务器角色或 **msdb** 数据库角色的名称。 名称为 **nvarchar (256)**，默认值为 NULL。  
  
`[ @proxy_id = ] id` 要列出其信息的代理的代理标识号。 *Proxy_id* 的值为 **int**，默认值为 NULL。 可以指定 *id* 或 *proxy_name* 。  
  
`[ @proxy_name = ] 'proxy_name'` 要列出其信息的代理的名称。 *Proxy_name* 的值为 **sysname**，默认值为 NULL。 可以指定 *id* 或 *proxy_name* 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|代理服务器标识号。|  
|**proxy_name**|**sysname**|代理服务器的名称。|  
|name|**sysname**|关联的安全主体服务器的名称。|  
|**flag**|**int**|安全主体服务器的类型。<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录<br /><br /> **1** = 固定系统角色<br /><br /> **2** = **msdb** 中的数据库角色|  
| &nbsp; | &nbsp; | &nbsp; |
  
## <a name="remarks"></a>备注  
 如果未提供任何参数， **sp_enum_login_for_proxy** 会列出有关每个代理的实例中所有登录名的信息。  
  
 当提供代理 id 或代理名称时， **sp_enum_login_for_proxy** 会列出有权访问代理的登录名。 如果提供了登录名， **sp_enum_login_for_proxy** 会列出登录名有权访问的代理。  
  
 当同时提供代理信息和登录名时，如果指定的登录有权访问指定的代理，则结果集将返回一行。  
  
 此存储过程位于 **msdb** 中。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-all-associations"></a>A. 列出所有关联  
 以下示例列出了在当前实例的登录和代理之间建立的所有权限。  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_login_for_proxy ;  
GO  
```  
  
### <a name="b-listing-proxies-for-a-specific-login"></a>B. 列出特定登录的代理  
 以下示例列出了登录 `terrid` 有权访问的代理。  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_login_for_proxy  
    @name = 'terrid' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_help_proxy &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-proxy-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
