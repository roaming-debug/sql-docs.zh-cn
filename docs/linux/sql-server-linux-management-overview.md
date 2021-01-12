---
title: 管理 Linux 上的 SQL Server
description: 本文提供了一些链接，这些链接指向 Linux 上运行的 SQL Server 的常见管理任务和工具。
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 6bd8eb0b-593d-467e-87ea-ab1c4dbcd1ea
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 04ece9daf696d73b9de96f48a08e32136fe6ab66
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094620"
---
# <a name="choose-the-right-tool-to-manage-sql-server-on-linux"></a>选择合适的工具管理 Linux 上的 SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

管理 Linux 上的 SQL Server 有多种方法。 下面这一节简要介绍了各种管理工具和方法，并指出了更多资源。

## <a name="mssql-conf"></a>mssql-conf 

使用 mssql-conf 工具配置 Linux 上的 SQL Server  。 有关详细信息，请参阅[使用 mssql-conf 配置 Linux 上的 SQL Server](sql-server-linux-configure-mssql-conf.md)。

## <a name="transact-sql"></a>Transact-SQL

几乎所有在客户端工具中可以完成的工作，都可以使用 Transact-SQL 语句来完成。 SQL Server 提供[动态管理视图 (DMV)](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) 来查询 SQL Server 的状态和配置。 此外，还提供 [Transact-SQL 命令](../t-sql/language-reference.md)来执行数据库管理任务。 可以在任何支持连接到 SQL Server 并运行 Transact-SQL 查询的客户端工具中运行这些命令，例如 [sqlcmd](sql-server-linux-setup-tools.md) 或 [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)。

## <a name="azure-data-studio"></a>Azure Data Studio

新 Azure Data Studio 是一款用于管理 SQL Server 的跨平台工具。 有关详细信息，请参阅 [Azure Data Studio 概述](../azure-data-studio/what-is-azure-data-studio.md)。

## <a name="sql-server-management-studio-on-windows"></a>Windows 上的 SQL Server Management Studio

SQL Server Management Studio (SSMS) 是一款 Windows 应用程序，它提供了一种用于管理 SQL Server 的图形用户界面。 尽管当前它只能在 Windows 上运行，但用户可以通过该应用程序远程连接到 Linux SQL Server 实例。 有关使用 SSMS 管理 SQL Server 的详细信息，请参阅[使用 SSMS 管理 Linux 上的 SQL Server](sql-server-linux-manage-ssms.md)。

## <a name="mssql-cli-preview"></a>mssql-cli（预览版）

Microsoft 已发布新的跨平台脚本工具用于 SQL Server，即[mssql cli](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/)。 此工具当前处于预览状态。

## <a name="powershell"></a>PowerShell

PowerShell 提供了丰富的命令行环境，用于管理 Linux 上的 SQL Server。 有关详细信息，请参阅[使用 PowerShell 管理 Linux 上的 SQL Server](sql-server-linux-manage-powershell.md)。

## <a name="next-steps"></a>后续步骤

有关 Linux 上 SQL Server 的详细信息，请参阅 [Linux 上的 SQL Server](sql-server-linux-overview.md)。