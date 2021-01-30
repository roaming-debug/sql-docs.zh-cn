---
description: sp_helpmergesubscription (Transact-SQL)
title: sp_helpmergesubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergesubscription
- sp_helpmergesubscription_TSQL
helpviewer_keywords:
- sp_helpmergesubscription
ms.assetid: da564112-f769-4e67-9251-5699823e8c86
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 274048dcfcd76e815d3f4ec159dded0571f66e43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210890"
---
# <a name="sp_helpmergesubscription-transact-sql"></a>sp_helpmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关对合并发布的订阅（推送订阅和请求订阅）的信息。 此存储过程用于在发布服务器上对发布数据库执行，或在正重新发布的订阅服务器上对订阅数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpmergesubscription [ [ @publication=] 'publication']  
    [ , [ @subscriber=] 'subscriber']  
    [ , [ @subscriber_db=] 'subscriber_db']  
    [ , [ @publisher=] 'publisher']  
    [ , [ @publisher_db=] 'publisher_db']  
    [ , [ @subscription_type=] 'subscription_type']  
    [ , [ @found=] 'found' OUTPUT]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 **%** 。 该发布必须已经存在，并符合标识符的相关规则。 如果为 NULL 或 **%** ，则返回有关当前数据库中所有合并发布和订阅的信息。  
  
`[ @subscriber = ] 'subscriber'` 订阅服务器的名称。 *订阅服务器* 的默认值为 **sysname**，默认值为 **%** 。 如果为 NULL 或 %，则返回有关对给定发布的所有订阅信息。  
  
`[ @subscriber_db = ] 'subscriber_db'` 订阅数据库的名称。 *subscriber_db* 的默认值为 **sysname**，默认值为 **%** ，它返回有关所有订阅数据库的信息。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 发布服务器必须为有效服务器。 *发布服务器* 的默认值为 **sysname**，默认值为 **%** ，它返回有关所有发布服务器的信息。  
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。 *publisher_db* 的默认值为 **sysname**，默认值为 **%** ，它返回所有发布服务器数据库的相关信息。  
  
`[ @subscription_type = ] 'subscription_type'` 订阅的类型。 *subscription_type* 为 **nvarchar (15)**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**推送** (默认值) |推送订阅|  
|**请求**|请求订阅|  
|**全部**|推送订阅和请求订阅|  
  
`[ @found = ] 'found'OUTPUT` 指示返回行的标志。 *找到* **int** 和 OUTPUT 参数，默认值为 NULL。 **1** 指示已找到发布。 **0** 表示找不到发布。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**subscription_name**|**sysname**|订阅的名称。|  
|**发布**|**sysname**|发布的名称。|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**publisher_db**|**sysname**|发布服务器数据库名。|  
|**订阅服务器**|**sysname**|订阅服务器的名称。|  
|**subscriber_db**|**sysname**|订阅数据库的名称。|  
|**status**|**int**|订阅状态：<br /><br /> **0** = 所有作业都在等待启动<br /><br /> **1** = 一个或多个作业正在启动<br /><br /> **2** = 所有作业都已成功执行<br /><br /> **3** = 至少有一个作业正在执行<br /><br /> **4** = 所有作业都已计划且空闲<br /><br /> **5** = 在上次失败后至少有一个作业正在尝试执行<br /><br /> **6** = 至少有一个作业无法成功执行|  
|**subscriber_type**|**int**|订阅服务器的类型。|  
|**subscription_type**|**int**|订阅的类型：<br /><br /> **0** = 推送<br /><br /> **1** = 请求<br /><br /> **2** = 两者|  
|**priority**|**float (8)**|指示订阅优先级的数字。|  
|**sync_type**|**tinyint**|订阅同步类型。|  
|description|**nvarchar(255)**|对该合并订阅的简短说明。|  
|**merge_jobid**|**binary(16)**|合并代理的作业 ID。|  
|**full_publication**|**tinyint**|指定订阅是完全发布还是筛选发布。|  
|**offload_enabled**|**bit**|指定复制代理的卸载执行是否设置为在订阅服务器上运行。 如果为 NULL，则执行将在发布服务器上运行。|  
|**offload_server**|**sysname**|运行代理的服务器名。|  
|**use_interactive_resolver**|**int**|返回在调节过程中是否使用交互式冲突解决程序。 如果为 **0**，则不使用交互式冲突解决程序。|  
|**hostname**|**sysname**|按 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 函数的值筛选订阅时提供的值。|  
|**subscriber_security_mode**|**smallint**|订阅服务器上的安全模式，其中 **1** 表示 Windows 身份验证， **0** 表示 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。|  
|**subscriber_login**|**sysname**|在订阅服务器上的登录名。|  
|**subscriber_password**|**sysname**|永远不会返回实际的订阅服务器密码。 结果由 " **\*\*\*\*\*\*** " 字符串屏蔽。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpmergesubscription** 用于合并复制，以返回存储在发布服务器或重新发布订阅服务器上的订阅信息。  
  
 对于匿名订阅， *subscription_type* 值始终为 **1** (请求) 。 但是，必须在订阅服务器上执行 [sp_helpmergepullsubscription](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md) ，才能获取有关匿名订阅的信息。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、 **db_owner** 固定数据库角色的成员或订阅所属的发布的发布访问列表才能执行 **sp_helpmergesubscription**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addmergesubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_changemergesubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
