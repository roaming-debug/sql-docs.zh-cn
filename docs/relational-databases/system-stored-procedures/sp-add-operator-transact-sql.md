---
description: sp_add_operator (Transact-SQL)
title: sp_add_operator (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_operator
- sp_add_operator_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_operator
ms.assetid: 817cd98a-4dff-4ed8-a546-f336c144d1e0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6cb3aed762c87430e75c14c954c80e58ab70d81e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195886"
---
# <a name="sp_add_operator-transact-sql"></a>sp_add_operator (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建用于警报和作业的操作员（通知收件人）。  
  
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_add_operator [ @name = ] 'name'   
     [ , [ @enabled = ] enabled ]   
     [ , [ @email_address = ] 'email_address' ]   
     [ , [ @pager_address = ] 'pager_address' ]   
     [ , [ @weekday_pager_start_time = ] weekday_pager_start_time ]   
     [ , [ @weekday_pager_end_time = ] weekday_pager_end_time ]   
     [ , [ @saturday_pager_start_time = ] saturday_pager_start_time ]   
     [ , [ @saturday_pager_end_time = ] saturday_pager_end_time ]   
     [ , [ @sunday_pager_start_time = ] sunday_pager_start_time ]   
     [ , [ @sunday_pager_end_time = ] sunday_pager_end_time ]   
     [ , [ @pager_days = ] pager_days ]   
     [ , [ @netsend_address = ] 'netsend_address' ]   
     [ , [ @category_name = ] 'category' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @name = ] 'name'` 操作员 (通知收件人) 的名称。 此名称必须唯一，并且不能包含 (**%**) 字符的百分比。 *名称* 为 **sysname**，无默认值。  
  
`[ @enabled = ] enabled` 指示运算符的当前状态。 *enabled* 为 **tinyint**，默认值为 **1** (启用) 。 如果为 **0**，则不启用操作员且不会收到通知。  
  
`[ @email_address = ] 'email_address'` 操作员的电子邮件地址。 此字符串将直接传递到电子邮件系统。 *email_address* 为 **nvarchar (100)**，默认值为 NULL。  
  
 可以为 *email_address* 指定物理电子邮件地址或别名。 例如：  
  
 "**jdoe**" 或 "**jdoe \@ xyz.com**"  
  
> [!NOTE]  
>  必须对数据库邮件使用电子邮件地址。  
  
`[ @pager_address = ] 'pager_address'` 操作员的寻呼地址。 此字符串将直接传递到电子邮件系统。 *pager_address* 为 **nvarchar (100)**，默认值为 NULL。  
  
`[ @weekday_pager_start_time = ] weekday_pager_start_time`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]代理在工作日（从星期一到星期五）向指定操作员发送寻呼通知的时间。 *weekday_pager_start_time* 的值为 **int**，默认值为 **090000**，指示 9:00 A.M。 并且必须使用 HHMMSS 格式输入。  
  
`[ @weekday_pager_end_time = ] weekday_pager_end_time` 一个时间，在此时间之后， **SQLServerAgent** 服务不再向星期一到星期五的工作日发送寻呼通知。 *weekday_pager_end_time* 的值为 **int**，默认值为180000，表示 6:00 P.M.。 并且必须使用 HHMMSS 格式输入。  
  
`[ @saturday_pager_start_time = ] saturday_pager_start_time` 一个时间，在此时间之后， **SQLServerAgent** 服务将寻呼通知发送到星期六上的指定操作员。 *saturday_pager_start_time* 的值为 **int**，默认值为090000，指示 9:00 A.M。 并且必须使用 HHMMSS 格式输入。  
  
`[ @saturday_pager_end_time = ] saturday_pager_end_time` 一个时间，在此之后， **SQLServerAgent** 服务不再向星期六的指定操作员发送寻呼通知。 *saturday_pager_end_time* 的值为 **int**，默认值为 **180000**，表示 6:00 P.M.。 并且必须使用 HHMMSS 格式输入。  
  
`[ @sunday_pager_start_time = ] sunday_pager_start_time` 一个时间，在此之后， **SQLServerAgent** 服务将寻呼通知发送到星期日上的指定操作员。 *sunday_pager_start_time* 的值为 **int**，默认值为 **090000**，指示 9:00 A.M。 并且必须使用 HHMMSS 格式输入。  
  
`[ @sunday_pager_end_time = ] sunday_pager_end_time` 时间，在此之后， **SQLServerAgent** 服务不再将寻呼通知发送到星期日上的指定操作员。 *sunday_pager_end_time* 的值为 **int**，默认值为 **180000**，表示 6:00 P.M.。 并且必须使用 HHMMSS 格式输入。  
  
`[ @pager_days = ] pager_days` 一个数字，用于指示在指定的开始/结束时间) ，操作员可用于的页数 (。 *pager_days* 为 **tinyint**，默认值为 **0** ，表示运算符从不可用于接收页面。 有效值为 **0** 至 **127**。 *pager_days* 是通过添加所需日期的各个值来计算的。 例如，从星期一到星期五是 **2** + **4** + **8** + **16** + **32**  =  **62**。 下表列出了一周中每天的值。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|星期日|  
|**2**|星期一|  
|**4**|星期二|  
|**8**|星期三|  
|**16**|星期四|  
|**32**|星期五|  
|**64**|星期六|  
  
`[ @netsend_address = ] 'netsend_address'` 向其发送网络消息的操作员的网络地址。 *netsend_address* 为 **nvarchar (100)**，默认值为 NULL。  
  
`[ @category_name = ] 'category'` 此运算符的类别名称。 *category 的类型* 为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 必须从 **msdb** 数据库运行 **sp_add_operator** 。  
  
 电子邮件系统支持寻呼，如果想使用寻呼，则该系统必须有将电子邮件发送到寻呼程序的功能。  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 为管理作业提供了一种图形化的简便方法，建议使用此方法来创建和管理作业基础结构。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_add_operator** 执行。  
  
## <a name="examples"></a>示例  
 下面的示例设置 `danwi` 的操作员信息。 操作员已被启用。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理通过寻呼在星期一到星期五的上午 8 点 至下午 5 点发送通知。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_operator  
    @name = N'Dan Wilson',  
    @enabled = 1,  
    @email_address = N'danwi',  
    @pager_address = N'5551290AW@pager.Adventure-Works.com',  
    @weekday_pager_start_time = 080000,  
    @weekday_pager_end_time = 170000,  
    @pager_days = 62 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_delete_operator &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)   
 [sp_help_operator &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [sp_update_operator &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
