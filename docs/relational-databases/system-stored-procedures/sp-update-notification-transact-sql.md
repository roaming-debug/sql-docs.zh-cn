---
description: sp_update_notification (Transact-SQL)
title: sp_update_notification (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_notification_TSQL
- sp_update_notification
dev_langs:
- TSQL
helpviewer_keywords:
- sp_updatenotification
ms.assetid: 3e1c3d40-8c24-46ce-a68e-ce6c6a237fda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7e1f54c6933945c6031e48d4f97ab89f0f966779
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184316"
---
# <a name="sp_update_notification-transact-sql"></a>sp_update_notification (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更新警报通知的通知方法。  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_update_notification  
          [@alert_name =] 'alert' ,  
     [@operator_name =] 'operator' ,  
     [@notification_method =] notification  
```  
  
## <a name="arguments"></a>参数  
`[ @alert_name = ] 'alert'` 与此通知关联的警报的名称。 *警报* 为 **sysname**，无默认值。  
  
`[ @operator_name = ] 'operator'` 警报发生时要通知的操作员。 *运算符* 的值为 **sysname**，无默认值。  
  
`[ @notification_method = ] notification` 通知操作员使用的方法。 *通知* 为 **tinyint**，无默认值，并且可以是这些值中的一个或多个。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|电子邮件|  
|**2**|寻呼机|  
|**4**|**net send**|  
|**7**|所有方法|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 必须从 **msdb** 数据库运行 **sp_update_notification** 。  
  
 您可以使用指定的 *notification_method* 为没有必需的地址信息的操作员更新通知。 如果发送电子邮件或寻呼通知时出现故障，则该故障将记录到 Microsoft SQL Server 代理程序错误日志中。  
  
## <a name="permissions"></a>权限  
 若要运行此存储过程，用户必须被授予 **sysadmin** 固定服务器角色。  
  
## <a name="examples"></a>示例  
 下面的示例修改发送给警报的通知的通知方法 `François Ajenstat` `Test Alert` 。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_notification  
   @alert_name = N'Test Alert',  
   @operator_name = N'François Ajenstat',  
   @notification_method = 7;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_notification &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_delete_notification &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-notification-transact-sql.md)   
 [sp_help_notification &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-notification-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
