---
title: 将 Windows 中的 SQL Server 数据库迁移到 Linux
description: 本教程演示如何在 Windows 上执行 SQL Server 数据库备份，并将其还原到运行 SQL Server 的 Linux 计算机。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/16/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 9ac64d1a-9fe5-446e-93c3-d17b8f55a28f
ms.openlocfilehash: 2ae765c8bc1f274ab9b9f62978a85b5f09f6ca58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349087"
---
# <a name="migrate-a-sql-server-database-from-windows-to-linux-using-backup-and-restore"></a>使用备份和还原将 SQL Server 数据库从 Windows 迁移到 Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

建议使用 SQL Server 的备份和还原功能将数据库从 Windows 上的 SQL Server 迁移到 Linux 上的 SQL Server。 在本教程中，将逐步完成使用备份和还原方法将数据库迁移到 Linux 的必需步骤。

> [!div class="checklist"]
> * 使用 SSMS 在 Windows 上创建备份文件
> * 在 Windows 上安装 Bash shell
> * 将备份文件从 Bash shell 移到 Linux
> * 在 Linux 上通过 Transact-SQL 还原备份文件
> * 运行查询以验证迁移

还可以创建一个 SQL Server Always On 可用性组，将 SQL Server 的数据库从 Windows 迁移到 Linux。 请参阅[在 Windows 和 Linux 上配置 SQL Server Always On 可用性组（跨平台）](sql-server-linux-availability-group-cross-platform.md)。

## <a name="prerequisites"></a>必备条件

若要完成本教程，需满足以下先决条件：

