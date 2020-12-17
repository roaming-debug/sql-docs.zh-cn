---
description: 创建用户定义事件
title: 创建用户定义事件
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent alerts, user-defined events
- user-defined events [SQL Server]
- multiple language support [SQL Server], alerts
- languages [SQL Server], alerts
- severity levels [SQL Server]
- global considerations [SQL Server], alerts
- events [SQL Server], user-defined
- SQL Server Agent alerts, multiple-language environments
- alerts [SQL Server], user-defined events
- alerts [SQL Server], multiple-language environments
- custom events [SQL Server Agent]
- international considerations [SQL Server], alerts
ms.assetid: 03d71a35-97fa-4bba-aa9a-23ac9c9cf879
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: de6830e6256a0cff840d34e179b198803f03016d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464418"
---
# <a name="create-a-user-defined-event"></a>创建用户定义事件
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

如果需要监视非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 预定义的事件，可以创建用户定义事件。 还可以为每个用户定义事件指定严重级别。  
  
> [!NOTE]  
> 使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 时，请为每个用户定义事件消息选择“写入 Windows 应用程序事件日志”选项，以确保记录该消息。 默认情况下，出现严重级别低于 19 的用户定义消息时，不会将其发送到 [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows 应用程序日志。 因此严重级别低于 19 的用户定义消息不会触发 SQL Server 代理警报。  
  
用户定义事件必须具有唯一的消息号。 用户定义事件的消息号必须大于 50,000。 可以使用多种语言来定义事件的消息。 但是，在添加其他语言的错误消息之前， **En-US** 错误消息必须已经存在。  
  
如果管理的是多语言 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 环境，可以使用所支持的每种语言来创建用户定义的消息。 例如，若要创建一个既用于英语服务器又用于德语服务器的新事件消息，可以在两个服务器上使用相同的消息号和严重级别，但为每个服务器指定不同的语言。  
  
下列任务介绍了如何创建用户定义事件以及响应这些事件的警报：  
  
**基于消息号创建警报**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-an-alert-using-an-error-number.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)  
  
**基于严重级别创建警报**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-an-alert-using-severity-level.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)  
  
**定义对警报的响应**  
  
-   [SQL Server Management Studio](../../ssms/agent/define-the-response-to-an-alert-sql-server-management-studio.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)  
  
**创建用户定义事件的错误消息**  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)  
  
**修改用户定义事件的错误消息**  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)  
  
**删除用户定义事件的错误消息**  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)  
  
**禁用或重新激活警报**  
  
-   [SQL Server Management Studio](../../ssms/agent/disable-or-reactivate-an-alert.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
[sp_update_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)  
