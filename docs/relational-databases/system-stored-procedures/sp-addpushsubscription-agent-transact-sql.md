---
description: sp_addpushsubscription_agent (Transact-SQL)
title: sp_addpushsubscription_agent (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpushsubscription_agent_TSQL
- sp_addpushsubscription_agent
helpviewer_keywords:
- sp_addpushsubscription_agent
ms.assetid: 1fdd2052-50d8-4318-8aa7-fc635d5cad18
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 396e90bb76296270bf25d236a7fd20cd34a063dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206667"
---
# <a name="sp_addpushsubscription_agent-transact-sql"></a>sp_addpushsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  添加新的预定代理作业，以使推送订阅与事务发布同步。 此存储过程在发布服务器上对发布数据库执行。  
  
> [!IMPORTANT]  
>  使用远程分发服务器配置发布服务器时，为所有参数提供的值（包括 *job_login* 和 *job_password*）都会以纯文本方式发送到该分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addpushsubscription_agent [ @publication= ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @job_name = ] 'job_name' ]   
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
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @distribution_job_name = ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @subscriber_provider = ] 'subscriber_provider' ]   
    [ , [ @subscriber_datasrc = ] 'subscriber_datasrc' ]   
    [ , [ @subscriber_location = ] 'subscriber_location' ]  
    [ , [ @subscriber_provider_string = ] 'subscriber_provider_string' ]   
    [ , [ @subscriber_catalog = ] 'subscriber_catalog' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @subscriber = ] 'subscriber'` 订阅服务器实例的名称，如果订阅服务器数据库为可用性组，则为 AG 侦听器的名称。 *订阅服务器* 的值为 **sysname**，默认值为 NULL。 

> [!NOTE]
> 服务器名称可指定为 `<Hostname>,<PortNumber>` 。 如果在 Linux 或 Windows 上使用自定义端口部署 SQL Server，并且禁用了 browser 服务，则可能需要指定连接的端口号。

`[ @subscriber_db = ] 'subscriber_db'` 订阅数据库的名称。 *subscriber_db* 的默认值为 **sysname**，默认值为 NULL。 对于非 SQL Server 订阅服务器，为 *subscriber_db* 指定 **(默认目标)** 的值。  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` 同步时连接到订阅服务器时使用的安全模式。 *subscriber_security_mode* 的值为 **int**，默认值为1。 **0** 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 **1** 指定 Windows 身份验证。  
  
> [!IMPORTANT]  
>  对于在队列中排队的更新订阅，请将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 验证用于与订阅服务器的连接，并为每个订阅服务器连接指定一个不同的帐户。 对于所有其他订阅，则使用 Windows 身份验证。  
  
`[ @subscriber_login = ] 'subscriber_login'` 同步时连接到订阅服务器时要使用的订阅服务器登录名。 *subscriber_login* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @subscriber_password = ] 'subscriber_password'` 订阅服务器密码。 如果 *subscriber_security_mode* 设置为 **0**，则 *subscriber_password* 是必需的。 *subscriber_password* 的默认值为 **sysname**，默认值为 NULL。 如果使用订阅服务器密码，将自动对密码进行加密。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。 如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
`[ @job_login = ] 'job_login'` 运行代理时所用的帐户的登录名。 在 Azure SQL 托管实例使用 SQL Server 帐户。 *job_login* 为 **nvarchar (257)**，默认值为 NULL。 使用 Windows 集成身份验证时，此 Windows 帐户始终用于代理与分发服务器的连接，以及与订阅服务器的连接。  
  
`[ @job_password = ] 'job_password'` 运行代理时所用的帐户的密码。 *job_password* **sysname**，无默认值。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
`[ @job_name = ] 'job_name'` 现有代理作业的名称。 *job_name* 的值为 **sysname**，默认值为 NULL。 只有在使用现有作业而不是新创建的作业（此为默认设置）来同步订阅时，才需要指定此参数。 如果您不是 **sysadmin** 固定服务器角色的成员，则在指定 *job_name* 时必须指定 *job_login* 和 *job_password* 。  
  
`[ @frequency_type = ] frequency_type` 用于计划分发代理的频率。 *frequency_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次性|  
|**2**|按需|  
|**4**|每天|  
|**8**|每周|  
|**16**|每月一次|  
|**32**|与“每月”选项相关|  
|**64** (默认值) |自动启动|  
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
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor`*Frequency_type* 使用的重复因子。 *frequency_recurrence_factor* 的值为 **int**，默认值为0。  
  
