---
description: sp_changesubscriber_schedule (Transact-SQL)
title: sp_changesubscriber_schedule (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubscriber_schedule
- sp_changesubscriber_schedule_TSQL
helpviewer_keywords:
- sp_changesubscriber_schedule
ms.assetid: ff84e8e2-d496-482c-b23e-38a6626596e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9e93079a747c3766a47ababf3fdd866053b40968
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171766"
---
# <a name="sp_changesubscriber_schedule-transact-sql"></a>sp_changesubscriber_schedule (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  更改订阅服务器的分发代理或合并代理调度。 此存储过程在发布服务器的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changesubscriber_schedule [ @subscriber = ] 'subscriber', [ @agent_type = ] type  
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
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @subscriber = ] 'subscriber'` 订阅服务器的名称。 *订阅服务器* 是 **sysname**。 订阅服务器的名称必须在数据库中唯一，不能已经存在，不能为 NULL。  
  
`[ @agent_type = ] type` 代理的类型。 *类型* 为 **smallint**，默认值为 **0**。 **0** 指示分发代理。 **1** 表示合并代理。  
  
`[ @frequency_type = ] frequency_type` 用于计划分发任务的频率。 *frequency_type* 的值为 **int**，默认值为 **64**。 有 10 个计划列。  
  
`[ @frequency_interval = ] frequency_interval` 应用于 *frequency_type* 设置的频率的值。 *frequency_interval* 的值为 **int**，默认值为 **1**。  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` 分发任务的日期。 *frequency_relative_interval* 的值为 **int**，默认值为 **1**。  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor`*Frequency_type* 使用的重复因子。 *frequency_recurrence_factor* 的值为 **int**，默认值为 **0**。  
  
`[ @frequency_subday = ] frequency_subday` 在定义的时间段内重新计划的频率（分钟）。 *frequency_subday* 的值为 **int**，默认值为 **4**。  
  
`[ @frequency_subday_interval = ] frequency_subday_interval`*Frequency_subday* 的间隔。 *frequency_subday_interval* 的值为 **int**，默认值为 **5**。  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` 第一次安排分发任务的时间。 *active_start_time_of_day* 的值为 **int**，默认值为 **0**。  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` 停止安排分发任务的时间。 *active_end_time_of_day* 的值为 **int**，默认值为 **235959**，表示 11:59:59 P.M.。 24小时制。  
  
`[ @active_start_date = ] active_start_date` 第一次安排分发任务的日期，格式为 YYYYMMDD。 *active_start_date* 的值为 **int**，默认值为 **0**。  
  
`[ @active_end_date = ] active_end_date` 停止安排分发任务的日期，格式为 YYYYMMDD。 *active_end_date* 的值为 **int**，默认值为 **99991231**，表示9999年12月31日。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  更改发布服务器上的项目属性时，不应使用 *publisher* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changesubscriber_schedule** 在所有类型的复制中使用。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_changesubscriber_schedule** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addsubscriber_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addsubscriber-schedule-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
