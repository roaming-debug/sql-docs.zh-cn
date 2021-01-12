---
title: 启动、停止、暂停、继续、重启 SQL Server 服务
description: 了解如何启动、停止、暂停、继续或重启各种 SQL Server 服务。 了解如何使用 Transact-SQL、PowerShell 和其他工具来执行这些操作。
ms.custom: ''
ms.date: 03/05/2020
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Configuration Manager, start and stop services
- stopping SQL Server Agent
- parameters [SQL Server], startup options
- SQL Server, startup options
- Database Engine [SQL Server], starting and stopping services
- pausing SQL Server
- PowerShell [SQL Server], starting and stopping services
- single-user mode [SQL Server], starting in
- SQL Server Management Studio [SQL Server], starting or stopping services
- stopping SQL Server Browser service
- starting SQL Server Agent
- default instances [SQL Server], starting and stopping
- SQL Server Agent, starting and stopping
- command prompt [SQL Server], starting and stopping SQL Server services
- continuing SQL Server
- starting SQL Server Database Engine
- net stop commands [SQL Server]
- command prompt [SQL Server], SQL Browser service
- Configuration Manager, start and stop services
- resuming SQL Server
- startup options [SQL Server]
- named instances [SQL Server], starting and stopping
- net start commands [SQL Server]
- SQL Server, starting and stopping
- stopping SQL Server
- starting SQL Server Browser service
- SQL Server Database Engine setting startup options
- administering SQL Server, starting and stopping services
- Management Studio [SQL Server], starting or stopping services
ms.assetid: 32660a02-e5a1-411a-9e57-7066ca459df6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 697cf043c4582796015b061c857145c214d105fb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091736"
---
# <a name="start-stop-pause-resume-restart-sql-server-services"></a>启动、停止、暂停、继续、重启 SQL Server 服务

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍如何使用 SQL Server 配置管理器、SQL Server Management Studio (SSMS)、命令提示符中的 net 命令、Transact-SQL 或 PowerShell 启动、停止、暂停、恢复或重启 SQL Server 数据库引擎、SQL Server 代理或 SQL Server Browser 服务。

## <a name="identify-the-service"></a>识别服务

SQL Server 组件是作为 Windows 服务运行的可执行程序。 作为 Windows 服务运行的程序可以继续运行，而不在计算机屏幕上显示任何活动。

### <a name="database-engine-service"></a>数据库引擎服务

可执行进程是 SQL Server 数据库引擎。 数据库引擎可以是默认实例（每台计算机只有一个），也可以是多个数据库引擎命名实例中的一个。 使用 SQL Server 配置管理器确定在计算机上安装哪些数据库引擎实例。 默认实例（如果安装）作为 SQL Server (MSSQLSERVER) 列出。 命名实例（如果安装）作为 SQL Server (<instance_name>) 列出。 默认情况下，SQL Server Express 作为 SQL Server (SQLEXPRESS) 安装。

### <a name="sql-server-agent-service"></a>SQL Server 代理服务

一种 Windows 服务，可执行计划的管理任务（称为作业和警报）。 有关详细信息，请参阅 [SQL Server Agent](../../ssms/agent/sql-server-agent.md)。 并不是所有版本的 SQL Server 都提供 SQL Server 代理。 有关 SQL Server 各版本支持的功能列表，请参阅 [SQL Server 2019 各个版本支持的功能](../../sql-server/editions-and-components-of-sql-server-version-15.md)。

### <a name="sql-server-browser-service"></a>SQL Server Browser 服务

一种 Windows 服务，可侦听对 SQL Server 资源的传入请求并为客户端提供有关计算机上安装的 SQL Server 实例的信息。 SQL Serve Browser 服务的单个实例用于计算机上安装的所有 SQL Server 实例。

### <a name="additional-information"></a>其他信息

- 暂停数据库引擎服务将阻止新用户连接到数据库引擎，但是已连接的用户可以继续工作到连接断开时为止。 当希望在您停止该服务前等待用户完成工作时，请使用“暂停”。 这使他们能够完成正在进行的事务。 “继续”允许数据库引擎再次接受新连接。 SQL Server 代理服务无法暂停或继续。  

