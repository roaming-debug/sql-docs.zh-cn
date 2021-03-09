---
title: Linux 上的 SQL Server 2017 的新增功能
description: 本文介绍可用于在 Linux 上运行的 SQL Server 2017 的主要功能和服务。
author: VanMSFT
ms.author: vanto
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: c863cb129f1ba40c2bb73fff51adb859a8a7bade
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464646"
---
# <a name="whats-new-for-sql-server-2017-on-linux"></a>Linux 上的 SQL Server 2017 的新增功能

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍了可用于在 Linux 上运行的 SQL Server 2017 的主要功能和服务。

> [!NOTE]
> 除了本文中的这些功能之外，会定期发布累积更新。 这些累积更新提供许多改进和修复。 有关最新累积更新发布的详细信息，请参阅[https://aka.ms/sql2017cu](https://aka.ms/sql2017cu)。 有关包下载和已知问题，请参阅[发行说明](sql-server-linux-release-notes.md)。

## <a name="ubuntu-1804-supported"></a>支持 Ubuntu 18.04

自 SQL Server 2017 CU20 起，现已开始支持 Ubuntu 18.04。 查看[在 Ubuntu 上安装 SQL Server 并创建数据库](quickstart-install-connect-ubuntu.md)快速入门。

## <a name="rhel-8-supported"></a>支持 RHEL 8

自 SQL Server 2017 CU20 起，现已开始支持 RHEL 8。 查看[在 Red Hat 上安装 SQL Server 并创建数据库](quickstart-install-connect-red-hat.md)快速入门。

## <a name="sql-server-database-engine"></a>SQL Server 数据库引擎

- 已启用核心 SQL Server 数据库引擎功能。
- 支持本机 Linux 路径。
- IPV6 支持。
- 支持 NFS 上的数据库文件。
- 已启用[传输层安全性](sql-server-linux-encrypted-connections.md) (TLS) 加密。
- 已启用 [Active Directory 身份验证](sql-server-linux-active-directory-authentication.md)。
- 可提供高可用性的[可用性组功能](sql-server-linux-availability-group-overview.md)。
- [全文搜索](sql-server-linux-setup-full-text-search.md)支持。

## <a name="sql-server-agent"></a>SQL Server 代理

- 已对以下任务启用 [SQL Server 代理](sql-server-linux-setup-sql-agent.md)支持：
  - [Transact-SQL 作业](sql-server-linux-run-sql-server-agent-job.md)
  - [DB 邮件](sql-server-linux-db-mail-sql-agent.md)
  - [日志传送](sql-server-linux-use-log-shipping.md)

## <a name="sql-server-integration-services-ssis"></a>SQL Server Integration Services (SSIS)

- 能够在 Linux 上运行 SSIS 包。 有关详细信息，请参阅[使用 ssis-conf 配置 Linux 上的 SQL Server Integration Services](sql-server-linux-configure-ssis.md)。

## <a name="other-improvements"></a>其他改进

- 命令行配置工具 [mssql-conf](sql-server-linux-configure-mssql-conf.md)。
- 包含[环境变量](sql-server-linux-configure-environment-variables.md)的无人参与安装支持。
- 跨平台 [Visual Studio Code mssql-server 扩展](../tools/visual-studio-code/sql-server-develop-use-vscode.md)。
- 跨平台脚本生成器 [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli/blob/dev/doc/usage_guide.md)。
- 跨平台动态管理视图 (DMV) 监视器 [DBFS 工具](https://github.com/Microsoft/dbfs)。

## <a name="next-steps"></a>后续步骤

要在 Linux 上安装 SQL Server，请使用以下教程之一：

- [在 Red Hat Enterprise Linux 上安装](quickstart-install-connect-red-hat.md)
- [在 SUSE Linux Enterprise Server 上安装](quickstart-install-connect-suse.md)
- [在 Ubuntu 上安装](quickstart-install-connect-ubuntu.md)
- [在 Docker 上运行](quickstart-install-connect-docker.md)
- [在 Azure 中预配 SQL VM](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

有关常见问题的解答，请参阅 [Linux 上的 SQL Server 常见问题解答](sql-server-linux-faq.yml)。 要查看 SQL Server 2017 中引入的其他改进，请参阅[SQL Server 2017 的新增功能](../sql-server/what-s-new-in-sql-server-2017.md)。

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
