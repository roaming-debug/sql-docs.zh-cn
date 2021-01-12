---
description: sysmail_mailattachments (Transact-SQL)
title: sysmail_mailattachments (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_mailattachments_TSQL
- sysmail_mailattachments
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_mailattachments database mail view
ms.assetid: aee87059-a4c1-459a-a95c-641b4e3f0e73
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9fe55316cc27c21849379afe400d2950a5bcf882
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100120"
---
# <a name="sysmail_mailattachments-transact-sql"></a>sysmail_mailattachments (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  提交到数据库邮件的每个附件都在视图中占一行。 如果需要有关数据库附件的信息，则请使用该视图。 若要查看处理的所有电子邮件数据库邮件使用 [sysmail_allitems &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**attachment_id**|**int**|附件的标识符。|  
|**mailitem_id**|**int**|包含附件的邮件项的标识符。|  
|**filename**|**nvarchar (520)**|附件的文件名。 如果 **attach_query_result** 为1且 **query_attachment_filename** 为 NULL，数据库邮件将创建任意文件名。|  
|**filesize**|**int**|附件的大小（字节）。|  
|**接触**|**varbinary(max)**|附件的内容。|  
|**last_mod_date**|**datetime**|上次修改行的日期和时间。|  
|**last_mod_user**|**sysname**|上次修改行的用户。|  
  
## <a name="remarks"></a>备注  
 排除数据库邮件故障时，请使用该视图来查看附件的属性。  
  
 存储在系统表中的附件可能会导致 **msdb** 数据库增长。 使用 **sysmail_delete_mailitems_sp** 删除邮件项及其关联的附件。 有关详细信息，请参阅 [创建 SQL Server 代理作业来存档数据库邮件消息和事件日志](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)。  
  
## <a name="permissions"></a>权限  
 授予 **sysadmin** 固定服务器角色和 **DatabaseMailUserRole** 数据库角色。 当由 **sysadmin** 固定服务器角色的成员执行时，此视图将显示所有附件。 所有其他用户仅可查看他们已提交的消息的附件。  
  
## <a name="see-also"></a>另请参阅  
 [sysmail_allitems &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_faileditems &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [sysmail_sentitems &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md)   
 [sysmail_unsentitems &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md)   
 [sysmail_event_log (Transact-SQL)](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)  
  
  
