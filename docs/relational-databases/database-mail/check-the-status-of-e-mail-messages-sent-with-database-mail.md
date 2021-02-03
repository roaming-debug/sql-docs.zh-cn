---
description: 检查使用数据库邮件发送的电子邮件的状态
title: 使用数据库邮件发送的电子邮件的状态
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- e-mail [SQL Server], status information
- mail [SQL Server], status information
- Database Mail [SQL Server], message status
- status information [Database Mail]
ms.assetid: eb290f24-b52f-46bc-84eb-595afee6a5f3
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
ms.openlocfilehash: 0b230cc2a880b16b62934c3a89af475dd5cecf96
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251089"
---
# <a name="check-the-status-of-e-mail-messages-sent-with-database-mail"></a>检查使用数据库邮件发送的电子邮件的状态
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中如何使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]检查通过数据库邮件发送的电子邮件的状态。  
  
-   **开始之前：**  
  
-   **若要查看通过数据库邮件发送的电子邮件的状态，使用：**  [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 数据库邮件保留发送的电子邮件的副本，并在 **msdb** 数据库的 **sysmail_allitems**、 **sysmail_sentitems**、 **sysmail_unsentitems** 、 **sysmail_faileditems** 视图中显示它们。 数据库邮件外部程序记录活动，并通过 Windows 应用程序事件日志以及 **msdb** 数据库的 **sysmail_event_log** 视图显示日志。 若要检查电子邮件的状态，请对此视图运行查询。 电子邮件可以处于下列四种可能的状态之一： **sent**、 **unsent**、 **retrying** 和 **failed**。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **查看通过数据库邮件发送的电子邮件的状态**  
  
1.  从 **sysmail_allitems** 表中选择，按 **mailitem_id** 或 **sent_status** 指定要检查的邮件。  
  
2.  若要检查外部程序返回的电子邮件的状态，请将 **sysmail_allitems** 联接到 **sysmail_event_log** 视图中的 **mailitem_id** 列，如下所示。  
  
     默认情况下，外部程序不会记录有关发送成功的邮件的信息。 若要记录所有邮件，请使用 **“数据库邮件配置向导”** 的 **“配置系统参数”** 页，将日志级别设置为“详细”。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 下面的示例列出了有关发送到 `danw` 的所有电子邮件（外部程序无法成功发送）的信息。 该语句列出了邮件的主题、外部程序发送邮件失败的日期和时间以及来自数据库邮件日志的错误消息。  
  
```  
USE msdb ;  
GO  
  
-- Show the subject, the time that the mail item row was last  
-- modified, and the log information.  
-- Join sysmail_faileditems to sysmail_event_log   
-- on the mailitem_id column.  
-- In the WHERE clause list items where danw was in the recipients,  
-- copy_recipients, or blind_copy_recipients.  
-- These are the items that would have been sent  
-- to danw.  
  
SELECT items.subject,  
    items.last_mod_date  
    ,l.description FROM dbo.sysmail_faileditems as items  
INNER JOIN dbo.sysmail_event_log AS l  
    ON items.mailitem_id = l.mailitem_id  
WHERE items.recipients LIKE '%danw%'    
    OR items.copy_recipients LIKE '%danw%'   
    OR items.blind_copy_recipients LIKE '%danw%'  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件日志和审核](../../relational-databases/database-mail/database-mail-log-and-audits.md)  
  
  
