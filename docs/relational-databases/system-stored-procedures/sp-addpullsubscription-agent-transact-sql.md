---
description: sp_addpullsubscription_agent (Transact-SQL)
title: sp_addpullsubscription_agent (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpullsubscription_agent
- sp_addpullsubscription_agent_TSQL
helpviewer_keywords:
- sp_addpullsubscription_agent
ms.assetid: b9c2eaed-6d2d-4b78-ae9b-73633133180b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 031911ae4e9f6f81e82ec9d0b4fb20c73d94a2cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206691"
---
# <a name="sp_addpullsubscription_agent-transact-sql"></a>sp_addpullsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
 
  向事务发布添加用于同步请求订阅的新计划的代理作业。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addpullsubscription_agent [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]          , [ @publication = ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor = ] 'distributor' ]  
    [ , [ @distribution_db = ] 'distribution_db' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]  
    [ , [ @distributor_login = ] 'distributor_login' ]  
    [ , [ @distributor_password = ] 'distributor_password' ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @distribution_jobid = ] distribution_jobid OUTPUT ]  
    [ , [ @encrypted_distributor_password = ] encrypted_distributor_password ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port = ] ftp_port ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]  
    [ , [ @working_directory = ] 'working_directory' ]  
    [ , [ @use_ftp = ] 'use_ftp' ]  
    [ , [ @publication_type = ] publication_type ]  
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @offloadagent = ] 'remote_agent_activation' ]  
    [ , [ @offloadserver = ] 'remote_agent_server_name']  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  

> [!NOTE]
> 服务器名称可指定为 `<Hostname>,<PortNumber>` 。 如果在 Linux 或 Windows 上使用自定义端口部署 SQL Server，并且禁用了 browser 服务，则可能需要指定连接的端口号。
  
`[ @publisher_db = ] 'publisher_db'_` 发布服务器数据库的名称。 *publisher_db* 的值为 **sysname**，默认值为 NULL。 Oracle 发布服务器将忽略 *publisher_db* 。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @subscriber = ] 'subscriber'` 订阅服务器实例的名称，如果订阅服务器数据库位于可用性组中，则为 AG 侦听器的名称。 *订阅服务器* 的值为 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  不推荐使用此参数，保留它是为了让脚本能够向后兼容。  
  
`[ @subscriber_db = ] 'subscriber_db'` 订阅数据库的名称。 *subscriber_db* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  不推荐使用此参数，保留它是为了让脚本能够向后兼容。  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` 同步时连接到订阅服务器时使用的安全模式。 *subscriber_security_mode* 的值为 **int，** 默认值为 NULL。 **0** 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 **1** 指定 Windows 身份验证。  
  
> [!NOTE]  
>  不推荐使用此参数，保留它是为了让脚本能够向后兼容。 分发代理始终使用 Windows 身份验证连接到本地订阅服务器。 如果为此参数指定了 NULL 或 **1** 以外的值，则返回一条警告消息。  
  
`[ @subscriber_login = ] 'subscriber_login'` 同步时连接到订阅服务器时要使用的订阅服务器登录名。*subscriber_login* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  不推荐使用此参数，保留它是为了让脚本能够向后兼容。 如果为此参数指定了值，则将返回一条警告消息，但将忽略该值。  
  
`[ @subscriber_password = ] 'subscriber_password'` 订阅服务器密码。 如果 *subscriber_security_mode* 设置为 **0**，则 *subscriber_password* 是必需的。 *subscriber_password* 的默认值为 **sysname**，默认值为 NULL。 如果使用订阅服务器密码，将自动对密码进行加密。  
  
> [!NOTE]  
>  不推荐使用此参数，保留它是为了让脚本能够向后兼容。 如果为此参数指定了值，则将返回一条警告消息，但将忽略该值。  
  
`[ @distributor = ] 'distributor'` 分发服务器的名称。 *分发服务器* 为 **sysname**，默认值为 *发布服务器* 指定的值。  
  
`[ @distribution_db = ] 'distribution_db'` 分发数据库的名称。 *distribution_db* 的值为 **sysname**，默认值为 NULL。  
  
`[ @distributor_security_mode = ] distributor_security_mode` 同步时连接到分发服务器时使用的安全模式。 *distributor_security_mode* 的值为 **int**，默认值为 **1**。 **0** 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 **1** 指定 Windows 身份验证。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` 同步时连接到分发服务器时使用的分发服务器登录名。 如果 *distributor_security_mode* 设置为 **0**，则 *distributor_login* 是必需的。 *distributor_login* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @distributor_password = ] 'distributor_password'` 分发服务器密码。 如果 *distributor_security_mode* 设置为 **0**，则 *distributor_password* 是必需的。 *distributor_password* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。 如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
`[ @optional_command_line = ] 'optional_command_line'` 是提供给分发代理的可选命令提示符。 例如， **-DefinitionFile** C:\Distdef.txt 或 **-CommitBatchSize** 10。 *optional_command_line* 为 **nvarchar (4000)**，默认值为空字符串。  
  
`[ @frequency_type = ] frequency_type` 用于计划分发代理的频率。 *frequency_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次性|  
|**2** （默认值）|按需|  
|**4**|每天|  
|**8**|每周|  
|**16**|每月一次|  
|**32**|与“每月”选项相关|  
|**64**|自动启动|  
|**128**|重复执行|  
  
> [!NOTE]  
>  指定值 **64** 将导致分发代理在连续模式下运行。 这对应于为代理设置 **-连续** 参数。 有关详细信息，请参阅 [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md)。  
  
