---
description: sp_revoke_proxy_from_subsystem (Transact-SQL)
title: sp_revoke_proxy_from_subsystem (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_revoke_login_from_subsystem
- sp_revoke_login_from_subsystem_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_revoke_proxy_from_subsystem
ms.assetid: b87bc8ba-3ea8-4aed-b54b-32c3d82d9d2a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 9457a1d9dd0d9fd01a7418cd56e772902d593b6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211853"
---
# <a name="sp_revoke_proxy_from_subsystem-transact-sql"></a>sp_revoke_proxy_from_subsystem (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  撤消代理对子系统的访问权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_revoke_proxy_from_subsystem   
    [ @proxy_id = ] proxy_id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @subsystem_id = ] subsystem_id,  
    [ @subsystem_name = ] 'subsystem_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @proxy_id = ] id` 要从中撤消访问权限的代理的代理标识号。 *Proxy_id* 的值为 **int**，默认值为 NULL。 必须指定 *proxy_id* 或 *proxy_name* ，但不能同时指定两者。  
  
`[ @proxy_name = ] 'proxy_name'` 要从中撤消访问权限的代理的名称。 *Proxy_name* 的值为 **sysname**，默认值为 NULL。 必须指定 *proxy_id* 或 *proxy_name* ，但不能同时指定两者。  
  
`[ @subsystem_id = ] id` 要撤消对其访问权限的子系统的 id 号。 *Subsystem_id* 的值为 **int**，默认值为 NULL。 必须指定 *subsystem_id* 或 *subsystem_name* ，但不能同时指定两者。 下表列出了每个子系统的值。  
  
|值|描述|  
|-----------|-----------------|  
|**2**|ActiveX 脚本<br /><br /> **\* \* 重要 \* 提示 \*** 在的未来版本中，将从代理中删除 ActiveX 脚本编写子系统 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。|  
|**3**|操作系统 (CmdExec)|  
|**4**|复制快照代理|  
|**5**|复制日志读取器代理|  
|**6**|复制分发代理|  
|**7**|Replication Merge Agent|  
|**8**|复制队列读取器代理|  
|**9**|Analysis Services 命令|  
|**10**|Analysis Services 查询|  
|**11**|[!INCLUDE[ssIS](../../includes/ssis-md.md)] 包执行|  
|**12**|PowerShell 脚本|  
  
`[ @subsystem_name = ] 'subsystem_name'` 要撤消对其访问权限的子系统的名称。 *Subsystem_name* 的值为 **sysname**，默认值为 NULL。 必须指定 *subsystem_id* 或 *subsystem_name* ，但不能同时指定两者。 下表列出了每个子系统的值。  
  
|值|说明|  
|-----------|-----------------|  
|ActiveScripting|ActiveX 脚本|  
|CmdExec|操作系统 (CmdExec)|  
|快照|复制快照代理|  
|LogReader|复制日志读取器代理|  
|分发|复制分发代理|  
|合并|Replication Merge Agent|  
|QueueReader|复制队列读取器代理|  
|ANALYSISQUERY|Analysis Services 命令|  
|ANALYSISCOMMAND|Analysis Services 查询|  
|Dts|[!INCLUDE[ssIS](../../includes/ssis-md.md)] 包执行|  
|PowerShell|PowerShell 脚本|  
  
## <a name="remarks"></a>备注  
 撤消对子系统的访问权限不会更改代理中指定的主体数据库的权限。  
  
> [!NOTE]  
>  若要确定哪些作业步骤引用代理，请右键单击 "Microsoft 中 **SQL Server 代理**" 下的 "**代理**" 节点 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，然后单击 "**属性**"。 在 " **代理帐户属性** " 对话框中，选择 " **引用** " 页，以查看引用此代理的所有作业步骤。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_revoke_proxy_from_subsystem** 执行。  
  
## <a name="examples"></a>示例  
 以下示例撤消代理 `Catalog application proxy` 对 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 子系统的访问权限。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_revoke_proxy_from_subsystem  
    @proxy_name = 'Catalog application proxy',  
    @subsystem_name = N'Dts';  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 代理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_grant_proxy_to_subsystem &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
  
  