`[ @frequency_subday = ] frequency_subday` 在定义的时间段内重新计划的频率。 *frequency_subday* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次|  
|**2**|秒|  
|**4** (默认值) |Minute|  
|**8**|小时|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval`*Frequency_subday* 的间隔。 *frequency_subday_interval* 的值为 **int**，默认值为5。  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` 第一次计划分发代理的时间，格式为 HHMMSS。 *active_start_time_of_day* 的值为 **int**，默认值为0。  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` 停止计划分发代理的时间，格式为 HHMMSS。 *active_end_time_of_day* 的值为 **int**，默认值为235959。  
  
`[ @active_start_date = ] active_start_date` 第一次计划分发代理的日期，格式为 YYYYMMDD。 *active_start_date* 的值为 **int**，默认值为0。  
  
`[ @active_end_date = ] active_end_date` 停止计划分发代理的日期，格式为 YYYYMMDD。 *active_end_date* 的值为 **int**，默认值为99991231。  
  
`[ @dts_package_name = ] 'dts_package_name'` 指定数据转换服务 (DTS) 包的名称。 *dts_package_name* 是 **sysname** ，默认值为 NULL。 例如，若要指定的包名称 `DTSPub_Package` ，则参数将为 `@dts_package_name = N'DTSPub_Package'` 。  
  
`[ @dts_package_password = ] 'dts_package_password'` 指定运行包所需的密码。 *dts_package_password* 的值为 **sysname** ，默认值为 NULL。  
  
> [!NOTE]  
>  如果指定了 *dts_package_name* ，则必须指定密码。  
  
`[ @dts_package_location = ] 'dts_package_location'` 指定包位置。 *dts_package_location* 是 **nvarchar (12)**，默认值为分发服务器。 包的位置可以是 **分发服务器** 或 **订阅服务器**。  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` 指示是否可以通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 同步管理器同步订阅。 *enabled_for_syncmgr* 为 **nvarchar (5)**，默认值为 FALSE。 如果 **为 false**，则不向同步管理器注册订阅。 如果 **为 true**，则会向同步管理器注册订阅，并在不启动的情况下同步订阅 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。  
  
`[ @distribution_job_name = ] 'distribution_job_name'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
`[ @subscriber_provider = ] 'subscriber_provider'` 用于注册非数据源 OLE DB 提供程序的唯一编程标识符 (PROGID) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *subscriber_provider* 的值为 **sysname**，默认值为 NULL。 对于分发服务器上安装的 OLE DB 提供程序， *subscriber_provider* 必须是唯一的。 只有非订阅服务器才支持 subscriber_provider [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @subscriber_datasrc = ] 'subscriber_datasrc'` OLE DB 提供程序理解的数据源的名称。 *subscriber_datasrc* 为 **nvarchar (4000)**，默认值为 NULL。 *subscriber_datasrc* 作为 DBPROP_INIT_DATASOURCE 属性传递以初始化 OLE DB 提供程序。 只有非订阅服务器才支持 subscriber_datasrc [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @subscriber_location = ] 'subscriber_location'` OLE DB 提供程序理解的数据库位置。 *subscriber_location* 为 **nvarchar (4000)**，默认值为 NULL。 *subscriber_location* 作为 DBPROP_INIT_LOCATION 属性传递以初始化 OLE DB 提供程序。 只有非订阅服务器才支持 subscriber_location [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @subscriber_provider_string = ] 'subscriber_provider_string'` 标识数据源 OLE DB 特定于提供程序的连接字符串。 *subscriber_provider_string* 为 **nvarchar (4000)**，默认值为 NULL。 *subscriber_provider_string* 传递给 IDataInitialize，或设置为 DBPROP_INIT_PROVIDERSTRING 属性以初始化 OLE DB 提供程序。 只有非订阅服务器才支持 subscriber_provider_string [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @subscriber_catalog = ] 'subscriber_catalog'` 与 OLE DB 提供程序建立连接时要使用的目录。 *subscriber_catalog* 的值为 **sysname**，默认值为 NULL。 *subscriber_catalog* 作为 DBPROP_INIT_CATALOG 属性传递以初始化 OLE DB 提供程序。 只有非订阅服务器才支持 subscriber_catalog [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addpushsubscription_agent** 用于快照复制和事务复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/sp-addpushsubscription-a_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addpushsubscription_agent**。  
  
## <a name="see-also"></a>另请参阅  
 [ssSDSFull](../../relational-databases/replication/create-a-push-subscription.md)   
 [为非 SQL Server 订阅服务器创建订阅](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [复制存储过程 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_addsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
