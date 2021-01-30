---
description: sp_helpmergepullsubscription (Transact-SQL)
title: sp_helpmergepullsubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepullsubscription
- sp_helpmergepullsubscription_TSQL
helpviewer_keywords:
- sp_helpmergepullsubscription
ms.assetid: 6f3125f3-0dfa-40bd-b725-8aa1591234f6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 91266e8d825fd669979e925753804230fd62027c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160597"
---
# <a name="sp_helpmergepullsubscription-transact-sql"></a>sp_helpmergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关订阅服务器中存在的请求订阅的信息。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpmergepullsubscription [ [ @publication=] 'publication']  
    [ , [ @publisher=] 'publisher']  
    [ , [ @publisher_db=] 'publisher_db']  
    [ , [ @subscription_type=] 'subscription_type']  
```  
  
## <a name="argument"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 **%** 。 如果 *是* **%** ，则返回有关当前数据库中所有合并发布和订阅的信息。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname** 为，默认值为 **%** 。  
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。 *publisher_db* 的默认值为 **sysname**，默认值为 **%** 。  
  
`[ @subscription_type = ] 'subscription_type'` 指示是否显示请求订阅。 *subscription_type* 为 **nvarchar (10)**，默认值为 **"pull"**。 有效值为 **"push"**、 **"pull"** 或 **"both"**。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**subscription_name**|**nvarchar(1000)**|订阅的名称。|  
|**发布**|**sysname**|发布的名称。|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**publisher_db**|**sysname**|发布服务器数据库名。|  
|**订阅服务器**|**sysname**|订阅服务器的名称。|  
|**subscription_db**|**sysname**|订阅数据库的名称。|  
|**status**|**int**|订阅状态：<br /><br /> **0** = 非活动订阅<br /><br /> **1** = 有效订阅<br /><br /> **2** = 已删除订阅<br /><br /> **3** = 已分离订阅<br /><br /> **4** = 附加订阅<br /><br /> **5** = 已将订阅标记为重新初始化并上传<br /><br /> **6** = 附加订阅失败<br /><br /> **7** = 从备份还原的订阅|  
|**subscriber_type**|**int**|订阅服务器的类型：<br /><br /> **1** = 全局<br /><br /> **2** = 本地<br /><br /> **3** = 匿名|  
|**subscription_type**|**int**|订阅的类型：<br /><br /> **0** = 推送<br /><br /> **1** = 请求<br /><br /> **2** = 匿名|  
|**priority**|**float (8)**|订阅优先级。 该值必须小于 **100.00**。|  
|**sync_type**|**tinyint**|订阅同步类型：<br /><br /> **1** = 自动<br /><br /> **2** = 不使用快照。|  
|description|**nvarchar(255)**|对请求订阅的简短说明。|  
|**merge_jobid**|**binary(16)**|合并代理的作业 ID。|  
|**enabled_for_syncmgr**|**int**|指示是否可以通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 同步管理器同步订阅。|  
|**last_updated**|**nvarchar (26)**|合并代理上次成功同步订阅的时间。|  
|**publisher_login**|**sysname**|发布服务器登录名。|  
|**publisher_password**|**sysname**|发布者密码。|  
|**publisher_security_mode**|**int**|指定发布服务器的安全模式：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证<br /><br /> **1** = Windows 身份验证|  
|**发行人**|**sysname**|分发服务器的名称。|  
|**distributor_login**|**sysname**|分发服务器登录名。|  
|**distributor_password**|**sysname**|分发服务器密码。|  
|**distributor_security_mode**|**int**|指定分发服务器的安全模式：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证<br /><br /> **1** = Windows 身份验证|  
|**ftp_address**|**sysname**|仅为向后兼容而提供。 是分发服务器的文件传输协议 (FTP) 服务的网络地址。|  
|**ftp_port**|**int**|仅为向后兼容而提供。 分发服务器 FTP 服务的端口号。|  
|**ftp_login**|**sysname**|仅为向后兼容而提供。 用于连接到 FTP 服务的用户名。|  
|**ftp_password**|**sysname**|仅为向后兼容而提供。 用于连接到 FTP 服务的用户密码。|  
|**alt_snapshot_folder**|**nvarchar(255)**|存储快照文件夹的位置（如果该位置是默认位置以外的位置）。|  
|**working_directory**|**nvarchar(255)**|如果指定了使用 FTP 的选项，则是使用 FTP 将快照文件传输到的目录的完全限定路径。|  
|**use_ftp**|**bit**|订阅正通过 Internet 订阅发布，且已配置 FTP 寻址属性。 如果为 **0**，则订阅不使用 FTP。 如果为 **1**，则订阅使用 FTP。|  
|**offload_agent**|**bit**|指定是否可以远程激活和运行代理。 如果为 **0**，则不能远程激活代理。|  
|**offload_server**|**sysname**|用于远程激活的服务器的名称。|  
|**use_interactive_resolver**|**int**|返回在调节过程中是否使用交互式冲突解决程序。 如果为 **0**，则不使用交互式冲突解决程序。|  
|**subid**|**uniqueidentifier**|订阅服务器的 ID。|  
|**dynamic_snapshot_location**|**nvarchar(255)**|保存快照文件的文件夹路径。|  
|**last_sync_status**|**int**|同步状态：<br /><br /> **1** = 正在启动<br /><br /> **2** = 成功<br /><br /> **3** = 正在进行<br /><br /> **4** = 空闲<br /><br /> **5** = 在上次失败后重试<br /><br /> **6** = 失败<br /><br /> **7** = 验证失败<br /><br /> **8** = 通过验证<br /><br /> **9** = 请求关机|  
|**last_sync_summary**|**sysname**|对上一次同步结果的说明。|  
|**use_web_sync**|**bit**|指定是否可以通过 HTTPS 同步订阅，如果值为 **1** ，则表示已启用此功能。|  
|**internet_url**|**nvarchar(260)**|表示 Web 同步的复制侦听器位置的 URL。|  
|**internet_login**|**nvarchar(128)**|在使用基本身份验证连接到承载 Web 同步的 Web 服务器时，合并代理所使用的登录名。|  
|**internet_password**|**nvarchar (524)**|在使用基本身份验证连接到承载 Web 同步的 Web 服务器时，合并代理所使用的登录密码。|  
|**internet_security_mode**|**int**|连接到承载 Web 同步的 Web 服务器时使用的身份验证模式。 值 **1** 表示 Windows 身份验证，值 **0** 表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。|  
|**internet_timeout**|**int**|Web 同步请求过期之前的时间长度（秒）。|  
|**hostname**|**nvarchar(128)**|指定在参数化行筛选器的 WHERE 子句中使用此函数时 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 的重载值。|  
|**job_login**|**nvarchar(512)**|是运行合并代理时所用的 Windows 帐户，它以 "*域* 用户名" 的格式返回 \\ 。|  
|**job_password**|**sysname**|出于安全原因，始终返回值 " **\*\*\*\*\*\*\*\*\*\*** "。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpmergepullsubscription** 用于合并复制。 在结果集中， **last_updated** 中返回的日期的格式为 *YYYYMMDD hh： mm： ss*。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员和 **db_owner** 固定数据库角色的成员才能执行 **sp_helpmergepullsubscription**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addmergepullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
