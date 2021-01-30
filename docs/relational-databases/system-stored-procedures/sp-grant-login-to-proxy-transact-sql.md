---
description: sp_grant_login_to_proxy (Transact-SQL)
title: sp_grant_login_to_proxy (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_grant_login_to_proxy
- sp_grant_login_to_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grant_login_to_proxy
ms.assetid: 90e1a6d5-a692-4462-a163-4b0709d83150
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: c59f8b19e63f491757386ba5562ea9e0106e92a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159811"
---
# <a name="sp_grant_login_to_proxy-transact-sql"></a>sp_grant_login_to_proxy (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  授予安全主体数据库访问代理的权限。  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_grant_login_to_proxy   
     { [ @login_name = ] 'login_name'   
     | [ @fixed_server_role = ] 'fixed_server_role'   
     | [ @msdb_role = ] 'msdb_role' } ,   
     { [ @proxy_id = ] id | [ @proxy_name = ] 'proxy_name' }  
```  
  
## <a name="arguments"></a>参数  
`[ @login_name = ] 'login_name'` 要向其授予访问权限的登录名。 *Login_name* 为 **nvarchar (256)**，默认值为 NULL。 必须指定 **\@ login_name**、 **\@ fixed_server_role** 或 **\@ msdb_role** 之一，否则存储过程将失败。  
  
`[ @fixed_server_role = ] 'fixed_server_role'` 要向其授予访问权限的固定服务器角色。 *Fixed_server_role* 为 **nvarchar (256)**，默认值为 NULL。 必须指定 **\@ login_name**、 **\@ fixed_server_role** 或 **\@ msdb_role** 之一，否则存储过程将失败。  
  
`[ @msdb_role = ] 'msdb_role'` 要向其授予访问权限的 **msdb** 数据库中的数据库角色。 *Msdb_role* 为 **nvarchar (256)**，默认值为 NULL。 必须指定 **\@ login_name**、 **\@ fixed_server_role** 或 **\@ msdb_role** 之一，否则存储过程将失败。  
  
`[ @proxy_id = ] id` 要为其授予访问权限的代理的标识符。 *Id* 为 **int**，默认值为 NULL。 必须指定 **\@ proxy_id** 或 **\@ proxy_name** 之一，否则存储过程将失败。  
  
`[ @proxy_name = ] 'proxy_name'` 要为其授予访问权限的代理的名称。 *Proxy_name* 为 **nvarchar (256)**，默认值为 NULL。 必须指定 **\@ proxy_id** 或 **\@ proxy_name** 之一，否则存储过程将失败。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 必须从 **msdb** 数据库运行 **sp_grant_login_to_proxy** 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_grant_login_to_proxy** 执行。  
  
## <a name="examples"></a>示例  
 以下示例允许登录名 `adventure-works\terrid` 使用代理 `Catalog application proxy` 。  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_grant_login_to_proxy  
    @login_name = N'adventure-works\terrid',  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_add_proxy &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
