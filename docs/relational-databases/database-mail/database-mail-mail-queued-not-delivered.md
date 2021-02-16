---
description: 数据库邮件：邮件已排队，但未送达
title: 数据库邮件：邮件已排队，但未送达 | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ebf8096c4b679cc6d5ac01b164bb9ae4bb014e6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352943"
---
# <a name="database-mail-mail-queued-not-delivered"></a>数据库邮件：邮件已排队，但未送达 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

本主题说明如何解决电子邮件已成功排队但没有传送的问题。

## <a name="diagnose-the-problem"></a>诊断问题 

数据库邮件外部程序在 msdb 数据库中记录电子邮件活动。

首先，要确认数据库邮件是否已启用，请使用 sp_configure 系统存储过程的[数据库邮件 XP 选项](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md)，如以下示例所示：

```sql 
EXEC sp_configure 'show advanced', 1;  
RECONFIGURE; 
EXEC sp_configure; 
GO
```

然后在 msdb 数据库中执行下面的语句，检查邮件队列的状态：

```sql
sysmail_help_queue_sp @queue_type = 'Mail' ;
```

有关列的详细说明，请参阅 [sysmail_help_queue_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-queue-sp-transact-sql.md#result-set)。

检查 sysmail_event_log 视图中的活动。 该视图应当包含这样一项，声明数据库邮件外部程序已经启动。 如果 sysmail_event_log 视图中没有任何项，则可在 sysmail_event_log 中看到[消息已排队，但没有任何项](database-mail-common-errors.md#database-mail-queued-no-entries-in-sysmail_event_log-or-windows-application-event-log)。 如果 sysmail_event_log 视图中存在错误，请解决特定的错误。

如果 sysmail_event_log 视图中有一些项，请在 sysmail_allitems 视图中检查邮件的状态。

## <a name="message-status-unsent"></a>“未发送”邮件状态 

“未发送”状态表示[数据库邮件外部程序](database-mail-external-program.md)尚未处理电子邮件。 数据库邮件外部程序可能在处理邮件方面滞后；外部程序处理邮件的速度取决于网络条件、重试超时、邮件量以及 SMTP 服务器的容量。 如果仍然存在问题，请考虑使用多个配置文件在多个 SMTP 服务器之间分配邮件。

检查已成功传送的邮件的最新修改日期。 如果上次成功发送发生在一段时间以前，请检查 sysmail_event_log 视图，验证外部程序是否由 Service Broker 成功启动。 如果上一次尝试没有启动外部程序，请验证数据库邮件外部程序是否位于正确目录中，并且 SQL Server 服务帐户是否具有运行可执行文件的权限。

   > [!NOTE]
   > 若要删除旧的未发送邮件，请等待无法送达的邮件成为队列中最早的邮件，然后使用 [sysmail_delete_mailitems_sp](../system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md) 删除它们。

## <a name="message-status-retrying"></a>“正在重试”邮件状态

正在重试的状态表示数据库邮件外部程序已经尝试将邮件传送到 SMTP 服务器，但没有成功。 这通常是由网络中断、SMTP 服务器故障或数据库邮件帐户配置不正确导致的。 邮件传送最终不是成功就是失败，并在事件日志中记录一条消息。

## <a name="message-status-sent"></a>“已发送”邮件状态

“已发送”状态表示数据库邮件外部程序已将电子邮件成功发送给 SMTP 服务器。 如果邮件没有抵达目标地址，说明 SMTP 服务器从数据库邮件外部程序接受了邮件，但未将邮件传送给最终收件人。 请检查 SMTP 服务器的日志，或与 SMTP 服务器的管理员联系。 您还可以使用另外的客户端（如 Outlook Express）来测试 SMTP 邮件服务器。

## <a name="message-status-failed"></a>“失败”邮件状态

“失败”状态表示数据库邮件外部程序无法将邮件发送给 SMTP 服务器。 在这种情况下，sysmail_event_log 视图中会包含来自外部程序的详细信息。 有关联接 sysmail_faileditems 和 sysmail_event_log 以检索详细的错误消息的示例查询，请参阅[检查使用数据库邮件发送的电子邮件的状态](check-the-status-of-e-mail-messages-sent-with-database-mail.md)。 最常见的失败原因是目标地址不正确，或者由于网络问题而导致外部程序无法到达一个或多个故障转移帐户。 SMTP 服务器的问题也可能会导致该服务器拒绝邮件。 可以使用数据库邮件配置向导，将“日志记录级别”更改为“详细”，然后发送一封测试邮件来查找故障点。



##  <a name="see-also"></a><a name="RelatedContent"></a> 另请参阅
  
-  [数据库邮件概述](database-mail.md)

  
  
