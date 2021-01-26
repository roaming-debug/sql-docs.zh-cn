---
title: 使用 SSMS 管理 Linux 上的 SQL Server
description: 本文介绍 QL Server Management Studio，它是用于访问、配置、管理和开发 SQL Server 的组件的集成环境。
author: VanMSFT
ms.author: vanto
ms.date: 05/21/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: b2fcf858-21c3-462a-8d49-50c85647d092
ms.openlocfilehash: f5ba3ea9260da9e278273ef39d0f5893b67c120e
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596431"
---
# <a name="use-sql-server-management-studio-on-windows-to-manage-sql-server-on-linux"></a>使用 Windows 上的 SQL Server Management Studio 管理 Linux 上的 SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍 [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md)，并演示几个常见任务。 SSMS 是一个 Windows 应用程序，因此请在 Windows 计算机可连接到 Linux 上的远程 SQL Server 实例时使用 SSMS。

> [!TIP]
> 如果没有运行 SSMS 的 Windows 计算机，请考虑新的 [Azure Data Studio](../azure-data-studio/index.yml)。 它提供了管理 SQL Server 的图形工具，并在 Linux 和 Windows 上运行。

[SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) 是 SQL 工具套件的一部分，Microsoft 免费提供此工具套件，用于满足开发和管理需求。 SSMS 是用于访问、配置、管理和开发所有 SQL Server 的组件的集成环境。 它可以连接到在本地、在 Docker 容器中和云中的任何平台上运行的 SQL Server。 它还连接到 Azure SQL 数据库和 Azure Synapse Analytics。 SSMS 将大量图形工具与丰富的脚本编辑器相结合，各种技术水平的开发人员和管理员都能访问 SQL Server。

SSMS 提供适用于 SQL Server 的大量开发和管理功能，包括执行以下任务的工具：

- 配置、监视和管理一个或多个 SQL Server 实例
- 部署、监视和升级数据层组件（如数据库和数据仓库）
- 备份和还原数据库
- 生成并执行 T-SQL 查询和脚本，再查看结果
- 生成数据库对象的 T-SQL 脚本
- 查看和编辑数据库中的数据
- 以直观方式设计 T-SQL 查询和数据库对象，例如视图、表和存储过程

有关 SSMS 的详细信息，请参阅[什么是 SSMS？](../ssms/sql-server-management-studio-ssms.md)。

## <a name="install-the-newest-version-of-sql-server-management-studio-ssms"></a>安装最新版本的 SQL Server Management Studio (SSMS)

使用 SQL Server 时，应始终使用最新版本的 SQL Server Management Studio (SSMS)。 最新版本的 SSMS 不断进行更新和优化，目前适用于 Linux 上的 SQL Server。 若要下载和安装最新版本，请参阅 [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。 为保持使用最新版本，有可供下载的新版本时，最新版本的 SSMS 会发出提示。

> [!NOTE]
> 使用 SSMS 管理 Linux 前，请参阅 Linux 上关于 SSMS 的[已知问题](sql-server-linux-release-notes.md)。

## <a name="connect-to-sql-server-on-linux"></a>连接到 Linux 上的 SQL Server

使用以下基本步骤进行连接：

1. 在 Windows 搜索框内键入 Microsoft SQL Server Management Studio 以启动 SSMS，然后单击桌面应用。

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png)

1. 在“连接到服务器”窗口中，输入下列信息（如果 SSMS 正在运行，请单击“连接”>“数据库引擎”，打开“连接到服务器”窗口）  ：

   | 设置 | 说明 |
   |-----|-----|
   | **服务器类型** | 默认为数据库引擎；请勿更改此值。 |
   | **服务器名称** | 输入目标 Linux SQL Server 计算机的名称，或者输入它的 IP 地址和端口（格式为 `IP,port`）。 |
   | **身份验证** | 对于 Linux 上的 SQL Server，请使用 SQL Server 身份验证。 |
   | **登录** | 输入对服务器上的数据库具有访问权限的用户名（例如，在安装时创建的默认 SA 帐户）。 |
   | **密码** | 为指定的用户输入密码（如果是 SA 帐户，则在安装时已创建密码）。 |

    ![SQL Server Management Studio：连接到 SQL Database 服务器](./media/sql-server-linux-manage-ssms/connect.png)

