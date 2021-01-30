---
description: sp_helpsubscription (Transact-SQL)
title: sp_helpsubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscription_TSQL
- sp_helpsubscription
helpviewer_keywords:
- sp_helpsubscription
ms.assetid: ff96bcbf-e2b9-4da8-8515-d80d4ce86c16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73fc43bf20a9306d0224392d984b3f2bff74ec6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192900"
---
# <a name="sp_helpsubscription-transact-sql"></a>sp_helpsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  列出与特定的发布、项目、订阅服务器或订阅集关联的订阅信息。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpsubscription [ [ @publication = ] 'publication' ]   
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @destination_db = ] 'destination_db' ]   
    [ , [ @found=] found OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 关联发布的名称。 *发布* 为 **sysname**，默认值为 **%** ，它返回此服务器的所有订阅信息。  
  
`[ @article = ] 'article'` 项目的名称。 *项目* 的默认值为 **sysname**，默认值为 **%** ，它返回所选发布和订阅服务器的所有订阅信息。 如果为 **all**，则只为发布的完整订阅返回一个条目。  
  
`[ @subscriber = ] 'subscriber'` 要获取订阅信息的订阅服务器的名称。 *订阅服务器* 的默认值为 **sysname**，默认值为 **%** ，它返回所选发布和项目的所有订阅信息。  
  
`[ @destination_db = ] 'destination_db'` 目标数据库的名称。 *destination_db* 的默认值为 **sysname**，默认值为 **%** 。  
  
`[ @found = ] 'found'OUTPUT` 指示返回行的标志。 *找到* **int** 和 OUTPUT 参数，默认值为23456。  
  
 **1** 指示已找到发布。  
  
 **0** 表示找不到发布。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 为 **sysname**，默认值为当前服务器的名称。  
  
> [!NOTE]  
>  不应指定 *发布服务器*，除非它是 Oracle 发布服务器。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**订阅服务器**|**sysname**|订阅服务器的名称。|  
|**发布**|**sysname**|发布的名称。|  
|**文章**|**sysname**|项目的名称。|  
|**目标数据库**|**sysname**|用于存放复制数据的目标数据库的名称。|  
|**订阅状态**|**tinyint**|订阅状态：<br /><br /> **0** = 非活动<br /><br /> **1** = 已订阅<br /><br /> **2** = 活动|  
|**同步类型**|**tinyint**|订阅同步类型：<br /><br /> **1** = 自动<br /><br /> **2** = 无|  
|**订阅类型**|**int**|订阅的类型：<br /><br /> **0** = 推送<br /><br /> **1** = 请求<br /><br /> **2** = 匿名|  
|**full subscription**|**bit**|指示是否订阅发布中的所有项目：<br /><br /> 0 = 否<br /><br /> 1 = 是|  
|**订阅名称**|**nvarchar(255)**|订阅的名称。|  
|**update mode**|**int**|**0** = 只读<br /><br /> **1** = 立即更新订阅|  
|**distribution job id**|**binary(16)**|分发代理的作业 ID。|  
|**loopback_detection**|**bit**|环回检测将确定分发代理是否将在订阅服务器上发起的事务发送回订阅服务器：<br /><br /> **0** = 发送回。<br /><br /> **1** = 不发送回。<br /><br /> 与双向事务复制一起使用。 有关详细信息，请参阅 [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md)。|  
|**offload_enabled**|**bit**|指定复制代理的卸载执行是否已设置为在订阅服务器上运行。<br /><br /> 如果为 **0**，则在发布服务器上运行代理。<br /><br /> 如果为 **1**，则在订阅服务器上运行代理。|  
|**offload_server**|**sysname**|启用了远程代理激活的服务器的名称。 如果为 NULL，则使用 [MSdistribution_agents](../../relational-databases/system-tables/msdistribution-agents-transact-sql.md) 表中列出的当前 offload_server。|  
|dts_package_name|**sysname**|指定 Data Transformation Services (DTS) 包的名称。|  
|dts_package_location|**int**|为订阅分配了一个 DTS 包时，此包的位置。 如果有一个包，则值为 **0** 时，将在 **分发服务器** 上指定包的位置。 如果值为 **1** ，则指定 **订阅服务器**。|  
|**subscriber_security_mode**|**smallint**|订阅服务器上的安全模式，其中 **1** 表示 Windows 身份验证， **0** 表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。|  
|**subscriber_login**|**sysname**|在订阅服务器上的登录名。|  
|**subscriber_password**||永远不会返回实际的订阅服务器密码。 结果由 "**&#42;&#42;&#42;&#42;&#42;&#42;**" 字符串屏蔽。|  
|**job_login**|**sysname**|分发代理运行时所用的 Windows 帐户的名称。|  
|**job_password**||从不返回实际的作业密码。 结果由 "**&#42;&#42;&#42;&#42;&#42;&#42;**" 字符串屏蔽。|  
|**distrib_agent_name**|**nvarchar (100)**|同步订阅的代理作业的名称。|  
|**subscriber_type**|**tinyint**|订阅服务器的类型，可以是下列类型之一：<br /><br /> **0** = SQL Server 订阅服务器<br /><br /> **1** = ODBC 数据源服务器<br /><br /> **2** = (弃用的 Microsoft JET 数据库) <br /><br /> **3** = OLE DB 提供程序|  
|**subscriber_provider**|**sysname**|非 SQL Server 数据源的 OLE DB 访问接口用于注册的唯一编程标识符 (PROGID)。|  
|**subscriber_datasource**|**nvarchar(4000)**|OLE DB 访问接口识别的数据源的名称。|  
|**subscriber_providerstring**|**nvarchar(4000)**|OLE DB 访问接口特定的连接字符串，用于标识数据源。|  
|**subscriber_location**|**nvarchar(4000)**|OLE DB 访问接口所了解的数据库的位置|  
|**subscriber_catalog**|**sysname**|在与 OLE DB 访问接口建立连接时要使用的目录。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpsubscription** 用于快照复制和事务复制。  
  
## <a name="permissions"></a>权限  
 Execute 权限默认授予 **public** 角色。 只为用户返回他们创建的订阅的信息。 所有订阅的信息都将返回给发布服务器上 **sysadmin** 固定服务器角色的成员或发布数据库上 **db_owner** 固定数据库角色的成员。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubstatus &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
