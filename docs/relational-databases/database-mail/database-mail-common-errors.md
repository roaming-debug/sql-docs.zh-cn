---
description: 数据库邮件的常见错误
title: 数据库邮件的常见错误 | Microsoft Docs
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
ms.openlocfilehash: 442e95c2e579fa67cb54b801fe6ebbdcd4098ae3
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96125236"
---
# <a name="common-errors-with-database-mail"></a>数据库邮件的常见错误 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

本文介绍了数据库邮件可能遇到的一些常见错误及其解决方案。

## <a name="could-not-find-stored-procedure-sp_send_dbmail"></a>找不到存储过程“sp_send_dbmail”
[sp_send_dbmail](../system-stored-procedures/sp-send-dbmail-transact-sql.md) 存储过程安装在 msdb 数据库中。 必须从 msdb 数据库运行 sp_send_dbmail，或为存储过程指定一个由三部分构成的名称。

示例：
```sql
EXEC msdb.dbo.sp_send_dbmail ...
```

或：

```sql
USE msdb;
GO
EXEC dbo.sp_send_dbmail ...
```

使用[数据库邮件配置向导](configure-database-mail.md)启用和配置数据库邮件。

## <a name="profile-not-valid"></a>配置文件无效
导致出现此消息的可能原因有两个。 指定的配置文件不存在，或者运行 [sp_send_dbmail (Transact-SQL)](../system-stored-procedures/sp-send-dbmail-transact-sql.md) 的用户无权访问配置文件。

要检查配置文件的权限，请使用配置文件名称运行存储过程 [sysmail_help_principalprofile_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-principalprofile-sp-transact-sql.md)。 使用存储过程 [sysmail_add_principalprofile_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-principalprofile-sp-transact-sql.md) 或[数据库邮件配置向导](configure-database-mail.md)为 msdb 用户或组授予访问配置文件的权限。

## <a name="permission-denied-on-sp_send_dbmail"></a>拒绝了针对 sp_send_dbmail 的权限

本主题介绍如何对报告尝试发送数据库邮件的用户不具有执行 sp_send_dbmail 的权限的错误消息进行故障排除。

错误文本如下：

```
EXECUTE permission denied on object 'sp_send_dbmail', 
database 'msdb', schema 'dbo'.
```

要发送数据库邮件，用户必须是 msdb 数据库中的用户，并且是 msdb 数据库中的 DatabaseMailUserRole 数据库角色的成员。 要将 msdb 用户或组添加到此角色中，请使用 SQL Server Management Studio 或对需要发送数据库邮件的用户或角色执行以下语句。

```sql
EXEC msdb.dbo.sp_addrolemember @rolename = 'DatabaseMailUserRole'
    ,@membername = '<user or role name>';
GO
```
有关详细信息，请参阅 [sp_addrolemember](../system-stored-procedures/sp-addrolemember-transact-sql.md) 和 [sp_droprolemember](../system-stored-procedures/sp-droprolemember-transact-sql.md)。

## <a name="database-mail-queued-no-entries-in-sysmail_event_log-or-windows-application-event-log"></a>数据库邮件已排队，但 sysmail_event_log 或 Windows 应用程序事件日志中没有任何项 

数据库邮件使用 Service Broker 来对电子邮件进行排队。 如果数据库邮件已停止，或者尚未在 msdb 数据库中激活 Service Broker 消息传递功能，数据库邮件将对消息进行排队，但无法传递消息。 在这种情况下，Service Broker 消息将保留在 Service Broker 邮件队列中。 由于 Service Broker 未激活外部程序，因此 sysmail_event_log 中没有日志项，并且 sysmail_allitems 和相关视图中的项状态也没有更新。

执行以下语句，检查 msdb 数据库中是否启用了 Service Broker：

```sql
SELECT is_broker_enabled FROM sys.databases WHERE name = 'msdb';
```

值为 0 表示未在 msdb 数据库中激活 Service Broker 消息传递功能。 要解决此问题，请使用以下 Transact-SQL 命令在数据库中激活 Service Broker：

```sql
USE master ;
GO

ALTER DATABASE msdb SET ENABLE_BROKER ;
GO
``` 

数据库邮件依赖于多个内部存储过程。 为了减少外围应用，在新安装的 SQL Server 中，这些存储过程是禁用的。 要启用这些存储过程，请使用 sp_configure 系统存储过程的[数据库邮件 XP 选项](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md)，如以下示例所示：

```sql
EXEC sp_configure 'show advanced options', 1;  
RECONFIGURE;
EXEC sp_configure 'Database Mail XPs', 1;  
RECONFIGURE  
GO  

Database Mail may be stopped in the **msdb** database. To check status of Database Mail, execute the following statement:

```sql
EXECUTE dbo.sysmail_help_status_sp;
```

要在邮件主机数据库中启用数据库邮件，请在 msdb 数据库中运行以下命令：

```sql
EXECUTE dbo.sysmail_start_sp;
```

激活 Service Broker 后，它将检查消息对话框生存期；因此，已处于 Service Broker 传递队列中的对话框生存期长于已配置的对话框生存期的任何消息将立即失败。 数据库邮件会在 [sysmail_allitems](../system-catalog-views/sysmail-allitems-transact-sql.md) 和相关视图中更新失败邮件的状态。 必须决定是否再次发送电子邮件。 有关配置数据库邮件使用的对话框生存期的详细信息，请参阅 [sysmail_configure_sp (Transact-SQL)](../system-stored-procedures/sysmail-configure-sp-transact-sql.md)。



##  <a name="see-also"></a><a name="RelatedContent"></a> 另请参阅
  
-  [数据库邮件概述](database-mail.md)
-  [创建数据库邮件配置文件](create-a-database-mail-profile.md)
  
  
