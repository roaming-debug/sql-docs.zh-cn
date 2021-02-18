---
title: 在运行 T-SQL 调试器之前配置防火墙规则
description: 了解如何在连接到 SQL Server Management Studio 中的 SQL Server 时，在运行 Transact-SQL (T-SQL) 调试器之前配置 Windows 防火墙规则。
ms.custom: seo-lt-2019
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.error.sqlde_accessdenied
- vs.debug.error.sqlde_register_failed
- vs.debug.error.sqlde_firewall.remotemachines
helpviewer_keywords:
- Transact-SQL debugger, remote connections
- Windows Firewall [Database Engine], Transact-SQL debugger
- Transact-SQL debugger, Windows Firewall
- Transact-SQL debugger, configuring
- ports [SQL Server], Transact-SQL debugger
- TCP/IP [SQL Server], port numbers
ms.assetid: f50e0b0d-eaf0-4f4a-be83-96f5be63e7ea
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.date: 10/20/2016
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a55de0f9f6a6b8ae80161adaa0252b3d5c4a774
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354689"
---
# <a name="configure-firewall-rules-before-running-the-tsql-debugger"></a>运行 TSQL 调试器之前配置防火墙规则

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

必须配置 Windows 防火墙规则，以便在连接到 [!INCLUDE[tsql](../../includes/tsql-md.md)] 实例（运行该实例的计算机不同于运行 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器的计算机）时启用 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 调试。

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="configuring-the-transact-sql-debugger"></a>配置 Transact-SQL 调试器

[!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器包括服务器端和客户端组件。 服务器端调试器组件与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) 或更高版本中的每个数据库引擎实例一起安装。 包括客户端调试器组件：

- 在从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 或更高版本安装客户端工具时。

- 在安装 Microsoft Visual Studio 2010 或更高版本时。

- 在从 Web 下载安装 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 时。

如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 与 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 实例在同一台计算机上运行，则对于运行 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]调试器没有配置要求。 但是，要在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的远程实例时运行此调试器，必须在两台计算机上的 Windows 防火墙中启用程序和端口规则。 这些规则可通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序创建。 如果您在尝试打开远程调试会话时系统显示错误，则请确保在您的计算机上定义以下防火墙规则。

使用 **“高级安全 Windows 防火墙”** 应用程序管理防火墙规则。 在 [!INCLUDE[win7](../../includes/win7-md.md)] 和 [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)]中，打开 **“控制面板”**，打开 **“Windows 防火墙”**，然后选择 **“高级设置”**。 在 [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)] 中，您还可以打开 **“服务管理器”**，在左侧的窗格中展开 **“配置”** ，然后展开 **“高级安全 Windows 防火墙”**。

> [!CAUTION]
>  在“Windows 防火墙”中启用规则可能会导致您的计算机暴露在防火墙在设计上可以阻止的安全风险之中。 为远程调试启用规则将取消阻止在本主题中列出的端口和程序。

## <a name="firewall-rules-on-the-server"></a>服务器上的防火墙规则
在运行 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的计算机上，使用 **“高级安全 Windows 防火墙”** 可以指定以下信息：

- 为 sqlservr.exe 添加入站程序规则。 对于需要支持远程调试会话的每个实例，您必须具有一个规则。

   1. 在“高级安全 Windows 防火墙”的左窗格中，右键单击“入站规则”，然后在操作窗格中选择“新建规则”。

   2. 在 **“规则类型”** 对话框中，选择 **“程序”**，然后单击 **“下一步”**。

   3. 在 **“程序”** 对话框中，选择 **“此程序路径:”** ，然后输入指向此实例的 sqlservr.exe 的完整路径。 默认情况下，sqlservr.exe 安装在 C:\Program Files\Microsoft SQL Server\MSSQL13.*InstanceName*\MSSQL\Binn 中，其中， *InstanceName* 对于默认实例为 MSSQLSERVER，对于任何指定的实例则为相应实例的名称。

   4. 在 **“操作”** 对话框中，选择 **“允许连接”**，然后单击 **“下一步”**。

   5. 在 **“配置文件”** 对话框中，选择在您想要打开针对该实例的调试会话时描述计算机连接环境的任何配置文件，然后单击 **“下一步”**。

   6. 在 **“名称”** 对话框中，键入针对此规则的名称和说明，然后单击 **“完成”**。

   7. 在 **“入站规则”** 列表中，右键单击您创建的规则，然后在操作窗格中选择 **“属性”** 。

   8. 选择 **“协议和端口”** 选项卡。

   9. 在 **“协议类型:”** 框中选择 **“TCP”** ，在 **“本地端口:”** 框中选择 **“RPC 动态端口”** ，单击 **“应用”**，然后单击 **“确定”**。

- 添加针对 svchost.exe 的入站程序规则，以便启用从远程调试器会话的 DCOM 通信。

   1. 在“高级安全 Windows 防火墙”的左窗格中，右键单击“入站规则”，然后在操作窗格中选择“新建规则”。

   2. 在 **“规则类型”** 对话框中，选择 **“程序”**，然后单击 **“下一步”**。

   3. 在 **“程序”** 对话框中，选择 **“此程序路径:”** ，然后输入指向 svchost.exe 的完整路径。 默认情况下，svchost.exe 安装在 %systemroot%\System32\svchost.exe 中。

   4. 在 **“操作”** 对话框中，选择 **“允许连接”**，然后单击 **“下一步”**。

   5. 在 **“配置文件”** 对话框中，选择在您想要打开针对该实例的调试会话时描述计算机连接环境的任何配置文件，然后单击 **“下一步”**。

   6. 在 **“名称”** 对话框中，键入针对此规则的名称和说明，然后单击 **“完成”**。

   7. 在 **“入站规则”** 列表中，右键单击您创建的规则，然后在操作窗格中选择 **“属性”** 。

   8. 选择 **“协议和端口”** 选项卡。

   9. 在 **“协议类型:”** 框中选择 **“TCP”** ，在 **“本地端口:”** 框中选择 **“RPC 端点映射程序”** ，单击 **“应用”**，然后单击 **“确定”**。

