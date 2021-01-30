---
description: sp_revoke_login_from_proxy (Transact-SQL)
title: sp_revoke_login_from_proxy (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_revoke_login_from_proxy_TSQL
- sp_revoke_login_from_proxy
dev_langs:
- TSQL
helpviewer_keywords:
- sp_revoke_login_from_proxy
ms.assetid: e4546c13-9fba-4bab-8b42-d6f18b33ec25
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 9c95315de974558eaf82878d087af09d2ac8e81b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183071"
---
# <a name="sp_revoke_login_from_proxy-transact-sql"></a>sp_revoke_login_from_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除对安全主体服务器的代理的访问权。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_revoke_login_from_proxy   
    [ @name = ] 'name' ,  
    [ @proxy_id = ] id ,  
    [ @proxy_name = ] 'proxy_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @name = ] 'name'`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]要删除其访问权限的登录名、服务器角色或 **msdb** 数据库角色的名称。 *name* 为 **nvarchar (256)** ，无默认值。  
  
`[ @proxy_id = ] id` 要删除其访问权限的代理的 id。 必须指定 *id* 或 *proxy_name* ，但不能同时指定两者。 *Id* 为 **int**，默认值为 NULL。  
  
`[ @proxy_name = ] 'proxy_name'` 要删除其访问权限的代理的名称。 必须指定 *id* 或 *proxy_name* ，但不能同时指定两者。 *Proxy_name* 的值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 引用此代理的登录名所拥有的作业将无法运行。  
  
## <a name="permissions"></a>权限  
 若要执行此存储过程，用户必须为 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 以下示例将撤消登录名 `terrid` 访问代理 `Catalog application proxy` 的访问权。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_revoke_login_from_proxy  
    @name = N'terrid',  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 代理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_help_proxy &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-proxy-transact-sql.md)  
  
  
