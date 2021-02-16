---
title: 在 Linux 上为 SQL Server 创建和运行作业
description: 了解如何使用 Transact-SQL 和 SQL Server Management Studio (SSMS) 在 Linux 上创建 SQL Server 代理作业。
author: VanMSFT
ms.author: vanto
ms.date: 02/20/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1d93d95e-9c89-4274-9b3f-fa2608ec2792
ms.openlocfilehash: bc52f0a2052d0b9abd1700e92af3a30c0e02bad2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346441"
---
# <a name="create-and-run-sql-server-agent-jobs-on-linux"></a>在 Linux 上创建和运行 SQL Server 代理作业

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

SQL Server 作业用于在 SQL Server 数据库中定期执行相同的命令序列。 本教程提供使用 Transact-SQL 和 SQL Server Management Studio (SSMS) 在 Linux 上创建 SQL Server 代理作业的示例。

> [!div class="checklist"]
> * 在 Linux 上安装 SQL Server 代理
> * 创建新作业以执行每日数据库备份
> * 计划并运行作业
> * 在 SSMS 中执行相同的步骤（可选）

有关 Linux 上的 SQL Server 代理的已知问题，请参阅[发行说明](sql-server-linux-release-notes.md)。

## <a name="prerequisites"></a>必备条件

若要完成本教程，需满足以下先决条件：

* 满足以下先决条件的 Linux 计算机：
  * 带有命令行工具的 SQL Server（[RHEL](quickstart-install-connect-red-hat.md)、[SLES](quickstart-install-connect-suse.md) 或 [Ubuntu](quickstart-install-connect-ubuntu.md)）。

以下先决条件是可选的：

