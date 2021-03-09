---
title: 对 Linux 上的 SQL Server 进行故障排除
description: 对 Linux 上或 Docker 容器中运行的 Microsoft SQL Server 进行故障排除。 了解在何处可以找到有关支持的功能和已知限制的信息。
author: VanMSFT
ms.author: vanto
ms.date: 05/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 99636ee8-2ba6-4316-88e0-121988eebcf9S
ms.openlocfilehash: 1f2df7f7cb0660cb879334a1099f6b7ce639d211
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464632"
---
# <a name="troubleshoot-sql-server-on-linux"></a>对 Linux 上的 SQL Server 进行故障排除

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文档介绍如何对 Linux 上或 Docker 容器中运行的 Microsoft SQL Server 进行故障排除。 在对 Linux 上的 SQL Server 进行故障排除时，请记得查看 [Linux 上的 SQL Server 发行说明](sql-server-linux-release-notes.md)中的支持功能和已知限制。

> [!TIP]
> 有关常见问题的解答，请参阅 [Linux 上的 SQL Server 常见问题解答](sql-server-linux-faq.yml)。

## <a name="troubleshoot-connection-failures"></a><a id="connection"></a> 解决连接失败问题
如果在连接到 Linux SQL Server 时存在问题，可以检查以下几点。

- 如果无法使用 localhost 进行本地连接，请尝试改用 IP 地址 127.0.0.1。 Localhost 可能未正确映射到此地址。

