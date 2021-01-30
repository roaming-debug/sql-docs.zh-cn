---
title: 'sp_MSchange_snapshot_agent_properties (T-sql) '
description: 描述用于更改用于 SQL Server 复制的快照代理的属性的 sp_MSchange_snapshot_agent_properties 存储过程。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_snapshot_agent_properties_TSQL
- sp_MSchange_snapshot_agent_properties
helpviewer_keywords:
- sp_MSchange_snapshot_agent_properties
ms.assetid: 7947a788-3fd7-469f-84db-b03ba89a153c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f07673e9dd7b613e34a5f56d6fa4c3629d3e1540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174493"
---
# <a name="sp_mschange_snapshot_agent_properties-transact-sql"></a>sp_MSchange_snapshot_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本的分发服务器上运行的快照代理作业的属性。 当发布服务器在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 实例上运行时，可使用此存储过程更改属性。 此存储过程在分发服务器上对分发数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_MSchange_snapshot_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @frequency_type= ] frequency_type  
        , [ @frequency_interval= ] frequency_interval  
        , [ @frequency_subday= ] frequency_subday  
        , [ @frequency_subday_interval= ] frequency_subday_interval  
        , [ @frequency_relative_interval= ] frequency_relative_interval  
        , [ @frequency_recurrence_factor= ] frequency_recurrence_factor  
        , [ @active_start_date= ] active_start_date  
        , [ @active_end_date= ] active_end_date  
        , [ @active_start_time_of_day= ] active_start_time_of_day  
        , [ @active_end_time_of_day= ] active_end_time_of_day  
        , [ @snapshot_job_name = ] 'snapshot_agent_name'  
        , [ @publisher_security_mode = ] publisher_security_mode  
        , [ @publisher_login = ] 'publisher_login'  
        , [ @publisher_password = ] 'publisher_password'   
        , [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
        , [ @publisher_type = ] 'publisher_type'  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 发布数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @frequency_type = ] frequency_type` 执行快照代理的频率。 *frequency_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次|  
|**2**|按需|  
|**4**|每天|  
|**8**|每周|  
|**10**|每月一次|  
|**20**|每月，相对于频率间隔|  
|**40**|当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理启动时|  
  
`[ @frequency_interval = ] frequency_interval` 要应用于 *frequency_type* 设置的频率的值。 *frequency_interval* 为 **int**，没有默认值。  
  
`[ @frequency_subday = ] frequency_subday`*Freq_subday_interval* 的单位。 *frequency_subday* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次|  
|**2**|秒|  
|**4**|Minute|  
|**8**|小时|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval`*Frequency_subday* 的间隔。 *frequency_subday_interval* 为 **int**，没有默认值。  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` 快照代理运行的日期。 *frequency_relative_interval* 为 **int**，没有默认值。  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor`*Frequency_type* 使用的重复因子。 *frequency_recurrence_factor* 为 **int**，没有默认值。  
  
`[ @active_start_date = ] active_start_date` 第一次计划快照代理的日期，格式为 YYYYMMDD。 *active_start_date* 为 **int**，没有默认值。  
  
`[ @active_end_date = ] active_end_date` 停止计划快照代理的日期，格式为 YYYYMMDD。 *active_end_date* 为 **int**，没有默认值。  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` 第一次计划快照代理的时间，格式为 HHMMSS。 *active_start_time_of_day* 为 **int**，没有默认值。  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` 停止计划快照代理的时间，格式为 HHMMSS。 *active_end_time_of_day* 为 **int**，没有默认值。  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` 如果正在使用现有作业，则为现有快照代理作业名称。 *snapshot_agent_name* 为 **nvarchar (100)**，无默认值。  
  
`[ @publisher_security_mode = ] publisher_security_mode` 连接到发布服务器时代理所使用的安全模式。 *publisher_security_mode* 为 **int**，没有默认值。 **0** 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证， **1** 指定 Windows 身份验证。 对于非发布服务器，必须指定 **0** 值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` 连接到发布服务器时使用的登录名。 *publisher_login* **sysname**，无默认值。 当 *publisher_security_mode* 为 **0** 时，必须指定 *publisher_login* 。 如果 *publisher_login* 为 NULL，并且发布服务器 *_ * * security_mode* 为 **1**，则连接到发布服务器时将使用 *job_login* 中指定的 Windows 帐户。  
  
`[ @publisher_password = ] 'publisher_password'` 连接到发布服务器时使用的密码。 *publisher_password* 为 **nvarchar (524)**，无默认值。  
  
> [!IMPORTANT]  
>  请不要将身份验证信息存储在脚本文件中。 为了提高安全性，建议您在运行时提供登录名和密码。  
  
`[ @job_login = ] 'job_login'` 用于运行代理的 Windows 帐户的登录名。 *job_login* 为 **nvarchar (257)**，无默认值。 此 Windows 帐户总是用于与分发服务器建立代理连接。 创建新的快照代理作业时，必须提供此参数。 *不能更改非* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]*发布者。*  
  
`[ @job_password = ] 'job_password'` 运行代理所用的 Windows 帐户的密码。 *job_password* **sysname**，无默认值。 创建新的快照代理作业时，必须提供此参数。  
  
> [!IMPORTANT]  
>  请不要将身份验证信息存储在脚本文件中。 为了提高安全性，建议您在运行时提供登录名和密码。  
  
`[ @publisher_type = ] 'publisher_type'` 指定发布服务器未在实例中运行时的发布服务器类型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *publisher_type* **sysname**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**MSSQLSERVER**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。|  
|**联手**|指定标准的 Oracle 发布服务器。|  
|**ORACLE GATEWAY**|指定 Oracle 网关发布服务器。|  
  
 有关 Oracle 发布服务器和 Oracle 网关发布服务器之间的差异的详细信息，请参阅 [Oracle 发布概述](../../relational-databases/replication/non-sql/oracle-publishing-overview.md)。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_MSchange_snapshot_agent_properties** 用于快照复制、事务复制和合并复制。  
  
 执行 **sp_MSchange_snapshot_agent_properties** 时必须指定所有参数。 执行 [sp_helppublication_snapshot](../../relational-databases/system-stored-procedures/sp-helppublication-snapshot-transact-sql.md) 以返回快照代理作业的当前属性。  
  
 当发布服务器在或更高版本的实例上运行时 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ，应使用 [sp_changepublication_snapshot](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md) 更改快照代理作业的属性。  
  
## <a name="permissions"></a>权限  
 只有分发服务器上 **sysadmin** 固定服务器角色的成员才能 **sp_MSchange_snapshot_agent_properties** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addpublication_snapshot (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)  
  
  
