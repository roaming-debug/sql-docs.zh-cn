---
title: 为 Linux 上的 SQL Server 配置环境变量
description: 本文介绍如何使用环境变量在 Linux 上配置特定的 SQL Server 2017 设置。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: fb9455970c58daa65f8068792a4edc8ae7e20f51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343170"
---
# <a name="configure-sql-server-settings-with-environment-variables-on-linux"></a>在 Linux 上使用环境变量配置 SQL Server 设置

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

可在 Linux 上使用多种不同的环境变量配置 SQL Server 2017。 这些变量用于两个方案：

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

可在 Linux 上使用多种不同的环境变量配置 SQL Server 2019。 这些变量用于两个方案：

::: moniker-end

- 使用 `mssql-conf setup` 命令配置初始设置。
- [在 Docker 中配置新的 SQL Server 容器](quickstart-install-connect-docker.md)。

> [!TIP]
> 如果需要在完成这些设置方案之后配置 SQL Server，请参阅[使用 mssql-conf 工具配置 Linux 上的 SQL Server](sql-server-linux-configure-mssql-conf.md)。

## <a name="environment-variables"></a>环境变量

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| 环境变量 | 说明 |
|-----|-----|
| **ACCEPT_EULA** | 将 **ACCEPT_EULA** 变量设置为任意值，以确认接受 [最终用户许可协议](https://go.microsoft.com/fwlink/?LinkId=746388)。 SQL Server 映像的必需设置。 |
| **MSSQL_SA_PASSWORD** | 配置 SA 用户密码。 |
| **MSSQL_PID** | 设置 SQL Server 版本或产品密钥。 可能的值包括： </br></br>**Evaluation**</br>**开发人员**</br>**Express**</br>**Web**</br>**Standard**</br>**Enterprise**</br>**产品密钥**</br></br>如果指定产品密钥，则必须采用 #####-#####-#####-#####-##### 格式，其中“#”是数字或字母。|
| **MSSQL_LCID** | 设置用于 SQL Server 的语言 ID。 例如，1036 为法语。 |
| **MSSQL_COLLATION** | 设置 SQL Server 的默认排序规则。 这将替代语言 ID (LCID) 到排序规则的默认映射。 |
| **MSSQL_MEMORY_LIMIT_MB** | 设置 SQL Server 可以使用的最大内存量（以 MB 为单位）。 默认情况下，它为物理内存总量的 80%。 |
| **MSSQL_TCP_PORT** | 配置 SQL Server 侦听的 TCP 端口（默认为 1433）。 |
| **MSSQL_IP_ADDRESS** | 设置 IP 地址。 目前，IP 地址必须为 IPv4 样式 (0.0.0.0)。 |
| **MSSQL_BACKUP_DIR** | 设置默认备份目录位置。 |
| **MSSQL_DATA_DIR** | 更改新的 SQL Server 数据库数据文件 (.mdf) 的创建目录。 |
| **MSSQL_LOG_DIR** | 更改新的 SQL Server 数据库日志文件 (.ldf) 的创建目录。 |
| **MSSQL_DUMP_DIR** | 更改 SQL Server 存放内存转储和其他故障排除文件的默认目录。 |
| **MSSQL_ENABLE_HADR** | 启用可用性组。 例如，“1”为已启用，“0”为已禁用 |
| **MSSQL_AGENT_ENABLED** | 启用 SQL Server 代理。 例如，“true”为已启用，“false”为已禁用。 默认情况下，代理处于禁用状态。  |
| **MSSQL_MASTER_DATA_FILE** | 设置主数据库数据文件的位置。 在首次运行 SQL Server 之前，必须将其命名为 **master.mdf**。 |
| **MSSQL_MASTER_LOG_FILE** | 设置主数据库日志文件的位置。 在首次运行 SQL Server 之前，必须将其命名为 **mastlog.ldf**。 |
| **MSSQL_ERROR_LOG_FILE** | 设置错误日志文件的位置。 |

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

| 环境变量 | 说明 |
|-----|-----|
| **ACCEPT_EULA** | 将 **ACCEPT_EULA** 变量设置为任意值，以确认接受 [最终用户许可协议](https://go.microsoft.com/fwlink/?LinkId=746388)。 SQL Server 映像的必需设置。 |
| **MSSQL_SA_PASSWORD** | 配置 SA 用户密码。 |
| **MSSQL_PID** | 设置 SQL Server 版本或产品密钥。 可能的值包括： </br></br>**Evaluation**</br>**开发人员**</br>**Express**</br>**Web**</br>**Standard**</br>**Enterprise**</br>**产品密钥**</br></br>如果指定产品密钥，则必须采用 #####-#####-#####-#####-##### 格式，其中“#”是数字或字母。|
| **MSSQL_LCID** | 设置用于 SQL Server 的语言 ID。 例如，1036 为法语。 |
| **MSSQL_COLLATION** | 设置 SQL Server 的默认排序规则。 这将替代语言 ID (LCID) 到排序规则的默认映射。 |
| **MSSQL_MEMORY_LIMIT_MB** | 设置 SQL Server 可以使用的最大内存量（以 MB 为单位）。 默认情况下，它为物理内存总量的 80%。 |
| **MSSQL_TCP_PORT** | 配置 SQL Server 侦听的 TCP 端口（默认为 1433）。 |
| **MSSQL_IP_ADDRESS** | 设置 IP 地址。 目前，IP 地址必须为 IPv4 样式 (0.0.0.0)。 |
| **MSSQL_BACKUP_DIR** | 设置默认备份目录位置。 |
| **MSSQL_DATA_DIR** | 更改新的 SQL Server 数据库数据文件 (.mdf) 的创建目录。 |
| **MSSQL_LOG_DIR** | 更改新的 SQL Server 数据库日志文件 (.ldf) 的创建目录。 |
| **MSSQL_DUMP_DIR** | 更改 SQL Server 存放内存转储和其他故障排除文件的默认目录。 |
| **MSSQL_ENABLE_HADR** | 启用可用性组。 例如，“1”为已启用，“0”为已禁用 |
| **MSSQL_AGENT_ENABLED** | 启用 SQL Server 代理。 例如，“true”为已启用，“false”为已禁用。 默认情况下，代理处于禁用状态。  |
| **MSSQL_MASTER_DATA_FILE** | 设置主数据库数据文件的位置。 在首次运行 SQL Server 之前，必须将其命名为 **master.mdf**。 |
| **MSSQL_MASTER_LOG_FILE** | 设置主数据库日志文件的位置。 在首次运行 SQL Server 之前，必须将其命名为 **mastlog.ldf**。 |
| **MSSQL_ERROR_LOG_FILE** | 设置错误日志文件的位置。 |

::: moniker-end

## <a name="use-with-initial-setup"></a>与初始设置配合使用

此示例使用已配置的环境变量运行 `mssql-conf setup`。 指定了以下环境变量：

- **ACCEPT_EULA** 接受最终用户许可协议。
- **MSSQL_PID** 指定用于非生产用途的免费许可的 SQL Server Developer Edition。
- **MSSQL_SA_PASSWORD** 设置强密码。
- **MSSQL_TCP_PORT** 将 SQL Server 侦听的 TCP 端口设置为 1234。

```bash
sudo ACCEPT_EULA='Y' MSSQL_PID='Developer' MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' MSSQL_TCP_PORT=1234 /opt/mssql/bin/mssql-conf setup
```

## <a name="use-with-docker"></a>与 Docker 配合使用

此示例 docker 命令使用以下环境变量来创建新的 SQL Server 容器：

- **ACCEPT_EULA** 接受最终用户许可协议。
- **MSSQL_PID** 指定用于非生产用途的免费许可的 SQL Server Developer Edition。
- **MSSQL_SA_PASSWORD** 设置强密码。
- **MSSQL_TCP_PORT** 将 SQL Server 侦听的 TCP 端口设置为 1234。 这意味着在此示例中，必须使用 `-p 1234:1234` 命令映射自定义 TCP 端口，而不是将端口 1433（默认）映射到主机端口。

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

如果在 Linux/macOS 上运行 Docker，请将以下语法与单引号配合使用：

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2017-latest
```

如果在 Windows 上运行 Docker，请将以下语法与双引号配合使用：

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="<YourStrong!Passw0rd>" -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2017-latest
```

> [!NOTE]
> 在容器中运行生产版本的过程略有不同。 有关详细信息，请参阅[运行生产容器映像](./sql-server-linux-docker-container-deployment.md#production)。

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

如果在 Linux/macOS 上运行 Docker，请将以下语法与单引号配合使用：

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

如果在 Windows 上运行 Docker，请将以下语法与双引号配合使用：

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="<YourStrong!Passw0rd>" -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

::: moniker-end

## <a name="next-steps"></a>后续步骤

有关此处未列出的其他 SQL Server 设置，请参阅[使用 mssql-conf 工具配置 Linux 上的 SQL Server](sql-server-linux-configure-mssql-conf.md)。

有关如何在 Linux 上安装和运行 SQL Server 的详细信息，请参阅[在 Linux 上安装 SQL Server](sql-server-linux-setup.md)。