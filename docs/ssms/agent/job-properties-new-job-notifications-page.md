---
description: 作业属性 - 新建作业（“通知”页）
title: 作业属性 - 新建作业（“通知”页）
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.job.notifications.f1
ms.assetid: ed393cbd-4496-4399-a177-e5baa92fb689
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: eec2f2b28b800a39fdef76ac2f3d2f7ef2d37014
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480758"
---
# <a name="job-properties---new-job-notifications-page"></a>作业属性 - 新建作业（“通知”页）
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此页可以设置在作业完成时 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理要执行的操作。  
  
## <a name="options"></a>选项  
**电子邮件**  
选择此选项将在作业完成时发送电子邮件。 选择此选项后，选择要通知的操作员以及触发该通知的条件：“当作业成功时”；“当作业失败时”；或“当作业完成时”。  
  
**第**  
选择此选项将在作业完成时将电子邮件发送到操作员的寻呼程序。 选择此选项后，指定要通知的操作员以及触发该通知的条件：“当作业成功时”；“当作业失败时”；或“当作业完成时”。  
  
**Net send**  
选择此选项将在作业完成时通过 net send 通知操作员。 选择此选项后，指定要通知的操作员以及触发该通知的条件：“当作业成功时”；“当作业失败时”；或“当作业完成时”。  
  
**写入 Windows 应用程序事件日志**  
选择此选项将在作业完成时将条目写入到应用程序事件日志中。 选择此选项后，指定导致写入条目的条件：“当作业成功时”；“当作业失败时”；或“当作业完成时”。  
  
**自动删除作业**  
选择此选项将在作业完成时删除该作业。 选择此选项后，指定触发删除作业的条件：“当作业成功时”；“当作业失败时”；或“当作业完成时”。  
  
## <a name="see-also"></a>另请参阅  
[执行作业](../../ssms/agent/implement-jobs.md)  
[如何：配置 SQL Server 代理邮件以使用数据库邮件](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
