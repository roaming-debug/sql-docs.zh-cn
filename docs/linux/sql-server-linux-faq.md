---
title: Linux 上的 SQL Server 常见问题解答
description: 本文提供有关 Linux 上运行的 SQL Server 的常见问题解答 (FAQ)。
author: VanMSFT
ms.author: vanto
ms.date: 10/23/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: dc5d54c3d64d43eb4b68a6762e3bd024587937ab
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094647"
---
# <a name="sql-server-on-linux-frequently-asked-questions-faq"></a>Linux 上的 SQL Server 的常见问题解答 (FAQ)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

以下部分提供了有关 Linux 上运行的 SQL Server 的常见问题和解答。

## <a name="general-questions"></a>常规问题

1. 支持哪些 Linux 平台？

   SQL Server 目前在 Red Hat Enterprise Server、SUSE Linux Enterprise Server 和 Ubuntu 上受支持。 还支持使用 Docker 在容器中运行。 有关支持的版本的最新信息，请参阅[支持的平台](sql-server-linux-setup.md#supportedplatforms)。

1. Linux 上的 SQL Server 未来是否可以在其他平台上运行？

   SQL Server 在 Linux 上针对之前列出的发行版进行了测试且受支持。 其他 Linux 发行版密切相关并且可能可以运行 SQL Server（例如，CentOS 与 Red Hat Enterprise Server 密切相关）。 但是，如果选择在不受支持的操作系统上安装 SQL Server，请查看 [Microsoft SQL Server 的技术支持策略](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server)的“支持策略”部分，以了解支持含义。 另请注意，如果基础操作系统出现故障，一些社区维护的 Linux 发行版将没有获得支持的正式途径。

1. Linux 与 Windows 上的 SQL Server 是否相同？

   SQL Server 的核心数据库引擎在 Linux 上与在 Windows 上是相同的。 不过，Linux 当前不支持某些功能。 有关 Linux 不支持的功能的列表，请参阅[不支持的功能和服务](sql-server-linux-editions-and-components-2019.md#Unsupported)。 另请参阅[已知问题](sql-server-linux-release-notes.md#known-issues)。 除非在这些列表中指定，否则 Linux 均支持其他 SQL Server 功能和服务。

1. SQL Server 的支持策略是什么？

   若要了解支持策略，请参阅 [SQL Server 的技术支持策略](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server)。

1. 我一直使用 Windows SQL Server。是否有可帮助了解如何使用 Linux 上的 SQL Server 的相关资源？

   [快速入门](sql-server-linux-setup.md#platforms)中提供了有关如何在 Linux 上安装 SQL Server 和运行 Transact-SQL 查询的分步说明。 其他教程提供了有关在 Linux 上使用 SQL Server 的其他说明。 要获取第三方提示列表，请参阅 [Linux 上的 SQL Server 的 MSSQLTIPS 列表提示](https://www.mssqltips.com/sql-server-tip-category/226/sql-server-on-linux/)。

## <a name="licensing"></a>授权

1. 如何在 Linux 上授予许可？

   在 Windows 和 Linux 上为 SQL Server 授予许可的方法是相同的。 事实上，为 SQL Server 授予许可后，即可在所选的平台上使用该许可。 有关详细信息，请参阅[如何为 SQL Server 授与许可](https://www.microsoft.com/sql-server/sql-server-2017-pricing)。

1. 在已购买 SQL Server 时应选择哪个版本？

   运行 mssql-conf 安装程序时，会显示以下选项：
   
   ```
   Choose an edition of SQL Server:
      1. Evaluation (free, no production use rights, 180-day limit)
      2. Developer (free, no production use rights)
      3. Express (free)
      4. Web (PAID)
      5. Standard (PAID)
      6. Enterprise (PAID)
      7. Enterprise Core (PAID)
      8. I bought a license through a retail sales channel and have a product key to enter.
   ```
     
   如果已通过批量许可（在企业协议中）或通过 MSDN 订阅获得许可证，则需要选择选项 4 到 7。 此步骤不会要求输入许可证，但必须先为配置购买相应的许可证。 如果已通过零售渠道购买了标准版，请选择选项 8。 此选项会提示输入密钥。 

1. 如何验证已安装的版本和 Linux 上的 SQL Server 的版本？

   使用 sqlcmd、mssql-cli 或 Visual Studio Code 等客户端工具连接到 SQL Server 实例 。 然后，运行以下 Transact-SQL 查询以验证版本和正在运行的 SQL Server 的版本： 

   ```sql
   SELECT @@VERSION
   SELECT SERVERPROPERTY('Edition')
   ```

## <a name="installation"></a>安装

1. 如何在我的 Linux 服务器上安装 SQL Server？

   Microsoft 维护用于安装 SQL Server 的包存储库，并支持通过本机包管理器（如 yum、zypper 和 apt）进行安装。 若要快速安装，请参阅其中一篇[快速入门](sql-server-linux-setup.md#platforms)文章。

1. 能否在 Windows 10 的 Linux 子系统上安装 SQL Server？

   不是。 在 Windows 10 上运行的 Linux 目前不是 SQL Server 及其相关工具的受支持平台。

1. SQL Server 可以将哪些 Linux 文件系统用于数据文件？

   Linux 上的 SQL Server 目前支持 ext4 和 XFS。 将来会按需添加对其他文件系统的支持。

1. 是否可以下载安装包以脱机安装 SQL Server？

   是的。 有关详细信息，请参阅[发行说明](sql-server-linux-release-notes.md)中的包下载链接。 另请参阅[脱机安装说明](sql-server-linux-setup.md#offline)。

1. 是否可以在 Linux 上执行无人参与安装 SQL Server？

   是的。 有关无人参与安装的介绍，请参阅 [Linux 上的 SQL Server 的安装指南](sql-server-linux-setup.md#unattended)。 请参阅 [Red Hat](sample-unattended-install-redhat.md)、[SUSE Linux Enterprise Server](sample-unattended-install-suse.md) 和 [Ubuntu](sample-unattended-install-ubuntu.md) 的示例脚本。 还可以查看 SQL Server 客户顾问团队创建的[此示例脚本](/archive/blogs/sqlcat/unattended-install-and-configuration-for-sql-server-2017-on-linux)。

## <a name="tools"></a>工具

1. 是否可以使用 Windows 上的 SQL Server Management Studio 客户端访问 Linux 上的 SQL Server？

   可以。可以使用在 Windows 上运行的所有现有工具来访问 Linux 上的 SQL Server。 其中包括 Microsoft 提供的工具（如 SQL Server Management Studio (SSMS)、SQL Server Data Tools (SSDT) 和 OSS）以及第三方工具。

1. 是否有可在 Linux 上运行的类似 SSMS 的工具？

   新 Azure Data Studio 是一款用于管理 SQL Server 的跨平台工具。 有关详细信息，请参阅[什么是 Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)。

1. Linux 上是否提供 sqlcmd 和 bcp 等命令？

   提供。Linux、macOS 和 Windows 上本机提供 [sqlcmd 和 bcp](sql-server-linux-setup-tools.md)。 此外，可使用 Linux、macOS 或 Windows 上的新 [mssql-scripter](https://github.com/Microsoft/mssql-scripter) 命令行工具为在任何位置运行的 SQL 数据库生成 T-SQL 脚本。 另请参阅 [mssql-cli](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/) 的预览版本。

1. 通过 Windows 上的 SSMS 进行连接时，是否可以针对 Linux 上运行的实例查看 Activity Monitor？

   可以。可以使用 Windows 上的 SSMS 进行远程连接，并对 Linux 实例使用 Activity Monitor 命令之类的工具/功能。

1. 有哪些工具可用于监视 Linux 上的 SQL Server 性能？

   可使用[系统动态管理视图 (DMV)](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) 收集有关 SQL Server 的各种类型的信息，包括 Linux 进程信息。 可使用[查询存储](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)提高查询性能。 其他工具（例如，内置[性能仪表板](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in)）可在 Windows 的 SQL Server Management Studio (SSMS) 中远程工作。

   > [!TIP]
   > 正确配置 Linux 操作系统和 SQL Server 实例是提高性能的一种方法。 有关详细信息，请参阅 [Linux 上的 SQL Server 的性能最佳做法和配置指南](sql-server-linux-performance-best-practices.md)。

## <a name="administration"></a>管理

1. Microsoft 是否在 Linux 上创建了类似 SQL Server 配置管理器的应用？

   是的，存在适用于 Linux 上的 SQL Server 的配置工具：[mssql-conf](sql-server-linux-configure-mssql-conf.md)。

1. Linux 上的 SQL Server 是否支持同一主机上的多个实例？

   我们建议在主机上运行多个容器以获得多个不同的实例。 使用 docker 很容易实现这一点，但每个容器都需要侦听不同的端口。 有关详细信息，请参阅[运行多个 SQL Server 容器](./sql-server-linux-docker-container-deployment.md#multiple)。

1. **Linux 上是否支持 Active Directory 身份验证？**

   是的。 有关详细信息，请参阅[对 Linux 上的 SQL Server 使用 Active Directory 身份验证](sql-server-linux-active-directory-authentication.md)。

1. Linux 是否支持 Always On 和群集？

   Linux 上使用 Pacemaker 实现故障转移群集和高可用性。 有关详细信息，请参阅 [业务连续性和数据库恢复 - Linux 上的 SQL Server](sql-server-linux-business-continuity-dr.md)。

1. 是否可以配置 Linux 与 Windows 之间的相互复制？

   可以在 Windows 和 Linux 之间使用读取规模副本进行单向数据复制。

1. 是否可以将旧版 SQL Server 中的现有数据库从 Windows 迁移到 Linux？

   可以。可以通过[几种方法](sql-server-linux-migrate-overview.md)实现此目的。

1. 是否可以将 Oracle 和其他数据库引擎中的数据迁移到 Linux 上的 SQL Server？

   是的。 SSMA 支持从几种类型的数据库引擎进行迁移：Microsoft Access、DB2、MySQL、Oracle 和 SAP ASE（以前称为 SAP Sybase ASE）。 有关如何使用 SSMA 的示例，请参阅[使用 SQL Server 迁移助手将 Oracle 架构迁移到 Linux 上的 SQL Server](../ssma/oracle/sql-server-linux-convert-from-oracle.md?toc=/sql/toc/toc.json)。

1. SQL Server 文件需要哪些权限？

   `/var/opt/mssql` 文件夹中的所有文件都应归 mssql 用户所有且属于 mssql 组 。 mssql 用户和组都应具有所有文件和目录的读写权限。 请注意以下涉及文件和目录权限的特殊情况：

   * 用于存储 SQL Server 文件的已装载网络共享必须拥有 mssql 所有者和组的权限。
   * 如果在非默认目录中找到数据库文件或备份，则也必须为该目录设置权限。
   * 如果更改 0022 中的默认根 umask，则安装后 SQL Server 配置将失败。 然后，必须手动将所需权限应用于 SQL Server 启动帐户。

1. 是否可以更改已安装的 mssql 帐户和组中的 SQL Server 文件和目录的所有权？

   我们不支持更改默认安装中的 SQL Server 目录和文件的所有权。 mssql 帐户和组专门用于 SQL Server 且不支持交互式登录访问。
   
 1. SQL Server 数据和日志目录是否支持符号链接？ 
    
    否，SQL Server 数据和日志目录不支持符号链接。 若要更改默认数据和日志目录，请参阅[更改默认数据或日志目录位置](sql-server-linux-configure-mssql-conf.md#datadir)。
    
[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]