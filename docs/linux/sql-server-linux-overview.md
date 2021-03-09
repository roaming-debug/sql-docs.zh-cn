---
title: Linux 上的 SQL Server 概述
description: 本文介绍 SQL Server 如何在 Linux 上运行，并提供有关如何了解详细信息的信息。
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 9dcc6a90-0add-42c2-815b-862e4e2a21ac
ms.openlocfilehash: 0cc87c2bfc33c0c710e27017da023194fcd2828b
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465195"
---
# <a name="sql-server-on-linux"></a>Linux 上的 SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

::: moniker range="= sql-server-2017 || = sql-server-linux-2017"
从 SQL Server 2017 开始，SQL Server 在 Linux 上运行。 它属于相同的 SQL Server 数据库引擎，具有许多相似的功能和服务，且不受操作系统的影响。

> [!TIP]
> [SQL Server 2019](sql-server-linux-overview.md?view=sql-server-ver15&preserve-view=true) 现已可用！ 要了解最新版本中的 Linux 新增功能，请参阅[适用于 Linux 的 SQL Server 2019 中的新增功能](sql-server-linux-whats-new-2019.md?view=sql-server-ver15&preserve-view=true)。
::: moniker-end

::: moniker range=">= sql-server-ver15 || >= sql-server-linux-ver15"
SQL Server 2019 在 Linux 上运行。 它属于相同的 SQL Server 数据库引擎，具有许多相似的功能和服务，且不受操作系统的影响。 有关此版本的详细信息，请参阅[适用于 Linux 的 SQL Server 2019 中的新增功能](sql-server-linux-whats-new-2019.md)。
::: moniker-end

## <a name="install"></a>安装

若要开始，请使用以下快速入门之一安装 Linux 上的 SQL Server：

- [在 Red Hat Enterprise Linux 上安装](quickstart-install-connect-red-hat.md)
- [在 SUSE Linux Enterprise Server 上安装](quickstart-install-connect-suse.md)
- [在 Ubuntu 上安装](quickstart-install-connect-ubuntu.md)
- [在 Docker 上运行](quickstart-install-connect-docker.md)
- [在 Azure 中预配 SQL VM](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

> [!NOTE]
> Docker 本身可在多个平台上运行，这意味着可在 Linux、Mac 和 Windows 上运行 Docker 映像。

## <a name="connect"></a>连接

安装完成后，连接到 Linux 计算机上的 SQL Server 实例。 可以本地或远程连接，也可以使用各种工具和驱动程序连接。 快速入门演示了如何使用 [sqlcmd](sql-server-linux-setup-tools.md) 命令行工具。 其他工具包括：

| 工具 | 教程 |
|-----|-----|
| Visual Studio Code (VS Code) | [将 VS Code 用于 Linux 上的 SQL Server](../tools/visual-studio-code/sql-server-develop-use-vscode.md) |
| SQL Server Management Studio (SSMS) | [使用 Windows 上的 SSMS 连接到 Linux 上的 SQL Server](sql-server-linux-manage-ssms.md) |
| SQL Server Data Tools (SSDT) | [将 SSDT 用于 Linux 上的 SQL Server](sql-server-linux-develop-use-ssdt.md) |

## <a name="explore"></a>探究

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

SQL Server 2017 在所有支持的平台（包括 Linux）上具有相同的基础数据库引擎。 因此，在 Linux 上，许多现有功能运行方式相同。 文档的这一部分从 Linux 的角度展示了其中部分功能。 它还展示了 Linux 上有独特要求的部分。

如果你已熟悉 SQL Server，请查看[发行说明](sql-server-linux-release-notes.md)，了解此版本的常规指南和已知问题。 然后查看 [Linux 上的 SQL Server 的新增功能](sql-server-linux-whats-new.md)以及 [SQL Server 2017 的新增功能摘要](../sql-server/what-s-new-in-sql-server-2017.md)。

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15"

[!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] 在所有支持的平台（包括 Linux）上具有相同的基础数据库引擎。 因此，在 Linux 上，许多现有功能运行方式相同。 文档的这一部分从 Linux 的角度展示了其中部分功能。 它还展示了 Linux 上有独特要求的部分。

如果你已熟悉 Linux 上的 SQL Server，请查看[发行说明](sql-server-linux-release-notes-2019.md)，了解此版本的常规指南和已知问题。 然后查看 [Linux 上的 SQL Server 2019 的新增功能](../sql-server/what-s-new-in-sql-server-ver15.md)。

::: moniker-end


### <a name="all-versions-of-sql-server"></a>所有版本的 SQL Server

SQL Server 2017 和 [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] 在所有支持的平台（包括 Linux）上都具有相同的基础数据库引擎。 因此，在 Linux 上，许多现有功能运行方式相同。 文档的这一部分从 Linux 的角度展示了其中部分功能。 它还展示了 Linux 上有独特要求的部分。

如果你已熟悉 Linux 上的 SQL Server，请查看发行说明：

- [SQL Server 2017 发行说明](sql-server-linux-release-notes.md)
- [SQL Server 2019 发行说明](sql-server-linux-release-notes-2019.md)

然后查看新增功能：

- [SQL Server 2017 的新增功能](sql-server-linux-whats-new.md)
- [Linux 上的 SQL Server 2019 的新增功能](../sql-server/what-s-new-in-sql-server-ver15.md#sql-server-on-linux)

> [!TIP]
> 有关常见问题的解答，请参阅 [Linux 上的 SQL Server 常见问题解答](sql-server-linux-faq.yml)。

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
