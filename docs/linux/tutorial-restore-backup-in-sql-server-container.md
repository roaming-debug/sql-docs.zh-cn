---
title: 在 Docker 中还原 SQL Server 数据库
description: 本教程说明如何在新的 Linux Docker 容器中还原 SQL Server 数据库备份。
author: VanMSFT
ms.author: vanto
ms.date: 03/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: b5fcfbf30028c904be96ca17be1ebb7feeb6f91d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471418"
---
# <a name="restore-a-sql-server-database-in-a-linux-docker-container"></a>在 Linux Docker 容器中还原 SQL Server 数据库

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

本教程演示如何将 SQL Server 备份文件移动和还原到在 Docker 上运行的 SQL Server 2017 Linux 容器映像。

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

本教程演示如何将 SQL Server 备份文件移动和还原到在 Docker 上运行的 SQL Server 2019 Linux 容器映像。

::: moniker-end

> [!div class="checklist"]
> * 拉取并运行最新的 SQL Server Linux 容器映像。
> * 将 Wide World Importers 数据库文件复制到容器中。
> * 还原容器中的数据库。
> * 运行 Transact-SQL 语句来查看和修改数据库。
> * 备份修改后的数据库。

## <a name="prerequisites"></a>先决条件

* 任何受支持的 Linux 分发或用于 Mac/Windows 的 Docker 上的 Docker 引擎 1.8+。 有关详细信息，请参阅 [Install Docker](https://docs.docker.com/engine/installation/)（安装 Docker）。
* 至少 2 GB 的磁盘空间
* 至少 2 GB 的 RAM
* [Linux 上的 SQL Server 的系统要求](sql-server-linux-setup.md#system)。

## <a name="pull-and-run-the-container-image"></a>拉取并运行容器映像

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. 在 Linux/Mac 上打开 bash 终端，或在 Windows 上打开提升的 PowerShell 会话。

1. 从 Docker 中心请求 SQL Server 2017 Linux 容器映像。

   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```

   > [!TIP]
   > 本教程中提供了分别适用于 bash shell (Linux/Mac) 和 PowerShell (Windows) 的 docker 命令示例。

1. 要通过 Docker 运行容器映像，可使用下列命令：

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql1' -p 1401:1433 \
      -v sql1data:/var/opt/mssql \
      -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql1" -p 1401:1433 `
      -v sql1data:/var/opt/mssql `
      -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   此命令使用开发人员版本创建 SQL Server 2017 容器（默认）。 SQL Server 端口 1433 在主机上公开为端口 1401   。 可选的 `-v sql1data:/var/opt/mssql` 参数创建名为 sql1ddata  的数据卷容器。 这用于保留 SQL Server 创建的数据。

   > [!IMPORTANT]
   > 该示例在 Docker 中使用数据卷容器。 如果改为选择映射主机目录，请注意，在用于 Mac 和 Windows 的 Docker 上此方法存在一些限制。 有关详细信息，请参阅 [在 Docker 上配置 SQL Server 容器映像](./sql-server-linux-docker-container-configure.md#persist)。

1. 要查看 Docker 容器，请使用 `docker ps` 命令。

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

1. 如果“状态”列显示“正常运行”，则 SQL Server 将在容器中运行，并侦听“端口”列中指定的端口    。 如果 SQL Server 容器的“状态”列显示“已退出”，则参阅[配置指南的疑难解答部分](./sql-server-linux-docker-container-troubleshooting.md)。

  ```bash
  $ sudo docker ps -a

  CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
  941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour    0.0.0.0:1401->1433/tcp   sql1
  ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. 在 Linux/Mac 上打开 bash 终端，或在 Windows 上打开提升的 PowerShell 会话。

1. 从 Docker Hub 拉取 SQL Server 2019 Linux 容器映像。

   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   > [!TIP]
   > 本教程中提供了分别适用于 bash shell (Linux/Mac) 和 PowerShell (Windows) 的 docker 命令示例。

1. 要通过 Docker 运行容器映像，可使用下列命令：

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql1' -p 1401:1433 \
      -v sql1data:/var/opt/mssql \
      -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql1" -p 1401:1433 `
      -v sql1data:/var/opt/mssql `
      -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   此命令使用开发人员版本创建 SQL Server 2019 容器（默认）。 SQL Server 端口 1433 在主机上公开为端口 1401   。 可选的 `-v sql1data:/var/opt/mssql` 参数创建名为 sql1ddata  的数据卷容器。 这用于保留 SQL Server 创建的数据。

1. 要查看 Docker 容器，请使用 `docker ps` 命令。

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

1. 如果“状态”列显示“正常运行”，则 SQL Server 将在容器中运行，并侦听“端口”列中指定的端口    。 如果 SQL Server 容器的“状态”列显示“已退出”，则参阅[配置指南的疑难解答部分](./sql-server-linux-docker-container-troubleshooting.md)。

   ```bash
   $ sudo docker ps -a

   CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
   941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour    0.0.0.0:1401->1433/tcp   sql1
   ```

::: moniker-end

## <a name="change-the-sa-password"></a>更改 SA 密码

[!INCLUDE [Change docker password](../includes/sql-server-linux-change-docker-password.md)]

## <a name="copy-a-backup-file-into-the-container"></a>将备份文件复制到容器中

本教程使用 [Wide World Importers 示例数据库](../samples/wide-world-importers-what-is.md)。 使用以下步骤下载 Wide World Importers 数据库备份文件并将其复制到 SQL Server 容器。

1. 首先，使用 docker exec  来创建备份文件夹。 以下命令在 SQL Server 容器中创建 /var/opt/mssql/backup 目录  。

   ```bash
   sudo docker exec -it sql1 mkdir /var/opt/mssql/backup
   ```

   ```PowerShell
   docker exec -it sql1 mkdir /var/opt/mssql/backup
   ```

1. 接下来，将 [WideWorldImporters-Full.bak](https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0) 文件下载到主机。 以下命令导航到主页/用户目录，并将备份文件下载为 wwi.bak  。

   ```bash
   cd ~
   curl -L -o wwi.bak 'https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak'
   ```

   ```PowerShell
   curl -OutFile "wwi.bak" "https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak"
   ```

1. 使用 docker cp 将备份文件复制到 /var/opt/mssql/backup 目录的容器中   。

   ```bash
   sudo docker cp wwi.bak sql1:/var/opt/mssql/backup
   ```

   ```PowerShell
   docker cp wwi.bak sql1:/var/opt/mssql/backup
   ```

## <a name="restore-the-database"></a>还原数据库

备份文件现在位于容器内。 在还原备份之前，请务必了解备份中的逻辑文件名和文件类型。 以下 Transact-SQL 命令使用容器中的 sqlcmd 检查备份并执行还原  。

> [!TIP]
> 本教程在容器中使用 sqlcmd，因为容器内预安装了该工具  。 不过，也可以使用容器外的其他客户端工具，如 [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md) 或 [SQL Server Management Studio](sql-server-linux-manage-ssms.md)，来运行 Transact-SQL 语句。 若要连接，请使用映射到容器中的 1433 端口的主机端口。 在此示例中，为主机上的“localhost,1401”和远程的“Host_IP_Address,1401”   。

1. 在容器中运行 sqlcmd，列出备份中的逻辑文件名和路径  。 这是通过“RESTORE FILELISTONLY”Transact-SQL 语句实现的  。

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost \
      -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'RESTORE FILELISTONLY FROM DISK = "/var/opt/mssql/backup/wwi.bak"' \
      | tr -s ' ' | cut -d ' ' -f 1-2
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost `
      -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/wwi.bak'"
   ```

   会得到类似于下面的输出：

   ```
   LogicalName   PhysicalName
   ------------------------------------------
   WWI_Primary   D:\Data\WideWorldImporters.mdf
   WWI_UserData   D:\Data\WideWorldImporters_UserData.ndf
   WWI_Log   E:\Log\WideWorldImporters.ldf
   WWI_InMemory_Data_1   D:\Data\WideWorldImporters_InMemory_Data_1
   ```

1. 调用 RESTORE DATABASE 命令，还原容器中的数据库  。 为上一步中的每个文件指定新路径。

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'RESTORE DATABASE WideWorldImporters FROM DISK = "/var/opt/mssql/backup/wwi.bak" WITH MOVE "WWI_Primary" TO "/var/opt/mssql/data/WideWorldImporters.mdf", MOVE "WWI_UserData" TO "/var/opt/mssql/data/WideWorldImporters_userdata.ndf", MOVE "WWI_Log" TO "/var/opt/mssql/data/WideWorldImporters.ldf", MOVE "WWI_InMemory_Data_1" TO "/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1"'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "RESTORE DATABASE WideWorldImporters FROM DISK = '/var/opt/mssql/backup/wwi.bak' WITH MOVE 'WWI_Primary' TO '/var/opt/mssql/data/WideWorldImporters.mdf', MOVE 'WWI_UserData' TO '/var/opt/mssql/data/WideWorldImporters_userdata.ndf', MOVE 'WWI_Log' TO '/var/opt/mssql/data/WideWorldImporters.ldf', MOVE 'WWI_InMemory_Data_1' TO '/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1'"
   ```

   会得到类似于下面的输出：

   ```
   Processed 1464 pages for database 'WideWorldImporters', file 'WWI_Primary' on file 1.
   Processed 53096 pages for database 'WideWorldImporters', file 'WWI_UserData' on file 1.
   Processed 33 pages for database 'WideWorldImporters', file 'WWI_Log' on file 1.
   Processed 3862 pages for database 'WideWorldImporters', file 'WWI_InMemory_Data_1' on file 1.
   Converting database 'WideWorldImporters' from version 852 to the current version 869.
   Database 'WideWorldImporters' running the upgrade step from version 852 to version 853.
   Database 'WideWorldImporters' running the upgrade step from version 853 to version 854.
   Database 'WideWorldImporters' running the upgrade step from version 854 to version 855.
   Database 'WideWorldImporters' running the upgrade step from version 855 to version 856.
   Database 'WideWorldImporters' running the upgrade step from version 856 to version 857.
   Database 'WideWorldImporters' running the upgrade step from version 857 to version 858.
   Database 'WideWorldImporters' running the upgrade step from version 858 to version 859.
   Database 'WideWorldImporters' running the upgrade step from version 859 to version 860.
   Database 'WideWorldImporters' running the upgrade step from version 860 to version 861.
   Database 'WideWorldImporters' running the upgrade step from version 861 to version 862.
   Database 'WideWorldImporters' running the upgrade step from version 862 to version 863.
   Database 'WideWorldImporters' running the upgrade step from version 863 to version 864.
   Database 'WideWorldImporters' running the upgrade step from version 864 to version 865.
   Database 'WideWorldImporters' running the upgrade step from version 865 to version 866.
   Database 'WideWorldImporters' running the upgrade step from version 866 to version 867.
   Database 'WideWorldImporters' running the upgrade step from version 867 to version 868.
   Database 'WideWorldImporters' running the upgrade step from version 868 to version 869.
   RESTORE DATABASE successfully processed 58455 pages in 18.069 seconds (25.273 MB/sec).
   ```

## <a name="verify-the-restored-database"></a>验证还原的数据库

运行以下查询以在容器中显示数据库名称的列表：

```bash
sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
   -Q 'SELECT Name FROM sys.Databases'
```

```PowerShell
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
   -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
   -Q "SELECT Name FROM sys.Databases"
```

数据库列表中应会显示 WideWorldImporters  。

## <a name="make-a-change"></a>做出更改

以下步骤在数据库做出更改。

1. 运行查询，查看 Warehouse.StockItems 表中的前 10 项  。

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT TOP 10 StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems ORDER BY StockItemID'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT TOP 10 StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems ORDER BY StockItemID"
   ```

   应会看到项标识符和名称的列表：

   ```
   StockItemID StockItemName
   ----------- -----------------
             1 USB missile launcher (Green)
             2 USB rocket launcher (Gray)
             3 Office cube periscope (Black)
             4 USB food flash drive - sushi roll
             5 USB food flash drive - hamburger
             6 USB food flash drive - hot dog
             7 USB food flash drive - pizza slice
             8 USB food flash drive - dim sum 10 drive variety pack
             9 USB food flash drive - banana
            10 USB food flash drive - chocolate bar
   ```

1. 用以下 UPDATE 语句更新第一项的说明  ：

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'UPDATE WideWorldImporters.Warehouse.StockItems SET StockItemName="USB missile launcher (Dark Green)" WHERE StockItemID=1; SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "UPDATE WideWorldImporters.Warehouse.StockItems SET StockItemName='USB missile launcher (Dark Green)' WHERE StockItemID=1; SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   会得到类似于下面文本的输出：

   ```
   (1 rows affected)
   StockItemID StockItemName
   ----------- ------------------------------------
             1 USB missile launcher (Dark Green)
   ```

## <a name="create-a-new-backup"></a>新建备份

将数据库还原到容器后，可能还需要定期在正在运行的容器中创建数据库备份。 步骤与之前步骤的模式类似，但顺序相反。

1. 使用“BACKUP DATABASE”Transact-SQL 命令在容器中创建数据库备份  。 本教程在之前创建的 /var/opt/mssql/backup 目录中创建新的备份文件 wwi_2.bak。

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q "BACKUP DATABASE [WideWorldImporters] TO DISK = N'/var/opt/mssql/backup/wwi_2.bak' WITH NOFORMAT, NOINIT, NAME = 'WideWorldImporters-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "BACKUP DATABASE [WideWorldImporters] TO DISK = N'/var/opt/mssql/backup/wwi_2.bak' WITH NOFORMAT, NOINIT, NAME = 'WideWorldImporters-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
   ```

   会得到类似于下面的输出：

   ```
   10 percent processed.
   20 percent processed.
   30 percent processed.
   40 percent processed.
   50 percent processed.
   60 percent processed.
   70 percent processed.
   Processed 1200 pages for database 'WideWorldImporters', file 'WWI_Primary' on file 1.
   Processed 53096 pages for database 'WideWorldImporters', file 'WWI_UserData' on file 1.
   80 percent processed.
   Processed 3865 pages for database 'WideWorldImporters', file 'WWI_InMemory_Data_1' on file 1.
   Processed 938 pages for database 'WideWorldImporters', file 'WWI_Log' on file 1.
   100 percent processed.
   BACKUP DATABASE successfully processed 59099 pages in 25.056 seconds (18.427 MB/sec).
   ```

1. 接下来，将备份文件从容器中复制到主计算机上。

   ```bash
   cd ~
   sudo docker cp sql1:/var/opt/mssql/backup/wwi_2.bak wwi_2.bak
   ls -l wwi*
   ```

   ```PowerShell
   cd ~
   docker cp sql1:/var/opt/mssql/backup/wwi_2.bak wwi_2.bak
   ls wwi*
   ```

## <a name="use-the-persisted-data"></a>使用持久化数据

除了获取数据库备份来保护数据外，还可以使用数据卷容器。 本教程的开头使用 `-v sql1data:/var/opt/mssql` 参数创建了 sql1 容器。 即使已删除了容器，sql1data 数据卷容器仍会保留 /var/opt/mssql 数据   。 以下步骤完全删除 sql1 容器  ，然后使用保存的数据创建新的容器 sql2  。

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. 停止 sql1 容器  。

   ```bash
   sudo docker stop sql1
   ```

   ```PowerShell
   docker stop sql1
   ```

1. 删除容器。 这不会删除以前创建的 sql1data 数据卷容器和其中保存的数据  。

   ```bash
   sudo docker rm sql1
   ```

   ```PowerShell
   docker rm sql1
   ```

1. 创建新的容器 **sql2**，并重新使用 **sql1data** 数据卷容器。

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql2' -e 'MSSQL_PID=Developer' -p 1401:1433 \
      -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql2" -e "MSSQL_PID=Developer" -p 1401:1433 `
      -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

1. Wide World Importers 数据库现在位于新容器中。 运行查询以验证以前所做的更改。

   ```bash
   sudo docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   > [!NOTE]
   > SA 密码不是你为 sql2 容器指定的密码  `MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>`。 所有 SQL Server 数据都是从 sql1 还原的，其中包括本教程前面部分中已更改的密码  。 实际上，由于在 /var/opt/mssql. 中还原数据，与此类似的某些选项会被忽略。 出于此原因，密码是 `<YourNewStrong!Passw0rd>`，如下所示。

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. 停止 sql1 容器  。

   ```bash
   sudo docker stop sql1
   ```

   ```PowerShell
   docker stop sql1
   ```

1. 删除容器。 这不会删除以前创建的 sql1data 数据卷容器和其中保存的数据  。

   ```bash
   sudo docker rm sql1
   ```

   ```PowerShell
   docker rm sql1
   ```

1. 创建新的容器 **sql2**，并重新使用 **sql1data** 数据卷容器。

    ```bash
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
       --name 'sql2' -e 'MSSQL_PID=Developer' -p 1401:1433 \
       -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
    ```

    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
       --name "sql2" -e "MSSQL_PID=Developer" -p 1401:1433 `
       -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
    ```

1. Wide World Importers 数据库现在位于新容器中。 运行查询以验证以前所做的更改。

   ```bash
   sudo docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   > [!NOTE]
   > SA 密码不是你为 sql2 容器指定的密码  `MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>`。 所有 SQL Server 数据都是从 sql1 还原的，其中包括本教程前面部分中已更改的密码  。 实际上，由于在 /var/opt/mssql. 中还原数据，与此类似的某些选项会被忽略。 出于此原因，密码是 `<YourNewStrong!Passw0rd>`，如下所示。

::: moniker-end

## <a name="next-steps"></a>后续步骤

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

在本教程中，已学习如何在 Windows 上备份数据库，并将其移动到运行 SQL Server 的 Linux 服务器 2017。 你已了解如何执行以下操作：

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

在本教程中，了解了如何在 Windows 上备份数据库，以及如何将其移动到运行 SQL Server 2019 的 Linux 服务器。 你已了解如何执行以下操作：

::: moniker-end

> [!div class="checklist"]
> * 创建 SQL Server Linux 容器映像。
> * 将 SQL Server 数据库备份复制到容器中。
> * 在容器中使用 sqlcmd 运行 Transact-SQL 语句  。
> * 创建和从容器中提取备份文件。
> * 使用 Docker 中的数据卷容器来持久保存 SQL Server 数据。

接下来，查看其他 Docker 配置和故障排除方案：

> [!div class="nextstepaction"]
>[Docker 上的 SQL Server 2017 配置指南](./sql-server-linux-docker-container-deployment.md)