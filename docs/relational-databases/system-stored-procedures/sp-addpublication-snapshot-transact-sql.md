---
description: sp_addpublication_snapshot (Transact-SQL)
title: sp_addpublication_snapshot (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpublication_snapshot_TSQL
- sp_addpublication_snapshot
helpviewer_keywords:
- sp_addpublication_snapshot
ms.assetid: 192b6214-df6e-44a3-bdd4-9d933a981619
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f7fb5fbea8a87d1bacb4a086825ecfe62b292f76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209365"
---
# <a name="sp_addpublication_snapshot-transact-sql"></a>sp_addpublication_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  为指定的发布创建快照代理。 此存储过程在发布服务器上对发布数据库执行。  
  
> [!IMPORTANT]  
>  使用远程分发服务器配置发布服务器时，为所有参数提供的值（包括 *job_login* 和 *job_password*）都会以纯文本方式发送到该分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addpublication_snapshot [ @publication= ] 'publication'  
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
    [ , [ @snapshot_job_name = ] 'snapshot_agent_name' ]  
    [ , [ @publisher_security_mode = ] publisher_security_mode ]  
    [ , [ @publisher_login = ] 'publisher_login' ]  
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]  
    [ , [ @job_password = ] 'job_password' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @frequency_type = ] frequency_type` 执行快照代理的频率。 *frequency_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次。|  
|**4** (默认值) |每天。|  
|**8**|每周。|  
|**16**|每月。|  
|**32**|每月，相对于频率间隔。|  
|**64**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理启动时。|  
|**128**|计算机空闲时运行|  
  
`[ @frequency_interval = ] frequency_interval` 要应用于 *frequency_type* 设置的频率的值。 *frequency_interval* 为 **int**，可以是下列值之一。  
  
|frequency_type 的值|对 frequency_interval 的影响|  
|------------------------------|-----------------------------------|  
|**1**|*frequency_interval* 未使用。|  
|**4** (默认值) |每 *frequency_interval* 天，默认为每天。|  
|**8**|*frequency_interval* 是以下 (中的一个或多个 [&#124; (按位 or)](../../t-sql/language-elements/bitwise-or-transact-sql.md) 逻辑运算符) ：<br /><br /> **1** = 星期日 &#124;<br /><br /> **2** = 星期一 &#124;<br /><br /> **4** = 星期二 &#124;<br /><br /> **8** = 星期三 &#124;<br /><br /> **16** = 星期四 &#124;<br /><br /> **32** = 星期五 &#124;<br /><br /> **64** = 星期六|  
|**16**|*Frequency_interval* 月中的第几天。|  
|**32**|*frequency_interval* 是以下项之一：<br /><br /> **1** = 星期日 &#124;<br /><br /> **2** = 星期一 &#124;<br /><br /> **3** = 星期二 &#124;<br /><br /> **4** = 星期三 &#124;<br /><br /> **5** = 星期四 &#124;<br /><br /> **6** = 星期五 &#124;<br /><br /> **7** = 星期六 &#124;<br /><br /> **8** = 日 &#124;<br /><br /> **9** = 工作日 &#124;<br /><br /> **10** = 周末|  
|**64**|*frequency_interval* 未使用。|  
|**128**|*frequency_interval* 未使用。|  
  
`[ @frequency_subday = ] frequency_subday`*Freq_subday_interval* 的单位。 *frequency_subday* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次|  
|**2**|秒|  
|**4** (默认值) |Minute|  
|**8**|小时|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval`*Frequency_subday* 的间隔。 *frequency_subday_interval* 的值为 **int**，默认值为5，表示每5分钟一次。  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` 快照代理运行的日期。 *frequency_relative_interval* 的值为 **int**，默认值为1。  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor`*Frequency_type* 使用的重复因子。 *frequency_recurrence_factor* 的值为 **int**，默认值为0。  
  
`[ @active_start_date = ] active_start_date` 第一次计划快照代理的日期，格式为 YYYYMMDD。 *active_start_date* 的值为 **int**，默认值为0。  
  
`[ @active_end_date = ] active_end_date` 停止计划快照代理的日期，格式为 YYYYMMDD。 *active_end_date* 的值为 **int**，默认值为99991231，表示9999年12月31日。  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` 第一次计划快照代理的时间，格式为 HHMMSS。 *active_start_time_of_day* 的值为 **int**，默认值为0。  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` 停止计划快照代理的时间，格式为 HHMMSS。 *active_end_time_of_day* 的值为 **int**，默认值为235959，表示 11:59:59 P.M.。 以24小时制计量。  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` 如果正在使用现有作业，则为现有快照代理作业名称。 *snapshot_agent_name* 为 **nvarchar (100)** ，默认值为 NULL。 此参数只限内部使用，创建新发布时不应指定它。 如果指定 *snapshot_agent_name* ，则 *job_login* 和 *job_password* 必须为 NULL。  
  
`[ @publisher_security_mode = ] publisher_security_mode` 连接到发布服务器时代理所使用的安全模式。 *publisher_security_mode* 为 **smallint**，默认值为1。 **0** 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证， **1** 指定 Windows 身份验证。 对于非发布服务器，必须指定 **0** 值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` 连接到发布服务器时使用的登录名。 *publisher_login* 的默认值为 **sysname**，默认值为 NULL。 当 *publisher_security_mode* 为 **0** 时，必须指定 *publisher_login* 。 如果 *publisher_login* 为 NULL 且 *publisher_security_mode* 为 **1**，则连接到发布服务器时将使用 *job_login* 中指定的帐户。  
  
`[ @publisher_password = ] 'publisher_password'` 连接到发布服务器时使用的密码。 *publisher_password* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!IMPORTANT]  
>  请不要将身份验证信息存储在脚本文件中。 为了提高安全性，建议您在运行时提供登录名和密码。  
  
`[ @job_login = ] 'job_login'` 运行代理时所用的帐户的登录名。 在 Azure SQL 托管实例上，使用 SQL Server 帐户。 *job_login* 为 **nvarchar (257)**，默认值为 NULL。 此帐户始终用于到分发服务器的代理连接。 创建新的快照代理作业时，必须提供此参数。  
  
> [!NOTE]
>  对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器，此登录名必须与 [&#40;transact-sql&#41;sp_adddistpublisher ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)中指定的相同。  
  
`[ @job_password = ] 'job_password'` 运行代理所用的 Windows 帐户的密码。 *job_password* **sysname**，无默认值。 创建新的快照代理作业时，必须提供此参数。  
  
> [!IMPORTANT]  
>  请不要将身份验证信息存储在脚本文件中。 为了提高安全性，建议您在运行时提供登录名和密码。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  在发布服务器上创建快照代理时，不应使用 *publisher* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addpublication_snapshot** 用于快照复制、事务复制和合并复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addpublication-snapsh_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addpublication_snapshot**。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [创建并应用快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [sp_addpublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication_snapshot &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md)   
 [sp_startpublication_snapshot &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-startpublication-snapshot-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