- 如果域策略要求通过 IPSec 进行网络通信，还必须添加打开 UDP 端口 4500 和 UDP 端口 500 的入站规则。

## <a name="firewall-rules-on-the-client"></a>客户端上的防火墙规则

在正运行 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器的计算机上，SQL Server 安装程序或 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 安装程序可能已将 Windows 防火墙配置为允许远程调试。

如果您在尝试打开远程调试会话时系统显示错误，则可以通过使用 **“高级安全 Windows 防火墙”** 配置防火墙规则来手动配置程序和端口例外：

- 为 svchost 添加程序条目:

   1. 在“高级安全 Windows 防火墙”的左窗格中，右键单击“入站规则”，然后在操作窗格中选择“新建规则”。

   2. 在 **“规则类型”** 对话框中，选择 **“程序”**，然后单击 **“下一步”**。

   3. 在 **“程序”** 对话框中，选择 **“此程序路径:”** ，然后输入指向 svchost.exe 的完整路径。 默认情况下，svchost.exe 安装在 %systemroot%\System32\svchost.exe 中。

   4. 在 **“操作”** 对话框中，选择 **“允许连接”**，然后单击 **“下一步”**。

   5. 在 **“配置文件”** 对话框中，选择在您想要打开针对该实例的调试会话时描述计算机连接环境的任何配置文件，然后单击 **“下一步”**。

   6. 在 **“名称”** 对话框中，键入针对此规则的名称和说明，然后单击 **“完成”**。

   7. 在 **“入站规则”** 列表中，右键单击您创建的规则，然后在操作窗格中选择 **“属性”** 。

   8. 选择 **“协议和端口”** 选项卡。

   9. 在 **“协议类型:”** 框中选择 **“TCP”** ，在 **“本地端口:”** 框中选择 **“RPC 端点映射程序”** ，单击 **“应用”**，然后单击 **“确定”**。

- 为承载 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器的应用程序添加程序条目。 如果您需要在同一台计算机上从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 和 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 都打开远程调试会话，则必须为这两者都添加一个程序规则：

   1. 在“高级安全 Windows 防火墙”的左窗格中，右键单击“入站规则”，然后在操作窗格中选择“新建规则”。

   2. 在 **“规则类型”** 对话框中，选择 **“程序”**，然后单击 **“下一步”**。

   3. 在 **“程序”** 对话框中，选择 **“此程序路径:”** ，然后输入以下三个值之一。

       - 对于 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]，输入指向 ssms.exe 的完整路径。 默认情况下，ssms.exe 安装在 C:\Program Files (x86)\Microsoft SQL Server\130\Tools\Binn\Management Studio 下。

       - 对于 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] ，输入指向 devenv.exe 的完整路径：

            1. 默认情况下，针对 Visual Studio 2010 的 devenv.exe 位于 C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE 中。

            2. 默认情况下，针对 Visual Studio 2012 的 devenv.exe 位于 C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE 中。

            3. 您可以从用来启动 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]的快捷方式中找到指向 ssms.exe 的路径。 您可以从用来启动 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]的快捷方式中找到指向 devenv.exe 的路径。 右键单击该快捷方式并选择 **“属性”**。 可执行文件和路径将在 **“目标”** 框中列出。

   4. 在 **“操作”** 对话框中，选择 **“允许连接”**，然后单击 **“下一步”**。

   5. 在 **“配置文件”** 对话框中，选择在您想要打开针对该实例的调试会话时描述计算机连接环境的任何配置文件，然后单击 **“下一步”**。

   6. 在 **“名称”** 对话框中，键入针对此规则的名称和说明，然后单击 **“完成”**。

   7. 在 **“入站规则”** 列表中，右键单击您创建的规则，然后在操作窗格中选择 **“属性”** 。

   8. 选择 **“协议和端口”** 选项卡。

   9. 在 **“协议类型:”** 框中选择 **“TCP”** ，在 **“本地端口:”** 框中选择 **“RPC 动态端口”** ，单击 **“应用”**，然后单击 **“确定”**。

## <a name="requirements-for-starting-the-debugger"></a>启动调试器的要求

启动 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器的所有尝试还必须满足以下要求：

- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 必须在作为 sysadmin 固定服务器角色成员的 Windows 帐户下运行。

- [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口必须使用 Windows 身份验证来连接，或使用作为 sysadmin 固定服务器角色成员的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录名来连接。

- [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口必须从 [!INCLUDE[ssDE](../../includes/ssde-md.md)] Service Pack 2 (SP2) 或更高版本连接到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例。 如果查询编辑器窗口连接到处于单用户模式下的实例，您将无法运行调试器。

- 服务器需要通过 RPC 回复客户端通信。 运行 SQL Server 服务的帐户必须具有对客户端的身份验证权限

## <a name="see-also"></a>另请参阅

- [Transact-SQL 调试器](./transact-sql-debugger.md)
- [运行 Transact-SQL 调试器](./run-the-transact-sql-debugger.md)
- [逐句通过 Transact-SQL 代码](./step-through-transact-sql-code.md)
- [Transact-SQL 调试器信息](./transact-sql-debugger-information.md)
- [数据库引擎查询编辑器 (SQL Server Management Studio)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)