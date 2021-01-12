---
description: sysmail_event_log (Transact-SQL)
title: sysmail_event_log (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_event_log
- sysmail_event_log_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_event_log database mail view
ms.assetid: 440bc409-1188-4175-afc4-c68e31e44fed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 58c8e734ed9acdef41b22772c8d3955b825a46de
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096630"
---
# <a name="sysmail_event_log-transact-sql"></a>sysmail_event_log (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  数据库邮件系统返回的每个 Windows 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 消息都在视图中占一行。 此上下文中 (消息是指一条消息，例如错误消息，而不是电子邮件。 ) 使用数据库邮件配置向导的 "**配置系统参数**" 对话框或 [sysmail_configure_sp](../../relational-databases/system-stored-procedures/sysmail-configure-sp-transact-sql.md)存储过程来确定返回的消息，从而配置 **日志记录级别** 参数。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**Log_id**|**int**|日志中项的标识符。|  
|event_type|**varchar (11)**|插入日志中的通告的类型。 可能值为错误、警告、信息性消息、成功消息以及其他内部消息。|  
|**log_date**|**datetime**|记录日志条目的日期和时间。|  
|description|**nvarchar(max)**|要记录的消息的文本。|  
|**process_id**|**int**|数据库邮件外部程序的进程 ID。 每当数据库邮件外部程序启动时，该进程 ID 通常都会更改。|  
|**mailitem_id**|**int**|邮件队列中邮件项的标识符。 如果消息与特定的电子邮件项不相关，则该值为 NULL。|  
|**account_id**|**int**|与事件相关的帐户的 **account_id** 。 如果消息与特定的帐户不相关，则该值为 NULL。|  
|**last_mod_date**|**datetime**|上次修改行的日期和时间。|  
|**last_mod_user**|**sysname**|上次修改行的用户。 对于电子邮件，这是发送邮件的用户。 对于数据库邮件外部程序生成的消息，这是程序的用户上下文。|  
  
## <a name="remarks"></a>备注  
 在对数据库邮件进行故障排除时，请在 **sysmail_event_log** 视图中搜索与电子邮件失败相关的事件。 某些消息（例如，数据库邮件外部程序的失败）并不与特定的电子邮件相关。 若要搜索与特定电子邮件相关的错误，请在 " **sysmail_faileditems** " 视图中查找失败电子邮件的 **mailitem_id** ，然后在 **sysmail_event_log** 中搜索与该 **mailitem_id** 相关的消息。 如果从 **sp_send_dbmail** 返回错误，则不会将电子邮件提交到数据库邮件系统，并且错误不会显示在此视图中。  
  
 如果单个帐户传递尝试失败，则在重试过程中数据库邮件将会包含错误消息，直到邮件项传递成功或者失败为止。 如果最终成功，所有累计错误都将记录为单独的警告，包括 **account_id**。 这样，即使电子邮件已被发送，也会出现警告。 如果出现终极传递失败，则所有以前的警告都将记录为一条错误消息，而不包含 **account_id**，因为所有帐户都失败了。  
  
## <a name="permissions"></a>权限  
 您必须是 **sysadmin** 固定服务器角色或 **DatabaseMailUserRole** 数据库角色的成员才能访问此视图。 不是 **sysadmin** 角色成员的 **DatabaseMailUserRole** 成员只能看到他们提交的电子邮件的事件。  
  
## <a name="see-also"></a>另请参阅  
 [sysmail_faileditems &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [数据库邮件外部程序](../../relational-databases/database-mail/database-mail-external-program.md)  
  
  
