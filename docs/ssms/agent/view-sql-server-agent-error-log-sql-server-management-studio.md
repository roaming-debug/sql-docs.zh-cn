---
description: 查看 SQL Server 代理错误日志
title: 查看 SQL Server 代理错误日志
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], SQL Server Agent
- viewing SQL Server Agent error logs
- displaying SQL Server Agent error logs
- SQL Server Agent, errors
- errors [SQL Server Agent]
ms.assetid: de920425-fa44-469f-b83d-49e3f97e97f4
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: aafe4b17c6e6b3592417ac9270124f84f938074b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341132"
---
# <a name="view-sql-server-agent-error-log"></a>查看 SQL Server 代理错误日志

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍如何使用  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中查看 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]代理错误日志。  
  
日志文件查看器显示来自许多不同组件的日志信息。 打开日志文件查看器后，请使用 **“选择日志”** 窗格选择要显示的日志。 每个日志显示适合于该类别日志的列。 日志是否可用取决于日志文件查看器的打开方式。  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限  
“对象资源管理器”仅在您拥有使用权限时才显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理节点。  
  
### <a name="security"></a><a name="Security"></a>安全性  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，必须将 **代理配置为使用** sysadmin [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]固定服务器角色的成员帐户的凭据，才能执行其功能。 该帐户必须拥有以下 Windows 权限：  
  
-   以服务身份登录 (SeServiceLogonRight)  
  
-   替换进程级别标记 (SeAssignPrimaryTokenPrivilege)  
  
-   跳过遍历检查 (SeChangeNotifyPrivilege)  
  
-   调整进程的内存配额 (SeIncreaseQuotaPrivilege)  
  
有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户所需的 Windows 权限的详细信息，请参阅 [为 SQL Server 代理服务选择帐户](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) 和 [设置 Windows 服务帐户](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-view-the-ssnoversion-agent-error-log"></a>查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理错误日志  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开包含要查看的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理错误日志的服务器。  
  
2.  单击加号以展开 **“SQL Server 代理”**。  
  
3.  单击加号以展开 **“错误日志”** 文件夹。  
  
4.  右键单击要查看的错误日志，并选择“查看代理日志”。  
  
    “日志文件查看器 - server_name”对话框中提供以下选项：  
  
    **加载日志**  
    打开一个对话框，您可以在其中指定要加载的日志文件。  
  
    **导出**  
    打开一个对话框，你可以使用该对话框将“日志文件摘要”  网格中显示的信息导入到文本文件中。  
  
    **“刷新”**  
    刷新选定日志的视图。 在应用任何筛选器设置时， **“刷新”** 按钮重新从目标服务器中读取选定的日志。  
  
    **筛选器**  
    打开一个对话框，你可以使用该对话框指定用于筛选日志文件的设置，例如“连接” 、“日期” 或其他“常规”  筛选条件。  
  
    **搜索**  
    在日志文件中搜索特定文本。 不支持在搜索中使用通配符。  
  
    **Stop**  
    停止加载日志文件条目。 例如，如果远程或脱机日志文件需要较长时间才能加载，并且您只想查看最新的条目，则可以使用此选项。  
  
    **日志文件摘要**  
    此信息窗格显示日志文件筛选摘要。 如果未对文件进行筛选，您将看到以下文本： **“未应用任何筛选器”** 。 如果对日志应用了筛选器，将看到以下文本：基于以下条件筛选日志项：<filter criteria>。  
  
    **所选行详细信息**  
    选择一行可以在页面底部显示有关所选事件行的其他详细信息。 在网格中，通过将列拖动到的新位置可以重新排列各列的顺序。 通过将网格标题中的列分隔条向左或向右拖动，可以调列的大小。 双击网格标题中的列分隔条，可以按内容宽度自动调整列的大小。  
  
    **实例**  
    发生事件的实例的名称。 这显示为：计算机名称\\实例名称 。  
  
    **Date**  
    显示事件的日期。  
  
    **数据源**  
    显示从其创建事件的源功能，例如服务的名称（如 MSSQLSERVER）。 并非对所有日志类型都显示此项。  
  
    **消息**  
    显示与事件相关联的任何消息。  
  
    **日志类型**  
    显示事件所属的日志类型。 所有选定的日志都显示在日志文件摘要窗口中。  
  
    **日志源**  
    显示在其中捕获事件的源日志的说明。  
  
5.  完成后，单击“关闭”。  
