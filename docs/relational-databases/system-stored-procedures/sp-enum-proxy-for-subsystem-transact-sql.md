---
description: sp_enum_proxy_for_subsystem (Transact-SQL)
title: sp_enum_proxy_for_subsystem (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_proxy_for_subsystem_TSQL
- sp_enum_proxy_for_subsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_proxy_for_subsystems
ms.assetid: 580cc3be-1068-4a96-8d15-78ca3a5bb719
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: bac523afb9f802e3f6feb8a23c47835c82bac807
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157073"
---
# <a name="sp_enum_proxy_for_subsystem-transact-sql"></a>sp_enum_proxy_for_subsystem (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  列出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理访问子系统所需的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_enum_proxy_for_subsystem  
    [ @proxy_id = ] proxy_id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @subsystem_id = ] subsystem_id,  
    [ @subsystem_name = ] 'subsystem_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @proxy_id = ] proxy_id` 要列出其信息的代理的标识号。 *Proxy_id* 的值为 **int**，默认值为 NULL。 可以指定 *id* 或 *proxy_name* 。  
  
`[ @proxy_name = ] 'proxy_name'` 要列出其信息的代理的名称。 *Proxy_name* 的值为 **sysname**，默认值为 NULL。 可以指定 *id* 或 *proxy_name* 。  
  
`[ @subsystem_id = ] subsystem_id` 要列出其信息的子系统的标识号。 *Subsystem_id* 的值为 **int**，默认值为 NULL。 可以指定 *subsystem_id* 或 *subsystem_name* 。  
  
`[ @subsystem_name = ] 'subsystem_name'` 要列出其信息的子系统的名称。 *Subsystem_name* 的值为 **sysname**，默认值为 NULL。 可以指定 *subsystem_id* 或 *subsystem_name* 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|子系统标识号。|  
|**subsystem_name**|**sysname**|子系统的名称。|  
|**proxy_id**|**int**|代理服务器标识号。|  
|**proxy_name**|**sysname**|代理服务器的名称。|  
| &nbsp; | &nbsp; | &nbsp; |
  
## <a name="remarks"></a>备注  
 如果未提供任何参数， **sp_enum_proxy_for_subsystem** 会列出有关每个子系统的实例中的所有代理的信息。  
  
 当提供代理 id 或代理名称时， **sp_enum_proxy_for_subsystem** 会列出代理有权访问的子系统。 提供子系统 id 或子系统名称后， **sp_enum_proxy_for_subsystem** 列出有权访问该子系统的代理。  
  
 当同时提供代理信息和子系统信息时，如果指定的代理有权访问指定的子系统，则结果集将返回一行。  
  
 此存储过程位于 **msdb** 中。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-all-associations"></a>A. 列出所有关联  
 以下示例列出了在当前实例的代理和子系统之间建立的所有权限。  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_proxy_for_subsystem ;  
GO  
```  
  
### <a name="b-determining-if-a-proxy-has-access-to-a-specific-subsystem"></a>B. 确定代理是否有权访问特定的子系统  
 如果代理 `Catalog application proxy` 有权访问 `ActiveScripting` 子系统，则以下示例将返回一行。 否则，该示例将返回空结果集。  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_proxy_for_subsystem  
    @subsystem_name = 'ActiveScripting',  
    @proxy_name = 'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_grant_proxy_to_subsystem &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
  
  
