---
description: 使用数据库邮件发送测试电子邮件
title: 使用数据库邮件发送测试电子邮件 | Microsoft Docs
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
ms.openlocfilehash: 58b9594c7069347f7e458a5a375bdaf2e2d29103
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272348"
---
# <a name="send-a-test-email-with-database-mail"></a>使用数据库邮件发送测试电子邮件  
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

使用“发送测试电子邮件”对话框来测试使用特定配置文件发送邮件的能力。

## <a name="permissions"></a>权限

只有 sysadmin 固定服务器角色的成员才能使用“发送测试电子邮件”对话框。 如果用户不是 sysadmin 固定服务器角色的成员，可以使用 [sp_send_dbmail](../system-stored-procedures/sp-send-dbmail-transact-sql.md) 过程来测试数据库邮件。

## <a name="procedure"></a>过程

1. 使用 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) 中的对象资源管理器，连接到配置了数据库邮件的 SQL Server 数据库引擎实例，展开“管理”，右键单击“数据库邮件”，然后选择“发送测试电子邮件”。 如果不存在数据库邮件配置文件，将通过一个对话框提示用户创建配置文件，同时还会打开“数据库邮件配置向导”。
1. 在 <instance name> 中的“发送测试电子邮件”对话框中，在“数据库邮件配置文件”框中，选择要测试的配置文件。
1. 在“收件人”框中，键入测试电子邮件收件人的电子邮件名称。
1. 在“主题”框中，键入测试电子邮件的主题行。 更改默认主题，以便更好地标识电子邮件以进行故障排除。
1. 在“正文”框中，键入测试电子邮件的正文。 更改默认主题，以便更好地标识电子邮件以进行故障排除。
1. 选择“发送测试电子邮件”，将测试电子邮件发送到数据库邮件队列。
1. 发送测试电子邮件将打开“数据库邮件测试电子邮件”对话框。 请记下“发送电子邮件”框中显示的数字。 这是测试电子邮件的 mailitem_id。 选择“确定”。
1. 在工具栏上，选择“新建查询”以打开“查询编辑器”窗口。 运行以下 T-SQL 语句以确定测试电子邮件的状态：

    ```sql
    SELECT * FROM msdb.dbo.sysmail_allitems 
    WHERE mailitem_id = <the mailitem_id from the previous step> ;
    ```

    sent_status 列将指示是否已发送测试电子邮件。

1. 如果发生错误，则执行以下语句以查看错误消息：

    ```sql
    SELECT * FROM msdb.dbo.sysmail_event_log 
    WHERE mailitem_id = <the mailitem_id from the previous step> ;
    ```


##  <a name="see-also"></a><a name="RelatedContent"></a> 另请参阅 
  
-   [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)
-   [数据库邮件消息处理对象](../../relational-databases/database-mail/database-mail-messaging-objects.md)
-   [数据库邮件外部程序](../../relational-databases/database-mail/database-mail-external-program.md)
-   [数据库邮件日志和审核](../../relational-databases/database-mail/database-mail-log-and-audits.md)
-   [配置 SQL Server 代理邮件以使用数据库邮件](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)
  
  
