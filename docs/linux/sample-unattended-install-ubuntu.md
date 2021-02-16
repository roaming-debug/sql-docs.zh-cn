---
title: 在 Ubuntu 上以无人参与的方式安装 SQL Server
titleSuffix: SQL Server
description: 了解如何使用示例 Bash 脚本在没有交互式输入的 Ubuntu 16.04 上安装 SQL Server 2017。
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 573eadb498351662a965e973e7ea38d4226c6cc8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339136"
---
# <a name="sample-unattended-sql-server-installation-script-for-ubuntu"></a>示例：Ubuntu 的无人参与 SQL Server 安装脚本

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

此示例 Bash 脚本可在 Ubuntu 16.04 上安装 SQL Server 2017，且无需交互式输入。 它提供数据库引擎、SQL Server 命令行工具和 SQL Server 代理的安装示例，并执行安装后步骤。 可以选择安装全文搜索并创建管理用户。

> [!TIP]
> 如果不需要无人参与安装脚本，则安装 SQL Server 最快速的方法是遵循 [Ubuntu 的快速入门](quickstart-install-connect-ubuntu.md)。 有关其他设置信息，请参阅 [Linux 上的 SQL Server 的安装指南](sql-server-linux-setup.md)。

## <a name="prerequisites"></a>先决条件

