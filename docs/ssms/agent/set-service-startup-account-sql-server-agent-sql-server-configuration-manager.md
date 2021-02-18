---
description: 为 SQL Server 代理设置服务启动帐户（SQL Server 配置管理器）
title: 设置服务启动帐户
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, service accounts
- startup accounts [SQL Server]
- service startup accounts [SQL Server Agent]
ms.assetid: 46ffe818-ebb5-43a0-840b-923f219a2472
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 30944838b0ed66bc87bc49ff26fbba0efc05a683
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344702"
---
# <a name="set-the-service-startup-account-for-sql-server-agent-sql-server-configuration-manager"></a>为 SQL Server 代理设置服务启动帐户（SQL Server 配置管理器）

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务启动帐户定义了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在运行时所用的 Windows 帐户及其网络权限。 本主题说明了如何使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中通过 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 配置管理器设置 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]代理服务帐户。  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限  
  
-   从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]开始， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理不再要求服务启动帐户为 [!INCLUDE[msCoName](../../includes/msconame_md.md)] Administrators 组的成员。 但是， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务启动帐户必须是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]sysadmin 固定服务器角色的成员。 如果使用多服务器作业处理，帐户还必须是主服务器上 msdb 数据库角色 TargetServersRole 的成员。  
  
-   “对象资源管理器”仅在您拥有使用权限时才显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理节点。  
  
### <a name="security"></a><a name="Security"></a>安全性  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，必须将 **代理配置为使用** sysadmin [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]固定服务器角色的成员帐户的凭据，才能执行其功能。 该帐户必须拥有以下 Windows 权限：  
  
-   以服务身份登录 (SeServiceLogonRight)  
  
-   替换进程级别标记 (SeAssignPrimaryTokenPrivilege)  
  
-   跳过遍历检查 (SeChangeNotifyPrivilege)  
  
-   调整进程的内存配额 (SeIncreaseQuotaPrivilege)  
  
有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户所需的 Windows 权限的详细信息，请参阅 [为 SQL Server 代理服务选择帐户](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) 和 [设置 Windows 服务帐户](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-set-the-service-startup-account-for-sql-server-agent"></a>为 SQL Server 代理设置服务启动帐户  
  
1.  在 **“已注册的服务器”** 中，单击加号以便展开 **“数据库引擎”**。  
  
2.  单击加号以便展开 **“本地服务器组”** 文件集。  
  
3.  右键单击要设置服务启动帐户的服务器实例，然后选择“SQL Server 配置管理器…”。  
  
4.  在“用户帐户控制”对话框中，单击“是”。  
  
5.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器的控制台窗格中，选择 **“SQL Server 服务”**。  
  
6.  在详细信息窗格中，右键单击“SQL Server 代理 _(server\_name)_ ”（其中 server_name 是要更改其服务启动帐户的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理实例的名称），然后选择“属性”。  
  
7.  在“SQL Server 代理(server\_name)_属性”_ 对话框的“登录”选项卡中，选择“登录身份”下的以下选项之一：  
  
    -   **内置帐户**：如果你的作业仅需要本地服务器中的资源，则选择此选项。 有关如何选择 Windows 内置帐户类型的信息，请参阅 [为 SQL Server 代理服务选择帐户](./select-an-account-for-the-sql-server-agent-service.md)。  
  
        > [!IMPORTANT]  
        > [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务不支持  中的 Local Service [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]帐户。  
  
    -   **本帐户**：如果作业需要网络上的资源（包括应用程序资源），如果要将事件转发到其他 Windows 应用程序日志，或者如果要通过电子邮件或寻呼程序来通知操作员，则选择此选项。  
  
        如果您选择此选项：  
  
        1.  在 **“帐户名称”** 框中，输入将用来运行 SQL Server 代理的帐户。 或者，单击 **“浏览”** 打开 **“选择用户或组”** 对话框并选择要使用的帐户。  
  
        2.  在 **“密码”** 框中，输入帐户密码。 在“确认密码”框中重新输入密码。  
  
8.  单击“确定”。  
  
9. 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器中，单击 **“关闭”** 按钮。  
