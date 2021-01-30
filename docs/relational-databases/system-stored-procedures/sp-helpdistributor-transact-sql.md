---
description: sp_helpdistributor (Transact-SQL)
title: sp_helpdistributor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdistributor_TSQL
- sp_helpdistributor
helpviewer_keywords:
- sp_helpdistributor
ms.assetid: 37b0983e-3b69-4f0f-977e-20efce0a0b97
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c23613a7b79ea7bfa418425a32b6d2ee9bf62699
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186981"
---
# <a name="sp_helpdistributor-transact-sql"></a>sp_helpdistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  列出有关分发服务器、分发数据库、工作目录和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理用户帐户的信息。 该存储过程在发布服务器上对发布数据库或任何数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpdistributor [ [ @distributor= ] 'distributor' OUTPUT ]  
    [ , [ @distribdb= ] 'distribdb' OUTPUT ]  
    [ , [ @directory= ] 'directory' OUTPUT ]  
    [ , [ @account= ] 'account' OUTPUT ]  
    [ , [ @min_distretention= ] min_distretention OUTPUT ]  
    [ , [ @max_distretention= ] max_distretention OUTPUT ]  
    [ , [ @history_retention= ] history_retention OUTPUT ]  
    [ , [ @history_cleanupagent= ] 'history_cleanupagent' OUTPUT ]  
    [ , [ @distrib_cleanupagent = ] 'distrib_cleanupagent' OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @local = ] 'local' ]  
    [ , [ @rpcsrvname= ] 'rpcsrvname' OUTPUT ]  
    [ , [ @publisher_type = ] 'publisher_type' OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @distributor = ] 'distributor' OUTPUT` 分发服务器的名称。 分发服务器的默认值为 **sysname**，默认 **%** 值为，它是返回结果集的唯一值。  
  
`[ @distribdb = ] 'distribdb' OUTPUT` 分发数据库的名称。 *distribdb* 的类型为 **sysname**，默认 **%** 值为，该值是返回结果集的唯一值。  
  
`[ @directory = ] 'directory' OUTPUT` 工作目录。 *目录* 为 **nvarchar (255)**，默认 **%** 值为，该值是返回结果集的唯一值。  
  
`[ @account = ] 'account' OUTPUT` 是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 用户帐户。 *帐户* 为 **nvarchar (255)**，默认 **%** 值为，该值是返回结果集的唯一值。  
  
`[ @min_distretention = ] _min_distretentionOUTPUT` 最小分发保持期（以小时为单位）。 *min_distretention* 的值为 **int**，默认值为 **-1**。  
  
`[ @max_distretention = ] _max_distretentionOUTPUT` 最大分发保持期（以小时为单位）。 *max_distretention* 的值为 **int**，默认值为 **-1**。  
  
`[ @history_retention = ] _history_retentionOUTPUT` 历史记录保持期（以小时为单位）。 *history_retention* 的值为 **int**，默认值为 **-1**。  
  
`[ @history_cleanupagent = ] 'history_cleanupagent' OUTPUT` 历史记录清除代理的名称。 *history_cleanupagent* 为 **nvarchar (100)**，默认 **%** 值为，该值是返回结果集的唯一值。  
  
`[ @distrib_cleanupagent = ] 'distrib_cleanupagent' OUTPUT` 分发清除代理的名称。 *distrib_cleanupagent* 为 **nvarchar (100)**，默认 **%** 值为，该值是返回结果集的唯一值。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
`[ @local = ] 'local'` 是否 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应获得本地服务器值。 *local* 为 **nvarchar (5)**，默认值为 NULL。  
  
`[ @rpcsrvname = ] 'rpcsrvname' OUTPUT` 发出远程过程调用的服务器的名称。 *rpcsrvname* 的类型为 **sysname**，默认 **%** 值为，该值是返回结果集的唯一值。  
  
`[ @publisher_type = ] 'publisher_type' OUTPUT` 发布服务器的发布服务器类型。 *publisher_type* 的默认值为 **sysname**，默认 **%** 值为，该值是返回结果集的唯一值。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**发行人**|**sysname**|分发服务器的名称。|  
|**分发数据库**|**sysname**|分发数据库的名称。|  
|**文件夹**|**nvarchar(255)**|工作目录的名称。|  
|**帐户**|**nvarchar(255)**|Windows 用户帐户的名称。|  
|**min distrib retention**|**int**|最小分发保持期。|  
|**max distrib retention**|**int**|最大分发保持期。|  
|**history retention**|**int**|历史记录保持期。|  
|**history cleanup agent**|**nvarchar (100)**|历史记录清除代理的名称。|  
|**distribution cleanup agent**|**nvarchar (100)**|分发清除代理的名称。|  
|**rpc server name**|**sysname**|远程分发服务器或本地分发服务器的名称。|  
|**rpc login name**|**sysname**|用于对远程分发服务器的远程过程调用的登录名。|  
|**发布服务器类型**|**sysname**|发布服务器的类型；可以为下列值之一：<br /><br /> **MSSQLSERVER**<br /><br /> **联手**<br /><br /> **ORACLE GATEWAY**|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpdistributor** 在所有类型的复制中使用。  
  
 如果在执行 **sp_helpdistributor** 时指定了一个或多个输出参数，则在退出时将为设置为 NULL 的所有输出参数赋值，并且不返回任何结果集。 如果未指定输出参数，则返回结果集。  
  
## <a name="permissions"></a>权限  
 以下结果集列或输出参数将返回给发布服务器上 **sysadmin** 固定服务器角色的成员和发布数据库上的 **db_owner** 固定数据库角色：  
  
|结果集列|输出参数|  
|-----------------------|----------------------|  
|account|**\@账号**|  
|min distrib retention|**\@min_distretention**|  
|max distrib retention|**\@max_distretention**|  
|history retention|**\@history_retention**|  
|history cleanup agent|**\@history_cleanupagent**|  
|distribution cleanup agent|**\@distrib_cleanupagent**|  
|rpc login name|无|  
  
 以下结果集列返回给分发服务器上的某个发布的发布访问列表中的用户:  
  
-   目录  
  
 以下结果集列返回给所有用户。  
  
|结果集列|输出参数|  
|-----------------------|----------------------|  
|distributor|**\@发行人**|  
|分发数据库 (distribution database)|**\@distribdb**|  
|rpc server name|**\@rpcsrvname**|  
|publisher type|**\@publisher_type**|  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)  
  
  
