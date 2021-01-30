---
description: sp_grant_proxy_to_subsystem (Transact-SQL)
title: sp_grant_proxy_to_subsystem (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_grant_login_to_subsystem_TSQL
- sp_grant_login_to_subsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grant_proxy_to_subsystem
ms.assetid: 866aaa27-a1e0-453a-9b1b-af39431ad9c2
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 41cbd629572229b4a21c17aca989bc3ab25d5413
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171744"
---
# <a name="sp_grant_proxy_to_subsystem-transact-sql"></a>sp_grant_proxy_to_subsystem (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  授权代理访问子系统。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_grant_proxy_to_subsystem  
     { [ @proxy_id = ] proxy_id | [ @proxy_name = ] 'proxy_name' },  
     { [ @subsystem_id = ] subsystem_id | [ @subsystem_name = ] 'subsystem_name' }  
```  
  
## <a name="arguments"></a>参数  
`[ @proxy_id = ] id` 要为其授予访问权限的代理的代理标识号。 *Proxy_id* 的值为 **int**，默认值为 NULL。 必须指定 *proxy_id* 或 *proxy_name* ，但不能同时指定两者。  
  
`[ @proxy_name = ] 'proxy_name'` 要为其授予访问权限的代理的名称。 *Proxy_name* 的值为 **sysname**，默认值为 NULL。 必须指定 *proxy_id* 或 *proxy_name* ，但不能同时指定两者。  
  
`[ @subsystem_id = ] id` 要向其授予访问权限的子系统的 id 号。 *Subsystem_id* 的值为 **int**，默认值为 NULL。 必须指定 *subsystem_id* 或 *subsystem_name* ，但不能同时指定两者。 下表列出了每个子系统的值。  
  
|值|描述|  
|-----------|-----------------|  
|**2**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] ActiveX 脚本<br /><br /> **\* \* 重要 \* 提示 \*** 在的未来版本中，将从代理中删除 ActiveX 脚本编写子系统 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。|  
|**3**|操作系统 (**CmdExec**)|  
|**4**|复制快照代理|  
|**5**|复制日志读取器代理|  
|**6**|复制分发代理|  
|**7**|Replication Merge Agent|  
|**8**|复制队列读取器代理|  
|**9**|Analysis Services 查询|  
|**10**|Analysis Services 命令|  
|**11**|[!INCLUDE[ssIS](../../includes/ssis-md.md)] 包执行|  
|**12**|PowerShell 脚本|  
| &nbsp; | &nbsp; |
  
`[ @subsystem_name = ] 'subsystem_name'` 要向其授予访问权限的子系统的名称。 **Subsystem_name** 的值为 **sysname**，默认值为 NULL。 必须指定 *subsystem_id* 或 *subsystem_name* ，但不能同时指定两者。 下表列出了每个子系统的值。  
  
|值|说明|  
|-----------|-----------------|  
|**ActiveScripting**|ActiveX 脚本|  
|**CmdExec**|操作系统 (**CmdExec**)|  
|**快照**|复制快照代理|  
|**LogReader**|复制日志读取器代理|  
|**Distribution**|复制分发代理|  
|**合并**|Replication Merge Agent|  
|**QueueReader**|复制队列读取器代理|  
|**ANALYSISQUERY**|Analysis Services 查询|  
|**ANALYSISCOMMAND**|Analysis Services 命令|  
|**Dts**|SSIS 包执行|  
|**PowerShell**|PowerShell 脚本|  
| &nbsp; | &nbsp; |
  
## <a name="remarks"></a>备注  
 授权代理访问子系统将不更改代理中指定的主体服务器的权限。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_grant_proxy_to_subsystem** 执行。  
  
## <a name="examples"></a>示例  
  
### <a name="a-granting-access-to-a-subsystem-by-id"></a>A. 按 ID 授权访问子系统  
 以下示例授权代理 `Catalog application proxy` 访问 ActiveX Scripting 子系统。  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_grant_proxy_to_subsystem  
    @proxy_name = 'Catalog application proxy',  
    @subsystem_id = 2;  
GO  
```  
  
### <a name="b-granting-access-to-a-subsystem-by-name"></a>B. 按名称授权访问子系统。  
 以下示例授权代理 `Catalog application proxy` 访问 SSIS 包执行子系统。  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_grant_proxy_to_subsystem  
    @proxy_name = N'Catalog application proxy',  
    @subsystem_name = N'Dts' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_revoke_proxy_from_subsystem &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-revoke-proxy-from-subsystem-transact-sql.md)   
 [sp_add_proxy &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_delete_proxy &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md)   
 [sp_update_proxy &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-proxy-transact-sql.md)  
  
  
