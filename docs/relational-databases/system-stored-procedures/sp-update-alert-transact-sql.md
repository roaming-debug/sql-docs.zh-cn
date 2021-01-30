---
description: sp_update_alert (Transact-SQL)
title: sp_update_alert (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_alert_TSQL
- sp_update_alert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_alert
ms.assetid: 4bbaeaab-8aca-4c9e-abc1-82ce73090bd3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c9dce24d3f8dd20d2138ba153fee74212e603e6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184757"
---
# <a name="sp_update_alert-transact-sql"></a>sp_update_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更新现有警报的设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_update_alert   
     [ @name =] 'name'   
     [ , [ @new_name =] 'new_name']   
     [ , [ @enabled =] enabled]   
     [ , [ @message_id =] message_id]   
     [ , [ @severity =] severity]   
     [ , [ @delay_between_responses =] delay_between_responses]   
     [ , [ @notification_message =] 'notification_message']   
     [ , [ @include_event_description_in =] include_event_description_in]   
     [ , [ @database_name =] 'database']   
     [ , [ @event_description_keyword =] 'event_description_keyword']   
     [ , [ @job_id =] job_id | [@job_name =] 'job_name']   
     [ , [ @occurrence_count = ] occurrence_count]   
     [ , [ @count_reset_date =] count_reset_date]   
     [ , [ @count_reset_time =] count_reset_time]   
     [ , [ @last_occurrence_date =] last_occurrence_date]   
     [ , [ @last_occurrence_time =] last_occurrence_time]   
     [ , [ @last_response_date =] last_response_date]   
     [ , [ @last_response_time =] last_response _time]  
     [ , [ @raise_snmp_trap =] raise_snmp_trap]  
     [ , [ @performance_condition =] 'performance_condition' ]   
     [ , [ @category_name =] 'category']  
     [ , [ @wmi_namespace = ] 'wmi_namespace' ]  
     [ , [ @wmi_query = ] 'wmi_query' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @name = ] 'name'` 要更新的警报的名称。 *名称* 为 **sysname**，无默认值。  
  
`[ @new_name = ] 'new_name'` 警报的新名称。 该名称必须是唯一的。 *new_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @enabled = ] enabled` 指定是否 (**1**) 或未启用 (**0**) 启用警报。 *enabled* 为 **tinyint**，默认值为 NULL。 必须启用警报，才能激发警报。  
  
`[ @message_id = ] message_id` 警报定义的新消息或错误号。 通常， *message_id* 与 **sysmessages** 表中的错误号相对应。 *message_id* 的值为 **int**，默认值为 NULL。 仅当警报的严重性级别设置为 **0** 时，才能使用消息 ID。  
  
`[ @severity = ] severity` 警报定义 (**1** 到 **25**) 的新严重性级别。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发送到具有指定严重性的 Windows 应用程序日志的任何消息都将激活警报。 *严重性* 为 **int**，默认值为 NULL。 仅当警报的消息 ID 设置为 **0** 时，才能使用严重性级别。  
  
`[ @delay_between_responses = ] delay_between_responses` 警报响应之间的新等待时间（以秒为单位）。 *delay_between_responses* 的值为 **int**，默认值为 NULL。  
  
`[ @notification_message = ] 'notification_message'` 作为电子邮件、 **net send** 或寻呼通知的一部分发送给操作员的其他消息的修订文本。 *notification_message* 为 **nvarchar (512)**，默认值为 NULL。  
  
`[ @include_event_description_in = ] include_event_description_in` 指定是否 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应该在通知消息中包含 Windows 应用程序日志中的错误说明。 *include_event_description_in* 的数据值为 **tinyint**，默认值为 NULL，可以是下列值中的一个或多个。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|无|  
|**1**|电子邮件|  
|**2**|寻呼机|  
|**4**|**net send**|  
|**7**|All|  
  
`[ @database_name = ] 'database'` 必须发生错误才能触发警报的数据库的名称。 *数据库* 为 **sysname。** 不允许用方括号 ([ ]) 将名称括起来。 默认值为 NULL。  
  
`[ @event_description_keyword = ] 'event_description_keyword'` 必须在错误消息日志中的错误说明中找到的字符序列。 可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] LIKE 表达式模式匹配字符。 *event_description_keyword* 为 **nvarchar (100)**，默认值为 NULL。 此参数用于筛选对象名称，例如 **% customer_table%**)  (。  
  
`[ @job_id = ] job_id` 作业标识号。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。 如果指定 *job_id* ，则必须省略 *job_name* 。  
  
`[ @job_name = ] 'job_name'` 为响应此警报而执行的作业的名称。 *job_name* 的默认值为 **sysname**，默认值为 NULL。 如果指定 *job_name* ，则必须省略 *job_id* 。  
  
`[ @occurrence_count = ] occurrence_count` 重置警报发生的次数。 *occurrence_count* 的值为 **int**，默认值为 NULL，且只能设置为 **0**。  
  
`[ @count_reset_date = ] count_reset_date` 重置上一次重置发生计数的日期。 *count_reset_date* 的值为 **int**，默认值为 NULL。  
  
`[ @count_reset_time = ] count_reset_time` 重置上一次重置发生计数的时间。 *count_reset_time* 的值为 **int**，默认值为 NULL。  
  
`[ @last_occurrence_date = ] last_occurrence_date` 重置警报上一次发生的日期。 *last_occurrence_date* 的值为 **int**，默认值为 NULL，且只能设置为 **0**。  
  
`[ @last_occurrence_time = ] last_occurrence_time` 重置警报上一次发生的时间。 *last_occurrence_time* 的值为 **int**，默认值为 NULL，且只能设置为 **0**。  
  
`[ @last_response_date = ] last_response_date` 重置 SQLServerAgent 服务上一次响应警报的日期。 *last_response_date* 的值为 **int**，默认值为 NULL，且只能设置为 **0**。  
  
`[ @last_response_time = ] last_response_time` 重置 SQLServerAgent 服务上一次响应警报的时间。 *last_response_time* 的值为 **int**，默认值为 NULL，且只能设置为 **0**。  
  
`[ @raise_snmp_trap = ] raise_snmp_trap` 保护.  
  
`[ @performance_condition = ] 'performance_condition'` 以 **"**_itemcomparatorvalue_**"** 格式表示的值。 *performance_condition* 为 **nvarchar (512)**，默认值为 NULL，其中包含这些元素。  
  
|格式元素|描述|  
|--------------------|-----------------|  
|*Item*|性能对象、性能计数器或计数器的命名实例|  
|*比较运算符*|以下运算符之一： **>** 、 **<** 、 **=**|  
|值|计数器的数值|  
  
`[ @category_name = ] 'category'` 警报类别的名称。 *category 的类型* 为 **sysname** ，默认值为 NULL。  
  
`[ @wmi_namespace = ] 'wmi_namespace'` 要查询事件的 WMI 命名空间。 *wmi_namespace* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @wmi_query = ] 'wmi_query'` 指定警报的 WMI 事件的查询。 *wmi_query* 为 **nvarchar (512)**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 只有写入到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 应用程序日志的 sysmessages 才会触发警报。  
  
 **sp_update_alert** 仅更改为其提供参数值的警报设置。 如果省略某一参数，则保留其当前设置。  
  
## <a name="permissions"></a>权限  
 若要运行此存储过程，用户必须是 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 以下示例将已启用的 `Test Alert` 设置更改为 `0`。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_alert  
    @name = N'Test Alert',  
    @enabled = 0 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sp_help_alert &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-alert-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
