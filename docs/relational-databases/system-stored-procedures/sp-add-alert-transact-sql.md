---
description: sp_add_alert (Transact-SQL)
title: sp_add_alert (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_alert
- sp_add_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_alert
ms.assetid: d9b41853-e22d-4813-a79f-57efb4511f09
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b6d7d1e451949d17dc7e4637dc82b86c4225f06c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186040"
---
# <a name="sp_add_alert-transact-sql"></a>sp_add_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建一个警报。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_add_alert [ @name = ] 'name'   
     [ , [ @message_id = ] message_id ]   
     [ , [ @severity = ] severity ]   
     [ , [ @enabled = ] enabled ]  
     [ , [ @delay_between_responses = ] delay_between_responses ]   
     [ , [ @notification_message = ] 'notification_message' ]   
     [ , [ @include_event_description_in = ] include_event_description_in ]   
     [ , [ @database_name = ] 'database' ]   
     [ , [ @event_description_keyword = ] 'event_description_keyword_pattern' ]   
     [ , { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ]   
     [ , [ @raise_snmp_trap = ] raise_snmp_trap ]   
     [ , [ @performance_condition = ] 'performance_condition' ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @wmi_namespace = ] 'wmi_namespace' ]  
     [ , [ @wmi_query = ] 'wmi_query' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @name = ] 'name'` 警报的名称。 该名称显示在为响应警报而发送的电子邮件或寻呼消息中。 它必须唯一，并且可以包含 (**%**) 字符的百分比。 *名称* 为 **sysname**，无默认值。  
  
`[ @message_id = ] message_id` 定义警报的消息错误号。  (它通常与 **sysmessages** 表中的错误号相对应。 ) *message_id* 的值为 **int**，默认值为 **0**。 如果使用 *严重性* 来定义警报，则 *message_id* 必须为 **0** 或 NULL。  
  
> [!NOTE]  
>  只有写入 Microsoft Windows 应用程序日志的 **sysmessages** 错误才会导致发送警报。  
  
`[ @severity = ] severity` 严重性级别 (**1** 到 **25** 个) 来定义警报。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **sysmessages** 表中存储的所有消息都以 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 指定的严重性发送到 Windows 应用程序日志，则会导致发送警报。 *严重性* 为 **int**，默认值为0。 如果使用 *message_id* 来定义警报，则 *严重性* 必须为 **0**。  
  
`[ @enabled = ] enabled` 指示警报的当前状态。 *enabled* 为 **tinyint**，默认值为 1 (启用) 。 如果为 **0**，则不启用警报，也不触发警报。  
  
`[ @delay_between_responses = ] delay_between_responses` 警报响应之间的等待时间（以秒为单位）。 *delay_between_responses* 的值为 **int**，默认值为 **0**，这意味着在每次出现警报时都不会等待响应 (生成响应) 。 响应可以为下面的一种或两种形式：  
  
-   通过电子邮件或寻呼发送的一个或多个通知。  
  
-   要执行的作业。  
  
 例如，当警报在短时间内重复产生时，通过设置该值就有可能避免发送重复的电子邮件。  
  
`[ @notification_message = ] 'notification_message'` 作为电子邮件、 **net send** 或寻呼通知的一部分发送给操作员的可选附加消息。 *notification_message* 为 **nvarchar (512)**，默认值为 NULL。 指定 *notification_message* 可用于添加特定注释，如补救过程。  
  
`[ @include_event_description_in = ] include_event_description_in` 错误的说明是否 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应作为通知消息的一部分包含。 *include_event_description_in* 为 **tinyint**，默认值为 **5** (电子邮件和 **net send**) ，并且可以将这些值中的一个或多个与 **or** 逻辑运算符组合在一起。  
  
> [!IMPORTANT]
>  在 **的未来版本中，将从** 代理中删除寻呼程序和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] net send [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]选项。 请避免在新的开发工作中使用这些功能，并考虑修改当前使用这些功能的应用程序。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|无|  
|**1**|电子邮件|  
|**2**|寻呼机|  
|**4**|**net send**|  
  
`[ @database_name = ] 'database'` 必须发生错误才能触发警报的数据库。 如果未提供 *数据库*，则会触发警报，而不考虑错误发生的位置。 *数据库* 为 **sysname**。 不允许用方括号 ([ ]) 将名称括起来。 默认值为 NULL。  
  
`[ @event_description_keyword = ] 'event_description_keyword_pattern'` 错误说明所需的字符序列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] LIKE 表达式模式匹配字符。 *event_description_keyword_pattern* 为 **nvarchar (100)**，默认值为 NULL。 此参数用于筛选对象名称，例如 **% customer_table%**)  (。  
  
`[ @job_id = ] job_id` 为了响应此警报而运行的作业的标识号。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。  
  
`[ @job_name = ] 'job_name'` 为响应此警报而执行的作业的名称。 *job_name* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。  
  
`[ @raise_snmp_trap = ] raise_snmp_trap` 在版本7.0 中未实现 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *raise_snmp_trap* 为 **tinyint**，默认值为0。  
  
`[ @performance_condition = ] 'performance_condition'` 以 "*itemcomparatorvalue*" 格式表示的值。 *performance_condition* 为 **nvarchar (512)** ，默认值为 NULL，其中包含这些元素。  
  
|格式元素|描述|  
|--------------------|-----------------|  
|*Item*|性能对象、性能计数器或计数器的命名实例|  
|*比较运算符*|以下运算符之一： >、< 或 =|  
|值|计数器的数值|  
  
`[ @category_name = ] 'category'` 警报类别的名称。 *category 的类型* 为 **sysname**，默认值为 NULL。  
  
`[ @wmi_namespace = ] 'wmi_namespace'` 要查询事件的 WMI 命名空间。 *wmi_namespace* 的默认值为 **sysname**，默认值为 NULL。 只支持本地服务器上的命名空间。  
  
`[ @wmi_query = ] 'wmi_query'` 指定警报的 WMI 事件的查询。 *wmi_query* 为 **nvarchar (512)**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 必须从 **msdb** 数据库运行 **sp_add_alert** 。  
  
 在下列情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应用程序产生的错误/消息将发送到 Windows 应用程序日志，并因此引发警报：  
  
-   严重性19或更高 **的 sys. 消息** 错误  
  
-   任何使用 WITH LOG 语法调用的 RAISERROR 语句  
  
-   使用 **sp_altermessage** 修改或创建的任何 **sys.databases 消息** 错误  
  
-   使用 **xp_logevent** 记录的任何事件  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供了一种易用的图形方式来管理整个警报系统，这也是配置警报基础结构的推荐方式。  
  
 如果一个警报没有正常工作，请检查：  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务是否正在运行。  
  
-   事件是否出现在 Windows 应用程序日志中。  
  
-   警报是否已被启用。  
  
-   用 **xp_logevent** 生成的事件在 master 数据库中发生。 因此，除非警报的 \@database_name 为“master”或 NULL，否则 xp_logevent 不会触发警报。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才能执行 **sp_add_alert**。  
  
## <a name="examples"></a>示例  
 以下示例在触发警报时添加一个运行 `Back up the AdventureWorks2012 Database` 作业的警报（测试警报）。  
  
> [!NOTE]  
>  此示例假定消息 55001 和 `Back up the AdventureWorks2012 Database` 作业已存在。 此示例只用于说明用途。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_alert  
    @name = N'Test Alert',  
    @message_id = 55001,   
   @severity = 0,   
   @notification_message = N'Error 55001 has occurred. The database will be backed up...',   
   @job_name = N'Back up the AdventureWorks2012 Database' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_notification &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_altermessage &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [sp_delete_alert &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md)   
 [sp_help_alert &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-alert-transact-sql.md)   
 [sp_update_alert &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)   
 [sys.sysperfinfo &#40;Transact-sql&#41;](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