1. 单击“连接”。

    > [!TIP]
    > 如果连接失败，先尝试诊断错误消息中所述的问题。 然后查看[连接故障排除建议](sql-server-linux-troubleshooting-guide.md#connection)。
 
1. 成功连接到 SQL Server 之后，将打开“对象资源管理器”，现在即可访问数据库来执行管理任务或查询数据。

## <a name="run-transact-sql-queries"></a>运行 Transact-SQL 查询

连接到服务器后，可以连接到数据库并运行 Transact-SQL 查询。 Transact-SQL 查询几乎可以用于任何数据库任务。

1. 在“对象资源管理器”中，导航到服务器上的目标数据库。 例如，展开“系统数据库”以使用 master 数据库 。

1. 右键单击该数据库，然后选择“新建查询”。

1. 在查询窗口中编写 Transact-SQL 查询，以选择返回服务器上所有数据库的名称。

   ```sql
   SELECT [Name]
   FROM sys.Databases
   ```

   如果不熟悉编写查询，请参阅[编写 Transact-SQL 语句](../t-sql/tutorial-writing-transact-sql-statements.md)。

1. 单击“执行”按钮以运行查询并查看结果。

   ![成功。 连接到 SQL Database 服务器：SQL Server Management Studio](./media/sql-server-linux-manage-ssms/execute-query.png)

尽管可以使用 Transact-SQL 查询执行几乎任何的管理任务，但 SSMS 是一种可更轻松管理 SQL Server 的图形工具。 以下部分提供使用图形用户界面的一些示例。

## <a name="create-and-manage-databases"></a>创建和管理数据库

连接到主数据库时，可以在服务器上创建数据库，并修改或删除现有数据库。 以下步骤介绍如何通过 Management Studio 完成几项常见的数据库管理任务。 为执行这些任务，请确保已连接到主数据库（使用在 Linux 上设置 SQL Server 时创建的服务器级别主体登录名）。

### <a name="create-a-new-database"></a>新建数据库

1. 启动 SSMS 并连接到 Linux 上的 SQL Server 中的服务器

2. 在对象资源管理器中，右键单击“数据库”文件夹，然后单击“新建数据库...”

3. 在“新建数据库”对话框中，输入新数据库的名称，然后单击“确定” 

已成功在服务器中创建新数据库。 如果想使用 T-SQL 创建新数据库，请参阅 [CREATE DATABASE (SQL Server Transact-SQL)](../t-sql/statements/create-database-transact-sql.md)。

### <a name="drop-a-database"></a>删除数据库

1. 启动 SSMS 并连接到 Linux 上的 SQL Server 中的服务器

2. 在对象资源管理器中，展开“数据库”文件夹，查看服务器上的所有数据库的列表。

3. 在对象资源管理器中，右键单击要删除的数据库，然后单击“删除”

4. 在“删除对象”对话框中，选择“关闭现有连接”，然后单击“确定”  

已成功从服务器中删除数据库。 如果想使用 T-SQL 删除数据库，则请参阅 [DROP DATABASE (SQL Server Transact-SQL)](../t-sql/statements/drop-database-transact-sql.md)。

## <a name="use-activity-monitor-to-see-information-about-sql-server-activity"></a>使用活动监视器查看有关 SQL Server 活动的信息

[活动监视器](../relational-databases/performance-monitor/activity-monitor.md)工具是 SQL Server Management Studio (SSMS) 的内置工具，用于显示 SQL Server 进程以及这些进程如何影响 SQL Server 当前实例的相关信息。

1. 启动 SSMS 并连接到 Linux 上的 SQL Server 中的服务器

1. 在对象资源管理器中，右键单击“服务器”节点，然后单击“活动监视器” 

活动监视器显示可展开和可折叠的窗格，提供以下信息：

- 概述
- 进程
- 资源等待
- 数据文件 I/O
- 最近耗费大量资源的查询
- 耗费大量资源的活动查询

展开某个窗格时，活动监视器会查询实例获取相关信息。 折叠窗格时，该窗格的所有查询活动都将停止。 可以同时展开一个或多个窗格，以查看实例上不同种类的活动。

## <a name="see-also"></a>另请参阅
- [什么是 SSMS？](../ssms/sql-server-management-studio-ssms.md)
- [使用 SSMS 导出和导入数据库](sql-server-linux-migrate-ssms.md)
- [教程：SQL Server Management Studio](../ssms/quickstarts/ssms-connect-query-sql-server.md)
- [教程：编写 Transact-SQL 语句](../t-sql/tutorial-writing-transact-sql-statements.md)
- [服务器性能和活动监视](../relational-databases/performance/server-performance-and-activity-monitoring.md)