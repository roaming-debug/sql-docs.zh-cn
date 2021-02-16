---
title: Linux 上的 SQL 代理的 DB 邮件和电子邮件警报
description: 了解如何使用 DB 邮件以及使用 Linux 上的 SQL Server 代理 (mssql-server-agent) 设置电子邮件警报。
author: VanMSFT
ms.author: vanto
ms.date: 02/20/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: tbd
ms.openlocfilehash: 882ab5c8bff2acd7c93cc8cbeeba135152ce76ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348176"
---
# <a name="db-mail-and-email-alerts-with-sql-agent-on-linux"></a>Linux 上的 SQL 代理的 DB 邮件和电子邮件警报

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

以下步骤说明了如何设置 DB 邮件以及如何与 Linux 上的 SQL Server 代理 (mssql-server-agent) 一起使用  。 

## <a name="1-enable-db-mail"></a>1.启用 DB 邮件

```sql
USE master 
GO 
sp_configure 'show advanced options',1 
GO 
RECONFIGURE WITH OVERRIDE 
GO 
sp_configure 'Database Mail XPs', 1 
GO 
RECONFIGURE  
GO  
```

## <a name="2-create-a-new-account"></a>2.创建新帐户
```sql
EXECUTE msdb.dbo.sysmail_add_account_sp 
@account_name = 'SQLAlerts', 
@description = 'Account for Automated DBA Notifications', 
@email_address = 'sqlagenttest@gmail.com', 
@replyto_address = 'sqlagenttest@gmail.com', 
@display_name = 'SQL Agent', 
@mailserver_name = 'smtp.gmail.com', 
@port = 587, 
@enable_ssl = 1, 
@username = 'sqlagenttest@gmail.com', 
@password = '<password>' 
GO
```

## <a name="3-create-a-default-profile"></a>3.创建默认配置文件

```sql
EXECUTE msdb.dbo.sysmail_add_profile_sp 
@profile_name = 'default', 
@description = 'Profile for sending Automated DBA Notifications' 
GO
```

## <a name="4-add-the-database-mail-account-to-a-database-mail-profile"></a>4.将数据库邮件帐户添加到数据库邮件配置文件中
```sql
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp 
@profile_name = 'default', 
@principal_name = 'public', 
@is_default = 1 ; 
 ```
 
## <a name="5-add-account-to-profile"></a>5.将帐户添加到配置文件中 
```sql
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp   
@profile_name = 'default',   
@account_name = 'SQLAlerts',   
@sequence_number = 1;  
 ```
 
## <a name="6-send-test-email"></a>6.发送测试电子邮件
> [!NOTE]
> 你可能需要转到电子邮件客户端，启用“允许不太安全的客户端发送邮件。” 并非所有客户端都将 DB 邮件识别为电子邮件守护程序。

```
EXECUTE msdb.dbo.sp_send_dbmail 
@profile_name = 'default', 
@recipients = 'recipient-email@gmail.com', 
@Subject = 'Testing DBMail', 
@Body = 'This message is a test for DBMail' 
GO
```

## <a name="7-set-db-mail-profile-using-mssql-conf-or-environment-variable"></a>7.使用 mssql-conf 或环境变量设置 DB 邮件配置文件
可以使用 mssql-conf 实用程序或环境变量来注册 DB 邮件配置文件。 在这种情况下，我们会调用配置文件默认值。

```bash
# via mssql-conf
sudo /opt/mssql/bin/mssql-conf set sqlagent.databasemailprofile default
# via environment variable
MSSQL_AGENT_EMAIL_PROFILE=default
```

## <a name="8-set-up-an-operator-for-sqlagent-job-notifications"></a>8.设置 SQLAgent 作业通知的操作员 

```sql
EXEC msdb.dbo.sp_add_operator 
@name=N'JobAdmins',  
@enabled=1, 
@email_address=N'recipient-email@gmail.com',  
@category_name=N'[Uncategorized]' 
GO 
```

## <a name="9-send-email-when-agent-test-job-succeeds"></a>9.“代理测试作业”成功后发送电子邮件 

```
EXEC msdb.dbo.sp_update_job 
@job_name='Agent Test Job', 
@notify_level_email=1, 
@notify_email_operator_name=N'JobAdmins' 
GO
```

## <a name="next-steps"></a>后续步骤
有关如何使用 SQL Server 代理创建、计划和运行作业的详细信息，请参阅[在 Linux 上运行 SQL Server 代理作业](sql-server-linux-run-sql-server-agent-job.md)。