- SQL Server 配置管理器和 SSMS 使用以下图标显示服务的当前状态。  

    **SQL Server 配置管理器**

  - 服务名称旁边的图标上的绿色箭头表示服务已启动。

  - 服务名称旁边的图标上的红色正方形表示服务已停止。

  - 服务名称旁边的图标上的两条蓝色竖线表示服务已暂停。

  - 重启数据库引擎时，用红色正方形表示服务已停止，然后用绿色箭头表示服务已成功启动。

    **SQL Server Management Studio (SSMS)**

  - 服务名称旁边的绿色圆圈图标上的白色箭头表示服务已启动。  

  - 服务名称旁边的红色圆圈图标上的白色正方形表示服务已停止。  

  - 服务名称旁边的蓝色圆圈图标上的两条白色竖线表示服务已暂停。  

- 使用 SQL Server 配置管理器或 SSMS 时，只提供可用的选项。 例如，如果服务已启动，“启动”不可用。

- 在群集上运行时，最好使用群集管理器来管理 SQL Server 数据库引擎服务。  

### <a name="security"></a>安全性

#### <a name="permissions"></a>权限

默认情况下，只有本地管理员组的成员能够启动、停止、暂停、继续或重启服务。 若要向管理员之外的用户授予管理服务的权限，请参阅 [如何授予用户管理 Windows Server 2003 中的服务的权限](https://support.microsoft.com/kb/325349)。 （此过程在其他 Windows Server 版本上是类似的。）

使用 Transact-SQL SHUTDOWN 命令停止数据库引擎需要 sysadmin 或 serveradmin 固定服务器角色的成员资格并且不可转让  。

## <a name="sql-server-configuration-manager"></a>SQL Server 配置管理器

### <a name="starting-sql-server-configuration-manager"></a>启动 SQL Server 配置管理器

在 **“开始”** 菜单中，依次指向 **“所有程序”** 、 **“Microsoft SQL Server”** 和 **“配置工具”** ，然后单击 **“SQL Server 配置管理器”** 。

因为 SQL Server 配置管理器是 Microsoft 管理控制台程序的一个管理单元而不是单独的程序，所以 SQL Server 配置管理器在新版本的 Windows 中不显示为一个应用程序。 以下是在 C 盘安装 Windows 的情况下最新的四个版本的路径。  

|版本|路径|
|-|-|
|SQL Server 2019|C:\Windows\SysWOW64\SQLServerManager15.msc|
|SQL Server 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|
|SQL Server 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|
|SQL Server 2014|C:\Windows\SysWOW64\SQLServerManager12.msc|
|SQL Server 2012|C:\Windows\SysWOW64\SQLServerManager11.msc|

#### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="configmande"></a> 启动、停止、暂停、继续或重启 SQL Server 数据库引擎的实例

1. 使用上面的说明启动 SQL Server 配置管理器。

2. 如果此时出现 **“用户帐户控制”** 对话框，请单击 **“是”** 。

3. 在 SQL Server 配置管理器的左窗格中，单击“SQL Server 服务”。

4. 在结果窗格中，右键单击 **SQL Server (MSSQLServer)** 或某个命名实例，然后单击“启动”、“停止”、“暂停”、“继续”或“重启”。

5. 单击“确定”，以关闭 SQL Server 配置管理器。

> [!NOTE]
> 若要使用启动选项启动一个 SQL Server 数据库引擎的实例，请参阅[配置服务器启动选项（SQL Server 配置管理器）](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)。  

#### <a name="to-start-stop-pause-resume-or-restart-the-sql-server-browser-or-an-instance-of-the-sql-server-agent"></a>启动、停止、暂停、继续或重启 SQL Server Browser 或 SQL Server 代理的实例

1. 使用上面的说明启动 SQL Server 配置管理器。

2. 如果此时出现 **“用户帐户控制”** 对话框，请单击 **“是”** 。

3. 在 SQL Server 配置管理器的左窗格中，单击“SQL Server 服务”。

4. 在结果窗格中，右键单击命名实例的“SQL Server Browser”或者“SQL Server 代理 (MSSQLServer)”或“SQL Server 代理 (<instance_name>)”，然后单击“开始”、“停止”、“暂停”、“恢复”或“重启”       。  

5. 单击“确定”，以关闭 SQL Server 配置管理器。  

> [!NOTE]
> SQL Server 代理无法暂停。

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="ssmsde"></a> 启动、停止、暂停、继续或重启 SQL Server 数据库引擎的实例

1. 在对象资源管理器中，连接到数据库引擎的实例，右键单击要启动的数据库引擎的实例，然后单击“启动”、“停止”、“暂停”、“继续”或“重启”    。

    或者，在“已注册的服务器”中，右键单击要启动的数据库引擎的实例，指向“服务控制”，然后单击“启动”、“停止”、“暂停”、“继续”或“重启”     。

2. 如果此时出现 **“用户帐户控制”** 对话框，请单击 **“是”** 。

3. 系统提示你是否要执行操作时，请单击“是”。  

#### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-agent"></a>启动、停止或重启 SQL Server 代理的实例

1. 在对象资源管理器中，连接到数据库引擎的实例，右键单击“SQL Server 代理”，然后单击“启动”、“停止”或“重启”   。

2. 如果此时出现 **“用户帐户控制”** 对话框，请单击 **“是”** 。

3. 系统提示你是否要执行操作时，请单击“是”。

## <a name="command-prompt-window-using-net-commands"></a><a name="CommandPrompt"></a> 使用 net 命令的命令提示符窗口

可以使用 Microsoft Windows net 命令启动、停止或暂停 Microsoft SQL Server 服务。

### <a name="to-start-the-default-instance-of-the-database-engine"></a><a name="dbDefault"></a> 启动数据库引擎的默认实例

- 在命令提示符下，输入下列命令之一：  
  
    **net start "SQL Server (MSSQLSERVER)"**

   -或-  

    **net start MSSQLSERVER**

### <a name="to-start-a-named-instance-of-the-database-engine"></a><a name="dbNamed"></a> 启动数据库引擎的命名实例

- 在命令提示符下，输入下列命令之一。 使用要管理的实例的名称替换 \<instancename>。  
  
    **net start "SQL Server (** *instancename* **)"**
  
   -或-  
  
    **net start MSSQL$** *instancename*  
  
### <a name="to-start-the-database-engine-with-startup-options"></a><a name="dbStartup"></a> 使用启动选项启动数据库引擎  

- 将启动选项添加到 **net start "SQL Server (MSSQLSERVER)"** 语句末尾，之间用空格分隔开。 使用 **net start** 启动时，启动选项使用正斜杠 (/) 而不是连字符 (-)。  
  
    **net start "SQL Server (MSSQLSERVER)" /f /m**
  
   -或-  
  
    **net start MSSQLSERVER /f /m**
  
  > [!NOTE]
  >  有关启动选项的详细信息，请参阅 [数据库引擎服务启动选项](../../database-engine/configure-windows/database-engine-service-startup-options.md)。  
  
###  <a name="to-start-the-sql-server-agent-on-the-default-instance-of-sql-server"></a><a name="agDefault"></a> 在 SQL Server 的默认实例上启动 SQL Server 代理
  
- 在命令提示符下，输入下列命令之一：  
  
    **net start "SQL Server Agent (MSSQLSERVER)"**
  
   -或-  
  
    **net start SQLSERVERAGENT**
  
###  <a name="to-start-the-sql-server-agent-on-a-named-instance-of-sql-server"></a><a name="agNamed"></a> 在 SQL Server 的命名实例上启动 SQL Server 代理  
  
- 在命令提示符下，输入下列命令之一。 将 *instancename* 替换为要管理的实例的名称。  
  
    **net start "SQL Server Agent(** *instancename* **)"**
  
   -或-  
  
    **net start SQLAgent$** *instancename*  
  
 有关如何在详细模式中运行 SQL Server 代理以执行故障排除的信息，请参阅 [sqlagent90 应用程序](../../tools/sqlagent90-application.md)。  

### <a name="to-start-the-sql-server-browser"></a><a name="Browser"></a> 启动 SQL Server Browser  

- 在命令提示符下，输入下列命令之一：  
  
    **net start "SQL Server Browser"**
  
   -或-  
  
    **net start SQLBrowser**
  
### <a name="to-pause-or-stop-services-from-the-command-prompt-window"></a><a name="pauseStop"></a> 在命令提示符窗口中暂停或停止服务  

- 要暂停或停止服务，请通过以下方式修改命令。  

- 若要暂停服务，请将 **net start** 替换为 **net pause**。  

- 若要停止服务，请将 **net start** 替换为 **net stop**。  

## <a name="transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL

可以通过使用 SHUTDOWN 语句来停止数据库引擎。  

### <a name="to-stop-the-database-engine-using-transact-sql"></a>使用 Transact-SQL 停止数据库引擎

- 要等待当前运行的 Transact-SQL 语句和存储过程完成后停止数据库引擎，请执行以下语句。  
  
    ```sql
    SHUTDOWN;
    ```
  
- 要立即停止数据库引擎，请执行以下语句。  
  
    ```sql
    SHUTDOWN WITH NOWAIT;
    ```

有关 **SHUTDOWN** 语句的详细信息，请参阅 [SHUTDOWN (Transact-SQL)](../../t-sql/language-elements/shutdown-transact-sql.md)。  

## <a name="powershell"></a><a name="PowerShellProcedure"></a> PowerShell

### <a name="to-start-and-stop-database-engine-services"></a>启动和停止数据库引擎服务

1. 在命令提示符窗口中，通过执行以下命令启动 SQL Server PowerShell。  

    ```cmd
    sqlps
    ```

2. 在 SQL Server PowerShell 命令提示符下，执行以下命令。 使用您的计算机名称替换 `computername` 。  

    ```powershell
    # Get a reference to the ManagedComputer class.
    CD SQLSERVER:\SQL\computername
    $Wmi = (get-item .).ManagedComputer
    ```

3. 标识要停止或启动的服务。 选取下列行之一。 使用命名实例的名称替换 `instancename` 。

    - 获取对数据库引擎默认实例的引用。

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQLSERVER']
        ```

    - 获取对数据库引擎命名实例的引用。

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQL$instancename']
        ```

    - 获取对数据库引擎默认实例上 SQL Server 代理服务的引用。  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLSERVERAGENT']
        ```

    - 获取对数据库引擎命名实例上 SQL Server 代理服务的引用。  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLAGENT$instancename']
        ```

    - 获取对 SQL Server Browser 服务的引用。

        ```powershell
        $DfltInstance = $Wmi.Services['SQLBROWSER']
        ```

4. 完成示例以启动然后停止所选服务。
  
    ```powershell
    # Display the state of the service.
    $DfltInstance
    # Start the service.
    $DfltInstance.Start();
    # Wait until the service has time to start.
    # Refresh the cache.  
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    # Stop the service.
    $DfltInstance.Stop();
    # Wait until the service has time to stop.
    # Refresh the cache.
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    ```  
  
##  <a name="using-service-controller-class"></a><a name="ServiceController"></a> 使用服务控制器类

可以使用 ServiceController 类来控制 SQL Server 服务或其他任何 Windows 服务。 有关如何做到这一点的示例，请参阅 [ServiceController 类](/dotnet/api/system.serviceprocess.servicecontroller)。

## <a name="manage-the-sql-server-service-on-linux"></a>在 Linux 上管理 SQL Server 服务

### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-database-engine"></a>启动、停止或重启 SQL Server 数据库引擎的实例

下文说明如何在 Linux 上启动、停止、重启 [SQL Server 服务](../../linux/sql-server-linux-troubleshooting-guide.md#manage-the-sql-server-service)并检查其状态。

使用以下命令检查 SQL Server 服务的状态：

   ```bash
   sudo systemctl status mssql-server
   ```

可根据需要使用以下命令停止、启动或重启 SQL Server 服务：

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

## <a name="next-steps"></a>后续步骤

- [SQL Server 安装文档概述](../install-windows/install-sql-server.md)
- [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)
- [以最小配置启动 SQL Server](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md)
- [SQL Server 2016 版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)