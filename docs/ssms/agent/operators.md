---
description: 运算符
title: 运算符
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- operators (users) [Database Engine]
- fail-safe operator [SQL Server]
- aliases [SQL Server], operators
- SQL Server Agent alerts, operators
- contact information [SQL Server Agent]
- net send [SQL Server]
- e-mail [SQL Server], SQL Server Agent operators
- pager notifications [SQL Server Agent]
- mail [SQL Server], SQL Server Agent operators
- operators (users) [Database Engine], defining
- jobs [SQL Server Agent], operators
- alerts [SQL Server], operators
ms.assetid: 38e8488f-2669-4cea-b9c3-5f394a663678
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 770f6c82bfdafb20d826ae11b52ada0a0b1a5702
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478858"
---
# <a name="operators"></a>运算符
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

操作员是在完成作业或出现警报时可以接收电子通知的人员或组的别名。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务支持通过操作员通知管理员的功能。 运算员可以通知和监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的功能。  
  
## <a name="operator-attributes-and-concepts"></a>运算符属性和概念  
操作员的主要属性如下：  
  
-   操作员名称  
  
-   联系信息  
  
### <a name="naming-an-operator"></a>命名操作员  
每个操作员都必须有一个名称。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中，操作员名称必须是唯一的，并且长度不得超过 **128** 个字符。  
  
### <a name="contact-information"></a>联系信息  
操作员的联系信息决定了通知该操作员的方式。 可以通过电子邮件、寻呼程序或 **net send** ：  
  
> [!IMPORTANT]
> 在 **的未来版本中，将从** 代理中删除寻呼程序和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] net send [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]选项。 请避免在新的开发工作中使用这些功能，并考虑修改当前使用这些功能的应用程序。  
  
-   **电子邮件通知**  
  
    电子邮件通知是向操作员发送电子邮件。 对于电子邮件通知，需要提供操作员的电子邮件地址。  
  
-   **寻呼通知**  
  
    寻呼是通过电子邮件实现的。 对于寻呼通知，需要提供操作员接收寻呼消息的电子邮件地址。 若要设置寻呼通知，必须在邮件服务器上安装软件，处理入站邮件并将其转换为寻呼消息。 该软件可以采用若干种方法，其中包括：  
  
    -   将邮件转发到寻呼服务提供商站点的远程邮件服务器上。  
  
        寻呼服务提供商必须提供这项服务，尽管本地邮件系统上通常都有所需软件。 有关详细信息，请参阅寻呼文档。  
  
    -   通过 Internet 将电子邮件传送到寻呼服务提供商站点的电子邮件服务器。  
  
        这是第一种方法的变体。  
  
    -   使用附加的调制解调器处理入站电子邮件并拨叫寻呼服务。  
  
        此软件为寻呼服务提供商专有。 此软件充当电子邮件客户程序，定期处理收件箱中的邮件，把电子邮件的全部或部分地址信息解释为寻呼号，或将电子邮件名称与转换表中的寻呼号相匹配。  
  
        如果所有操作员共享一个寻呼服务提供商的服务，则可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 指定寻呼到电子邮件系统需要的任何特殊电子邮件格式。 特殊格式可以是前缀或后缀，可以包含在电子邮件的下列行中：  
  
        **使用者：**  
  
        **抄送**：  
  
        **收件人**：  
  
    > [!NOTE]  
    > 如果使用的是一个小容量的字母数字寻呼系统，则可以通过从寻呼通知中排除错误文本，缩短发送文本的长度。 每页限制在 64 个字符内就是一个小容量的字母数字寻呼系统。  
  
-   **net sendnotification**  
  
    此方式通过 **net send** 命令向操作员发送消息。 对于 **net send**，需要指定网络消息的收件人（计算机或用户）。  
  
    > [!NOTE]  
    > **net send** 命令使用 Microsoft Windows Messenger。 若要成功发送警报，此服务必须同时在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机和操作员使用的计算机上运行。  
  
## <a name="alerting-and-fail-safe-operators"></a>警报和防故障操作员  
可以选择在发生警报时要通知的操作员。 例如，可以通过计划警报来指定操作员轮流接收通知， 如果星期一、星期三和星期五出现警报，则通知操作员 A；如果星期二、星期四和星期六出现警报，则通知操作员 B；  
  
向指定操作员发送的所有寻呼通知失败后，防故障操作员将收到警报通知。 例如，如果定义了三个接收寻呼通知的操作员，但无法呼叫到任一指定操作员，则将通知防故障操作员。  
  
在下列情况下将通知防故障操作员：  
  
-   无法通过寻呼呼叫负责警报的操作员。  
  
    无法到达主要操作员的原因包括寻呼地址错误和操作员不在岗位。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理无法访问 **msdb** 数据库中的系统表。  
  
    **sysnotifications** 系统表可指定负责警报的操作员。  
  
防故障操作员是一种安全性能。 在未将防故障职责重新分配给其他操作员或未完全删除防故障分配的情况下，无法删除已分配防故障职责的操作员。  
  
## <a name="notifying-an-operator"></a>通知操作员  
为了通知操作员，必须设置以下一种或多种方式：  
  
-   若要使用数据库邮件功能发送电子邮件，必须具有访问支持 SMTP 的电子邮件服务器的权限。  
  
-   对于寻呼，必须具有第三方的寻呼到电子邮件软件和/或硬件。  
  
-   若要使用 **net send**，操作员必须登录到指定计算机，并且该指定计算机必须允许使用 Windows Messenger 的消息。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务|主题|  
|-|-|  
|与创建操作员相关的任务|[创建操作员](../../ssms/agent/create-an-operator.md)<br /><br />[Designate a Fail-Safe Operator](../../ssms/agent/designate-a-fail-safe-operator.md)|  
|与分配警报相关的任务|[向操作员分配警报](../../ssms/agent/assign-alerts-to-an-operator.md)<br /><br />[定义对警报的响应 (SQL Server Management Studio)](../../ssms/agent/define-the-response-to-an-alert-sql-server-management-studio.md)<br /><br />[sp_add_notification (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)<br /><br />[向操作员分配警报](../../ssms/agent/assign-alerts-to-an-operator.md)|  
  
## <a name="see-also"></a>另请参阅  
[数据库邮件](../../relational-databases/database-mail/database-mail.md)  