- 验证是否可从客户端计算机访问服务器名称或 IP 地址。

   > [!TIP]
   > 若要查找 Ubuntu 计算机的 IP 地址，可运行 ifconfig 命令，如以下示例所示：
   >
   >   ```bash
   >   sudo ifconfig eth0 | grep 'inet addr'
   >   ```
   > 对于 Red Hat，可使用 ip addr，如以下示例所示：
   >
   >   ```bash
   >   sudo ip addr show eth0 | grep "inet"
   >   ```
   > 但如果是 Azure VM，则此方法不适用。 对于 Azure VM，请[在 Azure 门户中查找 VM 的公共 IP](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#connect)。

- 如果适用，请检查是否已在防火墙上打开了 SQL Server 端口（默认为 1433）。

- 对于 Azure VM，请检查是否有[默认 SQL Server 端口的网络安全组规则](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#remote)。

- 验证用户名和密码是否存在任何拼写错误、多余空格或错误大小写。

- 尝试以显式方式设置协议和端口号，确保服务器名称如下所示：tcp:servername,1433。

- 网络连接问题也可能导致连接错误和超时。 验证连接信息和网络连接后，请再次尝试连接。

## <a name="manage-the-sql-server-service"></a>管理 SQL Server 服务

以下部分说明如何启动、停止、重启 SQL Server 服务并检查其状态。

### <a name="manage-the-mssql-server-service-in-red-hat-enterprise-linux-rhel-and-ubuntu"></a>在 Red Hat Enterprise Linux (RHEL) 和 Ubuntu 中管理 mssql-server 服务 

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

### <a name="manage-the-execution-of-the-mssql-docker-container"></a>管理 mssql Docker 容器的执行

通过运行以下命令，可以获得最新创建的 SQL Server Docker 容器的状态和容器 ID（ID 位于“CONTAINER ID”列下）：

   ```bash
   sudo docker ps -l
   ```
   
可根据需要使用以下命令停止或重启 SQL Server 服务：
   
   ```bash
   sudo docker stop <container ID>
   sudo docker restart <container ID>
   ```

> [!TIP]
> 有关 Docker 的更多故障排除提示，请参阅 [SQL Server Docker 容器疑难解答](./sql-server-linux-docker-container-troubleshooting.md)。

## <a name="access-the-log-files"></a>访问日志文件
   
SQL Server 引擎在 Linux 和 Docker 安装的 /var/opt/mssql/log/errorlog 文件中进行记录。 需要启用“超级用户”模式才能浏览此目录。

安装程序在此处记录：/var/opt/mssql/setup-< time stamp representing time of install>。可使用任何 UTF-16 兼容工具（如“vim”或“cat”）浏览错误日志文件，如下所示： 

   ```bash
   sudo cat errorlog
   ```

如果愿意，还可以使用以下命令将文件转换为 UTF-8，通过“more”或“less”读取它们：
   
   ```bash
   sudo iconv -f UTF-16LE -t UTF-8 <errorlog> -o <output errorlog file>
   ```
## <a name="extended-events"></a>扩展的事件

可通过 SQL 命令查询扩展事件。  可在[此处](../relational-databases/extended-events/extended-events.md)找到扩展事件的详细信息：

## <a name="crash-dumps"></a>故障转储 

在 Linux 中查看日志目录中的转储。 在 /var/opt/mssql/log 目录下查看 Linux Core 转储（扩展名为 .tar.gz2）或 SQL 小型转储（扩展名为 .mdmp）

对于 Core 转储 
   ```bash
   sudo ls /var/opt/mssql/log | grep .tar.gz2 
   ```

对于 SQL 转储 
   ```bash
   sudo ls /var/opt/mssql/log | grep .mdmp 
   ```
   
## <a name="start-sql-server-in-minimal-configuration-or-in-single-user-mode"></a>在最低配置或单用户模式下启动 SQL Server

### <a name="start-sql-server-in-minimal-configuration-mode"></a>在最低配置模式下启动 SQL Server
在配置值的设置（例如，过度分配内存）妨碍服务器启动时，这非常有用。
  
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -f
   ```

### <a name="start-sql-server-in-single-user-mode"></a>在单用户模式下启动 SQL Server
在某些情况下，可能必须使用 startup option -m 在单用户模式下启动 SQL Server 实例 例如，您可能要更改服务器配置选项或恢复已破坏的 master 数据库或其他系统数据库。 例如，你可能希望更改服务器配置选项或恢复已破坏的 master 数据库或其他系统数据库   

在单用户模式下启动 SQL Server
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m
   ```

在单用户模式下使用 SQLCMD 启动 SQL Server
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m SQLCMD
   ```
  
> [!WARNING]  
>  使用“mssql”用户启动 Linux 上的 SQL Server 以防止将来的启动问题。 例如，“sudo -u mssql /opt/mssql/bin/sqlservr [STARTUP OPTIONS]” 

如果你不小心使用其他用户启动了 SQL Server，则必须先将 SQL Server 数据库文件的所有权更改回“mssql”用户，然后才能使用 systemd 启动 SQL Server。 例如，若要将 /var/opt/mssql 下所有数据库文件的所有权更改为“mssql”用户，请运行以下命令

   ```bash
   chown -R mssql:mssql /var/opt/mssql/
   ```

## <a name="rebuild-system-databases"></a>重新生成系统数据库
作为最后手段，可以选择将 master 和模型数据库重新生成为默认版本。

> [!WARNING]
> 这些步骤将删除已配置的所有 SQL Server 系统数据！ 这包括有关用户数据库的信息（但不包括用户数据库本身）。 它还将删除存储在系统数据库中的其他信息，包括以下各项：主密钥信息、在 master 中加载的任何证书、SA 登录密码、msdb 中的作业相关信息、msdb 中的 DB 邮件信息以及 sp_configure 选项。 只有在了解其含义后才能使用！

1. 停止 SQL Server。

   ```bash
   sudo systemctl stop mssql-server
   ```

1. 使用“force-setup”参数运行 sqlservr。 

   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr --force-setup
   ```
   
   > [!WARNING]
   > 请参阅上一个警告！ 此外，还必须以“mssql”用户身份运行，如下所示。

1. 看到消息“恢复已完成”后，请按 Ctrl+C。 这将关闭 SQL Server

1. 重新配置 SA 密码。

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```
   
1. 启动 SQL Server 并重新配置服务器。 这包括还原或重新附加任何用户数据库。

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="improve-performance"></a>提高性能

影响性能的因素有很多，包括数据库设计、硬件和工作负载需求。 如果希望改善性能，请首先查看本文中的最佳做法，[适用于 Linux 上的 SQL Server 的性能最佳做法和配置指南](sql-server-linux-performance-best-practices.md)。 然后，浏览一些可用于解决性能问题的工具。

- [查询存储](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [系统动态管理视图 (DMV)](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [SQL Server Management Studio 中的性能仪表板](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in)

## <a name="common-issues"></a>常见问题

1. 无法连接到远程 SQL Server 实例。

   请参阅[连接到 Linux 上的 SQL Server](#connection) 文章的疑难解答部分。

2. 错误：主机名不得超过 15 个字符。

   这是一个已知问题，只要尝试安装 SQL Server Debian 包的计算机名超过 15 个字符就会出现此问题。 除更改计算机名外，目前尚无其他解决方法。 可以编辑主机名文件并重启计算机以更改此名称。 以下[网站指南](https://www.cyberciti.biz/faq/ubuntu-change-hostname-command/)详细说明了此解决方法。

3. 重置系统管理 (SA) 密码。

   如果忘记了系统管理员 (SA) 密码，或者出于其他原因需要重置密码，请遵循以下步骤。

   > [!NOTE]
   > 以下步骤将暂时停止 SQL Server 服务。

   登录到主机终端，运行以下命令并按照提示重置 SA 密码：

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf setup
   ```

4. 在密码中使用特殊字符。

   如果在 SQL Server 登录密码中使用某些字符，则可能需要使用反斜杠对其进行转义，然后再将其用于终端中的 Linux 命令。 例如，如果在终端命令/shell 脚本中使用美元符号 ($)，则必须对其进行转义：

   无效：

   ```bash
   sudo sqlcmd -S myserver -U sa -P Test$$
   ```

   有效：

   ```bash
   sqlcmd -S myserver -U sa -P Test\$\$
   ```

   资源：[特殊字符](https://tldp.org/LDP/abs/html/special-chars.html)
   [转义](https://tldp.org/LDP/abs/html/escapingsection.html)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]