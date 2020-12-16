---
title: 为 Linux 上的 SQL Server 配置使用情况和诊断数据收集
description: 介绍如何在 Linux 上收集和配置 SQL Server 客户使用情况及诊断数据。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: bfc63894d7e2ada81ca230c1a66d32bd49d6d91d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471398"
---
# <a name="configure-usage--diagnostic-data-collection-for-sql-server-on-linux"></a>为 Linux 上的 SQL Server 配置使用情况和诊断数据收集

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

默认情况下，Microsoft SQL Server 收集有关其客户如何使用应用程序的信息。 具体来说，SQL Server 收集有关安装体验、使用情况和性能的信息。 此信息有助于 Microsoft 改进产品以更好地满足客户需求。 例如，Microsoft 收集有关客户遇到的错误代码类型信息，这样我们就可以修复相关 bug，改进关于如何使用 SQL Server 的文档，并确定是否应将功能添加到产品中以更好地为客户服务。

本文档提供有关收集的信息类型以及如何配置 Linux 上的 Microsoft SQL Server 以将收集的信息发送给 Microsoft 的详细信息。 SQL Server 2017 包含隐私声明，其中对我们会从用户处收集的信息以及我们不会从用户处收集的信息进行了说明。 有关详细信息，请参阅[隐私声明](../sql-server/sql-server-privacy.md)。

具体而言，Microsoft 不会通过这种机制发送以下任何类型的信息：

- 用户表中的任何值
- 任何登录凭据或其他身份验证信息
- 个人身份信息 (PII)

SQL Server 2017 始终收集和发送从安装过程开始的安装体验相关信息，方便我们快速查找并修复客户遇到的任何安装问题。 通过 **mssql-conf** 可将 SQL Server 2017 配置为不向 Microsoft 发送信息（基于每个服务器实例）。 mssql-conf 是随 SQL Server 2017 for Red Hat Enterprise Linux、SUSE Linux Enterprise Server 和 Ubuntu 安装的配置脚本。

> [!NOTE]
> 只能在付费版本的 SQL Server 中禁用向 Microsoft 发送信息的功能。

## <a name="disable-usage-and-diagnostic-data-collection"></a>禁用使用情况和诊断数据收集

此选项允许更改 SQL Server 是否将使用情况和诊断数据收集发送给 Microsoft。 默认情况下，此值设置为 true。 若要更改该值，请运行以下命令：

> [!IMPORTANT]
> 无法关闭 SQL Server、Express 和 Developer 免费版本的使用情况和诊断数据收集。

### <a name="on-red-hat-suse-and-ubuntu"></a>在 Red Hat、SUSE 和 Ubuntu 上

1. 使用 **telemetry.customerfeedback** 的 **set** 命令以根身份运行 mssql-conf 脚本。 以下示例通过指定 **false** 来关闭使用情况和诊断数据收集。

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. 重启 SQL Server 服务：

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>在 Docker 上
若要在 docker 上禁用使用情况和诊断数据收集，则必须让 Docker [保留你的数据](./sql-server-linux-docker-container-deployment.md)。 

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. 在主机目录中添加包含行 `[telemetry]` 和 `customerfeedback = false` 的 `mssql.conf` 文件：
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```

2. 运行容器映像

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. 在主机目录中添加包含行 `[telemetry]` 和 `customerfeedback = false` 的 `mssql.conf` 文件：

   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```

2. 运行容器映像

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

::: moniker-end

## <a name="local-audit-for-sql-server-on-linux-usage-and-diagnostic-data-collection"></a>Linux 上的 SQL Server 使用情况和诊断数据收集的本地审核

Microsoft SQL Server 2017 包含支持 Internet 的功能，可以收集关于计算机或设备的信息（“标准计算机信息”）并发送给 Microsoft。 SQL Server 使用情况和诊断数据收集的本地审核组件将服务收集的数据写入指定文件夹（表示将发送给 Microsoft 的数据[日志]）。 本地审核的用途是使客户可以出于合规性、监管或隐私验证原因而查看 Microsoft 使用此功能收集的所有数据。

在 Linux 上的 SQL Server 中，本地审核可在实例级别针对 SQL Server 数据库引擎进行配置。 其他 SQL Server 组件和 SQL Server 工具没有用于使用情况和诊断数据收集的本地审核功能。

### <a name="enable-local-audit"></a>启用本地审核

此选项可启用本地审核，并允许设置创建本地审核日志的目录。

1. 为新的本地审核日志创建目标目录。 以下示例创建新的 **/tmp/audit** 目录：

   ```bash
   sudo mkdir /tmp/audit
   ```

2. 将目录的所有者和组更改为 **mssql** 用户：

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

3. 使用 **telemetry.userrequestedlocalauditdirectory** 的 **set** 命令以根身份运行 mssql-conf 脚本：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

4. 重启 SQL Server 服务：

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>在 Docker 上
若要在 docker 上启用本地审核，则必须让 Docker [保留你的数据](./sql-server-linux-docker-container-deployment.md)。 

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. 新的本地审核日志的目标目录将位于容器中。 在计算机的主机目录中为新的本地审核日志创建目标目录。 以下示例创建新的 **/audit** 目录：

   ```bash
   sudo mkdir <host directory>/audit
   ```

1. 在主机目录中添加包含行 `[telemetry]` 和 `userrequestedlocalauditdirectory = <host directory>/audit` 的 `mssql.conf` 文件：
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```

1. 运行容器映像

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. 新的本地审核日志的目标目录将位于容器中。 在计算机的主机目录中为新的本地审核日志创建目标目录。 以下示例创建新的 **/audit** 目录：

   ```bash
   sudo mkdir <host directory>/audit
   ```

1. 在主机目录中添加包含行 `[telemetry]` 和 `userrequestedlocalauditdirectory = <host directory>/audit` 的 `mssql.conf` 文件：
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```

1. 运行容器映像

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

::: moniker-end

## <a name="next-steps"></a>后续步骤

有关 Linux 上的 SQL Server 的详细信息，请参阅 [Linux 上的 SQL Server 概述](sql-server-linux-overview.md)。