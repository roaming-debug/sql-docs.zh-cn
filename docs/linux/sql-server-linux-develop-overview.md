---
title: 开发适用于 Linux 上 SQL Server 的应用程序
description: 可以通过各种编程语言和常用的 Web 框架创建连接和使用 Linux 上的 SQL Server 的应用程序。
author: VanMSFT
ms.author: vanto
ms.date: 11/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 758cb738-b018-465b-9ab0-59a24b892e66
ms.openlocfilehash: 01655b948afa721365b7ea7adebd1e22f8f6f851
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273078"
---
# <a name="how-to-get-started-developing-applications-for-sql-server-on-linux"></a>如何为 Linux 上的 SQL Server 开发应用程序

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

可以通过各种编程语言（如 C#、Java、Node.js、PHP、Python、Ruby 和 C ++）创建连接和使用 Linux 上的 SQL Server 的应用程序。 还可使用流行的 Web 框架和对象关系映射 (ORM) 框架。

> [!VIDEO https://channel9.msdn.com/events/Connect/2017/T153/player]

> [!TIP]
> 使用这些相同的开发选项可以面向其他平台上的 SQL Server。 应用程序可面向在本地或在云中、在 Linux、Windows 或 macOS 上的 Docker 中运行的 SQL Server。 也可面向 Azure SQL 数据库和 Azure Synapse Analytics。

## <a name="try-the-tutorials"></a>请按教程进行尝试

熟悉和使用 SQL Server 生成应用程序的最佳方式就是自己亲自尝试。

- 浏览[入门教程](https://aka.ms/sqldev)。
- 选择语言和开发平台。
- 尝试代码示例。

> [!TIP]
> 如果要开发在 Docker 上运行的 SQL Server，请参阅“macOS”教程  。

## <a name="create-new-applications"></a>创建新的应用程序

如果要创建新的应用程序，请查看[连接库](sql-server-linux-develop-connectivity-libraries.md)中的连接器摘要，以及适用于多种编程语言的热门框架。

## <a name="use-existing-applications"></a>使用现有应用程序

如果已有现有数据库应用程序，则只需将它的连接字符串更改为面向 Linux 上的 SQL Server。 请务必阅读 Linux 上的 SQL Server 的相关[已知问题](sql-server-linux-release-notes.md)。

## <a name="use-existing-sql-tools-on-windows-with-sql-server-on-linux"></a>通过 Linux 上的 SQL Server 使用 Windows 上的现有 SQL 工具

当前在 Windows 上运行的工具（如 SSMS、SSDT 和 PowerShell）同样可在 Linux 上的 SQL Server 中运行。 尽管它们不可在本机 Linux 上运行，但你仍然可以管理 Linux 上的远程 SQL Server 实例。 

有关详细信息，请参阅下列主题：

- [SQL Server Management Studio (SSMS)](sql-server-linux-manage-ssms.md)
- [SQL Server Data Tools (SSDT)](sql-server-linux-develop-use-ssdt.md)
- [SQL PowerShell](sql-server-linux-manage-powershell.md)

> [!Note]
> 为获得最佳体验，请确保使用最新版本的工具。

## <a name="use-new-sql-tools-for-linux"></a>使用适用于 Linux 的新 SQL 工具

可在 Linux、macOS 和 Windows 上使用 [Visual Studio Code](https://code.visualstudio.com) 的新 [mssql 扩展](https://aka.ms/mssql-marketplace)。 相关分布演练请参阅下列教程：

- [使用 Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)

还可使用对 Linux 而言属于本机的新命令行工具。 这些工具包括：

- [sqlcmd](../tools/sqlcmd-utility.md)
- [bcp](sql-server-linux-migrate-bcp.md)
- [mssql-conf](sql-server-linux-configure-mssql-conf.md)

## <a name="next-steps"></a>后续步骤

若要开始，请使用以下快速入门之一安装 Linux 上的 SQL Server：

- [在 Red Hat Enterprise Linux 上安装](quickstart-install-connect-red-hat.md)
- [在 SUSE Linux Enterprise Server 上安装](quickstart-install-connect-suse.md)
- [在 Ubuntu 上安装](quickstart-install-connect-ubuntu.md)
- [在 Docker 上运行](quickstart-install-connect-ubuntu.md)