* 带有 SSMS 的 Windows 计算机：
  * 用于可选 SSMS 步骤的 [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。

## <a name="enable-sql-server-agent"></a>启用 SQL Server 代理

若要在 Linux 上使用 SQL Server 代理，必须先在安装了 SQL Server 的计算机上启用 SQL Server 代理。

1. 若要启用 SQL Server 代理，请执行以下步骤。
  ```bash
  sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
  ```

1. 使用以下命令重新启动 SQL Server：
  ```bash
  sudo systemctl restart mssql-server
  ```

> [!NOTE]
> 从 SQL Server 2017 CU4 开始，SQL Server 代理包含在 **mssql-server** 包中，默认情况下处于禁用状态。 对于 CU4 之前的代理安装，请访问[在 Linux 上安装 SQL Server 代理](sql-server-linux-setup-sql-agent.md)。

## <a name="create-a-sample-database"></a>创建示例数据库

使用以下步骤创建名为“SampleDB”的示例数据库  。 此数据库用于每日备份作业。

1. 在 Linux 计算机上，打开 bash 终端会话。

1. 使用 **sqlcmd** 运行 Transact-SQL **CREATE DATABASE** 命令。

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'CREATE DATABASE SampleDB'
   ```

1. 通过列出服务器上的数据库来验证是否已创建该数据库。

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'SELECT Name FROM sys.Databases'
   ```

## <a name="create-a-job-with-transact-sql"></a>使用 Transact-SQL 创建作业

以下步骤使用 Transact-SQL 命令在 Linux 上创建 SQL Server 代理作业。 该作业运行示例数据库“SampleDB”的每日备份  。

> [!TIP]
> 可以使用任何 T-SQL 客户端运行这些命令。 例如，在 Linux 上可以使用 [sqlcmd](sql-server-linux-setup-tools.md) 或 [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)。 在远程 Windows Server 中，还可以在 SQL Server Management Studio (SSMS) 中运行查询，或使用 UI 界面进行作业管理，下一部分将对此进行说明。

1. 使用 [sp_add_job](../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md) 创建名为 `Daily SampleDB Backup` 的作业。

   ```sql
   -- Adds a new job executed by the SQLServerAgent service
   -- called 'Daily SampleDB Backup'
   USE msdb ;
   GO
   EXEC dbo.sp_add_job
      @job_name = N'Daily SampleDB Backup' ;
   GO
   ```

1. 调用 [sp_add_jobstep](../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md) 创建一个作业步骤，该步骤用于创建 `SampleDB` 数据库备份。

   ```sql
   -- Adds a step (operation) to the job
   EXEC sp_add_jobstep
      @job_name = N'Daily SampleDB Backup',
      @step_name = N'Backup database',
      @subsystem = N'TSQL',
      @command = N'BACKUP DATABASE SampleDB TO DISK = \
         N''/var/opt/mssql/data/SampleDB.bak'' WITH NOFORMAT, NOINIT, \
         NAME = ''SampleDB-full'', SKIP, NOREWIND, NOUNLOAD, STATS = 10',
      @retry_attempts = 5,
      @retry_interval = 5 ;
   GO
   ```

1. 然后使用 [sp_add_schedule](../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) 为作业创建每日计划。

   ```sql
   -- Creates a schedule called 'Daily'
   EXEC dbo.sp_add_schedule
      @schedule_name = N'Daily SampleDB',
      @freq_type = 4,
      @freq_interval = 1,
      @active_start_time = 233000 ;
   USE msdb ;
   GO
   ```

1. 使用 [sp_attach_schedule](../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md) 将作业计划附加到作业。

   ```sql
   -- Sets the 'Daily' schedule to the 'Daily SampleDB Backup' Job
   EXEC sp_attach_schedule
      @job_name = N'Daily SampleDB Backup',
      @schedule_name = N'Daily SampleDB';
   GO
   ```

1. 使用 [sp_add_jobserver](../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md) 将作业分配到目标服务器。 此示例中的目标是本地服务器。

   ```sql
   EXEC dbo.sp_add_jobserver
      @job_name = N'Daily SampleDB Backup',
      @server_name = N'(LOCAL)';
   GO
   ```
1. 使用 [sp_start_job](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) 启动作业。

   ```sql
   EXEC dbo.sp_start_job N' Daily SampleDB Backup' ;
   GO
   ```

## <a name="create-a-job-with-ssms"></a>使用 SSMS 创建作业

还可以在 Windows 上使用 SQL Server Management Studio (SSMS) 远程创建和管理作业。

1. 在 Windows 上启动 SSMS 并连接到 Linux SQL Server 实例。 有关详细信息，请参阅[使用 SSMS 管理 Linux 上的 SQL Server](sql-server-linux-manage-ssms.md)。

1. 验证是否已创建名为“SampleDB”的示例数据库  。

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-0.png" alt="Create a SampleDB database" style="width: 550px;"/>

1. 验证是否已[安装](sql-server-linux-setup-sql-agent.md)并正确配置了 SQL 代理。 在对象资源管理器中，找到 SQL Server 代理旁边的加号。 如果未启用 SQL Server 代理，请尝试在 Linux 上重新启动 **mssql-server** 服务。

   ![验证是否已安装 SQL Server 代理](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-1.png)

1. 创建新作业。

   ![创建新作业](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-2.png)

1. 指定作业名称并创建作业步骤。

   ![创建作业步骤](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-3.png)

1. 指定要使用的子系统以及作业步骤应执行的操作。

   ![作业子系统](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-4.png)

   ![作业步骤操作](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-5.png)

1. 创建新的作业计划。

   ![“新建作业”对话框的屏幕截图，其中突出显示了“计划”选项和“新建”选项。](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-6.png)

   ![“新建作业”对话框的屏幕截图，其中突出显示了“确定”选项。](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-8.png)

1. 启动作业。

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-9.png" alt="Start the SQL Server Agent job" style="width: 550px;"/>

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：

> [!div class="checklist"]
> * 在 Linux 上安装 SQL Server 代理
> * 使用 Transact-SQL 和系统存储过程来创建作业
> * 创建用于执行每日数据库备份的作业
> * 使用 SSMS UI 创建和管理作业

接下来，探索用于创建和管理作业的其他功能：

> [!div class="nextstepaction"]
>[SQL Server 代理文档](../ssms/agent/sql-server-agent.md)