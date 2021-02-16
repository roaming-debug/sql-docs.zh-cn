---
description: 常规数据库邮件故障排除步骤
title: 常规数据库邮件故障排除
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-dt-2019
ms.openlocfilehash: 2e9b7f5b5aec4f1b111983f893783e46ad14e021
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352948"
---
# <a name="general-database-mail-troubleshooting-steps"></a>常规数据库邮件故障排除步骤 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

数据库邮件故障排除涉及到对数据库邮件系统进行下列方面的常规检查。 这些过程按逻辑顺序介绍，但可以按任何顺序进行评估。

## <a name="permissions"></a>权限

只有 sysadmin 固定服务器角色的成员才能对数据库邮件的所有方面进行故障排除。 如果用户不是 sysadmin 固定服务器角色的成员，则只能获得他们尝试发送的电子邮件的有关信息，而不能获得其他用户发送的电子邮件的有关信息。

## <a name="is-database-mail-enabled"></a>是否启用数据库邮件

1. 在 SQL Server Management Studio 中，使用查询编辑器窗口连接到 SQL Server 实例，然后执行以下代码：

    ```sql
    sp_configure 'show advanced', 1; 
    GO
    RECONFIGURE;
    GO
    sp_configure;
    GO
    ```

   在结果窗格中，确认[数据库邮件 XP](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) 的 run_value 是否设置为 1。
   如果 run_value 不是 1，则未启用数据库邮件。 数据库邮件不会自动启用，以减少恶意用户可用来发起攻击的功能数。 有关详细信息，请参阅[了解外围应用配置](../security/surface-area-configuration.md)。

1. 如果您确定可以启用数据库邮件，请执行下面的代码：

    ```sql
    sp_configure 'Database Mail XPs', 1; 
    GO
    RECONFIGURE;
    GO
    ```

1. 要将 sp_configure 过程还原为不显示高级选项的默认状态，请执行下面的代码：

    ```sql 
    sp_configure 'show advanced', 0; 
    GO
    RECONFIGURE;
    GO
    ```

## <a name="are-users-properly-configured-to-send-mail"></a>用户是否已正确配置为发送邮件

1. 要发送数据库邮件，用户必须是 msdb 数据库中 DatabaseMailUserRole 数据库角色的成员。 sysadmin 固定服务器角色和 msdb db_owner 角色的成员将自动成为 DatabaseMailUserRole 角色的成员。 要列出 DatabaseMailUserRole 的所有其他成员，请执行以下语句：

    ```sql
    EXEC msdb.sys.sp_helprolemember 'DatabaseMailUserRole';
    ```

1. 要将用户添加到 DatabaseMailUserRole 角色中，请使用以下语句：

    ```sql
    USE msdb;
    GO
    
    sp_addrolemember @rolename = 'DatabaseMailUserRole'
    ,@membername = '<database user>';
    ```

1. 若要发送数据库邮件，用户必须有权访问至少一个数据库邮件配置文件。 若要列出用户（主体）及其有权访问的配置文件，请执行以下语句：

    ```sql
    EXEC msdb.dbo.sysmail_help_principalprofile_sp;
    ```

1. 使用数据库邮件配置向导创建配置文件并授予用户访问这些配置文件的权限。
 
## <a name="is-database-mail-started"></a>是否启动数据库邮件

1. 当有电子邮件要处理时，将激活[数据库邮件外部程序](database-mail-external-program.md)。 如果达到指定的超时期限时没有邮件要发送，该程序将退出。 要确认是否已启动数据库邮件激活，请执行以下语句：

    ```sql
    EXEC msdb.dbo.sysmail_help_status_sp;
    ```
1. 如果未启动数据库邮件激活，请执行以下语句将其启动：

    ```sql
    EXEC msdb.dbo.sysmail_start_sp;
    ```

1. 如果已启动数据库邮件外部程序，请使用以下语句检查邮件队列的状态：

    ```sql
    EXEC msdb.dbo.sysmail_help_queue_sp @queue_type = 'mail';
    ```
  
   邮件队列的状态应为 RECEIVES_OCCURRING。 邮件队列的状态可能会不时发生改变。 如果邮件队列的状态不是 RECEIVES_OCCURRING，请尝试重启队列。 请使用以下语句停止队列：
   
```sql
EXEC msdb.dbo.sysmail_stop_sp;
```

请使用以下语句启动队列：

```sql
EXEC msdb.dbo.sysmail_start_sp;
```

  > [!NOTE]
  >  使用 sysmail_help_queue_sp 结果集中的长度列确定邮件队列中的电子邮件数量。

## <a name="do-problems-affect-some-or-all-accounts"></a>问题是否影响某些或所有帐户

1. 如果确定只有某些配置文件可以发送邮件，而不是所有配置文件都可以，则可能是有问题的配置文件所使用的数据库邮件帐户存在问题。 若要确定哪些帐户可以成功发送邮件，请执行以下语句：

    ```sql
    SELECT sent_account_id, sent_date FROM msdb.dbo.sysmail_sentitems;
    ```

1. 如果有问题的配置文件未使用列出的任何帐户，那么可能是该配置文件可以使用的所有帐户都不能正常工作。 若要测试各个帐户，请使用数据库邮件配置向导创建一个只包含一个用户的新配置文件，然后通过“发送测试电子邮件”对话框使用新帐户发送邮件。 
1. 若要查看数据库邮件返回的错误消息，请执行以下语句：

    ```sql
    SELECT * FROM msdb.dbo.sysmail_event_log;
    ```

   > [!NOTE]
   > 当邮件被成功传递到 SMTP 邮件服务器上时，数据库邮件即认为邮件已被发送。 虽然后续错误（例如，收件人的电子邮件地址无效）仍然可能会使邮件无法传递，但是不会包含在数据库邮件日志中。

## <a name="retry-mail-delivery"></a>重试邮件发送

1. 如果确定数据库邮件失败是由于无法可靠地到达 SMTP 服务器而导致的，则可以通过增大数据库邮件尝试发送每封邮件的次数来增加邮件传送的成功率。 启动数据库邮件配置向导，然后选择“查看或更改系统参数”选项。 还可以将更多帐户与配置文件相关联，这样，当从主帐户进行故障转移时，数据库邮件将使用故障转移帐户发送电子邮件。
1. 在“配置系统参数”页上，“帐户重试次数”的默认值为 5 次，“帐户重试延迟时间”的默认值为 60 秒，这意味着如果在 5 分钟之内不能到达 SMTP 服务器，邮件发送将失败。 增大这些参数可以延长邮件发送失败之前的时间。

    > [!NOTE]
    > 当发送大量邮件时，虽然较大的默认值可以提高可靠性，但会显著增加资源的使用量，因为会一遍又一遍地尝试传递大量邮件。 若要从根本上解决问题，需要解决阻碍数据库邮件与 SMTP 服务器快速建立联系的网络问题或 SMTP 服务器问题。



##  <a name="see-also"></a><a name="RelatedContent"></a> 另请参阅
  
-   [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)  
  
-   [数据库邮件消息处理对象](../../relational-databases/database-mail/database-mail-messaging-objects.md)  
  
-   [数据库邮件外部程序](../../relational-databases/database-mail/database-mail-external-program.md)  
  
-   [数据库邮件日志和审核](../../relational-databases/database-mail/database-mail-log-and-audits.md)  
  
-   [配置 SQL Server 代理邮件以使用数据库邮件](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
  
  
