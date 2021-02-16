---
title: 使用 SSIS 在 Linux 上提取、转换和加载数据
description: 了解如何在 Linux 上运行 SQL Server Integration Services (SSIS) 包。 此外了解如何查找有关 SSIS 功能的详细信息。
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 01/09/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: e21864da00069c6a41138a8506c98ab49e07bf3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339126"
---
# <a name="extract-transform-and-load-data-on-linux-with-ssis"></a>使用 SSIS 在 Linux 上提取、转换和加载数据

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍如何在 Linux 上运行 SQL Server Integration Services (SSIS) 包。 SSIS 从多个源和格式中提取数据，转换和清理数据，并将数据加载到多个目标，从而解决复杂的数据集成问题。 

Linux 中运行的 SSIS 包可以连接到在本地或云的 Windows 中、在 Linux 上或在 Docker 中运行的 Microsoft SQL Server。 这些包还可以连接到 Azure SQL 数据库、Azure Synapse Analytics、ODBC 数据源、平面文件以及其他数据源（包括 ADO.NET 源、XML 文件和 OData 服务）。

有关 SSIS 功能的详细信息，请参阅 [SQL Server Integration Services](../integration-services/sql-server-integration-services.md)。

## <a name="prerequisites"></a>先决条件

若要在 Linux 计算机上运行 SSIS 包，首先必须安装 SQL Server Integration Services。 为 Linux 计算机安装 SQL Server 的操作不包括 SSIS。 有关安装说明，请参阅[安装 SQL Server Integration Services](sql-server-linux-setup-ssis.md)。

还必须有一台用来创建和维护包的 Windows 计算机。 SSIS 的设计和管理工具是目前对 Linux 计算机不可用的 Windows 应用程序。 

## <a name="run-an-ssis-package"></a>运行 SSIS 包

若要在 Linux 计算机上运行 SSIS 包，请执行以下操作：

1.  将 SSIS 包复制到 Linux 计算机。
2.  运行下面的命令：
    ```
    $ dtexec /F \<package name \> /DE <protection password>
    ```

## <a name="run-an-encrypted-password-protected-package"></a>运行加密（受密码保护的）包
运行使用密码加密的 SSIS 包有三种方法：

1.  设置环境变量 `SSIS_PACKAGE_DECRYPT` 的值，如以下示例中所示：

    ```
    SSIS_PACKAGE_DECRYPT=test /opt/ssis/bin/dtexec /f package.dtsx
    ```

2.  指定用于交互输入密码的 `/de[crypt]` 选项，如以下示例中所示：

    ```
    /opt/ssis/bin/dtexec /f package.dtsx /de
    
    Enter decryption password:
    ```

3.  指定在命令行上提供密码的 `/de` 选项，如以下示例中所示。 该方法使用命令历史记录中的命令来存储解密密码，因此不建议使用此方法。

    ```
    opt/ssis/bin/dtexec /f package.dtsx /de test
    
    Warning: Using /De[crypt] <password> may store decryption password in command history.
    
    You can use /De[crypt] instead to enter interactive mode,
    or use environment variable SSIS_PACKAGE_DECRYPT to set decryption password.
    ```

## <a name="design-packages"></a>设计包

**连接到 ODBC 数据源**。 由于 Linux CTP 2.1 刷新版和更高版本上有 SSIS，因此 SSIS 包可以使用 Linux 上的 ODBC 连接。 虽然已使用 SQL Server 和 MySQL ODBC 驱动程序测试过该功能，但也希望该功能可以与任何遵循 ODBC 规范的 Unicode ODBC 驱动程序搭配使用。 在设计阶段，可以提供 DSN 或连接字符串以连接到 ODBC 数据，还可以使用 Windows 身份验证。 有关详细信息，请参阅[宣布 Linux 上的 ODBC 支持的博客文章](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/)。

**路径**。 在 SSIS 包中提供 Windows 样式的路径。 Linux 上的 SSIS 不支持 Linux 样式的路径，但 SSIS 在运行时会将 Windows 样式的路径映射到 Linux 样式的路径。 例如，Linux 上的 SSIS 将 Windows 样式的路径 (`C:\test`) 映射到 Linux 样式的路径 (`/test`)。

## <a name="deploy-packages"></a>部署包
在此版本中，只能将包存储在 Linux 的文件系统中。 SSIS 目录数据库和旧 SSIS 服务在 Linux 上不适用于部署包和存储包。

## <a name="schedule-packages"></a>计划包
可使用 Linux 系统计划工具（如 `cron`）来计划包。 在此版本中，不能使用 Linux 上的 SQL 代理来计划包执行。 有关详细信息，请参阅[使用 Cron 在 Linux 上计划 SSIS 包](sql-server-linux-schedule-ssis-packages.md)。

## <a name="limitations-and-known-issues"></a>限制和已知问题

有关 Linux 上的 SSIS 的限制和已知问题的详细信息，请参阅 [Linux 上的 SSIS 的限制和已知问题](sql-server-linux-ssis-known-issues.md)。

## <a name="more-info-about-ssis-on-linux"></a>有关 Linux 上的 SSIS 的详细信息

有关 Linux 上的 SSIS 的详细信息，请参阅以下博客文章：

-   [SQL Server CTP 2.1 中提供 Linux 上的 SSIS](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/)
-   [Linux 上的 SSIS 支持 ODBC（SQL Server CTP 2.1 刷新版）](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/)

## <a name="more-info-about-ssis"></a>有关 SSIS 的详细信息

Microsoft SQL Server Integration Services (SSIS) 是一个可用于生成高性能数据集成解决方案的平台，其中包括数据仓库的提取、转换和加载 (ETL) 包。 有关 SSIS 的详细信息，请参阅 [SQL Server Integration Services](../integration-services/sql-server-integration-services.md)。

SSIS 包括以下功能：
- 在 Windows 上生成和调试包的图形工具和向导
- 执行工作流函数的各种任务（例如 FTP 操作、执行 SQL 语句和发送电子邮件）
- 用于提取和加载数据的各种数据源和目标
- 用于清理、聚合、合并和复制数据的各种转换
- 用自己的自定义脚本和组件扩展 SSIS 的应用程序编程接口 (API)

若要开始使用 SSIS，请下载最新版的 [SQL Server Data Tools (SSDT)](../integration-services/ssis-how-to-create-an-etl-package.md)。

若要了解有关 SSIS 的详细信息，请参阅以下文章：
- [了解有关 SQL Server Integration Services 的详细信息](../integration-services/sql-server-integration-services.md)
- [SQL Server Integration Services (SSIS) 开发和管理工具](../integration-services/integration-services-ssis-development-and-management-tools.md)
- [SQL Server Integration Services 教程](../integration-services/integration-services-tutorials.md)

## <a name="related-content-about-ssis-on-linux"></a>有关 Linux 上的 SSIS 的相关内容
-   [在 Linux 上安装 SQL Server Integration Services (SSIS)](sql-server-linux-setup-ssis.md)
-   [使用 ssis-conf 在 Linux 上配置 SQL Server Integration Services](sql-server-linux-configure-ssis.md)
-   [适用于 Linux 上 SSIS 的限制和已知问题](sql-server-linux-ssis-known-issues.md)
-   [使用 cron 在 Linux 上计划 SQL Server Integration Services 包执行](sql-server-linux-schedule-ssis-packages.md)