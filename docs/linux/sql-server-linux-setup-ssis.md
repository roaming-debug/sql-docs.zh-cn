---
title: 在 Linux 上安装 SQL Server Integration Services
description: 本文介绍如何在 Linux 上安装 SQL Server Integration Services (SSIS)。 可以在 Ubuntu 16.04 和 Red Hat Enterprise Linux 上安装 SSIS。
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 01/09/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: e34fd6c218950b86a46f43842c06408feefedfc9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471438"
---
# <a name="install-sql-server-integration-services-ssis-on-linux"></a>在 Linux 上安装 SQL Server Integration Services (SSIS)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

按照本文中的步骤在 Linux 上安装 SQL Server Integration Services (mssql-server-is)。 有关此版 Integration Services for Linux 支持的功能的信息，请参阅[发行说明](sql-server-linux-release-notes.md)。

可以在以下平台上安装 SQL Server Integration Services：

- [Ubuntu 16.04](#ubuntu)
- [Red Hat Enterprise Linux](#RHEL)

## <a name="install-ssis-on-ubuntu"></a><a name="ubuntu"></a>在 Ubuntu 上安装 SSIS

要在 Ubuntu 上安装 mssql-server-is 包，请按照以下步骤操作：

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. 导入公共存储库 GPG 密钥。

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. 注册 SQL Server Ubuntu 存储库。

   ```bash
   sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```

1. 运行以下命令，安装 SQL Server Integration Services。

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server-is
   ```

1. 安装 Integration Services 后，运行 ssis-conf。 有关详细信息，请参阅[使用 ssis-conf 在 Linux 上配置 SSIS](sql-server-linux-configure-ssis.md)。

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. 完成配置后，设置 PATH 环境变量。

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. 导入公共存储库 GPG 密钥。

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. 注册 SQL Server Ubuntu 存储库。

   ```bash
   sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   ```

1. 运行以下命令，安装 SQL Server Integration Services。

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server-is
   ```

1. 安装 Integration Services 后，运行 ssis-conf。 有关详细信息，请参阅[使用 ssis-conf 在 Linux 上配置 SSIS](sql-server-linux-configure-ssis.md)。

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. 完成配置后，设置 PATH 环境变量。

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

### <a name="update-ssis"></a>更新 SSIS

如果已安装 mssql-server-fts，可使用下列命令将其更新至最新版本：

```bash
sudo apt-get install mssql-server-is
```

### <a name="remove-ssis"></a>删除 SSIS

要删除 mssql-server-is，请运行以下命令：

```bash
sudo apt-get remove mssql-server-is
```

## <a name="install-ssis-on-rhel"></a><a name="RHEL"></a>在 RHEL 上安装 SSIS
要在 RHEL 上安装 mssql-server-is 包，请按照以下步骤操作：

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. 下载 SQL Server Red Hat 存储库配置文件。

   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
   ```

1. 运行以下命令，安装 SQL Server Integration Services。

   ```bash
   sudo yum install -y mssql-server-is
   ```

1. 安装后，运行 ssis-conf。 有关详细信息，请参阅[使用 ssis-conf 在 Linux 上配置 SSIS](sql-server-linux-configure-ssis.md)。

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. 完成配置后，设置 PATH 环境变量。

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. 下载 SQL Server Red Hat 存储库配置文件。

   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   ```

1. 运行以下命令，安装 SQL Server Integration Services。

   ```bash
   sudo yum install -y mssql-server-is
   ```

1. 安装后，运行 ssis-conf。 有关详细信息，请参阅[使用 ssis-conf 在 Linux 上配置 SSIS](sql-server-linux-configure-ssis.md)。

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. 完成配置后，设置 PATH 环境变量。

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

### <a name="update-ssis"></a>更新 SSIS

如果已安装 mssql-server-fts，可使用下列命令将其更新至最新版本：

```bash
sudo yum update mssql-server-is
```

### <a name="remove-ssis"></a>删除 SSIS
要删除 mssql-server-is，请运行以下命令：

```bash
sudo yum remove mssql-server-is
```

## <a name="unattended-installation"></a>无人参与的安装

要将 ssis-conf setup 作为无人参与的安装运行，请执行以下步骤：

1. 指定 -n（无提示）选项。
1. 通过设置环境变量来提供所需的值。

下面的示例执行以下操作：

- 安装 SSIS
- 通过为 SSIS_PID 环境变量提供值来指定开发人员版本
- 通过提供 ACCEPT_EULA 环境变量的值来接受 Microsoft 软件许可条款
- 通过指定 -n（无提示）选项来运行无人参与安装

```
sudo SSIS_PID=Developer ACCEPT_EULA=Y /opt/ssis/bin/ssis-conf -n setup 
```

### <a name="environment-variables-for-unattended-installation"></a>无人参与安装的环境变量

| 环境变量 | 说明 |
|---|---|
| ACCEPT_EULA | 设置为任何值（例如“Y”）时，都接受 SQL Server 许可条款。|
| SSIS_PID | 设置 SQL Server 版本或产品密钥。 可能的值有：<ul><li>计算</li><li>开发人员</li><li>Express</li><li>Web</li><li>Standard</li><li>企业</li><li>产品密钥</li></ul>指定产品密钥时，其格式必须为：#####-#####-#####-#####-#####，其中 # 为字母或数字     。  |
| | |

## <a name="next-steps"></a>后续步骤

若要在 Linux 上运行 SSIS 包，请参阅[使用 SSIS 在 Linux 上提取、转换和加载用于 SQL Server 的数据](sql-server-linux-migrate-ssis.md)。

若要在 linux 上配置其他 SSIS 设置，请参阅[使用 ssis-conf 在 linux 上配置 SQL Server Integration Services](sql-server-linux-configure-ssis.md)。

## <a name="related-content-about-ssis-on-linux"></a>有关 Linux 上的 SSIS 的相关内容

- [使用 SSIS 在 Linux 上提取、转换和加载数据](sql-server-linux-migrate-ssis.md)
- [使用 ssis-conf 在 Linux 上配置 SQL Server Integration Services](sql-server-linux-configure-ssis.md)
- [适用于 Linux 上 SSIS 的限制和已知问题](sql-server-linux-ssis-known-issues.md)
- [使用 cron 在 Linux 上计划 SQL Server Integration Services 包执行](sql-server-linux-schedule-ssis-packages.md)