* 安装了以下内容的 Windows 计算机：
  * [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)。
  * [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。
  * 要迁移的目标数据库。

* 安装了以下内容的 Linux 计算机：
  * 带有命令行工具的 SQL Server（[RHEL](quickstart-install-connect-red-hat.md)、[SLES](quickstart-install-connect-suse.md) 或 [Ubuntu](quickstart-install-connect-ubuntu.md)）。

## <a name="create-a-backup-on-windows"></a>在 Windows 上创建备份

有多种方法可在 Windows 上创建数据库的备份文件。 以下步骤使用 SQL Server Management Studio (SSMS)。

1. 在 Windows 计算机中启动 SQL Server Management Studio  。

1. 在连接“对话框”中，输入“localhost”  。

1. 在“对象资源管理器”中，展开“数据库”  。

1. 右键单击目标数据库，选择“任务”，再单击“备份...”   。

   ![使用 SSMS 创建备份文件](./media/sql-server-linux-migrate-restore-database/ssms-create-backup.png)

1. 在“备份数据库”对话框中，验证“备份类型”是否为“完全”，以及“备份到”是否为“磁盘”      。 注意文件的名称和位置。 例如，SQL Server 2016 上名为“YourDB”的数据库的默认备份路径为  `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak`。

1. 单击“确定”以备份数据库  。

> [!NOTE]
> 创建备份文件的另一种方法是运行 Transact-SQL 查询。 以下 Transact-SQL 命令对名为“YourDB”  的数据库执行与前面步骤相同的操作：
>
> ```sql
> BACKUP DATABASE [YourDB] TO  DISK =
> N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak'
> WITH NOFORMAT, NOINIT, NAME = N'YourDB-Full Database Backup',
> SKIP, NOREWIND, NOUNLOAD, STATS = 10
> GO
> ```

## <a name="install-a-bash-shell-on-windows"></a>在 Windows 上安装 Bash shell

若要还原数据库，首先必须将备份文件从 Windows 计算机传输到目标 Linux 计算机。 在本教程中，我们将该文件从在 Windows 上运行的 Bash shell（终端窗口）移动到 Linux。

1. 在支持“scp”（安全复制）和“ssh”（远程登录）命令的 Windows 计算机上安装 Bash shell   。 以下介绍两个示例：

   * [适用于 Linux 的 Windows 子系统](/windows/wsl/about) (Windows 10)
   * Git Bash Shell ([https://git-scm.com/downloads](https://git-scm.com/downloads))

1. 在 Windows 上打开 Bash 会话。

## <a name="copy-the-backup-file-to-linux"></a><a id="scp"></a> 将备份文件复制到 Linux

1. 在 Bash 会话中，导航到包含备份文件的目录。 例如：

   ```bash
   cd 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\'
   ```

1. 使用 scp 命令将文件传输到目标 Linux 计算机  。 下面的示例将 YourDB.bak  传输到 Linux 服务器上的 user1 的主目录，IP 地址为 192.0.2.9   ：

   ```bash
   scp YourDB.bak user1@192.0.2.9:./
   ```
   ![scp 命令](./media/sql-server-linux-migrate-restore-database/scp-command.png)

> [!TIP]
> 对于文件传输，可以使用 scp 替代方法。 一种方法是使用 [Samba](https://help.ubuntu.com/community/Samba) 在 Windows 和 Linux 之间配置 SMB 网络共享。 关于 Ubuntu 上的演练，请参阅[如何通过 Samba 创建网络共享](https://help.ubuntu.com/community/How%20to%20Create%20a%20Network%20Share%20Via%20Samba%20Via%20CLI%20%28Command-line%20interface/Linux%20Terminal%29%20-%20Uncomplicated,%20Simple%20and%20Brief%20Way!)。 建立后，可以通过 Windows 将它作为网络共享文件进行访问，如 **\\\\machinenameorip\\share**.

## <a name="move-the-backup-file-before-restoring"></a>在还原前移动备份文件

此时，备份文件位于你的用户的主目录中的 Linux 服务器上。 将数据库还原到 SQL Server 之前，必须将备份放入“/var/opt/mssql”的子目录中  。

1. 在同一 Windows Bash 会话中，通过 ssh  远程连接到目标 Linux 计算机。 以下示例以用户 user1 的身份连接到 Linux 计算机 192.0.2.9   。

   ```bash
   ssh user1@192.0.2.9
   ```

   你现在正在远程 Linux 服务器上运行命令。

1. 进入超级用户模式。

   ```bash
   sudo su
   ```

1. 创建新的备份目录。 如果目录已存在，-p 参数不会执行任何操作。

   ```bash
   mkdir -p /var/opt/mssql/backup
   ```

1. 将备份文件移动到该目录。 在下面的示例中，备份文件位于 user1 的主目录  。 更改命令，使其与你的备份文件的位置和文件名相匹配。

   ```bash
   mv /home/user1/YourDB.bak /var/opt/mssql/backup/
   ```

1. 退出超级用户模式。

   ```bash
   exit
   ```

## <a name="restore-your-database-on-linux"></a>在 Linux 上还原数据库

要还原数据库备份，可以使用 RESTORE DATABASE  Transact-SQL (TQL) 命令。

> [!NOTE]
> 下列步骤使用 sqlcmd  工具。 如果尚未安装 SQL Server 工具，请参阅[在 Linux 上安装 SQL Server 命令行工具](sql-server-linux-setup-tools.md)。

1. 在同一终端中，启动 sqlcmd  。 下面的示例以 SA 用户身份连接到本地 SQL Server  。 出现提示时输入密码，或使用 -P 参数指定密码  。

   ```bash
   sqlcmd -S localhost -U SA
   ```

1. 在 `>1` 提示符下，输入以下 RESTORE DATABASE  命令，并在每行后按 Enter（无法同时复制和粘贴整个多行命令）。 将出现的所有 `YourDB` 替换为数据库的名称。

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```

   应收到已成功还原数据库的消息。

   `RESTORE DATABASE` 可能会返回类似于以下示例的错误：

   ```bash
   File 'YourDB_Product' cannot be restored to 'Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf'. Use WITH MOVE to identify a valid location for the file.
   Msg 5133, Level 16, State 1, Server servername, Line 1
   Directory lookup for the file "Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf" failed with the operating system error 2(The system cannot find the file specified.).
   ```
   
   在这种情况下，数据库包含辅助文件。 如果未在 `RESTORE DATABASE` 的 `MOVE` 子句中指定这些文件，则还原过程将尝试在与原始服务器相同的路径中创建这些文件。 

   可以列出备份中包含的所有文件：
   ```sql
   RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   GO
   ```
   应会看到如下所示的列表（仅列出前两列）：
   ```sql
   LogicalName         PhysicalName                                                                 ..............
   ----------------------------------------------------------------------------------------------------------------------
   YourDB              Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB.mdf          ..............
   YourDB_Product      Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf  ..............
   YourDB_Customer     Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Customer.ndf ..............
   YourDB_log          Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Log.ldf      ..............
   ```
   
   可使用此列表为其他文件创建 `MOVE` 子句。 在本示例中，`RESTORE DATABASE` 为：

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Product' TO '/var/opt/mssql/data/YourDB_Product.ndf',
   MOVE 'YourDB_Customer' TO '/var/opt/mssql/data/YourDB_Customer.ndf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```


1. 通过列出服务器上的所有数据库来验证还原。 应该会列出已还原的数据库。

   ```sql
   SELECT Name FROM sys.Databases
   GO
   ```

1. 在已迁移的数据库上运行其他查询。 以下命令将上下文切换到 YourDB  数据库，并从其一个表中选择行。

   ```sql
   USE YourDB
   SELECT * FROM YourTable
   GO
   ```

1. 使用 sqlcmd  完成后，键入 `exit`。

1. 在远程 ssh  会话中完成操作后，再次键入 `exit`。

## <a name="next-steps"></a>后续步骤

在本教程中，了解了如何在 Windows 上备份数据库，并将其移动到运行 SQL Server 的 Linux 服务器。 你已了解如何执行以下操作：
> [!div class="checklist"]
> * 使用 SSMS 和 Transact-SQL 在 Windows 上创建备份文件
> * 在 Windows 上安装 Bash shell
> * 使用 scp  将备份文件从 Windows 移动到 Linux
> * 使用 ssh  远程连接到 Linux 计算机
> * 重新定位备份文件以准备还原
> * 使用 sqlcmd 运行 Transact-SQL 命令 
> * 通过 RESTORE DATABASE  命令还原数据库备份 
> * 运行查询以验证迁移

接下来，请浏览 Linux 上的 SQL Server 的其他迁移方案。 

> [!div class="nextstepaction"]
>[将数据库迁移到 Linux 上的 SQL Server](sql-server-linux-migrate-overview.md)