`[ @frequency_interval = ] frequency_interval` 要应用于 *frequency_type* 设置的频率的值。 *frequency_interval* 的值为 **int**，默认值为1。  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` 分发代理的日期。 如果 *frequency_type* 设置为 **32** (每月相对) ，则使用此参数。 *frequency_relative_interval* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1** （默认值）|First|  
|**2**|秒|  
|**4**|第三个|  
|**8**|第四个|  
|**16**|上一个|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor`*Frequency_type* 使用的重复因子。 *frequency_recurrence_factor* 的值为 **int**，默认值为 **1**。  
  
`[ @frequency_subday = ] frequency_subday` 在定义的时间段内重新计划的频率。 *frequency_subday* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1** （默认值）|一次|  
|**2**|秒|  
|**4**|Minute|  
|**8**|小时|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval`*Frequency_subday* 的间隔。 *frequency_subday_interval* 的值为 **int**，默认值为 **1**。  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` 第一次计划分发代理的时间，格式为 HHMMSS。 *active_start_time_of_day* 的值为 **int**，默认值为 **0**。  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` 停止计划分发代理的时间，格式为 HHMMSS。 *active_end_time_of_day* 的值为 **int**，默认值为 **0**。  
  
`[ @active_start_date = ] active_start_date` 第一次计划分发代理的日期，格式为 YYYYMMDD。 *active_start_date* 的值为 **int**，默认值为 **0**。  
  
`[ @active_end_date = ] active_end_date` 停止计划分发代理的日期，格式为 YYYYMMDD。 *active_end_date* 的值为 **int**，默认值为 **0**。  
  
`[ @distribution_jobid = ] _distribution_jobidOUTPUT` 此作业的分发代理 ID。 *distribution_jobid* 是 **二进制 (16)**，默认值为 NULL，它是一个 OUTPUT 参数。  
  
`[ @encrypted_distributor_password = ] encrypted_distributor_password` 不再支持设置 *encrypted_distributor_password* 。 尝试将此 **位** 参数设置为 **1** 将导致错误。  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` 指示是否可以通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 同步管理器同步订阅。 *enabled_for_syncmgr* 为 **nvarchar (5)**，默认值为 FALSE。 如果 **为 false**，则不向同步管理器注册订阅。 如果 **为 true**，则会向同步管理器注册订阅，并在不启动的情况下同步订阅 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。  
  
`[ @ftp_address = ] 'ftp_address'` 仅用于向后兼容。  
  
`[ @ftp_port = ] ftp_port` 仅用于向后兼容。  
  
`[ @ftp_login = ] 'ftp_login'` 仅用于向后兼容。  
  
`[ @ftp_password = ] 'ftp_password'` 仅用于向后兼容。  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'_` 指定快照的备用文件夹的位置。 *alternate_snapshot_folder* 为 **nvarchar (255)**，默认值为 NULL。  
  
`[ @working_directory = ] 'working_director'` 用于存储发布的数据和架构文件的工作目录的名称。 *working_directory* 为 **nvarchar (255)**，默认值为 NULL。 名称应按 UNC 格式指定。  
  
`[ @use_ftp = ] 'use_ftp'` 指定使用 FTP 而不是常规协议来检索快照。 *use_ftp* 为 **nvarchar (5)**，默认值为 FALSE。  
  
`[ @publication_type = ] publication_type` 指定发布的复制类型。 *publication_type* 为 **tinyint** ，默认值为 **0**。 如果为 **0**，则发布为事务类型。 如果为 **1**，则发布为快照类型。 如果为 **2**，则发布为合并类型。  
  
`[ @dts_package_name = ] 'dts_package_name'` 指定 DTS 包的名称。 *dts_package_name* 是 **sysname** ，默认值为 NULL。 例如，若要将包名称指定为 `DTSPub_Package`，则该参数应为 `@dts_package_name = N'DTSPub_Package'`。  
  
`[ @dts_package_password = ] 'dts_package_password'` 指定包的密码（如果有）。 *dts_package_password* 的 **sysname** 为，默认值为 NULL，这意味着包上不包含密码。  
  
> [!NOTE]  
>  如果指定了 *dts_package_name* ，则必须指定密码。  
  
`[ @dts_package_location = ] 'dts_package_location'` 指定包位置。 *dts_package_location* 为 **nvarchar (12)**，默认值为 **订阅服务器**。 包的位置可以是 **分发服务器** 或 **订阅服务器**。  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @offloadagent = ] 'remote_agent_activation'`
 > [!NOTE]  
>  已不推荐使用远程代理激活，也不再支持该功能。 支持此参数只是为了让脚本能够向后兼容。 如果将 *remote_agent_activation* 设置为 **false** 以外的值，则将生成错误。  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  已不推荐使用远程代理激活，也不再支持该功能。 支持此参数只是为了让脚本能够向后兼容。 将 *remote_agent_server_name* 设置为任何非 NULL 值将生成错误。  
  
`[ @job_name = ] 'job_name'` 现有代理作业的名称。 *job_name* 的值为 **sysname**，默认值为 NULL。 只有在使用现有作业而不是新创建的作业（此为默认设置）来同步订阅时，才需要指定此参数。 如果您不是 **sysadmin** 固定服务器角色的成员，则在指定 *job_name* 时必须指定 *job_login* 和 *job_password* 。  
  
`[ @job_login = ] 'job_login'` 用于运行代理的 Windows 帐户的登录名。 *job_login* 为 **nvarchar (257)**，无默认值。 代理始终可以使用此 Windows 帐户连接到订阅服务器。  
  
`[ @job_password = ] 'job_password'` 运行代理所用的 Windows 帐户的密码。 *job_password* **sysname**，无默认值。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addpullsubscription_agent** 用于快照复制和事务复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-a_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addpullsubscription_agent**。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_change_subscription_properties &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
