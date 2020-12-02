---
description: 使用 Transact-SQL 从 Visual Studio Code 运行 SSIS 包
title: 使用 Transact-SQL (VS Code) 运行 SSIS 包 | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: dcbd93a2e1e40c58cd92bd71449ba9a9155ba5b2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92006202"
---
# <a name="run-an-ssis-package-from-visual-studio-code-with-transact-sql"></a>使用 Transact-SQL 从 Visual Studio Code 运行 SSIS 包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


本快速入门演示如何使用 Visual Studio Code 连接到 SSIS 目录数据库，然后使用 Transact-SQL 语句运行存储在 SSIS 目录中的 SSIS 包。

Visual Studio Code 是支持扩展的代码编辑器，适用于 Windows、macOS 和 Linux，其支持的扩展包括用于连接到 Microsoft SQL Server、Azure SQL 数据库或 Azure Synapse Analytics 的 `mssql` 扩展。 有关 VS Code 的详细信息，请参阅 [Visual Studio Code](https://code.visualstudio.com/)。

## <a name="prerequisites"></a>必备条件

在开始之前，请确保已安装最新版本的 Visual Studio Code 并已加载 `mssql` 扩展。 若要下载这些工具，请参阅以下页面：
-   [下载 Visual Studio Code](https://code.visualstudio.com/Download)
-   [mssql 扩展](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)

## <a name="supported-platforms"></a>受支持的平台

可使用此快速入门中的信息在以下平台上运行 SSIS 包：

-   Windows 上的 SQL Server。

-   Azure SQL 数据库。 有关在 Azure 中部署和运行包的详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。

无法使用此快速入门中的信息在 Linux 上运行 SSIS 包。 有关在 Linux 上运行包的详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../linux/sql-server-linux-migrate-ssis.md)。

## <a name="set-language-mode-to-sql-in-vs-code"></a>在 VS Code 中将语言模式设置为 SQL

若要启用 `mssql` 命令和 T-SQL IntelliSense，请在 Visual Studio Code 中将语言模式设置为 SQL。

1. 打开 Visual Studio Code，然后打开一个新窗口。 

2. 在状态栏的右下角单击“纯文本”。

3. 在打开的“选择语言模式”下拉菜单中选择或输入“SQL”，然后按 ENTER 将语言模式设置为 SQL。 

## <a name="for-azure-sql-database-get-the-connection-info"></a>对于 Azure SQL 数据库，请获取连接信息

要在 Azure SQL 数据库上运行包，请获取连接到 SSIS 目录数据库 (SSISDB) 所需的连接信息。 在接下来的步骤中需要完全限定的服务器名称和登录信息。

1. 登录到 [Azure 门户](https://portal.azure.com/)。
2. 从左侧的菜单选择“SQL 数据库”，然后选择“SQL 数据库”页中的 SSISDB 数据库。 
3. 在数据库的“概述”页上，查看完全限定的服务器名称。 若想查看“单击以复制”选项，将鼠标悬停在服务器名称上。 
4. 如果忘记了 Azure SQL 数据库服务器登录信息，导航到 SQL 数据库服务器页以查看服务器管理员名称。 如有必要，可重置密码。

## <a name="connect-to-the-ssis-catalog-database"></a>连接到 SSIS 目录数据库

使用 Visual Studio Code 来建立到 SSIS 目录的连接。

> [!IMPORTANT]
> 在继续之前，请确保已准备好服务器、数据库和登录信息等相关信息。 开始输入连接配置文件信息后，如果将焦点从 Visual Studio Code 处移开，则需要重新进行连接配置文件的创建。

1. 在 VS Code 中，按 CTRL+SHIFT+P（或 F1）打开命令面板。

2. 键入 sqlcon，然后按 ENTER。

3. 按 **ENTER** 选择“创建连接配置文件”。 本步骤是为 SQL Server 实例创建连接配置文件。

4. 按照提示为新连接配置文件指定连接属性。 指定每个值后，按 **ENTER** 继续。 

   | 设置       | 建议的值 | 更多信息 |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **服务器名称** | 完全限定的服务器名称 | 如果要连接到 Azure SQL 数据库服务器，该名称为以下格式：`<server_name>.database.windows.net`。 |
   | **数据库名称** | **SSISDB** | 要连接到的数据库的名称。 |
   | **身份验证** | SQL 登录名 | 使用 SQL Server 身份验证，可连接到 SQL Server 或 Azure SQL 数据库。 如果连接到 Azure SQL 数据库服务器，则无法使用 Windows 身份验证。 |
   | **用户名** | 服务器管理员帐户 | 此帐户是在创建服务器时指定的帐户。 |
   | **密码(SQL 登录名)** | 服务器管理员帐户的密码 | 此密码是在创建服务器时指定的密码。 |
   | **是否保存密码？** | 是或否 | 如果不希望每次都输入密码，请选择“是”。 |
   | **输入此配置文件的名称** | 一个配置文件名称，如 mySSISServer | 保存一个配置文件名称可以加快后续登录时的连接速度。 | 

5. 按 ESC 键关闭信息消息，该消息通知你，配置文件已创建并且已连接。

6. 在状态栏中验证连接。

## <a name="run-the-t-sql-code"></a>运行 T-SQL 代码
运行以下 Transact-SQL 代码以便运行 SSIS 包。

1. 在“编辑器”窗口中，在空查询窗口中输入以下查询。 （此代码是由 SSMS “执行包”对话框中的“脚本”选项生成的代码。）

2. 为系统更新 `catalog.create_execution` 存储过程中的参数值。

3. 按 CTRL+SHIFT+E 运行代码并运行包。

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx',
    @execution_id=@execution_id OUTPUT,
    @folder_name=N'Deployed Projects',
      @project_name=N'Integration Services Project1',
    @use32bitruntime=False,
      @reference_id=Null
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,
    @object_type=50,
      @parameter_name=N'LOGGING_LEVEL',
      @parameter_value=@var0
EXEC [SSISDB].[catalog].[start_execution] @execution_id
GO
```

## <a name="next-steps"></a>后续步骤
- 考虑运行包的其他方式。
    - [使用 SSMS 运行 SSIS 包](./ssis-quickstart-run-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [从命令提示符运行 SSIS 包](./ssis-quickstart-run-cmdline.md)
    - [使用 PowerShell 运行 SSIS 包](ssis-quickstart-run-powershell.md)
    - [使用 C# 运行 SSIS 包](./ssis-quickstart-run-dotnet.md) 
