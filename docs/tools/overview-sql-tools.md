---
title: SQL 工具概述
description: 适用于 SQL Server、Azure SQL（Azure SQL 数据库、Azure SQL 托管实例、SQL 虚拟机）和 Azure Synapse Analytics 的 SQL 查询和管理工具。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ''
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/04/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 3595e84b4da525894c353c8b5d702b69b19f354d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338695"
---
# <a name="sql-tools-overview"></a>SQL 工具概述

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

若要管理数据库，需要一个工具。 无论数据库是在云中、Windows 上、macOS 上还是 [Linux](../linux/sql-server-linux-overview.md) 上运行，工具都不需要与数据库在相同的平台上运行。

可以在下表中查看指向不同 SQL 工具的链接。

> [!Note]
> 若要下载 SQL Server，请参阅[安装 SQL Server](../database-engine/install-windows/install-sql-server.md)。

## <a name="recommended-tools"></a>建议的工具

以下工具提供了图形用户界面 (GUI)。

| 工具 | 说明 | 操作系统 |
|:--|:--|:--|
| [ **![ADS 映像](../tools/media/overview-sql-tools/azure-data-studio.svg)</br></br>Azure Data Studio**](../azure-data-studio/download.md) | 可以运行按需 SQL 查询，查看结果并将其保存为文本、JSON 或 Excel 格式的轻型编辑器。 编辑数据，组织你最喜欢的数据库连接，并以熟悉的对象浏览体验浏览数据库对象。 | **Windows</br>macOS</br>Linux** |
| [ **![SSMS 映像](../tools/media/overview-sql-tools/ssms.svg)</br></br>SQL Server Management Studio (SSMS)**](../ssms/download-sql-server-management-studio-ssms.md) | 管理具有完整 GUI 支持的 SQL Server 实例或数据库。 访问、配置、管理和开发 SQL Server、Azure SQL 数据库和 Azure Synapse Analytics 的所有组件。 在一个综合实用工具中汇集了大量图形工具和丰富的脚本编辑器，为各种技能水平的开发者和数据库管理员提供对 SQL 的访问权限。 | **Windows** |
| [ **![SSDT 映像](../tools/media/overview-sql-tools/ssdt.svg)</br>SQL Server Data Tools (SSDT)**](../ssdt/download-sql-server-data-tools-ssdt.md) | 一款新式开发工具，用于生成 SQL Server 关系数据库、Azure SQL 数据库、Analysis Services (AS) 数据模型、Integration Services (IS) 包和 Reporting Services (RS) 报表。 使用 SSDT，你可以设计和部署任何 SQL Server 内容类型，就像在 [Visual Studio](https://visualstudio.microsoft.com/downloads/) 中开发应用程序一样轻松。 | **Windows** |
| [ **![VS Code 映像](../tools/media/overview-sql-tools/visual-studio-code.svg)</br></br>Visual Studio Code**](https://code.visualstudio.com/) | Visual Studio Code 的 [mssql 扩展](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql) 为官方 Visual Studio Code 扩展，它支持连接到 SQL Server，并在 Visual Studio Code 中为 T-SQL 提供丰富的编辑体验。 在轻型编辑器中编写 T-SQL 脚本。 | **Windows</br>macOS</br>Linux** |

## <a name="command-line-tools"></a>命令行工具

以下工具是主要的命令行工具。

| 工具 | 说明 | 操作系统 |
|:--|:--|:--|
|[**bcp**](bcp-utility.md)|大容量复制程序实用工具 (bcp) 可以在 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例和用户指定格式的数据文件间大容量复制数据     。| **Windows</br>macOS</br>Linux** |
|[**mssql-cli（预览版）**](mssql-cli.md)|**mssql-cli** 是一项用于查询 SQL Server 的交互式命令行工具。 此外，使用具有 IntelliSense、语法高亮等功能的命令行工具查询 SQL Server。 | **Windows</br>macOS</br>Linux** |
|[**mssql-conf**](../linux/sql-server-linux-configure-mssql-conf.md) | **mssql-conf** 配置在 Linux 上运行的 SQL Server。 | **Linux** |
|[**mssql-scripter（预览版）**](https://github.com/Microsoft/mssql-scripter) | mssql-scripter  是 SQL Server 数据库的多平台命令行体验。 | **Windows</br>macOS</br>Linux** |
| [**sqlcmd**](sqlcmd-utility.md) |可以在命令提示符下，使用 sqlcmd  实用工具输入 Transact-SQL 语句、系统过程和脚本文件。 | **Windows</br>macOS</br>Linux** |
| [**sqlpackage**](sqlpackage/sqlpackage.md) |sqlpackage  是一个命令行实用工具，可自动处理多个数据库开发任务。 |**Windows</br>macOS</br>Linux** |
|[**SQL Server PowerShell**](../powershell/sql-server-powershell.md)| SQL Server PowerShell  提供了用于处理 SQL 的 cmdlet。 | **Windows</br>macOS</br>Linux** |

## <a name="migration-and-other-tools"></a>迁移和其他工具

这些工具用于迁移、配置和提供 SQL 数据库的其他功能。

| 工具 | 说明 |
|:--|:--|
| **[配置管理器](../tools/configuration-manager/sql-server-configuration-manager-help.md)** | 使用 SQL Server 配置管理器可以配置 SQL Server 服务和网络连接。 配置管理器在 Windows 上运行|
| **[数据库实验助手](../dea/database-experimentation-assistant-overview.md)** | 使用数据库实验助手对给定工作负载的 SQL 目标版本进行评估。 |
| **[数据迁移助手](../dma/dma-overview.md)** | 数据迁移助手工具可以检测可能会影响新版 SQL Server 或 Azure SQL 数据库中数据库功能的兼容性问题，有助于你升级到新式数据平台。 |
| **[Distributed Replay](../tools/distributed-replay/install-distributed-replay-overview.md)** | Distributed Replay 功能可帮助你评估即将进行的 SQL Server 升级的影响。 还可以使用 Distributed Replay 来帮助评估硬件和操作系统升级以及 SQL Server 优化的影响。 |
| **[ssbdiagnose](../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)** | ssbdiagnose 实用工具可报告 Service Broker 会话或 Service Broker 服务配置中的问题。 |
| **[SQL Server 迁移助手](../ssma/sql-server-migration-assistant.md)** | 使用 SQL Server Migration Assistant 自动将数据库从 Microsoft Access、DB2、MySQL、Oracle 和 Sybase 迁移到 SQL Server。|

如果你正在寻找该页未提及的其他工具，请参阅 [SQL 命令提示实用工具](command-prompt-utility-reference-database-engine.md)和[下载 SQL Server 扩展功能和工具](download-sql-feature-packs.md)
