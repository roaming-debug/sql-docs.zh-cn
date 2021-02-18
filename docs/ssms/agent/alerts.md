---
description: 警报
title: 警报
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent alerts, event types
- SQL Server Agent alerts, names
- alerts [SQL Server], performance conditions
- alerts [SQL Server], about alerts
- WMI event responses [SQL Server Agent alerts]
- events [WMI]
- performance condition alerts [SQL Server Agent]
- alerts [SQL Server], event types
- SQL Server Agent alerts, performance conditions
- SQL Server Agent alerts, about alerts
- alerts [SQL Server], names
ms.assetid: 3f57d0f0-4781-46ec-82cd-b751dc5affef
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 934fa73f84989996be6797d2119f834f7e650053
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354871"
---
# <a name="alerts"></a>警报

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

事件由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成并被输入到 [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows 应用程序日志中。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理读取应用程序日志，并将写入的事件与定义的警报比较。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理找到匹配项时，它将发出自动响应事件的警报。 除了监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件以外， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理还监视性能条件和 Windows Management Instrumentation (WMI) 事件。  
  
若要定义警报，需要指定：  
  
-   警报的名称。  
  
-   触发警报的事件或性能条件。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理响应事件或性能条件所执行的操作。  
  
## <a name="naming-an-alert"></a>命名警报  
每个警报都必须有一个名称。 警报名称在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例内必须唯一，并且不能超过 **128** 个字符。  
  
## <a name="selecting-an-event-type"></a>选择事件类型  
一个警报响应一种特定的事件。 警报响应下列事件类型：  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 性能条件  
  
-   WMI 事件  
  
事件类型决定了用于指定具体事件的参数。  
  
## <a name="specifying-a-sql-server-event"></a>指定 SQL Server 事件  
可以指定一个警报响应一个或多个事件。 使用下列参数来指定触发警报的事件：  
  
-   **错误号**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在发生特定错误时发出警报。 例如，可以指定错误号 2571 来响应未经授权就尝试调用数据库控制台命令 (DBCC) 的操作。  
  
-   **严重级别**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在发生特定级别的严重错误时发出警报。 例如，可以指定严重级别 15 来响应 Transact-SQL 语句中的语法错误。  
  
-   **Database**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理仅在特定数据库中发生事件时才发出警报。 此选项是对错误号或严重级别的补充。 例如，如果实例中包含一个用于生产的数据库和一个用于报告的数据库，可以定义仅响应生产数据库中的语法错误的警报。  
  
-   **事件文本**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在指定事件的事件消息中包含特定文本字符串时发出警报。 例如，可以定义警报来响应包含特定表名或特定约束的消息。  
  
## <a name="selecting-a-performance-condition"></a>选择性能条件  
可以指定警报来响应特定的性能条件。 在这种情况下，需要指定要监视的性能计数器、警报的阈值以及警报发生时计数器必须执行的操作。 若要设置性能条件，必须在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的“新建警报”或“警报属性”对话框中的“常规”页上定义下列项：  
  
-   **Object**  
  
    对象是要监视的性能区域。  
  
-   **计数器**  
  
    计数器是要监视的区域的属性。  
  
-   **实例**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例定义了要监视的属性的特定实例（如果存在）。  
  
-   **计数器** 和 **值**  
  
    警报的阈值和导致警报的行为。 阈值是数字。 行为是下列之一：“低于”、“等于”或“大于”指定的值。 **值** 是描述性能条件计数器的数字。 例如，若要为性能对象 **SQLServer:Locks** 设置在 **Lock Wait Time** 超过 30 分钟时要发生的警报，则可以选择“大于”并“指定 30 作为值”。  
  
    又如，可以为性能对象 **SQLServer:Transactions** 指定在 **tempdb** 中的可用空间低于 1000 KB 时发出警报。 若要这样设置，应当选择计数器 **Free space in tempdb (KB)** 、“小于”和“值” **1000**。  
  
    > [!NOTE]  
    > 性能数据被周期性地采样，这会在达到阈值与发出性能警报之间造成短暂的延迟（几秒钟）。  
  
    > [!NOTE]  
    > 存储服务器名称的事件日志变量不得超过 32 个字符。 因此，如果主机名和实例名的总大小超过 32 个字符，可能会出现以下错误：
    
   ``` 
   Warning,[466] Failed to copy server name LONGNAMESQLSERV\LONGINSTANCENAME while generating performance counter alerts.
   ```
  
  
## <a name="selecting-a-wmi-event"></a>选择 WMI 事件  
可以指定发出警报来响应特定的 WMI 事件。 若要选择 WMI 事件，必须在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的“新建警报”或“警报属性”对话框中的“常规”页上定义下列内容：  
  
-   **Namespace**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作为 WMI 客户端在 WMI 命名空间（使用该命名空间查询事件）进行注册。  
  
-   **查询**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理使用所提供的 Windows Management Instrumentation 查询语言 (WQL) 语句来标识特定事件。  
  
下列链接指向常见的任务：  
  
**基于消息号创建警报**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-an-alert-using-an-error-number.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)  
  
**基于严重级别创建警报**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-an-alert-using-severity-level.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)  
  
**基于 WMI 事件创建警报**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-a-wmi-event-alert.md)  
  
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
[sp_update_alert (Transact-SQL)](../../relational-databases/performance-monitor/use-sql-server-objects.md)  