- 至少需要 2GB 内存才能运行 Linux 上的 SQL Server。
- 文件系统必须是 XFS 或 EXT4   。 其他文件系统（如 BTRFS）均不受支持  。
- 有关其他系统要求，请参阅 [Linux 上的 SQL Server 的系统要求](sql-server-linux-setup.md#system)。

## <a name="sample-script"></a>示例脚本

> [!NOTE]
> 如果 SQL Server 启动缓慢，则脚本可能会失败。 这是因为脚本将以非零状态退出。 删除第一行上的 `-e` 开关可以解决此问题。

```bash
#!/bin/bash -e

# Use the following variables to control your install:

# Password for the SA user (required)
MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>'

# Product ID of the version of SQL server you're installing
# Must be evaluation, developer, express, web, standard, enterprise, or your 25 digit product key
# Defaults to developer
MSSQL_PID='evaluation'

# Install SQL Server Agent (recommended)
SQL_INSTALL_AGENT='y'

# Install SQL Server Full Text Search (optional)
# SQL_INSTALL_FULLTEXT='y'

# Create an additional user with sysadmin privileges (optional)
# SQL_INSTALL_USER='<Username>'
# SQL_INSTALL_USER_PASSWORD='<YourStrong!Passw0rd>'

if [ -z $MSSQL_SA_PASSWORD ]
then
  echo Environment variable MSSQL_SA_PASSWORD must be set for unattended install
  exit 1
fi

echo Adding Microsoft repositories...
sudo curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
repoargs="$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
sudo add-apt-repository "${repoargs}"
repoargs="$(curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list)"
sudo add-apt-repository "${repoargs}"

echo Running apt-get update -y...
sudo apt-get update -y

echo Installing SQL Server...
sudo apt-get install -y mssql-server

echo Running mssql-conf setup...
sudo MSSQL_SA_PASSWORD=$MSSQL_SA_PASSWORD \
     MSSQL_PID=$MSSQL_PID \
     /opt/mssql/bin/mssql-conf -n setup accept-eula

echo Installing mssql-tools and unixODBC developer...
sudo ACCEPT_EULA=Y apt-get install -y mssql-tools unixodbc-dev

# Add SQL Server tools to the path by default:
echo Adding SQL Server tools to your path...
echo PATH="$PATH:/opt/mssql-tools/bin" >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc

# Optional SQL Server Agent installation:
if [ ! -z $SQL_INSTALL_AGENT ]
then
  echo Installing SQL Server Agent...
  sudo apt-get install -y mssql-server-agent
fi

# Optional SQL Server Full Text Search installation:
if [ ! -z $SQL_INSTALL_FULLTEXT ]
then
    echo Installing SQL Server Full-Text Search...
    sudo apt-get install -y mssql-server-fts
fi

# Configure firewall to allow TCP port 1433:
echo Configuring UFW to allow traffic on port 1433...
sudo ufw allow 1433/tcp
sudo ufw reload

# Optional example of post-installation configuration.
# Trace flags 1204 and 1222 are for deadlock tracing.
# echo Setting trace flags...
# sudo /opt/mssql/bin/mssql-conf traceflag 1204 1222 on

# Restart SQL Server after installing:
echo Restarting SQL Server...
sudo systemctl restart mssql-server

# Connect to server and get the version:
counter=1
errstatus=1
while [ $counter -le 5 ] && [ $errstatus = 1 ]
do
  echo Waiting for SQL Server to start...
  sleep 3s
  /opt/mssql-tools/bin/sqlcmd \
    -S localhost \
    -U SA \
    -P $MSSQL_SA_PASSWORD \
    -Q "SELECT @@VERSION" 2>/dev/null
  errstatus=$?
  ((counter++))
done

# Display error if connection failed:
if [ $errstatus = 1 ]
then
  echo Cannot connect to SQL Server, installation aborted
  exit $errstatus
fi

# Optional new user creation:
if [ ! -z $SQL_INSTALL_USER ] && [ ! -z $SQL_INSTALL_USER_PASSWORD ]
then
  echo Creating user $SQL_INSTALL_USER
  /opt/mssql-tools/bin/sqlcmd \
    -S localhost \
    -U SA \
    -P $MSSQL_SA_PASSWORD \
    -Q "CREATE LOGIN [$SQL_INSTALL_USER] WITH PASSWORD=N'$SQL_INSTALL_USER_PASSWORD', DEFAULT_DATABASE=[master], CHECK_EXPIRATION=ON, CHECK_POLICY=ON; ALTER SERVER ROLE [sysadmin] ADD MEMBER [$SQL_INSTALL_USER]"
fi

echo Done!
```

### <a name="running-the-script"></a>运行脚本

运行该脚本

1. 将示例粘贴到你最喜欢的文本编辑器中，并使用便于记忆的名称保存它，例如 `install_sql.sh`。

1. 自定义 `MSSQL_SA_PASSWORD`、`MSSQL_PID` 和要更改的任何其他变量。

1. 将脚本标记为可执行文件

   ```bash
   chmod +x install_sql.sh
   ```

1. 运行脚本

   ```bash
   ./install_sql.sh
   ```

### <a name="understanding-the-script"></a>了解脚本
Bash 脚本执行的第一件事是设置几个变量。 它们可以是脚本变量（例如本示例），也可以是环境变量。 `MSSQL_SA_PASSWORD` 变量是安装 SQL Server 所必需的变量，其余变量是为脚本创建的自定义变量  。 示例脚本执行以下步骤：

1. 导入公共 Microsoft GPG 密钥。

1. 注册用于 SQL Server 的 Microsoft 存储库和命令行工具。

1. 更新本地存储库

1. 安装 SQL Server

1. 将 SQL Server 配置为 ```MSSQL_SA_PASSWORD``` 并自动接受最终用户许可协议。

1. 自动接受 SQL Server 命令行工具的最终用户许可协议，安装这些工具，然后安装 unixodbc-dev 包。

1. 为便于使用，请将 SQL Server 命令行工具添加到路径。

1. 如果已设置 ```SQL_INSTALL_AGENT``` 变量的脚本，则将默认安装 SQL Server 代理。

1. 如果已设置 ```SQL_INSTALL_FULLTEXT``` 变量，则可以选择安装 SQL Server 全文搜索。

1. 在系统防火墙上取消阻止 TCP 端口 1433，这是从另一个系统连接到 SQL Server 所必需的。

1. （可选）设置跟踪标志以进行死锁跟踪。 （需要取消注释行）

1. SQL Server 现已安装，若要使其可操作，请重启该过程。

1. 验证是否已正确安装 SQL Server，并隐藏所有错误消息。

1. 如果同时设置 ```SQL_INSTALL_USER``` 和 ```SQL_INSTALL_USER_PASSWORD```，则将创建新的服务器管理用户。

## <a name="next-steps"></a>后续步骤

简化多个无人参与安装，并创建独立的 Bash 脚本来设置适当的环境变量。 可删除该示例脚本使用的任何变量，并将其放入自己的 Bash 脚本。

```bash
#!/bin/bash
export MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>'
export MSSQL_PID='evaluation'
export SQL_INSTALL_AGENT='y'
export SQL_INSTALL_USER='<Username>'
export SQL_INSTALL_USER_PASSWORD='<YourStrong!Passw0rd>'
export SQL_INSTALL_AGENT='y'
```

然后运行 Bash 脚本，如下所示：
```bash
. ./my_script_name.sh
```

有关 Linux 上的 SQL Server 的详细信息，请参阅 [Linux 上的 SQL Server 概述](sql-server-linux-overview.md)。
